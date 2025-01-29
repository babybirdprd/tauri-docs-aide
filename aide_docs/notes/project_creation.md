# Project Creation

## create-tauri-app
1. Quick Start
```bash
# Using package manager
pnpm create tauri-app

# Configuration
- Project name
- Bundle identifier
- Language choice
- Package manager
- UI framework
```

2. Framework Support
   - Vanilla JS/TS
   - Vue/React/Svelte
   - Angular/Preact
   - Yew/Leptos
   - Blazor/.NET

## Manual Setup
1. CLI Installation
```bash
# npm/yarn/pnpm
npm add -D @tauri-apps/cli

# cargo
cargo install tauri-cli
```

2. Project Init
```bash
# Initialize
tauri init

# Configure
- App name
- Window title
- Assets location
- Dev server URL
- Build commands
```

## Development
1. Commands
```bash
# Start dev
pnpm tauri dev

# Build
pnpm tauri build
```

2. Structure
   - src-tauri/
   - Frontend assets
   - Config files
   - Build scripts

## Best Practices
1. Setup
   - Framework choice
   - Dev server config
   - Asset locations
   - Build process

2. Configuration
   - Bundle settings
   - Window config
   - Dev commands
   - Build options