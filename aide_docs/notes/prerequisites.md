# Tauri Prerequisites

## System Dependencies
1. Linux
```bash
# Debian/Ubuntu
apt install libwebkit2gtk-4.1-dev \
	build-essential \
	libssl-dev \
	libayatana-appindicator3-dev
```

2. macOS
	 - Xcode/Command Line Tools
	 - WebKit included
	 - iOS deps optional

3. Windows
	 - C++ Build Tools
	 - WebView2 Runtime
	 - MSVC toolchain

## Development Tools
1. Rust Setup
```bash
# Install rustup
curl https://sh.rustup.rs -sSf | sh
# Windows: MSVC toolchain
rustup default stable-msvc
```

2. Node.js (Optional)
	 - LTS version
	 - npm/pnpm/yarn
	 - corepack support

## Mobile Development
1. Android
	 - Android Studio
	 - SDK Platform/Tools
	 - NDK
	 - Environment vars:
		 - JAVA_HOME
		 - ANDROID_HOME
		 - NDK_HOME

2. iOS (macOS only)
	 - Xcode full install
	 - Rust targets
	 - Cocoapods
	 - iOS simulators

## Best Practices
1. Installation
	 - Platform specific
	 - Version matching
	 - Path setup
	 - Tool verification

2. Configuration
	 - Environment vars
	 - SDK locations
	 - Build tools
	 - Dependencies