# flutter_responsive

![am-dev-responsive](https://user-images.githubusercontent.com/77476766/236667888-281f774b-9333-4766-9e3c-9aaadc17f784.jpg)

> A lightweight, production-ready responsive utility for Flutter — breakpoints, context extensions, builder widgets, and visibility control in a single file.

[![Flutter](https://img.shields.io/badge/Flutter-3.x-blue?logo=flutter)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.x-blue?logo=dart)](https://dart.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## ✨ Features

| Feature | Description |
|---|---|
| `Responsive` widget | Render different widgets per screen type |
| `ResponsiveBuilder` | Fine-grained builder exposing `ScreenType` & `Size` |
| `ResponsiveVisibility` | Show/hide widgets on specific screen types |
| `BuildContext` extensions | Ergonomic helpers like `context.isMobile`, `context.responsiveValue(...)` |
| `ResponsiveProvider` | Optional `InheritedWidget` for custom global breakpoints |
| Customisable breakpoints | Override defaults via `ResponsiveBreakpoints` |
| `MediaQuery.sizeOf` | Uses the modern, rebuild-efficient API (Flutter 3.10+) |
| Dart 3 pattern matching | `switch` expressions for clean, exhaustive branching |

---

## 📐 Default Breakpoints

| Screen Type | Width |
|---|---|
| Mobile | ≤ 649 px |
| Tablet | 650 – 1099 px |
| Desktop | ≥ 1100 px |

Override via `ResponsiveBreakpoints` — see [Custom Breakpoints](#custom-breakpoints).

---

## 🚀 Getting Started

Copy `responsive.dart` into your project (e.g. `lib/core/responsive/responsive.dart`).

> No pub.dev dependency required — it's a single self-contained file.

---

## 📖 Usage

### 1. `Responsive` widget

Swap entire layouts based on screen type. `tablet` is optional and falls back to `mobile`.

```dart
Scaffold(
  body: Responsive(
    mobile: const MobileLayout(),
    tablet: const TabletLayout(),   // optional
    desktop: const DesktopLayout(),
  ),
)
```

---

### 2. `BuildContext` extensions

Use the extensions anywhere you have a `BuildContext` — no extra imports needed.

```dart
// Boolean checks
if (context.isMobile) { ... }
if (context.isTabletOrLarger) { ... }

// ScreenType enum
final type = context.screenType; // ScreenType.mobile | .tablet | .desktop

// Pick a typed value per screen size
final columns = context.responsiveValue<int>(
  mobile: 1,
  tablet: 2,
  desktop: 4,
);

final padding = context.responsiveValue(
  mobile: const EdgeInsets.all(12),
  desktop: const EdgeInsets.all(24),
);
```

---

### 3. `ResponsiveBuilder`

Use when you need both the `ScreenType` and the exact `Size` inside a single builder.

```dart
ResponsiveBuilder(
  builder: (context, screenType, size) {
    return Text('Width: ${size.width.toStringAsFixed(0)} — $screenType');
  },
)
```

---

### 4. `ResponsiveVisibility`

Show or hide widgets declaratively.

```dart
// Show only on mobile & tablet (e.g. a bottom nav bar)
ResponsiveVisibility(
  targets: {ScreenType.mobile, ScreenType.tablet},
  child: const BottomNavigationBar(...),
)

// Show only on desktop (e.g. a side rail)
ResponsiveVisibility(
  targets: {ScreenType.desktop},
  child: const NavigationRail(...),
)

// Provide a replacement widget instead of SizedBox.shrink
ResponsiveVisibility(
  targets: {ScreenType.mobile},
  replacement: const SizedBox(height: 8),
  child: const BannerAd(...),
)
```

---

### 5. Custom Breakpoints

Wrap your `MaterialApp` (or any sub-tree) with `ResponsiveProvider`:

```dart
void main() {
  runApp(
    const ResponsiveProvider(
      breakpoints: ResponsiveBreakpoints(
        mobileMax: 600,
        tabletMax: 1024,
      ),
      child: MyApp(),
    ),
  );
}
```

All widgets and extensions in the sub-tree will use these breakpoints automatically.

---

## 🗂 API Reference

### `ResponsiveBreakpoints`

| Property | Type | Default | Description |
|---|---|---|---|
| `mobileMax` | `double` | `649` | Upper bound for mobile screens |
| `tabletMax` | `double` | `1099` | Upper bound for tablet screens |

---

### `BuildContext` extensions

| Extension | Type | Description |
|---|---|---|
| `screenType` | `ScreenType` | Current screen type enum value |
| `isMobile` | `bool` | `true` if mobile |
| `isTablet` | `bool` | `true` if tablet |
| `isDesktop` | `bool` | `true` if desktop |
| `isTabletOrLarger` | `bool` | `true` if tablet or desktop |
| `isMobileOrTablet` | `bool` | `true` if mobile or tablet |
| `responsiveValue<T>(...)` | `T` | Returns a typed value per screen type |

---

### `Responsive`

| Property | Type | Required | Description |
|---|---|---|---|
| `mobile` | `Widget` | ✅ | Widget for mobile screens |
| `tablet` | `Widget?` | ❌ | Widget for tablet screens (falls back to `mobile`) |
| `desktop` | `Widget` | ✅ | Widget for desktop screens |

---

### `ResponsiveBuilder`

| Property | Type | Required | Description |
|---|---|---|---|
| `builder` | `Widget Function(BuildContext, ScreenType, Size)` | ✅ | Builder callback |

---

### `ResponsiveVisibility`

| Property | Type | Default | Description |
|---|---|---|---|
| `child` | `Widget` | — | Widget to conditionally show |
| `targets` | `Set<ScreenType>` | — | Screen types on which `child` is visible |
| `replacement` | `Widget` | `SizedBox.shrink()` | Shown when `child` is hidden |
| `maintainState` | `bool` | `false` | Keep state when hidden |
| `maintainAnimation` | `bool` | `false` | Keep animation when hidden |
| `maintainSize` | `bool` | `false` | Keep layout space when hidden |

---

## 🔄 Migration from v1

```dart
// Before
Responsive.isMobile(context)
Responsive.isTablet(context)
Responsive.isDesktop(context)

// After (preferred — extension methods)
context.isMobile
context.isTablet
context.isDesktop

// Static helpers still available for backward compatibility
Responsive.isMobile(context)
```

---

## 📋 Requirements

- Flutter **3.10+**
- Dart **3.0+**

---

## 📄 License

MIT © [Furkan Uddin](https://github.com/frkudn)

