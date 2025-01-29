# Technical Context

## Core Technologies
1. Backend
   - Rust (core framework)
   - WebView2 (Windows)
   - WebKit (macOS)
   - WebKitGTK (Linux)

2. Frontend
   - Any web framework supported
   - Specific integrations:
     - Next.js
     - Nuxt
     - SvelteKit
     - Leptos
     - Qwik
     - Trunk
     - Vite

## Development Requirements
1. System Requirements
   - Rust toolchain
   - Node.js (optional, for JS/TS projects)
   - Platform-specific SDKs
   - WebView dependencies

2. Build Tools
   - Cargo (Rust package manager)
   - npm/yarn/pnpm (for JS projects)
   - Platform build tools
   - Code signing tools

3. Debug Tools
   - Crabnebula DevTools
   - VSCode extensions
   - Browser DevTools
   - Platform debuggers

## Testing Infrastructure
1. Test Frameworks
   - WebDriver support
   - Rust test framework
   - Frontend test tools
   - Mocking utilities

2. CI/CD Tools
   - GitHub Actions support
   - Platform-specific runners
   - Automated signing

## Deployment Requirements
1. Code Signing
   - Windows certificates
   - Apple certificates
   - Linux signing keys

2. Store Requirements
   - App Store requirements
   - Microsoft Store requirements
   - Linux distribution requirements

## Development Environment
1. IDE Support
   - VSCode integration
   - Rust-Analyzer
   - Debug configurations
   - Task runners

2. Local Tools
   - CLI tools
   - Development servers
   - Hot reload support
   - Build scripts