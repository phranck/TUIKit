# SwiftTUI - Current Status

## Date
2026-01-28

## Last work context
Added TStatusBar - a dynamic, context-sensitive status bar that displays keyboard shortcuts at the bottom of the terminal. The status bar is NEVER dimmed by overlays/modals!

### Core Framework
- TView protocol, TViewBuilder, all primitive and container views
- Full ANSI rendering pipeline (Terminal, ANSIRenderer, ViewRenderer, Renderable)
- TApp/TScene app lifecycle with run loop
- Color system (ANSI, bright, 256-palette, RGB, hex)
- Text styling (bold, italic, underline, strikethrough, dim, blink, inverted)

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
- `FocusManager` singleton for tracking focus state
- `Focusable` protocol for interactive views
- `FocusState` helper for views
- Tab/Shift+Tab navigation between focusable elements
- Auto-focus first element when registered

### Status Bar (NEW)
- `TStatusBar` view rendered separately at terminal bottom
- `TStatusBarItem` with shortcut, label, and action
- `TStatusBarItemProtocol` for custom items
- `TStatusBarStyle`: `.compact` (1 line) and `.bordered` (3 lines with block border)
- `StatusBarManager` singleton for context management:
  - Global items (always shown when no context active)
  - Context stack (push/pop for dialogs, focused views, etc.)
  - Case-sensitive shortcut matching ("n" vs "N")
- **Never dimmed by overlays** - always visible and usable

### Modifiers
- `.padding()`, `.frame()`, `.border()`, `.background()`
- `.overlay()` — layer content on top with alignment
- `.dimmed()` — apply ANSI dim effect to content
- `.modal()` — convenience for dimmed background + centered overlay
- `.onKeyPress()` — handle keyboard events

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

### Tests & Example
- **94 tests** across **18 suites**, all passing
- Example app restructured into separate files:
  - `main.swift` - entry point
  - `AppState.swift` - state management with StatusBar integration
  - `ContentView.swift` - page router
  - `Components/` - HeaderView, DemoSection
  - `Pages/` - MainMenuPage, TextStylesPage, ColorsPage, ContainersPage, OverlaysPage, LayoutPage, ButtonsPage

## Branch
`feature/tview-foundation`

## Latest commits
- `ca23d24` feat: Add TStatusBar with dynamic context-sensitive shortcuts
- `7a748ec` feat: Add Button view with focus system and keyboard support
- `7a73f6f` fix: onKeyPress handler now returns Bool to control event propagation

## Next steps
1. **TextField view** — text input with cursor management (needs focus system ✓)
2. **Improve HStack layout** — two-pass layout: measure children first, then position
3. **Spacer expansion** — proper expansion in stack contexts
4. **ScrollView** — for content larger than terminal
5. **Open PR** for the `feature/tview-foundation` branch

## Open questions/blockers
- HStack needs a two-pass layout: measure children first, then position horizontally
- Modal presentation currently static — need state binding for show/hide
- Should `TView` stay a plain protocol or become `TView: Sendable` for concurrency safety?

## Important decisions
- **TView is a protocol, not a class** — value type semantics like SwiftUI
- **No ncurses** — pure ANSI escape codes via Terminal/ANSIRenderer
- **macOS only** — no iOS/tvOS/watchOS
- **Renderable protocol** for primitive views — avoids impossible `some` type casting
- **Character-level compositing** for overlays — not just line replacement
- **Focus system is global** — FocusManager singleton, Tab navigation
- **StatusBar rendered separately** — never affected by dimming/overlays

## File Structure

```
Sources/SwiftTUI/
├── Core/
│   ├── TView.swift, TViewBuilder.swift, TupleViews.swift
│   ├── PrimitiveViews.swift, Color.swift, BorderStyle.swift
│   ├── ViewModifier.swift, State.swift, KeyEvent.swift, Focus.swift
├── Views/
│   ├── Text.swift, Stacks.swift, Spacer.swift, ForEach.swift
│   ├── Card.swift, Box.swift, Panel.swift
│   ├── Alert.swift, Dialog.swift, Menu.swift
│   ├── Button.swift, StatusBar.swift (NEW)
├── Modifiers/
│   ├── FrameModifier.swift, PaddingModifier.swift
│   ├── BorderModifier.swift, BackgroundModifier.swift
│   ├── OverlayModifier.swift, DimmedModifier.swift, KeyPressModifier.swift
├── Rendering/
│   ├── Terminal.swift, ANSIRenderer.swift, FrameBuffer.swift
│   ├── Renderable.swift, ViewRenderer.swift
├── App/
│   ├── TApp.swift (modified for StatusBar), TScene.swift
└── SwiftTUI.swift

Sources/SwiftTUIExample/
├── main.swift
├── AppState.swift
├── ContentView.swift
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

## Notes
- StatusBar shortcut matching is case-sensitive: "n" ≠ "N"
- StatusBarManager.onItemsChanged callback can be used for custom integrations
- The `renderElement` helper is duplicated — candidate for consolidation
