# Testing Framework

## Mock Testing
1. IPC Mocking
```javascript
// Mock IPC calls
mockIPC((cmd, args) => {
	if(cmd === "command") {
		return result;
	}
});

// Track calls
const spy = vi.spyOn(window.__TAURI_INTERNALS__);
```

2. Window Mocking
```javascript
// Mock windows
mockWindows('main', 'second');

// Access mocks
getCurrent(); // main window
getAll(); // all windows
```

## WebDriver Testing
1. Setup
```bash
# Install driver
cargo install tauri-driver --locked

# Platform deps
# Linux: WebKitWebDriver
# Windows: msedgedriver.exe
```

2. Platform Support
	 - Windows: Edge Driver
	 - Linux: WebKitWebDriver
	 - Android: Appium 2
	 - iOS: Appium 2
	 - macOS: Unsupported

## Test Types
1. Unit/Integration
	 - Mock runtime
	 - API simulation
	 - IPC validation
	 - State testing

2. E2E Testing
	 - WebDriver protocol
	 - Cross-platform
	 - UI automation
	 - System integration

## CI Integration
1. GitHub Actions
	 - tauri-action
	 - Platform runners
	 - Driver setup
	 - Test execution

2. Requirements
	 - System libs
	 - WebDriver deps
	 - Build tools
	 - Platform drivers