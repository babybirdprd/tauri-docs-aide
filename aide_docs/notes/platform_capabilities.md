# Platform & Window Capabilities

## Window Configuration
1. Static Setup
```json
{
	"windows": [
		{
			"label": "main",
			"title": "Main Window"
		}
	]
}
```

2. Dynamic Creation
```rust
WebviewWindowBuilder::new(app, "label", url)
	.title("Title")
	.build()?;
```

## Capability System
1. File Organization
```
/capabilities/
	filesystem.json
	dialog.json
	platform.json
```

2. Window Specific
```json
{
	"identifier": "fs-access",
	"windows": ["main"],
	"permissions": [
		"fs:allow-read"
	]
}
```

## Platform Features
1. Platform Targeting
```json
{
	"identifier": "capability",
	"platforms": ["linux", "windows"],
	"permissions": ["..."]
}
```

2. Available Platforms
	 - Windows
	 - macOS
	 - Linux
	 - Android
	 - iOS

## Best Practices
1. Security Model
	 - Minimal permissions
	 - Window isolation
	 - Platform checks
	 - Feature gates

2. Implementation
	 - Capability separation
	 - Clear identifiers
	 - Local restrictions
	 - Documentation