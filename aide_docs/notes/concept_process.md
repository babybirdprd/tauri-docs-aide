# Process Model

## Multi-Process Architecture
- Similar to Electron/browsers
- Crash isolation
- Better multi-core usage
- Principle of Least Privilege

## Core Process (Rust)
- App entry point
- Full OS access
- Window orchestration
- IPC routing
- Global state mgmt
- Memory safety via ownership

## WebView Process
- OS-provided webview libs
- HTML/CSS/JS runtime
- Dynamic linking
- Platform specific:
	- Win: Edge WebView2
	- Mac: WKWebView
	- Linux: webkitgtk

## Security Benefits
- Process isolation
- Minimal permissions
- Restartable components
- Small attack surface
- Business logic in Core
- No bundled webview