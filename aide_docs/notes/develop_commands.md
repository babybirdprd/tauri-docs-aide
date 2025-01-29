# Tauri Commands & IPC

## Command Implementation
1. Basic Structure
```rust
// Basic command
#[tauri::command]
fn basic_cmd(arg: String) -> String {
	format!("Received: {}", arg)
}

// Async command
#[tauri::command]
async fn async_cmd(arg: String) -> Result<String, String> {
	Ok(format!("Async: {}", arg))
}

// With state
#[tauri::command]
fn state_cmd(state: State<'_, AppState>) -> Result<String, Error> {
	Ok(state.data.clone())
}

// Registration
.invoke_handler(generate_handler![basic_cmd, async_cmd, state_cmd])
```

2. Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum Error {
	#[error("IO error: {0}")]
	Io(#[from] std::io::Error),
	#[error("Invalid data: {0}")]
	InvalidData(String)
}

#[tauri::command]
async fn cmd() -> Result<Data, Error> {
	let file = File::open("path").map_err(Error::Io)?;
	// Implementation
}
```

3. State Management
```rust
#[derive(Default)]
struct AppState {
	data: Arc<Mutex<String>>
}

// In command
#[tauri::command]
async fn update_state(
	state: State<'_, AppState>,
	new_data: String
) -> Result<(), Error> {
	*state.data.lock().unwrap() = new_data;
	Ok(())
}
```

## Frontend Integration
1. JavaScript Usage
```js
// Basic invoke
await invoke('cmd_name', { arg: 'value' });

// With error handling
try {
	const result = await invoke('cmd_name', {
		arg: 'value'
	});
} catch (e) {
	console.error(e);
}

// With binary data
const buffer = new Uint8Array([1,2,3]);
await invoke('upload', buffer, {
	headers: { 'Content-Type': 'application/octet-stream' }
});
```

2. Event System
```js
// Listen for events
const unlisten = await listen('event-name', (event) => {
	console.log(event.payload);
});

// Emit events
await emit('event-name', { data: 'value' });
```

## Common Patterns
1. Binary Data
```rust
#[tauri::command]
fn handle_binary(data: Vec<u8>) -> Response {
	tauri::ipc::Response::new(data)
}
```

2. Window Access
```rust
#[tauri::command]
async fn window_cmd(window: Window) {
	window.set_title("New Title").unwrap();
	window.maximize().unwrap();
}
```

3. App Handle
```rust
#[tauri::command]
async fn app_cmd(app: AppHandle) {
	let window = app.get_window("main").unwrap();
	app.global_shortcut_manager()
	   .register("CommandOrControl+Shift+I", move || {});
}
```

## Security Notes
- Commands exposed to frontend
- Input validation required
- State access control
- Error information leaks
- Binary data handling