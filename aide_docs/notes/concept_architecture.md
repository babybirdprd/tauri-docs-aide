# Tauri Architecture

## Core Components
1. tauri/
   - Main orchestrator
   ```rust
   fn main() {
       tauri::Builder::default()
           .setup(|app| {
               app.manage(AppState::default());
               let window = WindowBuilder::new(app, "main")
                   .title("My App")
                   .build()?;
               Ok(())
           })
           .run(generate_context!())
           .expect("error while running application");
   }
   ```
   - Conf reader (tauri.conf.json)
   - API host
   - Update mgr
   - Script injector

2. Runtime Stack
   ```rust
   struct MyRuntime;
   impl Runtime for MyRuntime {
       type Window = tauri::Window;
       type EventLoopMessage = ();
       
       fn create_window(
           &self,
           app: &AppHandle<Self>,
           label: String,
           builder: WindowBuilder
       ) -> Result<Self::Window> {
           builder.build(app)
       }
   }
   ```
   - tauri-runtime: webview lib bridge
   - tauri-macros: ctx/handler gen
   - tauri-utils: common utils/CSP
   - tauri-build: cargo features
   - tauri-codegen: asset/conf parser
   - tauri-runtime-wry: sys interactions

3. Plugin System
   ```rust
   pub struct MyPlugin<R: Runtime> {
       app: Option<AppHandle<R>>,
       config: PluginConfig
   }

   impl<R: Runtime> Plugin<R> for MyPlugin<R> {
       fn name(&self) -> &'static str {
           "my-plugin"
       }
       
       fn initialize(
           &mut self,
           app: &AppHandle<R>,
           config: serde_json::Value
       ) -> Result<()> {
           self.app = Some(app.clone());
           self.config = serde_json::from_value(config)?;
           Ok(())
       }
   }
   ```

## System Integration
```rust
#[tauri::command]
async fn create_native_window(app: AppHandle) -> Result<(), Error> {
    let window = WindowBuilder::new(&app, "native")
        .title("Native Window")
        .inner_size(800.0, 600.0)
        .center()
        .build()?;
    Ok(())
}

// System event handling
fn setup_system_events<R: Runtime>(app: &mut App<R>) -> Result<(), Error> {
    app.listen_global("system-event", |event| {
        match event.payload() {
            Some("suspend") => { /* Handle system suspend */ }
            Some("resume") => { /* Handle system resume */ }
            _ => {}
        }
    });
    Ok(())
}
```

## Process Management
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum ArchitectureError {
    #[error("Runtime error: {0}")]
    Runtime(String),
    #[error("Plugin error: {0}")]
    Plugin(String),
    #[error("System error: {0}")]
    System(#[from] std::io::Error)
}

#[tauri::command(permit_list = ["process:spawn"])]
async fn secure_spawn(
    app: AppHandle,
    cmd: String
) -> Result<(), Error> {
    validate_command(&cmd)?;
    app.runtime_authority()
       .enable_permission("process:spawn", Some(&cmd))?;
    Command::new(cmd).spawn()?;
    Ok(())
}
```

## Key Points
- No kernel wrapper
- Native OS calls via WRY/TAO
- MIT/Apache-2.0 licensed
- Cross-platform focus
- Process isolation and security