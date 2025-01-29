# Window Customization

## Configuration Methods
1. Static Config
```json
{
	"tauri": {
		"windows": [{
			"decorations": false,
			"titleBarStyle": "transparent"
		}]
	}
}
```

2. Runtime APIs
	 - JavaScript API
	 - Rust Window API
	 - Platform specific

## Custom Titlebar
1. Implementation
```html
<div data-tauri-drag-region class="titlebar">
	<div class="titlebar-button">...</div>
</div>
```

2. Features
	 - Window dragging
	 - Custom controls
	 - Event handling
	 - Style control

## Platform Features
1. macOS
```rust
#[cfg(target_os = "macos")]
{
	use cocoa::appkit::NSWindow;
	window.set_title_bar_style(TitleBarStyle::Transparent);
	// Custom background
}
```

2. Permissions
```json
{
	"permissions": [
		"core:window:default",
		"core:window:allow-start-dragging"
	]
}
```

## Best Practices
1. Implementation
	 - Platform checks
	 - Event handling
	 - Style isolation
	 - Permission mgmt

2. Features
	 - Drag regions
	 - Window controls
	 - Native features
	 - Custom styling