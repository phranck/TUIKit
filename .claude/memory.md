# SwiftTUI - Technical Reference

## Tech Stack

- **Language:** Swift 6.2 (strict concurrency)
- **Platform:** macOS only (10.15+)
- **Build System:** Swift Package Manager (swift-tools-version 6.2)
- **Testing:** Swift Testing framework (`@Test`, `#expect`)
- **Dependencies:** None (pure Swift, no ncurses, no C libs)

## Project Structure

```
SwiftTUI/
├── Package.swift
├── README.md
├── Sources/
│   ├── SwiftTUI/                  # Main library target
│   │   ├── SwiftTUI.swift         # Version constant, renderOnce() helper
│   │   ├── App/
│   │   │   ├── TApp.swift         # App protocol, AppRunner, run loop
│   │   │   └── TScene.swift       # TScene protocol, WindowGroup, SceneBuilder
│   │   ├── Core/
│   │   │   ├── TView.swift        # TView protocol (central)
│   │   │   ├── TViewBuilder.swift # @resultBuilder for declarative composition
│   │   │   ├── TupleViews.swift   # TupleView2–TupleView10
│   │   │   ├── PrimitiveViews.swift # Never, EmptyView, ConditionalView, TViewArray, Optional+TView
│   │   │   └── Color.swift        # Color struct, ANSIColor enum (standard, bright, 256, RGB, hex)
│   │   ├── Rendering/
│   │   │   ├── Terminal.swift      # Terminal singleton (raw mode, size, I/O, alternate screen)
│   │   │   ├── ANSIRenderer.swift  # ANSI escape code generation
│   │   │   ├── Renderable.swift    # Renderable protocol, RenderContext
│   │   │   └── ViewRenderer.swift  # Tree traversal + Renderable extensions for all view types
│   │   └── Views/
│   │       ├── Text.swift          # Text view + TextStyle + modifiers (bold, italic, color, etc.)
│   │       ├── Stacks.swift        # VStack, HStack, ZStack + Alignment types
│   │       ├── Spacer.swift        # Spacer, Divider
│   │       └── ForEach.swift       # ForEach (Identifiable, KeyPath, Range)
│   └── SwiftTUIExample/           # Executable example target
│       └── main.swift
└── Tests/
    └── SwiftTUITests/
        └── TViewTests.swift       # 24 tests across 5 suites
```

## Architecture

- **TView** is a protocol (value types by default, like SwiftUI's View)
- **@TViewBuilder** is a `@resultBuilder` supporting up to 10 children, conditionals, optionals, for-in
- **Primitive views** have `body: Never` and conform to `Renderable` for direct terminal output
- **Composite views** just define `body` — the renderer walks the tree recursively
- **Rendering pipeline:** ViewRenderer → checks Renderable → renders directly OR recurses into body
- **Terminal** is a singleton handling POSIX termios raw mode, ANSI output, cursor, alternate screen
- **TApp** protocol provides `main()` entry point with AppRunner (alternate screen, raw mode, signal handlers, run loop)

## Conventions

- All code comments and documentation in English
- Communication with user in German (informal Du)
- Feature work on dedicated branches (current: `feature/tview-foundation`)
- No external dependencies

## Current Branch

`feature/tview-foundation` — initial framework foundation, not yet committed
