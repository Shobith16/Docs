# Flutter App Optimization Guide

### Performance, Resource Hygiene, and Reduced Plugin Dependency for an Existing App

This document is written for a team that already ships a Flutter app and wants to make it feel smoother, leak less, depend on fewer third‑party packages, and stay easy to debug, scalable, secure, and reliable. It is deliberately opinionated and ordered so you can apply it incrementally without a rewrite.

It also corrects a few common myths repeated in popular "Flutter performance tips" posts (for example, the redundant nested `const`, and the "reduces rebuilds by 70%" style numbers, which are not measurable claims).

Reference material:
- https://docs.flutter.dev/perf/best-practices
- https://docs.flutter.dev/perf/rendering-performance
- https://docs.flutter.dev/perf/ui-performance

---

## 1. The one rule that governs everything: measure first

You cannot optimize what you have not measured, and on an existing app "obvious" fixes are frequently wrong. Adopt this loop and never skip it:

1. Reproduce the slow scenario.
2. Profile it on a **real device in profile mode** (never debug mode — debug mode is 3–10× slower and misleads you).
3. Form one hypothesis, change one thing.
4. Re‑measure against the baseline.
5. Keep the change only if the numbers moved.

`flutter run --profile` is your default while investigating. Debug mode is for correctness, release mode is for shipping, profile mode is for performance work.

### Know which thread is janking

This is the single most important concept the tip‑articles omit. There are two threads, and they fail for different reasons and have different fixes:

- **UI (Dart) thread** — runs your `build()`, layout, and application logic. Jank here means you are doing too much work per frame in Dart: rebuilding too much, sorting/filtering in `build`, decoding JSON on the main thread, etc.
- **Raster (GPU) thread** — turns the layer tree into pixels. Jank here means expensive painting: overdraw, large blurs, saveLayer/`Opacity`, clipping with anti‑aliasing, or (historically) shader compilation.

Open the performance overlay and look at the two graphs. If the **top** graph spikes, it is a raster problem. If the **bottom** graph spikes, it is a UI‑thread problem. Fixing the wrong one wastes days.

### The frame budget

| Refresh rate | Budget per frame | What must fit |
|---|---|---|
| 60 Hz | ~16.6 ms | build + layout + paint + raster |
| 90 Hz | ~11.1 ms | same |
| 120 Hz | ~8.3 ms | same |

Both threads must finish inside the budget. Aim to leave headroom; a frame that takes 16 ms on your flagship dev phone will drop on a mid‑range device.

### Tools you already have (no plugins required)

- **Flutter DevTools** — Performance view (frame timeline), CPU profiler, Memory view, and the widget rebuild/repaint tools.
- **Performance overlay** — `MaterialApp(showPerformanceOverlay: true)` or toggle from DevTools.
- **Track widget rebuilds** — in DevTools, "Track Widget Builds", or `debugProfileBuildsEnabled = true`, or `debugPrintRebuildDirtyWidgets = true`.
- **Repaint rainbow** — `debugRepaintRainbowEnabled = true` shows what repaints (each repaint changes colour). Static UI that keeps changing colour is repainting needlessly.
- **Memory view + leak tracking** — the framework ships a leak tracker in debug builds that reports disposed‑but‑retained objects; watch the console.

### A note on shader jank and Impeller

The classic "first run of an animation stutters" problem was shader compilation jank. Impeller — now the default rendering engine — precompiles its shaders and largely eliminates this class of jank, so the old SkSL warm‑up workaround is generally unnecessary on Impeller. If you still target the legacy Skia backend anywhere, be aware the tradeoff differs. Verify behaviour on your minimum‑spec device rather than assuming.

---

## 2. Rendering and rebuild optimisation

Everything here reduces work on the **UI thread**.

### 2.1 `const` correctly (and the myth to drop)

`const` widgets are canonicalised at compile time: identical const widgets are the *same* instance, so the framework can skip rebuilding that subtree because the element sees an unchanged widget. That is the real mechanism — not a vague "caching."

Correct usage, and the correction most tip‑posts get wrong:

```dart
// ❌ Redundant: the inner `const` is unnecessary and the analyzer flags it
// (unnecessary_const). In a const context, children are ALREADY const.
const Container(
  padding: EdgeInsets.all(16.0),
  child: const Text('Hello World'), // <-- drop this const
)

// ✅ Correct: one const at the top; children are implicitly const
const Container(
  padding: EdgeInsets.all(16.0),
  child: Text('Hello World'),
)
```

Turn on the lints so this is automatic rather than manual discipline. In `analysis_options.yaml`:

```yaml
include: package:flutter_lints/flutter.yaml

linter:
  rules:
    prefer_const_constructors: true
    prefer_const_constructors_in_immutables: true
    prefer_const_declarations: true
    prefer_const_literals_to_create_immutables: true
    unnecessary_const: true
```

Do **not** quote fixed percentages ("70% fewer rebuilds") in your own docs or PRs. The gain depends entirely on your tree; measure it in DevTools instead.

### 2.2 Push rebuilds down to the leaves

A `setState` rebuilds the widget *and its entire subtree*. The goal is to make the widget that calls `setState` as small and low in the tree as possible, so expensive siblings are untouched.

```dart
// ❌ Counter change rebuilds AppBar, header, and footer too
class BadHomePage extends StatefulWidget { /* ...counter lives at the top... */ }

// ✅ Only the counter subtree rebuilds; everything else is const and stable
class GoodHomePage extends StatelessWidget {
  const GoodHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('My App')),
      body: Column(
        children: const [
          ExpensiveHeader(), // stable, not rebuilt
          _Counter(),        // isolates its own state
          ExpensiveFooter(), // stable, not rebuilt
        ],
      ),
    );
  }
}

class _Counter extends StatefulWidget {
  const _Counter();
  @override
  State<_Counter> createState() => _CounterState();
}

class _CounterState extends State<_Counter> {
  int _count = 0;
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      children: [
        Text('Counter: $_count'),
        FloatingActionButton(
          onPressed: () => setState(() => _count++),
          child: const Icon(Icons.add),
        ),
      ],
    );
  }
}
```

### 2.3 Keep `build()` pure and cheap

`build()` can be called every frame. It must be fast and free of side effects.

- Never do I/O, network calls, or database reads in `build()`.
- Never `sort`, `filter`, or map large collections in `build()` — do it once and cache the result, or move it to a controller/notifier.
- Never allocate controllers (`AnimationController`, `ScrollController`, `TextEditingController`) in `build()` — create them in `initState` and dispose them.
- Be careful with `MediaQuery.of(context)` — depending on the whole `MediaQuery` rebuilds the widget on every metric change (keyboard, rotation). Prefer targeted accessors (`MediaQuery.sizeOf`, `MediaQuery.paddingOf`, etc.) so you only rebuild on the property you use.

### 2.4 `RepaintBoundary`: isolate the busy parts

Wrap a frequently‑repainting subtree (a spinner, a progress bar, an animated widget) in a `RepaintBoundary` so its repaints do not force the static content around it to repaint. Confirm with the repaint rainbow that you actually reduced repaint area — over‑using boundaries costs memory, so add them where the rainbow shows unwanted repaints, not everywhere.

### 2.5 Scoped rebuilds — the plugin‑free path

You do **not** need a state‑management package to get granular rebuilds. The SDK already gives you targeted listeners:

```dart
// A ValueNotifier + ValueListenableBuilder rebuilds ONLY the builder,
// not the surrounding widget. Zero third-party dependencies.
final counter = ValueNotifier<int>(0);

ValueListenableBuilder<int>(
  valueListenable: counter,
  builder: (context, value, child) => Text('Counter: $value'),
);

// ListenableBuilder works with any ChangeNotifier for slightly richer state.
// AnimatedBuilder + its `child` param rebuilds only the animated wrapper.
```

Reach for `ValueNotifier`/`ValueListenableBuilder`, `ChangeNotifier`/`ListenableBuilder`, or `InheritedNotifier` before adding Riverpod/BLoC/Provider. A package is justified when your state graph is large, cross‑cutting, and needs dependency wiring and testability at scale — not for a counter or a single screen. (See §5 on why fewer packages is a goal.)

---

## 3. Lists and scrolling

Long lists are the most common source of scroll jank and memory pressure.

- Always use `ListView.builder` / `ListView.separated` / `GridView.builder` — never a `ListView(children: [...])` for long or unbounded lists (that builds every item up front).
- If every row is the same height, set `itemExtent` (or `prototypeItem`) — it lets Flutter skip per‑item layout measurement. Only do this when heights are truly uniform, otherwise items render wrong.
- Give items stable `ValueKey`s so element/state identity survives reorders and insertions.
- Tune `cacheExtent` to pre‑build a little offscreen content for smoother flings; don't set it huge, that just builds more.
- Never nest scrollables of the same axis. For complex scroll UIs (headers that collapse, mixed content) use **slivers** (`CustomScrollView` + `SliverList`/`SliverAppBar`) rather than stacking widgets.
- Turn off `addAutomaticKeepAlives`/`addRepaintBoundaries` only when you have measured they aren't needed; they default on for good reasons.

```dart
ListView.builder(
  itemCount: items.length,
  itemExtent: 80.0,                 // only because rows are uniform height
  cacheExtent: 250.0,
  physics: const ClampingScrollPhysics(),
  itemBuilder: (context, i) => _Row(
    key: ValueKey(items[i].id),     // stable identity
    item: items[i],
  ),
);
```

**Paginate/lazy‑load** long remote lists (load the next page when you approach the end) so you never hold thousands of models and widgets in memory at once.

---

## 4. Images and memory (a big, plugin‑free win)

Images are usually the largest single memory consumer, and this is where teams reach for a plugin unnecessarily.

### The key fact: decode resizing is built in

An image decoded at its full resolution is stored in memory at full resolution — a 4000×3000 photo shown in a 200×200 box wastes ~24× the RAM it needs. Fix it with the **built‑in** `cacheWidth`/`cacheHeight` (or `ResizeImage`), no package required:

```dart
// ✅ Built-in memory optimisation. Decodes to the display size, not the source size.
Image.network(
  url,
  width: 200,
  height: 200,
  fit: BoxFit.cover,
  cacheWidth: 200,   // <-- decode at this width; huge RAM saving
  cacheHeight: 200,
  loadingBuilder: (c, child, progress) =>
      progress == null ? child : const Center(child: CircularProgressIndicator()),
  errorBuilder: (c, e, s) => const Icon(Icons.error),
);
```

`Image.network` also already uses Flutter's in‑memory `ImageCache`, so repeated loads of the same URL are not re‑downloaded within a session. You can bound it:

```dart
// e.g. in main(), tune the global image cache
PaintingBinding.instance.imageCache
  ..maximumSize = 200                    // number of images
  ..maximumSizeBytes = 120 << 20;        // ~120 MB ceiling
```

### When (and only when) a plugin is worth it

`cached_network_image` is a *plugin*. Its real added value is **disk‑persistent** caching across app launches — not memory sizing (which is built in) and not download dedup within a session (also built in). Add it only if you genuinely need cross‑session disk caching and don't want to write a small cache yourself. This is a concrete example of the pattern in §5: don't pull a package for something the SDK already does.

Other image wins, all plugin‑free:
- Ship right‑sized, multi‑resolution assets (`1.5x`, `2.0x`, `3.0x` folders) instead of one giant asset.
- Prefer WebP for photographic assets (smaller than PNG/JPEG at similar quality).
- `precacheImage()` critical above‑the‑fold images during a loading screen so they don't pop in.

---

## 5. Reducing plugin dependency (a first‑class goal)

Every third‑party package is a liability you carry forever: it can be abandoned, break on the next OS/Gradle/Xcode bump, conflict with another package's version constraints, bloat your binary, slow your upgrades, and — for native plugins — pull in code you can't easily audit. Fewer dependencies means a smaller attack surface, faster SDK upgrades, and less mysterious breakage.

This does **not** mean "reinvent everything." It means **add packages deliberately** and **isolate the ones you keep**.

### 5.1 A decision checklist before adding any package

Ask, in order:

1. **Is it already in the Flutter/Dart SDK?** (Networking, JSON, image resizing, scoped rebuilds, files, isolates, animations — often yes.)
2. **Is it a few files I can vendor and own?** (A small utility rarely justifies a dependency + its transitive tree.)
3. **Is it healthy?** Check pub.dev: recent publish date, high pub points, is it null‑safe/current, is it a **first‑party** (`flutter.dev`/`dart.dev`) or verified publisher, and how big is its transitive dependency tree.
4. **Does it pull native (platform) code?** Native plugins are the highest‑maintenance and highest‑risk category. Prefer pure‑Dart when a choice exists.
5. **Is the licence compatible** with your product?

If it clears all five, add it — and wrap it (§5.3).

### 5.2 Common plugin‑free (or first‑party) swaps

| Instead of reaching for… | Consider the built‑in / first‑party option |
|---|---|
| `cached_network_image` for memory sizing | `Image.network` + `cacheWidth`/`cacheHeight`; the plugin only earns its keep for **disk** caching |
| A state‑management package for simple state | `ValueNotifier` + `ValueListenableBuilder`, `ChangeNotifier` + `ListenableBuilder`, `InheritedNotifier` |
| A fat HTTP client wrapper | `package:http` (first‑party) or `dart:io HttpClient` |
| `get_it` for trivial DI | Constructor injection / a small `InheritedWidget` locator |
| A UUID package | Generate from `Random.secure()` / `dart:math` + `crypto` if you need it |
| An SVG renderer for static art | Pre‑rasterise to WebP/PNG at build time |
| A "shared prefs on steroids" wrapper | `shared_preferences` (first‑party) for simple keys; plain files for structured data |
| A date/format package | `intl` (first‑party) |

Keep the packages that genuinely pull their weight — secure storage, crash reporting, camera/BLE/platform integrations, and crypto are things you should **not** hand‑roll.

### 5.3 Isolate every plugin you do keep (ports & adapters)

Never let a package's types leak throughout your codebase. Put each dependency behind **your own interface**, and depend on the interface everywhere else:

```dart
// Your app depends on THIS, not on any package:
abstract interface class KeyValueStore {
  Future<String?> read(String key);
  Future<void> write(String key, String value);
  Future<void> delete(String key);
}

// One adapter wraps the current plugin. Swapping the plugin later
// (or writing a plugin-free implementation) touches ONLY this file.
class SecureKeyValueStore implements KeyValueStore {
  // ...delegates to flutter_secure_storage here...
}
```

Benefits: you can swap or remove the plugin by editing one file, you can unit‑test with a fake, and your architecture stays clean (§7). This is the mechanism that makes "reduce plugin dependency" *achievable over time* in an existing app rather than a one‑off purge.

### 5.4 Audit what you already have

```bash
flutter pub outdated          # what's stale / has newer majors
flutter pub deps              # full dependency tree, incl. transitive
dart pub deps --style=compact # quick overview
```

Remove packages nothing imports, consolidate two packages that do the same job, and list your native plugins explicitly — those are the ones most likely to break on the next platform update.

---

## 6. Async work, isolates, and never blocking the UI thread

Any CPU‑heavy or blocking work on the UI thread drops frames. Move it off.

```dart
// ❌ Blocks the UI thread: parsing a big payload on the main isolate
final parsed = expensiveParse(hugeJsonString);

// ✅ Modern API: run it on a background isolate
final parsed = await Isolate.run(() => expensiveParse(hugeJsonString));

// ✅ Also fine (older API, top-level or static function required):
final parsed = await compute(expensiveParse, hugeJsonString);
```

Guidelines:
- Use `Isolate.run` (or `compute`) for JSON parsing of large payloads, image processing, sorting/searching large datasets, cryptography, and any tight loop.
- Never `await` a slow future inside `build()`. Kick it off in `initState`/a controller and render loading/data/error states.
- Debounce rapid inputs (search‑as‑you‑type, scroll listeners) so you don't fire work every keystroke/pixel.
- Give every network call a **timeout** and handle cancellation so abandoned screens don't keep working.

**Critical safety rule:** after any `await` in a `State`, the widget may have been disposed. Guard `setState`:

```dart
Future<void> _load() async {
  final data = await repo.fetch();
  if (!mounted) return;           // <-- prevents "setState after dispose"
  setState(() => _data = data);
}
```

---

## 7. Animations

Mostly a **raster/UI** balance — keep them cheap and don't rebuild the world.

```dart
class Reveal extends StatefulWidget {
  const Reveal({super.key});
  @override
  State<Reveal> createState() => _RevealState();
}

class _RevealState extends State<Reveal> with SingleTickerProviderStateMixin {
  late final AnimationController _c =
      AnimationController(vsync: this, duration: const Duration(milliseconds: 300));
  late final Animation<double> _a =
      CurvedAnimation(parent: _c, curve: Curves.easeInOut);

  @override
  Widget build(BuildContext context) {
    return RepaintBoundary(                 // isolate the animation's repaints
      child: AnimatedBuilder(
        animation: _a,
        // `child` is built ONCE and reused each frame — big saving
        child: const ExpensiveWidget(),
        builder: (context, child) => Opacity(
          opacity: _a.value,
          child: Transform.translate(
            offset: Offset(0, 100 * (1 - _a.value)),
            child: child,
          ),
        ),
      ),
    );
  }

  @override
  void dispose() {
    _c.dispose();                            // ALWAYS dispose controllers
    super.dispose();
  }
}
```

- Prefer `AnimatedBuilder`/implicit animations (`AnimatedContainer`, `AnimatedOpacity`, `FadeTransition`) over driving `setState` yourself.
- Animate with `Transform`/opacity rather than triggering layout each frame.
- `Opacity` and `saveLayer` are expensive on the raster thread — prefer `FadeTransition`/`AnimatedOpacity`, or apply alpha to a colour, when you can.
- Wrap complex animations in `RepaintBoundary`.
- **Dispose every controller.** (See §8 — this is the #1 leak source.)

---

## 8. Resource management: fix the "wrong usage" and stop the leaks

This is where an existing app most often silently degrades: memory climbs the longer the app runs, and eventually everything janks. Almost all of it is failing to release what you acquired.

### 8.1 The acquire/release contract

Anything you create in `initState`/`didChangeDependencies` must be released in `dispose`. Anything you subscribe to must be cancelled.

| Resource | Acquire in | Release in |
|---|---|---|
| `AnimationController`, `TabController`, `PageController` | `initState` | `dispose()` → `.dispose()` |
| `ScrollController`, `TextEditingController`, `FocusNode` | `initState` | `dispose()` → `.dispose()` |
| `StreamSubscription` | `initState` | `dispose()` → `.cancel()` |
| `Timer` / `Ticker` | `initState` | `dispose()` → `.cancel()`/`.dispose()` |
| A listener you added (`addListener`, `addObserver`) | `initState` | `dispose()` → the matching `removeListener`/`removeObserver` |
| `Overlay`/`OverlayEntry` you inserted | on show | on hide → `.remove()` |

```dart
class _FeedState extends State<Feed> {
  final _scroll = ScrollController();
  StreamSubscription<Event>? _sub;
  Timer? _poll;

  @override
  void initState() {
    super.initState();
    _scroll.addListener(_onScroll);
    _sub = eventBus.stream.listen(_onEvent);
    _poll = Timer.periodic(const Duration(seconds: 30), (_) => _refresh());
  }

  @override
  void dispose() {
    _scroll.removeListener(_onScroll);   // remove listener…
    _scroll.dispose();                   // …then dispose the controller
    _sub?.cancel();                      // cancel the subscription
    _poll?.cancel();                     // cancel the timer
    super.dispose();
  }
}
```

### 8.2 The leak patterns to hunt for

- A `StreamSubscription` that is never cancelled (keeps the widget and its captured objects alive forever).
- `addListener` without a matching `removeListener`.
- `setState` called after `await` on a disposed widget (missing `if (!mounted) return;`).
- Closures that capture a `BuildContext` or a big object and outlive the widget.
- Long‑lived singletons/globals that accumulate entries and never evict (caches without a size limit — see §4).
- `GlobalKey`s held longer than needed.

### 8.3 Verify with tooling

Use the DevTools **Memory** view: take a snapshot, exercise the screen, navigate away, snapshot again — retained widgets/state that should be gone are leaks. In debug builds, watch the console for the framework's leak‑tracker warnings about disposed‑but‑retained objects. Fix the source, don't just null the reference.

### 8.4 Don't allocate on the hot path

Avoid creating new objects (lists, closures, `EdgeInsets`, `TextStyle`) inside `build()` when they never change — hoist them to `const`/`static final`. Constant churn creates GC pressure that shows up as periodic micro‑stutters.

---

## 9. Debuggability: make it easy to find and fix errors

Fast debugging is a design property, not luck.

- **Log, don't `print`.** Use `dart:developer`'s `log()` with names/levels so logs are filterable and stripped appropriately in release. Route through a thin logging interface so you can change backends later (§5.3).
- **Catch every error in one place** so nothing fails silently:

```dart
void main() {
  // Framework build/layout/paint errors
  FlutterError.onError = (details) {
    FlutterError.presentError(details);
    // forward to your logger / crash reporter
  };
  // Errors outside the framework (async, platform)
  WidgetsFlutterBinding.ensureInitialized();
  PlatformDispatcher.instance.onError = (error, stack) {
    // forward to your logger / crash reporter
    return true;
  };
  runApp(const MyApp());
}
```

- Provide a friendly release `ErrorWidget.builder` so a single widget error doesn't show users a red screen (or a blank one).
- Use `assert()` for invariants — they run in debug/profile and vanish in release, so they're free in production but catch bugs early.
- Give widgets meaningful `Key`s and clear names; anonymous deeply‑nested trees are miserable to debug in the widget inspector.
- Centralise environment/config and use `--dart-define` for build‑time flags (dev/staging/prod) instead of scattered constants.
- Keep a **consistent, discoverable folder structure** (see §10) so any engineer can find the code behind a screen quickly.

---

## 10. Scalability: architecture that grows without seizing up

Structure decides whether feature #200 is as easy as feature #2.

**Separate layers** and let dependencies point inward:

- **Presentation** — widgets + a controller/notifier per screen. UI is "dumb": it renders state and forwards intents.
- **Domain** — plain Dart: entities and use‑cases/business rules. No Flutter imports. Trivially unit‑testable.
- **Data** — repositories behind interfaces; data sources (network, db, cache) are implementation details.

**Feature‑first foldering** scales better than type‑first once you have more than a handful of screens:

```
lib/
  core/            # cross-cutting: logging, errors, theme, di, extensions
  features/
    auth/
      data/        # repositories + data sources (plugins live here, wrapped)
      domain/      # entities + use cases (pure Dart)
      presentation/# screens + controllers/notifiers
    feed/
    profile/
  app.dart
  main.dart
```

Principles that keep it scalable:
- **Dependency inversion + ports/adapters (§5.3):** plugins and platform code live only at the edges, behind interfaces. This is what simultaneously delivers scalability, testability, *and* the "reduce plugin dependency" goal — the rest of the app never knows or cares which package you used.
- **One state‑management approach**, applied consistently, so onboarding and debugging are predictable.
- **Modular routing** (a single typed router) and lazily‑constructed feature entry points so startup stays cheap as the app grows.
- Keep files small and single‑purpose; a 1,500‑line `build()` is an anti‑pattern regardless of performance.

---

## 11. Security

Fewer dependencies already shrinks your attack surface (§5). Add these:

- **Secrets/tokens:** store in the platform keychain/keystore via `flutter_secure_storage` — a *justified* plugin. **Never** put tokens, PII, or keys in `SharedPreferences` or plain files.
- **No secrets in source or the repo.** Inject at build with `--dart-define`/`--dart-define-from-file`; keep keys out of version control.
- **Transport:** HTTPS everywhere; add **certificate pinning** for high‑value endpoints; reject invalid certs.
- **Input & data:** validate and sanitise all external input; deserialize defensively; avoid dynamic code paths driven by untrusted data.
- **Obfuscate release builds:** `flutter build apk --obfuscate --split-debug-info=build/symbols` (and the same for iOS/appbundle). Keep the symbol files to de‑obfuscate crash traces.
- **Least privilege:** request only the permissions you use; remove permissions left over from a removed plugin.
- **Never log** tokens, passwords, or PII (ties back to §9's logging interface — scrub there).
- **Scan dependencies** for known vulnerabilities and keep them patched; each package you drop is one fewer to audit.

---

## 12. Reliability

- **Crash & error reporting** (Sentry, Crashlytics, or similar) wired into the handlers from §9 — a justified plugin. You cannot fix what you cannot see in the field.
- **Every async UI has three states:** loading, data, and error/empty — never a spinner that can hang forever or a blank screen on failure.
- **Resilient networking:** timeouts, retry with exponential backoff for idempotent calls, and graceful offline handling (cached/last‑known data where reasonable).
- **Testing pyramid:** many fast unit tests (pure domain logic is easy here thanks to §10), widget tests for UI behaviour, a few integration tests for critical flows, and golden tests to catch unintended visual changes.
- **CI on every PR:** `flutter analyze`, `dart format --set-exit-if-changed`, and the test suite must pass before merge.
- **Feature flags + staged rollout** so risky changes can be dark‑launched and rolled back without a new release.

---

## 13. Guidelines — quick Do / Don't reference

**Do**
- Profile in profile mode on a real, mid‑range device before and after every change.
- Identify UI‑thread vs raster‑thread jank before "fixing" anything.
- Mark everything you can `const`; enable the const lints.
- Keep `build()` pure, cheap, and allocation‑free on the hot path.
- Push `setState`/rebuilds down to the smallest possible subtree.
- Use `*.builder` lists, stable keys, and pagination for long/remote lists.
- Size images with built‑in `cacheWidth`/`cacheHeight`; bound the image cache.
- Move heavy work to `Isolate.run`/`compute`; guard `setState` with `if (!mounted)`.
- Dispose every controller, cancel every subscription/timer, remove every listener.
- Wrap each third‑party package behind your own interface at the app edge.
- Handle all errors centrally; log through an interface; report crashes from the field.

**Don't**
- Don't optimise from intuition or debug‑mode numbers.
- Don't nest a redundant `const` inside a const context.
- Don't quote invented "X% faster" figures — measure your own.
- Don't do I/O, sorting, or controller allocation inside `build()`.
- Don't build long lists with `ListView(children: [...])`.
- Don't load full‑resolution images into small widgets.
- Don't `await` slow work in `build()` or block the UI thread.
- Don't add a package for something the SDK or a first‑party package already does.
- Don't let a plugin's types leak across your codebase.
- Don't store secrets in `SharedPreferences` or commit them to the repo.
- Don't ship a screen without loading/error/empty states.

---

## 14. Master checklist

### A. Diagnosis & baseline
- [ ] The slow scenario is reproducible and documented.
- [ ] Profiled in **profile mode** on a real, mid‑range device.
- [ ] Determined whether jank is on the **UI** thread or the **raster** thread (performance overlay).
- [ ] Baseline frame times / memory captured before changes.
- [ ] Repaint rainbow checked for needless repaints.

### B. Rendering & rebuilds
- [ ] `const` applied wherever possible; `unnecessary_const` and `prefer_const_*` lints enabled.
- [ ] `build()` methods contain no I/O, sorting/filtering of large data, or controller allocation.
- [ ] `setState`/rebuilds pushed down to leaf widgets.
- [ ] Targeted `MediaQuery.*Of` used instead of full `MediaQuery.of` where possible.
- [ ] `RepaintBoundary` added around busy subtrees (verified with rainbow).
- [ ] Scoped rebuilds use `ValueListenableBuilder`/`ListenableBuilder` before any state package.

### C. Lists & scrolling
- [ ] All long/unbounded lists use `.builder`/`.separated`.
- [ ] `itemExtent`/`prototypeItem` set only where item heights are uniform.
- [ ] Stable `ValueKey`s on list items.
- [ ] Remote lists paginate / lazy‑load; no unbounded in‑memory model lists.
- [ ] No nested same‑axis scrollables; slivers used for complex scroll UIs.

### D. Images & memory
- [ ] `cacheWidth`/`cacheHeight` (or `ResizeImage`) used to decode at display size.
- [ ] Global `imageCache` size/bytes bounded.
- [ ] Resolution‑aware assets (1.5x/2x/3x) and WebP used where appropriate.
- [ ] `cached_network_image` present **only** where cross‑session disk caching is actually needed.
- [ ] Critical images `precache`d during loading.

### E. Async & isolates
- [ ] Heavy CPU work runs on `Isolate.run`/`compute`.
- [ ] No `await` on slow work inside `build()`.
- [ ] Rapid inputs (search, scroll) are debounced.
- [ ] Network calls have timeouts and cancellation.
- [ ] Every `setState` after `await` is guarded with `if (!mounted) return;`.

### F. Animations
- [ ] `AnimatedBuilder` uses the `child` param to avoid rebuilding static content.
- [ ] Implicit/`Transition` animations preferred over manual `setState`.
- [ ] Expensive `Opacity`/`saveLayer` avoided in favour of `FadeTransition`/alpha.
- [ ] Complex animations wrapped in `RepaintBoundary`.

### G. Resource cleanup (leaks)
- [ ] Every controller (`Animation`, `Scroll`, `Text`, `Tab`, `Page`, `Focus`) is disposed.
- [ ] Every `StreamSubscription` is cancelled.
- [ ] Every `Timer`/`Ticker` is cancelled/disposed.
- [ ] Every added listener/observer has a matching removal.
- [ ] Overlays/entries are removed when hidden.
- [ ] Memory snapshots before/after navigation show no retained screen state.
- [ ] Leak‑tracker console warnings are clean in debug.
- [ ] No per‑frame allocation of unchanging objects in `build()`.

### H. Plugin dependency
- [ ] Ran the 5‑question decision checklist before adding any new package.
- [ ] Preferred SDK / first‑party (`flutter.dev`/`dart.dev`) options where they exist.
- [ ] Each kept plugin is wrapped behind an app‑owned interface (ports/adapters).
- [ ] `flutter pub outdated` / `flutter pub deps` reviewed; unused packages removed.
- [ ] Overlapping packages consolidated; native plugins listed and tracked.

### I. Debuggability
- [ ] Logging goes through `dart:developer`/an interface, not `print`.
- [ ] `FlutterError.onError` and `PlatformDispatcher.onError` are wired up.
- [ ] Release `ErrorWidget.builder` shows a graceful fallback.
- [ ] Invariants use `assert`.
- [ ] Consistent feature‑first folder structure; meaningful keys/names.
- [ ] Environments configured via `--dart-define`.

### J. Scalability / architecture
- [ ] Presentation / domain / data layers are separated; domain is pure Dart.
- [ ] Dependencies point inward; plugins live only at the edges behind interfaces.
- [ ] One consistent state‑management approach.
- [ ] Typed, modular routing; features constructed lazily.
- [ ] Files are small and single‑purpose.

### K. Security
- [ ] Secrets/tokens in secure storage (keychain/keystore), never in prefs/plain files.
- [ ] No secrets in source or the repo; injected via `--dart-define`.
- [ ] HTTPS everywhere; certificate pinning on sensitive endpoints.
- [ ] External input validated/sanitised; safe deserialization.
- [ ] Release builds obfuscated with split debug symbols retained.
- [ ] Only required permissions requested; stale ones removed.
- [ ] Sensitive data never logged; dependencies scanned for vulnerabilities.

### L. Reliability & CI
- [ ] Crash/error reporting wired into the global handlers.
- [ ] Every async UI has loading, data, and error/empty states.
- [ ] Timeouts, retry‑with‑backoff, and offline handling in place.
- [ ] Unit/widget/integration/golden tests cover critical logic and flows.
- [ ] CI runs `flutter analyze`, `dart format --set-exit-if-changed`, and tests on every PR.
- [ ] Feature flags / staged rollout available for risky changes.

---

## 15. A pragmatic rollout order for an existing app

Don't try to do all of this at once. Sequence it so each step ships value and de‑risks the next:

1. **Instrument first.** Add crash/error reporting and structured logging (§9, §12). Now you can see reality.
2. **Measure the worst screens** (§1) and fix the top two or three jank sources — usually list/image/build issues (§2–4, §6).
3. **Stop the leaks** (§8). This alone often fixes "the app gets slow after 10 minutes."
4. **Introduce interfaces around your existing plugins** (§5.3) — no behaviour change, but now packages are swappable and testable.
5. **Audit and trim plugins** (§5.4), replacing the easy ones with built‑in/first‑party options.
6. **Harden security** (§11) and **backfill tests + CI** (§12) so regressions can't creep back in.
7. **Refactor toward the layered/feature‑first structure** (§10) opportunistically, one feature at a time, as you touch it.

Each pass leaves the app better, shippable, and never in a half‑rewritten state.

---

*Measure, change one thing, measure again. Prefer what the SDK already gives you, isolate what it doesn't, and clean up everything you acquire. Do that consistently and the app stays fast, lean, debuggable, scalable, secure, and reliable.*
Done