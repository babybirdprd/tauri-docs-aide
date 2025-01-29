# Tauri IPC System

## Core Concepts
- Async Message Passing
- JSON serialized req/resp
- Security-focused isolation
- Similar to web client-server

## Command-based IPC
1. Basic Command
```rust
// Rust side
#[tauri::command]
async fn greet(name: String) -> Result<String, Error> {
    Ok(format!("Hello, {}!", name))
}

// Register command
fn main() {
    tauri::Builder::default()
        .invoke_handler(generate_handler![greet])
        .run(tauri::generate_context!())
        .expect("error while running application");
}

// JavaScript side
import { invoke } from '@tauri-apps/api/core';
const response = await invoke('greet', { name: 'World' });
```

2. Complex Data Types
```rust
#[derive(Serialize, Deserialize)]
struct User {
    id: u64,
    name: String,
    roles: Vec<String>
}

#[tauri::command]
async fn update_user(user: User) -> Result<User, Error> {
    // Process user
    Ok(user)
}
```

## Event System
1. Global Events
```rust
// Emit from Rust
#[tauri::command]
async fn broadcast(app: AppHandle, msg: String) -> Result<(), Error> {
    app.emit_all("global-event", msg)
       .map_err(Error::from)
}

// Listen in JavaScript
import { listen } from '@tauri-apps/api/event';
const unlisten = await listen('global-event', (event) => {
    console.log('Got:', event.payload);
});
```

2. Window-Specific Events
```rust
// Emit to specific window
#[tauri::command]
async fn notify_window(
    window: Window,
    event: String,
    payload: Value
) -> Result<(), Error> {
    window.emit(&event, payload)
          .map_err(Error::from)
}
```

## Channel-based IPC
1. Binary Streaming
```rust
#[tauri::command]
async fn stream_data(
    channel: Channel<Vec<u8>>
) -> Result<(), Error> {
    let data = vec![1, 2, 3, 4, 5];
    channel.send(data)
           .map_err(Error::from)
}
```

## Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum IpcError {
    #[error("Invalid request: {0}")]
    InvalidRequest(String),
    #[error("Channel closed")]
    ChannelClosed,
    #[error("Serialization error: {0}")]
    SerdeError(String)
}

impl From<IpcError> for String {
    fn from(err: IpcError) -> Self {
        err.to_string()
    }
}
```

## Security Best Practices
1. Input Validation
```rust
fn validate_path(path: &Path) -> Result<(), Error> {
    let canonical = path.canonicalize()?;
    if !canonical.starts_with("/safe/path") {
        return Err(Error::InvalidPath);
    }
    Ok(())
}
```

2. Permission Checks
```rust
#[tauri::command(permit_list = ["fs:read"])]
async fn read_file(
    app: AppHandle,
    path: PathBuf
) -> Result<String, Error> {
    if !app.runtime_authority()
           .enable_permission("fs:read", Some(path.to_str().unwrap()))
           .is_ok() {
        return Err(Error::PermissionDenied);
    }
    fs::read_to_string(path).map_err(Error::from)
}
```