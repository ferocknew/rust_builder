# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a desktop class schedule management application built with Tauri 1.5. The frontend uses vanilla HTML/CSS/JavaScript (no frameworks), and the backend is Rust.

**Architecture**: Tauri desktop app with a simple frontend-backend split
- **Frontend**: `dist/` directory (static HTML/CSS/JS)
- **Backend**: `src-tauri/` directory (Rust with Tauri framework)
- **Build System**: Node.js for Tauri CLI, Cargo for Rust compilation

## Development Commands

### Local Development
```bash
# Install dependencies
npm install

# Development mode with hot reload
npm run tauri dev

# Build current platform
npm run build
```

### Direct Cargo Build (Portable Binaries)
```bash
# Build portable Rust binary (no installer)
cargo build --release --manifest-path=src-tauri/Cargo.toml
```

**Build Outputs**:
- macOS: `src-tauri/target/release/bundle/macos/ClassScheduleSystem.app` and `src-tauri/target/release/bundle/dmg/ClassScheduleSystem_*.dmg`
- Windows portable: `src-tauri/target/release/ClassScheduleSystem.exe`
- Windows installer (if built with tauri): `src-tauri/target/release/bundle/nsis/`

### Testing and Running Local Builds
```bash
# Run the compiled binary directly (macOS)
./src-tauri/target/release/ClassScheduleSystem

# Run the compiled binary directly (Windows)
./src-tauri/target/release/ClassScheduleSystem.exe

# Open the macOS app
open src-tauri/target/release/bundle/macos/ClassScheduleSystem.app
```

## Multi-Platform CI/CD

The project uses GitHub Actions to build Windows and macOS executables automatically.

### Triggering Builds
Push a version tag to trigger the workflow:
```bash
git tag v1.0.0
git push origin v1.0.0
```

### Workflow Architecture
`.github/workflows/build-multi-platform.yml`:
- `build-windows`: Compiles Windows portable exe using `cargo build --release`
- `build-macos`: Dynamically modifies `tauri.conf.json` to set `targets: ["dmg", "app"]`, then runs `npm run build`
- `release`: Downloads all artifacts and creates a unified GitHub Release

### Important CI/CD Constraints
1. **Product Name**: `productName` in `tauri.conf.json` MUST be English (e.g., "ClassScheduleSystem"). Chinese names cause file encoding issues in GitHub Actions.
2. **Portable Windows Binary**: Use `cargo build --release` instead of `tauri build` for Windows to avoid NSIS installer. The exe from `target/release/` is fully portable.
3. **macOS Dynamic Configuration**: The workflow uses Python to modify `tauri.conf.json` at runtime, changing `targets` from `["nsis"]` to `["dmg", "app"]`.
4. **Icons**: All icon files in `src-tauri/icons/` must be committed to git. They are NOT generated automatically.
5. **Release Creation**: Only the `release` job creates the GitHub Release, not the individual build jobs.

## Key Configuration Files

### `src-tauri/tauri.conf.json`
- **Window size**: 780x180 pixels, non-resizable
- **productName**: Must be English ("ClassScheduleSystem")
- **distDir**: Points to `../dist` (the frontend directory)
- **targets**: Default is `["nsis"]` for Windows, but dynamically changed for macOS builds

### `src-tauri/Cargo.toml`
- Uses Tauri 1.5.4 with "shell-open" feature
- Default feature: "custom-protocol"
- Edition: 2021

### `package.json`
- `build` script runs `tauri build`
- `tauri` script runs the Tauri CLI
- Only devDependency: `@tauri-apps/cli@^1.5.0`

## Frontend Structure

The frontend is a single-page application in `dist/index.html`:
- No build process or bundling
- Embedded styles in `<style>` tags
- Inline JavaScript for button interactions
- All UI logic is client-side only
- No API calls to Tauri backend currently

### UI Layout
- Two rows of buttons (8 total buttons)
- Each button has an emoji icon and Chinese text
- Clicking triggers `alert()` with test message
- Window is fixed size (780x180) with no scrollbars

## Backend Architecture

`src-tauri/src/main.rs` is minimal:
- Uses `tauri::Builder::default()` pattern
- No custom commands or IPC
- No backend business logic yet
- `#![cfg_attr(not(debug_assertions), windows_subsystem = "windows")]` prevents console window on Windows

## File Naming Conventions

- **Executable name**: Use English (`ClassScheduleSystem`), not Chinese
- **Icon files**: Must include all formats (32x32.png, 128x128.png, icon.ico, icon.icns)
- **Build artifacts**: Ignored by git (see `.gitignore`)

## Common Patterns

### Adding a New Feature to the UI
1. Edit `dist/index.html`
2. Add button to the appropriate `.button-row` div
3. Update the `showAlert()` function if needed
4. Test with `npm run tauri dev`

### Adding a Tauri Command (Backend)
1. Define command in `src-tauri/src/main.rs` using `#[tauri::command]`
2. Register command in `.invoke_handler()`
3. Update `tauri.conf.json` allowlist to enable the capability
4. Call from frontend using `invoke()` from `@tauri-apps/api`

### Debugging Build Issues
- **Windows**: Check that `productName` is English in `tauri.conf.json`
- **macOS**: Verify icons are present and `targets` includes `["dmg", "app"]`
- **CI/CD**: Check GitHub Actions logs for "Download all artifacts" step to see what files were generated

## Repository Details

- **GitHub**: https://github.com/ferocknew/rust_builder
- **Main branch**: `main`
- **Language**: Chinese (UI), English (code)
- **License**: MIT
