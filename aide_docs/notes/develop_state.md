# State Management

## Implementation Patterns
1. Basic State
```rust
// Immutable state
#[derive(Default)]
struct AppState {
    config: HashMap<String, String>,
    version: String
}

// In main.rs
let state = AppState {
    config: HashMap::from([
        ("theme".into(), "dark".into())
    ]),
    version: "1.0.0".into()
};
app.manage(state);

// Access in commands
#[tauri::command]
fn get_config(state: State<AppState>) -> Result<String, String> {
    Ok(state.config.get("theme")
        .cloned()
        .unwrap_or_default())
}
```

2. Mutable State
```rust
// Thread-safe mutable state
#[derive(Default)]
struct MutableState {
    counter: Arc<Mutex<i32>>,
    data: Arc<RwLock<Vec<String>>>
}

// In main.rs
app.manage(MutableState::default());

// Mutex access
#[tauri::command]
async fn increment(state: State<'_, MutableState>) -> Result<i32, String> {
    let mut counter = state.counter.lock().map_err(|e| e.to_string())?;
    *counter += 1;
    Ok(*counter)
}

// RwLock access
#[tauri::command]
async fn add_data(
    state: State<'_, MutableState>,
    item: String
) -> Result<(), String> {
    let mut data = state.data.write().map_err(|e| e.to_string())?;
    data.push(item);
    Ok(())
}
```

3. Async State
```rust
// Async state with Tokio
#[derive(Default)]
struct AsyncState {
    queue: Arc<Mutex<VecDeque<Task>>>,
    worker: Arc<Mutex<Option<JoinHandle<()>>>>
}

impl AsyncState {
    async fn process_queue(&self) {
        let queue = self.queue.lock().unwrap();
        for task in queue.iter() {
            // Process task
            tokio::time::sleep(Duration::from_secs(1)).await;
        }
    }
}

#[tauri::command]
async fn start_worker(state: State<'_, AsyncState>) -> Result<(), String> {
    let mut worker = state.worker.lock().unwrap();
    if worker.is_none() {
        *worker = Some(tokio::spawn(async move {
            state.process_queue().await;
        }));
    }
    Ok(())
}
```

## Common Patterns
1. State Composition
```rust
#[derive(Default)]
struct AppState {
    config: Arc<RwLock<Config>>,
    cache: Arc<Mutex<Cache>>,
    db: Arc<DbConnection>
}

impl AppState {
    fn new(config: Config) -> Self {
        Self {
            config: Arc::new(RwLock::new(config)),
            cache: Arc::new(Mutex::new(Cache::new())),
            db: Arc::new(DbConnection::new())
        }
    }
}
```

2. Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum StateError {
    #[error("Lock poisoned")]
    LockError,
    #[error("Database error: {0}")]
    DbError(String)
}

#[tauri::command]
async fn db_operation(
    state: State<'_, AppState>
) -> Result<Data, StateError> {
    let db = &state.db;
    db.query().map_err(|e| StateError::DbError(e.to_string()))?;
    Ok(Data::default())
}
```

3. State Initialization
```rust
fn main() {
    tauri::Builder::default()
        .manage(AppState::new(Config::load()?))
        .manage(ApiClient::new())
        .manage(Cache::default())
        .invoke_handler(generate_handler![...])
        .run(generate_context!())
        .expect("error while running application");
}
```

## Security Considerations
- Lock timeouts
- Error message safety
- State access control
- Resource cleanup
- Memory management