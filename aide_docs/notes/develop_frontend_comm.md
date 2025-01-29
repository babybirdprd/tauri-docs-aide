# Frontend Communication

## Event System
1. Global Events
```rust
// In Rust
#[tauri::command]
async fn broadcast_event(app: AppHandle, payload: Value) -> Result<(), String> {
    app.emit_all("global-event", payload)
       .map_err(|e| e.to_string())?;
    Ok(())
}

// With typed payload
#[derive(Serialize)]
struct EventPayload {
    message: String,
    timestamp: i64,
    data: Vec<u8>
}

app.emit_all("typed-event", EventPayload {
    message: "Update".into(),
    timestamp: SystemTime::now()
        .duration_since(UNIX_EPOCH)
        .unwrap()
        .as_secs() as i64,
    data: vec![1, 2, 3]
}).unwrap();
```

2. Window-Specific Events
```rust
// Target specific window
#[tauri::command]
async fn window_event(
    window: Window,
    event: String,
    payload: Value
) -> Result<(), String> {
    window.emit(&event, payload)
          .map_err(|e| e.to_string())?;
    Ok(())
}

// With filters
app.emit_filter(
    "filtered-event",
    payload,
    |handle| {
        matches!(handle.label(), "main" | "settings")
    }
).unwrap();
```

## Channels
1. Binary Streaming
```rust
#[tauri::command]
async fn stream_file(
    path: PathBuf,
    channel: Channel<Vec<u8>>
) -> Result<(), String> {
    let mut file = File::open(path)
        .map_err(|e| e.to_string())?;
    
    let mut buffer = vec![0; 8192];
    loop {
        let n = file.read(&mut buffer)
            .map_err(|e| e.to_string())?;
        if n == 0 { break; }
        
        channel.send(buffer[..n].to_vec())
            .map_err(|e| e.to_string())?;
    }
    Ok(())
}
```

2. Progress Tracking
```rust
#[derive(Serialize)]
struct Progress {
    percent: f32,
    bytes_processed: u64,
    total_bytes: u64
}

#[tauri::command]
async fn download_with_progress(
    url: String,
    channel: Channel<Progress>
) -> Result<(), String> {
    let response = reqwest::get(&url).await?;
    let total = response.content_length().unwrap_or(0);
    let mut downloaded = 0u64;
    
    let mut stream = response.bytes_stream();
    while let Some(chunk) = stream.next().await {
        let chunk = chunk.map_err(|e| e.to_string())?;
        downloaded += chunk.len() as u64;
        
        channel.send(Progress {
            percent: (downloaded as f32 / total as f32) * 100.0,
            bytes_processed: downloaded,
            total_bytes: total
        }).map_err(|e| e.to_string())?;
    }
    Ok(())
}
```

## JavaScript Integration
1. Direct Execution
```rust
// Execute JS in window context
#[tauri::command]
async fn update_ui(window: Window) -> Result<(), String> {
    window.eval("
        document.getElementById('status').textContent = 'Updated';
        console.log('UI Updated');
    ").map_err(|e| e.to_string())?;
    Ok(())
}

// With dynamic values
let js = format!(
    "window.APP_VERSION = '{}';
     window.FEATURES = {};",
    version,
    serde_json::to_string(&features)?
);
window.eval(&js)?;
```

2. Frontend Event Handling
```typescript
// TypeScript/JavaScript
import { listen, emit } from '@tauri-apps/api/event'

// Listen for events
const unlisten = await listen<Progress>('download-progress', 
    (event) => {
        const { percent, bytes_processed, total_bytes } = event.payload;
        updateProgressBar(percent);
    }
);

// Emit events
await emit('start-process', {
    id: 'task-1',
    params: { /* ... */ }
});

// Cleanup
unlisten();
```

## Best Practices
1. Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum CommError {
    #[error("Event error: {0}")]
    EventError(String),
    #[error("Channel closed")]
    ChannelClosed,
    #[error("Invalid payload")]
    InvalidPayload
}

#[tauri::command]
async fn safe_emit(
    window: Window,
    event: String,
    payload: Value
) -> Result<(), CommError> {
    window.emit(&event, payload)
        .map_err(|e| CommError::EventError(e.to_string()))?;
    Ok(())
}
```

2. Resource Management
- Close channels after use
- Remove event listeners
- Clear timeouts/intervals
- Handle window closure
- Cleanup subscriptions