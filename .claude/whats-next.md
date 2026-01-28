# SwiftTUI - Current Status

## Date
2026-01-28

## Last work context
**COMPLETED: StatusBar Refactoring + Environment System + .statusBarItems() Modifier**

Successfully refactored the StatusBar system from a singleton pattern to an Environment-based architecture, similar to SwiftUI's @Environment. Also implemented a declarative `.statusBarItems()` modifier for views to specify their status bar items.

### What Was Done This Session

#### 1. Environment System (`Sources/SwiftTUI/Core/Environment.swift`)
- `EnvironmentKey` protocol for defining custom environment values
- `EnvironmentValues` struct as container for all environment values
- `EnvironmentStorage` class for thread-local storage during rendering
- `@Environment` property wrapper for accessing values in views
- `EnvironmentModifier` for injecting values via `.environment()` modifier

#### 2. StatusBar State in TApp (`Sources/SwiftTUI/App/TApp.swift`)
- Created `StatusBarState` class (replaces old `StatusBarManager` singleton)
- Defined `StatusBarKey` as EnvironmentKey
- Added `\.statusBar` to EnvironmentValues
- `AppRunner` creates its own `StatusBarState` instance and injects it into Environment

#### 3. StatusBar.swift Cleanup (`Sources/SwiftTUI/Views/StatusBar.swift`)
- **Removed** the entire `StatusBarManager` singleton class
- Kept `TStatusBar` view with all rendering logic, alignment options, etc.

#### 4. StatusBarItems Modifier (`Sources/SwiftTUI/Modifiers/StatusBarItemsModifier.swift`) - NEW!
- `.statusBarItems([...])` - Set items from array
- `.statusBarItems { ... }` - Set items with builder syntax
- `.statusBarItems(context: "name") { ... }` - Push items to context stack
- `.statusBarItems(context: "name", items: [...])` - Push array to context

#### 5. Updated Example App
- `ContentView.swift` now uses `.statusBarItems()` modifier declaratively
- `AppState.swift` simplified - no more manual StatusBar reference needed
- Each page gets its status bar items via modifier

### Core Framework
- TView protocol, TViewBuilder, all primitive and container views
- Full ANSI rendering pipeline (Terminal, ANSIRenderer, ViewRenderer, Renderable)
- TApp/TScene app lifecycle with run loop
- Color system (ANSI, bright, 256-palette, RGB, hex)
- Text styling (bold, italic, underline, strikethrough, dim, blink, inverted)
- **NEW: Environment system** for passing values down view hierarchy

### Container Views
- VStack, HStack, ZStack
- Card (bordered container with padding/background)
- Box (simple bordered container)
- Panel (titled container with title in top border)

### Interactive Views
- `Button` view with action handler
- `ButtonRow` for horizontal button groups
- `ButtonStyle` presets: default, primary, destructive, success, plain
- Focus indicator (▸) for focused buttons
- Keyboard support: Enter/Space to activate

### Focus System
- `FocusManager` singleton for tracking focus state (candidate for Environment refactoring)
- `Focusable` protocol for interactive views
- `FocusState` helper for views
- Tab/Shift+Tab navigation between focusable elements
- Auto-focus first element when registered

### Status Bar System
- `TStatusBar` view rendered separately at terminal bottom
- `TStatusBarItem` with shortcut, label, and action
- `TStatusBarItemProtocol` for custom items
- `TStatusBarStyle`: `.compact` (1 line) and `.bordered` (3 lines with block border)
- `TStatusBarAlignment`: `.leading`, `.trailing`, `.center`, `.justified` (default)
- **`StatusBarState` class** in TApp.swift (owned by AppRunner, injected via Environment)
- **`.statusBarItems()` modifier** for declarative item registration
- Context stack for dialogs/modals with push/pop
- Case-sensitive shortcut matching ("n" vs "N")
- Never dimmed by overlays - always visible and usable

### Shortcut Constants
- `Shortcut` enum with predefined Unicode symbols:
  - Special keys: `.escape` (⎋), `.enter` (↵), `.tab` (⇥), `.backspace` (⌫)
  - Arrow keys: `.arrowUp` (↑), `.arrowsUpDown` (↑↓), `.arrowsAll` (↑↓←→)
  - Modifiers: `.command` (⌘), `.option` (⌥), `.control` (⌃), `.shift` (⇧)
  - Navigation: `.pageUp` (⇞), `.pageDown` (⇟), `.home`, `.end`
- Helper methods: `Shortcut.combine()`, `Shortcut.ctrl()`, `Shortcut.range()`

### Modifiers
- `.padding()`, `.frame()`, `.border()`, `.background()`
- `.overlay()` — layer content on top with alignment
- `.dimmed()` — apply ANSI dim effect to content
- `.modal()` — convenience for dimmed background + centered overlay
- `.onKeyPress()` — handle keyboard events
- `.environment()` — inject environment values
- **`.statusBarItems()`** — declaratively set status bar items

### Overlay System
- `Alert` view with title, message, actions, and preset styles
- `Dialog` view for flexible modal content
- FrameBuffer character-level compositing

### Menu View
- `Menu` with items, selection indicator, shortcuts
- `MenuItem` model with id, label, shortcut
- `AnyView` type-erased wrapper for conditional returns

### State Management
- `@TState` property wrapper
- `Binding<T>` for two-way data binding
- `AppState` singleton for triggering re-renders
- **`@Environment`** property wrapper for reading environment values

### Tests & Example
- **178 tests** across **26 suites**, all passing (run with `--no-parallel` due to singleton state in FocusManager)
- Example app restructured with declarative StatusBar items

## Branch
`feature/tview-foundation`

## Uncommitted Changes
- `Sources/SwiftTUI/Core/Environment.swift` (NEW)
- `Sources/SwiftTUI/Modifiers/StatusBarItemsModifier.swift` (NEW)
- `Sources/SwiftTUI/App/TApp.swift` (modified - StatusBarState, Environment integration)
- `Sources/SwiftTUI/Rendering/Renderable.swift` (modified - environment in RenderContext)
- `Sources/SwiftTUI/Views/StatusBar.swift` (modified - removed StatusBarManager)
- `Sources/SwiftTUIExample/ContentView.swift` (modified - uses .statusBarItems())
- `Sources/SwiftTUIExample/AppState.swift` (modified - simplified)
- `Tests/SwiftTUITests/StatusBarTests.swift` (modified - StatusBarState tests + modifier tests)
- Various new test files (Button, Focus, Color, Container, FrameBuffer, Rendering)

## Next steps (immediate)
1. **Commit all changes** - Environment system, StatusBar refactoring, .statusBarItems() modifier
2. **FocusManager Refactoring** - Move from singleton to Environment-based (fixes parallel test issues)
3. **TextField view** - Text input with cursor management
4. **Improve HStack layout** - Two-pass measurement
5. **ScrollView**
6. **Open PR** for `feature/tview-foundation` branch

## Open questions/blockers
- HStack needs a two-pass layout: measure children first, then position horizontally
- Modal presentation currently static — need state binding for show/hide
- FocusManager is still a singleton (candidate for Environment refactoring)
- Should `TView` stay a plain protocol or become `TView: Sendable` for concurrency safety?

## Important decisions
- **TView is a protocol, not a class** — value type semantics like SwiftUI
- **No ncurses** — pure ANSI escape codes via Terminal/ANSIRenderer
- **macOS only** — no iOS/tvOS/watchOS
- **Renderable protocol** for primitive views — avoids impossible `some` type casting
- **Character-level compositing** for overlays — not just line replacement
- **Focus system is global** — FocusManager singleton, Tab navigation
- **StatusBar rendered separately** — never affected by dimming/overlays
- **StatusBar owned by App** — not a global singleton, lifecycle tied to TApp
- **Environment system** — similar to SwiftUI's @Environment for passing values down

## File Structure

```
Sources/SwiftTUI/
├── Core/
│   ├── TView.swift, TViewBuilder.swift, TupleViews.swift
│   ├── PrimitiveViews.swift, Color.swift, BorderStyle.swift
│   ├── ViewModifier.swift, State.swift, KeyEvent.swift, Focus.swift
│   ├── Environment.swift  <-- NEW
├── Views/
│   ├── Text.swift, Stacks.swift, Spacer.swift, ForEach.swift
│   ├── Card.swift, Box.swift, Panel.swift
│   ├── Alert.swift, Dialog.swift, Menu.swift
│   ├── Button.swift, StatusBar.swift
├── Modifiers/
│   ├── FrameModifier.swift, PaddingModifier.swift
│   ├── BorderModifier.swift, BackgroundModifier.swift
│   ├── OverlayModifier.swift, DimmedModifier.swift, KeyPressModifier.swift
│   ├── StatusBarItemsModifier.swift  <-- NEW
├── Rendering/
│   ├── Terminal.swift, ANSIRenderer.swift, FrameBuffer.swift
│   ├── Renderable.swift, ViewRenderer.swift
├── App/
│   ├── TApp.swift (contains StatusBarState + AppRunner), TScene.swift
└── SwiftTUI.swift

Sources/SwiftTUIExample/
├── main.swift
├── AppState.swift (simplified)
├── ContentView.swift (uses .statusBarItems())
├── Components/
│   ├── HeaderView.swift
│   └── DemoSection.swift
└── Pages/
    ├── MainMenuPage.swift
    ├── TextStylesPage.swift
    ├── ColorsPage.swift
    ├── ContainersPage.swift
    ├── OverlaysPage.swift
    ├── LayoutPage.swift
    └── ButtonsPage.swift
```

## Example: Using .statusBarItems() Modifier

```swift
struct MyView: TView {
    var body: some TView {
        VStack {
            Text("Main Content")
        }
        .statusBarItems {
            TStatusBarItem(shortcut: "q", label: "quit")
            TStatusBarItem(shortcut: "h", label: "help") { showHelp() }
        }
    }
}

// For dialogs/modals - push to context stack
struct DialogView: TView {
    var body: some TView {
        Card {
            Text("Confirm?")
        }
        .statusBarItems(context: "dialog") {
            TStatusBarItem(shortcut: "y", label: "yes") { confirm() }
            TStatusBarItem(shortcut: "n", label: "no") { cancel() }
        }
    }
}
```

## Notes
- StatusBar shortcut matching is case-sensitive: "n" ≠ "N"
- The `renderElement` helper is duplicated — candidate for consolidation
- Run tests with `swift test --no-parallel` to avoid FocusManager singleton issues
