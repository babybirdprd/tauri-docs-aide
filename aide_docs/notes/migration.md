# Migration to Tauri 2.0

## Core Changes
1. Structure Changes
```rust
// Mobile Support
#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
	// shared code
}
```

2. Config Updates
```json
{
	"productName": "app",
	"mainBinaryName": "app",
	"app": {}, // was "tauri"
	"bundle": {} // moved top-level
}
```

## Plugin Migration
1. Core Plugins
	 - fs → tauri-plugin-fs
	 - http → tauri-plugin-http
	 - shell → tauri-plugin-shell
	 - dialog → tauri-plugin-dialog
	 - updater → tauri-plugin-updater

2. API Changes
```javascript
// Old
import { invoke } from '@tauri-apps/api/tauri'
// New
import { invoke } from '@tauri-apps/api/core'
```

## Security Model
1. Permissions
	 - New ACL system
	 - Plugin-specific perms
	 - Window scoping
	 - Domain controls

2. Configuration
```json
{
	"app": {
		"security": {
			"capabilities": ["cap1"]
		}
	}
}
```

## Best Practices
1. Migration Steps
	 - Use migrate command
	 - Update dependencies
	 - Check permissions
	 - Test functionality

2. Considerations
	 - Mobile support
	 - Plugin updates
	 - Config changes
	 - Security model