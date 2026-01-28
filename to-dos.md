# SwiftTUI — To-Dos

## IN PROGRESS

- [ ] Commit and open PR for `feature/tview-foundation`

## PLANNED

### Rendering & Layout
- [ ] Proper HStack horizontal rendering (two-pass layout: measure, then position)
- [ ] Spacer expansion in stack contexts
- [ ] ForEach rendering in ViewRenderer
- [ ] `renderOnce()` should return actual rendered line count

### New Views & Modifiers
- [ ] `Button` view with action handler
- [ ] `Group` view

### State & Interactivity
- [ ] `@TState` property wrapper
- [ ] Re-render on state change
- [ ] Input/event handling system (key dispatch)
- [ ] Focus system for interactive elements

### Infrastructure
- [ ] Deduplicate `renderElement` helper across TupleView/ConditionalView/TViewArray extensions
- [ ] Linux support (optional, later)

## COMPLETED

- [x] TView protocol
- [x] TViewBuilder result builder (up to 10 children, conditionals, optionals, arrays)
- [x] Primitive views: Text, EmptyView, Spacer, Divider
- [x] Container views: VStack, HStack, ZStack
- [x] New container views: Card, Box, Panel
- [x] ForEach (Identifiable, KeyPath, Range)
- [x] Color system (ANSI, bright, 256, RGB, hex)
- [x] Text styling (bold, italic, underline, strikethrough, dim, blink, inverted)
- [x] Terminal abstraction (raw mode, alternate screen, cursor, I/O)
- [x] ANSI escape code renderer
- [x] ViewRenderer with Renderable protocol
- [x] TApp/TScene/WindowGroup app lifecycle
- [x] `.padding()` modifier
- [x] `.frame(width:height:)` modifier
- [x] `.border()` modifier (with 8 border styles)
- [x] `.background()` modifier
- [x] **Overlay System** (2026-01-28)
  - [x] `.overlay()` modifier with alignment
  - [x] `.dimmed()` modifier
  - [x] `.modal()` convenience helper
  - [x] `Alert` view (with warning/error/info/success presets)
  - [x] `Dialog` view
  - [x] FrameBuffer character-level compositing
- [x] **Menu View** (2026-01-28)
  - [x] `Menu` view with items, selection, shortcuts
  - [x] `MenuItem` model
  - [x] `AnyView` type erasure helper
- [x] Example app with menu-based navigation
  - [x] Main Menu page
  - [x] Text Styles demo page
  - [x] Colors demo page
  - [x] Containers demo page
  - [x] Overlays demo page
  - [x] Layout demo page
- [x] Test suite (58 tests, 12 suites)
- [x] README with badges
- [x] All code comments in English
- [x] macOS-only platform restriction
