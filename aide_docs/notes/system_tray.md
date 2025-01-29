# System Tray

## Setup
1. Configuration
```toml
[dependencies]
tauri = { version = "2.0.0", features = ["tray-icon"] }
```

2. Implementation
```rust
// Rust
TrayIconBuilder::new()
	.icon(icon)
	.menu(&menu)
	.build(app)?;

// JavaScript
const tray = await TrayIcon.new({
	icon: icon,
	menu: menu
});
```

## Features
1. Menu System
```javascript
const menu = Menu.new({
	items: [{
		id: "quit",
		text: "Quit",
		action: handler
	}]
});
```

2. Event Handling
	 - Click events
	 - Double clicks
	 - Mouse enter/leave
	 - Position tracking
	 - Button states

## Platform Support
1. Core Features
	 - Custom icons
	 - Context menus
	 - Event listeners
	 - Position info

2. Customization
	 - Menu triggers
	 - Click handling
	 - Icon templates
	 - Visibility control

## Best Practices
1. Implementation
	 - Event delegation
	 - Menu organization
	 - Error handling
	 - Resource cleanup

2. Usage Patterns
	 - App controls
	 - Status display
	 - Quick actions
	 - Window mgmt