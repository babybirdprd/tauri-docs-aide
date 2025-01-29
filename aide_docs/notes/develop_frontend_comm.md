# Frontend Communication

## Event System
1. Global Events
```rust
// Emit to all listeners
app.emit("event-name", payload).unwrap();
```

2. Targeted Events
```rust
// Emit to specific window
app.emit_to("window-label", "event", payload);
// Filter targets
app.emit_filter("event", payload, |target| {...});
```

## Channels
1. Purpose
   - Fast streaming
   - Ordered delivery
   - Binary data
   - Progress tracking

2. Implementation
```rust
#[tauri::command]
fn stream(channel: Channel<Data>) {
	channel.send(data).unwrap();
}
```

## Direct Execution
1. JS Eval
```rust
webview.eval("console.log('msg')")?;
```

2. Key Features
   - Direct JS execution
   - Serialize-to-JS support
   - Window context
   - Error handling

## Best Practices
- Events for broadcasts
- Channels for streams
- JSON for small data
- Binary for large data
- Type-safe payloads