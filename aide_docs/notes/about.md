# About Tauri 2.0

## Core Philosophy
- Multi-frontend toolkit for desktop apps
- Rust core + Node.js CLI = polyglot approach

## Key Principles
1. Security First
   - Assumes compromised device
   - Defense in depth
   - Min attack surface
   - Runtime handle randomization
   - No ASAR, binary-only dist

2. Tech Stack
   - BE: Rust (core)
   - FE: Any framework
   - Future: Go/Nim/Python/C# possible
   - webview bindings extensible

3. FLOSS Focus
   - FSF compatible
   - GNU/Linux dist ready
   - Community-driven dev
   - Full open source

## Critical Points
- API endpoint selection
- Optional localhost server
- C-interop for backends
- Cross-platform focus