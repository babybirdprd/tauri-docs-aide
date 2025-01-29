# Plugin Permissions

## Configuration
1. Default Setup
```json
{
	"permissions": [
		"plugin:default",
		{
			"identifier": "plugin:command",
			"allow": [{"path": "$PATH"}]
		}
	]
}
```

2. File Structure
```
/src-tauri/
	/capabilities/
		default.json
		custom.json
	/permissions/
		default.toml
```

## Implementation
1. Plugin Integration
```rust
// Rust Setup
.plugin(tauri_plugin_name::init())

// Permission Check
fs:allow-write-text-file
fs:allow-read-file
```

2. Scope Control
	 - Path variables
	 - Command specific
	 - Allow/deny rules
	 - Default sets

## Best Practices
1. Security Model
	 - Minimal access
	 - Explicit perms
	 - Scoped paths
	 - Command isolation

2. Usage Pattern
	 - Default perms
	 - Custom scopes
	 - Path validation
	 - Error handling

## Key Concepts
- Permission sets
- Command access
- Path scoping
- Plugin defaults