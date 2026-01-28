# SwiftTUI - Current Status

## Date
2026-01-28

## Last work context
Built the complete foundation of the SwiftTUI framework and added an overlay/modal system:

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

### Modifiers
- `.padding()`, `.frame()`, `.border()`, `.background()`
- `.overlay()` — layer content on top with alignment
- `.dimmed()` — apply ANSI dim effect to content
- `.modal()` — convenience for dimmed background + centered overlay

### Overlay System (NEW)
- `Alert` view with title, message, actions, and preset styles (warning/error/info/success)
- `Dialog` view for flexible modal content
- FrameBuffer character-level compositing (`composited(with:at:)`)

### Menu View
- `Menu` with items, selection indicator, shortcuts
- `MenuItem` model with id, label, shortcut
- `AnyView` type-erased wrapper for conditional returns

### Tests & Example
- 58 tests across 12 suites, all passing
- Example app with menu-based navigation:
  - Main Menu (with feature boxes)
  - Text Styles demo
  - Colors demo (ANSI, bright, RGB, hex, semantic)
  - Containers demo (Card, Box, Panel, all border styles)
  - Overlays demo (modal Alert)
  - Layout demo (VStack, HStack, Spacer, padding, frame)

## Active tasks
- [ ] Commit all work on `feature/tview-foundation`
- [ ] Open PR for the foundation branch

## Next steps
1. **Focus/Event System** — key event dispatch, focus tracking
2. **Button view** — interactive element with action handler (needs focus system)
3. **TextField view** — text input (needs focus + event system)
4. State management (`@TState` property wrapper, re-rendering on change)
5. Proper HStack layout (two-pass: measure, then position)
6. Spacer expansion in stack contexts

## Open questions/blockers
- HStack needs a two-pass layout: measure children first, then position horizontally
- State management will require a re-render mechanism — how to trigger efficiently?
- Should `TView` stay a plain protocol or become `TView: Sendable` for concurrency safety?
- Modal presentation currently static — need state binding for show/hide

## Important decisions
- **TView is a protocol, not a class** — value type semantics like SwiftUI
- **No ncurses** — pure ANSI escape codes via Terminal/ANSIRenderer
- **macOS only** — no iOS/tvOS/watchOS
- **Renderable protocol** for primitive views — avoids impossible `some` type casting
- **Character-level compositing** for overlays — not just line replacement

## File Structure
```
Sources/SwiftTUI/
├── Core/           TView, TViewBuilder, TupleViews, Color, BorderStyle
├── Views/          Text, Stacks, Spacer, ForEach, Card, Box, Panel, Alert, Dialog
├── Modifiers/      Frame, Padding, Border, Background, Overlay, Dimmed
├── Rendering/      Terminal, ANSIRenderer, FrameBuffer, Renderable, ViewRenderer
├── App/            TApp, TScene
└── SwiftTUI.swift  Public API exports
```

## Notes
- The `renderElement` helper is duplicated — candidate for consolidation
- `renderOnce()` returns hardcoded 0 — TODO to return actual line count
- Alert static factory methods need explicit generic parameter in some contexts
