# SwiftTUI - Current Status

## Date
2026-01-28

## Last work context
**Session Focus: FocusManager Refactoring to Environment-based**

Refactored the FocusManager from singleton pattern to Environment-based injection:
- Removed `FocusManager.shared` singleton
- Added `FocusManagerKey` for Environment access via `\.focusManager`
- `FocusState` now reads from `EnvironmentStorage`
- `AppRunner` creates and injects FocusManager instance
- `Button` accesses FocusManager via `context.environment`
- Updated all tests to use independent FocusManager instances
- Added new test suite for Environment integration

**Key Result: Tests can now run in parallel!** No more `--no-parallel` needed.

## Active tasks
- [ ] Rename package from SwiftTUI to TUIKit (name collision with existing package)
- [ ] Open PR for `feature/tview-foundation`

## Next steps
1. **Rename to TUIKit** - Package name, imports, folder structure
2. **TextField View** - Text input with cursor management
3. **ScrollView** - Scrollable content area
4. **Improve HStack** - Two-pass layout (measure, then position)

## Open questions/blockers
- `AppState.shared` is still a singleton - should be replaced with Environment-based render trigger
- Should `DefaultTheme` be renamed to `ANSITheme`? What should be the actual default?

## Important decisions
- **No singletons for state** - Use Environment system instead
- **FocusManager via Environment** - `\.focusManager` environment key
- **Package rename to TUIKit** - Due to name collision with rensbreur/SwiftTUI

## Notes
- Run tests with `swift test` (parallel now works!)
- 181 tests across 27 suites, all passing
- FocusManager accessible via `@Environment(\.focusManager)` or `context.environment.focusManager`

## Commits this session
- `5b54c26` - refactor: Replace FocusManager singleton with Environment-based injection
