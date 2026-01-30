# TUIKit - Tasks & Feature Ideas

## 🚀 In Progress
- (keine)

## 📋 Open Tasks

### Documentation
- [ ] Expand DocC articles: add more guides and tutorials
- [ ] Improve inline Swift doc comments for better auto-generated API docs
- [ ] Create interactive code examples in documentation
- [ ] Document all 5 phosphor themes with visual examples
- [ ] Add keyboard shortcut reference guide

### Landing Page
- [ ] Build custom landing page under `/` (currently redirects to DocC)
- [ ] Design with feature highlights, quick links, GitHub badge

### CI/CD
- [ ] Add CI workflow for `swift build` + `swift test` on push/PR

### Testing & Validation
- [ ] Test documentation on mobile/tablet
- [ ] Validate all DocC symbol links resolve correctly

### Code Examples
- [ ] Create example: Simple counter app
- [ ] Create example: Todo list app
- [ ] Create example: Form with validation
- [ ] Create example: Table/list view
- [ ] Document Spotnik (Spotify player) as main example

## ✅ Completed

### DocC Documentation + GitHub Pages (2026-01-30)
- ✅ Removed all old documentation (VitePress, MkDocs, legacy DocC)
- ✅ Added `swift-docc-plugin` to Package.swift
- ✅ Created DocC Catalog at `Sources/TUIKit/TUIKit.docc/`
- ✅ Wrote articles: Getting Started, Architecture, State Management, Theming Guide
- ✅ Full API topic organization on landing page
- ✅ GitHub Actions workflow for auto-deployment (`docc.yml`)
- ✅ Custom domain: https://tuikit.layered.work
- ✅ Fixed blank page issue (missing `theme-settings.json`)
- ✅ Fixed GitHub Pages build type (`legacy` → `workflow`)
- ✅ Root redirect `/` → `/documentation/tuikit`
- ✅ Removed leftover VitePress workflow

### Documentation System (2026-01-29)
- ✅ Replaced DocC with MkDocs (later replaced by DocC again)
- ✅ VitePress migration (later replaced by DocC)

### Git Cleanup (2026-01-29)
- ✅ Removed `.claude/` folder from entire Git history
- ✅ Added `.claude/` to .gitignore

### Infrastructure
- ✅ README.md updated with Spotnik screenshot
- ✅ GitHub Pages configured with custom domain

## 🔍 Notes

### Why DocC (final choice)
- Native Swift documentation — auto-generates API docs from code comments
- Apple standard for Swift packages
- `swift-docc-plugin` integrates cleanly with SPM
- Requires `theme-settings.json` workaround for GitHub Pages (injected via CI)

### Why not VitePress/MkDocs
- Redundant when DocC provides Swift-native API documentation
- DocC auto-documents all public types, protocols, functions from source

---

**Last Updated:** 2026-01-30
