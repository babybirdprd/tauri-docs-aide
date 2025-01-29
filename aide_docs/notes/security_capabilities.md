# Capabilities System

## Core Concepts
- Permission mapping to windows
- Granular API control
- Platform-specific configs
- Remote access control

## Structure
```json
{
	"identifier": "cap-id",
	"description": "desc",
	"windows": ["main"],
	"permissions": [
		"core:default",
		"specific:permission"
	]
}
```

## Implementation
1. Config Location
	 - src-tauri/capabilities/
	 - JSON/TOML format
	 - Auto-enabled by default
	 - Schema validation

2. Platform Support
	 - Desktop: linux/macOS/win
	 - Mobile: iOS/android
	 - Default: all platforms
	 - Custom combinations

3. Remote Access
	 - Default: bundled only
	 - URL pattern matching
	 - Domain restrictions
	 - iframe limitations

## Security Notes
- Window label based
- Permission merging
- Boundary isolation
- Minimal exposure
- No core protection