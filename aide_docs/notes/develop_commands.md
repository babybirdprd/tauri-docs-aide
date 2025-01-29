# Tauri Commands & IPC

## Command System
1. Basic Structure
```rust
#[tauri::command]
fn cmd(arg: String) -> Result<T, E> {
	// implementation
}

// Registration
.invoke_handler(generate_handler![cmd])
```

2. Features
	 - Type-safe calls
	 - Async support
	 - Error handling
	 - State access
	 - Window context

## Implementation
1. Arguments
	 - JSON serialized
	 - Serde support
	 - camelCase default
	 - Optional renaming

2. Returns
	 - Serializable data
	 - Result<T,E>
	 - ArrayBuffer opt
	 - Async Promise

3. Access Patterns
	 - Window handle
	 - AppHandle
	 - Managed state
	 - Raw requests

## Event System
1. Global Events
	 - Broadcast model
	 - JSON payloads
	 - Async only
	 - No returns

2. Webview Events
	 - Target specific
	 - Catch-all option
	 - Emit/Listen API
	 - Cross-window comm

## Best Practices
- Unique cmd names
- Module organization
- Error type design
- Channel streaming
- State management