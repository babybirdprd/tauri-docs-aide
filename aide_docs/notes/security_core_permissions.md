# Core Permissions

## Default Groups
1. core:default includes:
   - app:default
   - event:default
   - image:default
   - menu:default
   - path:default
   - resources:default
   - tray:default
   - webview:default
   - window:default

## Key Components
1. App Controls
   - Version info
   - App visibility
   - Theme control
   - Window icons

2. Event System
   - Listen/unlisten
   - Emit events
   - Event targeting

3. Resource Access
   - Path operations
   - File handling
   - Resource cleanup

4. UI Controls
   - Window mgmt
   - Menu system
   - Tray features
   - WebView ops

## Permission Format
- core:<module>:<action>
- Allow/deny variants
- Scope optional
- Command specific

## Implementation
1. Configuration
```json
{
  "tauri": {
    "security": {
      "csp": "default-src 'self'; script-src 'self'",
      "permissions": [
        "app:default",
        {
          "identifier": "fs:read",
          "allow": ["$APPDATA/*", "$RESOURCE/*"],
          "deny": ["$APPDATA/secrets"]
        },
        {
          "identifier": "shell:execute",
          "allow": ["^git", "^npm"],
          "deny": ["^rm", "^sudo"]
        }
      ]
    }
  }
}
```

2. Command Protection
```rust
// Protected command
#[tauri::command(permit_list = ["fs:read"])]
async fn read_file(path: PathBuf) -> Result<String, Error> {
    // Implementation
}

// Multiple permissions
#[tauri::command(permit_list = ["fs:read", "fs:write"])]
async fn save_file(path: PathBuf, content: String) -> Result<(), Error> {
    // Implementation
}

// Scoped permissions
#[tauri::command(permit_list = ["shell:execute"])]
async fn git_pull(app: AppHandle) -> Result<(), Error> {
    Command::new("git")
        .args(["pull", "origin", "main"])
        .spawn()?;
    Ok(())
}
```

3. Runtime Checks
```rust
// Custom permission check
fn check_permission(
    app: &AppHandle,
    permission: &str,
    scope: Option<&str>
) -> bool {
    app.runtime_authority()
       .enable_permission(permission, scope)
       .is_ok()
}

// Usage in command
#[tauri::command]
async fn protected_operation(
    app: AppHandle,
    path: PathBuf
) -> Result<(), Error> {
    if !check_permission(&app, "fs:write", Some(path.to_str().unwrap())) {
        return Err(Error::PermissionDenied);
    }
    // Implementation
}
```

## Permission Groups
1. File System
```rust
// Read permission
#[tauri::command(permit_list = ["fs:read"])]
async fn read_config(path: PathBuf) -> Result<Config, Error> {
    let content = fs::read_to_string(path)?;
    serde_json::from_str(&content).map_err(Error::from)
}

// Write permission
#[tauri::command(permit_list = ["fs:write"])]
async fn write_config(
    path: PathBuf,
    config: Config
) -> Result<(), Error> {
    let content = serde_json::to_string_pretty(&config)?;
    fs::write(path, content)?;
    Ok(())
}
```

2. Shell Access
```rust
// Execute permission
#[tauri::command(permit_list = ["shell:execute"])]
async fn run_script(script: String) -> Result<(), Error> {
    Command::new("sh")
        .arg("-c")
        .arg(script)
        .output()
        .map_err(Error::from)?;
    Ok(())
}

// Scoped execution
#[tauri::command(permit_list = ["shell:execute"])]
async fn npm_install(
    package: String
) -> Result<(), Error> {
    if !package.chars().all(|c| c.is_alphanumeric() || c == '-') {
        return Err(Error::InvalidPackageName);
    }
    Command::new("npm")
        .args(["install", &package])
        .output()?;
    Ok(())
}
```

3. Window Management
```rust
// Window permission
#[tauri::command(permit_list = ["window:create"])]
async fn open_window(
    app: AppHandle,
    label: String
) -> Result<(), Error> {
    WindowBuilder::new(&app, label)
        .title("New Window")
        .build()?;
    Ok(())
}
```

## Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum Error {
    #[error("Permission denied")]
    PermissionDenied,
    #[error("Invalid path: {0}")]
    InvalidPath(String),
    #[error("IO error: {0}")]
    Io(#[from] std::io::Error)
}

impl From<Error> for String {
    fn from(err: Error) -> Self {
        err.to_string()
    }
}
```

## Security Best Practices
1. Permission Scoping
- Limit to specific paths
- Validate all inputs
- Use deny lists
- Explicit allows

2. Runtime Validation
- Check before execution
- Validate paths
- Sanitize inputs
- Log attempts

3. Error Handling
- Safe error messages
- No sensitive data
- Proper logging
- Audit trail