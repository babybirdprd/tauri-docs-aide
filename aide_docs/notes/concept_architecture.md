# Tauri Architecture

## Core Components
1. tauri/
   - Main orchestrator
   - Conf reader (tauri.conf.json)
   - API host
   - Update mgr
   - Script injector

2. Runtime Stack
   - tauri-runtime: webview lib bridge
   - tauri-macros: ctx/handler gen
   - tauri-utils: common utils/CSP
   - tauri-build: cargo features
   - tauri-codegen: asset/conf parser
   - tauri-runtime-wry: sys interactions

3. Upstream Deps
   - WRY: webview renderer
   - TAO: window mgmt (winit fork)

## Tools
- API: TS/JS endpoints (cjs/esm)
- Bundler: cross-plat builder
- CLI: Rust(.rs)/JS(.js) interface
- create-tauri-app: scaffolding

## Plugin System
- 3rd party extensible
- Rust backend + JS API
- Examples: fs/sql/stronghold

## Key Points
- No kernel wrapper
- Native OS calls via WRY/TAO
- MIT/Apache-2.0 licensed
- Cross-platform focus