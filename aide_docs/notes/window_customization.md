# Window Customization

## Window Creation
1. Basic Window
```rust
// In main.rs
let window = WindowBuilder::new(app, "main")
	.title("My App")
	.inner_size(800.0, 600.0)
	.min_inner_size(400.0, 300.0)
	.position(500.0, 300.0)
	.fullscreen(false)
	.resizable(true)
	.decorations(true)
	.always_on_top(false)
	.build()?;

// Runtime creation
#[tauri::command]
async fn create_window(app: AppHandle) -> Result<(), Error> {
	WindowBuilder::new(&app, "new-window")
		.title("New Window")
		.center()
		.build()
		.map_err(Error::from)
}
```

2. Configuration
```json
{
  "tauri": {
	"windows": [
	  {
		"label": "main",
		"title": "My App",
		"width": 800,
		"height": 600,
		"resizable": true,
		"fullscreen": false,
		"decorations": false,
		"transparent": true,
		"titleBarStyle": "overlay",
		"hiddenTitle": true,
		"theme": "Dark"
	  }
	]
  }
}
```

## Custom Titlebar
1. HTML/CSS Implementation
```html
<!-- index.html -->
<div data-tauri-drag-region class="titlebar">
  <div class="title">My App</div>
  <div class="controls">
	<button id="minimize">-</button>
	<button id="maximize">□</button>
	<button id="close">×</button>
  </div>
</div>
```

```css
.titlebar {
  height: 30px;
  background: #2f2f2f;
  user-select: none;
  display: flex;
  justify-content: space-between;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
}

[data-tauri-drag-region] {
  cursor: move;
}
```

2. Window Controls
```typescript
// window_controls.ts
import { appWindow } from '@tauri-apps/api/window';

document.getElementById('minimize')?.addEventListener('click', () => {
  appWindow.minimize();
});

document.getElementById('maximize')?.addEventListener('click', async () => {
  const isMaximized = await appWindow.isMaximized();
  isMaximized ? appWindow.unmaximize() : appWindow.maximize();
});

document.getElementById('close')?.addEventListener('click', () => {
  appWindow.close();
});
```

## Platform-Specific Features
1. macOS Customization
```rust
#[cfg(target_os = "macos")]
fn customize_macos_window(window: &Window) -> Result<(), Error> {
	use window_vibrancy::{apply_vibrancy, NSVisualEffectMaterial};
	
	apply_vibrancy(window, NSVisualEffectMaterial::HudWindow, None, None)?;
	
	window.set_transparent_titlebar(true);
	window.set_title_bar_buttons_visibility(false);
	window.set_title_bar_style(TitleBarStyle::Overlay);
	
	Ok(())
}

// Usage in command
#[tauri::command]
async fn setup_window(window: Window) -> Result<(), Error> {
	#[cfg(target_os = "macos")]
	customize_macos_window(&window)?;
	
	Ok(())
}
```

2. Windows Customization
```rust
#[cfg(target_os = "windows")]
fn customize_windows_window(window: &Window) -> Result<(), Error> {
	use window_shadows::set_shadow;
	use window_vibrancy::apply_blur;
	
	set_shadow(window, true)?;
	apply_blur(window, Some((18, 18, 18, 125)))?;
	
	Ok(())
}
```

## Event Handling
1. Window Events
```rust
#[tauri::command]
async fn handle_window_events(window: Window) -> Result<(), Error> {
	window.on_window_event(|event| match event {
		WindowEvent::Focused(focused) => {
			// Handle focus change
		}
		WindowEvent::CloseRequested { api, .. } => {
			// Handle close request
			api.prevent_close();
		}
		WindowEvent::Moved(position) => {
			// Handle window movement
		}
		WindowEvent::Resized(size) => {
			// Handle window resize
		}
		_ => {}
	});
	Ok(())
}
```

2. Monitor Management
```rust
#[tauri::command]
async fn manage_monitors(window: Window) -> Result<(), Error> {
	let monitor = window.current_monitor()?
		.ok_or(Error::NoMonitor)?;
	
	let size = monitor.size();
	window.set_size(PhysicalSize::new(
		size.width / 2,
		size.height / 2
	))?;
	
	window.center()?;
	Ok(())
}
```

## Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum WindowError {
	#[error("Window creation failed: {0}")]
	Creation(String),
	#[error("Window not found: {0}")]
	NotFound(String),
	#[error("Platform error: {0}")]
	Platform(String),
	#[error(transparent)]
	Tauri(#[from] tauri::Error)
}

impl From<WindowError> for String {
	fn from(err: WindowError) -> Self {
		err.to_string()
	}
}
```

## Security Considerations
1. Permission Configuration
```json
{
  "tauri": {
	"security": {
	  "csp": "default-src 'self'; img-src 'self' data: https:; style-src 'self' 'unsafe-inline'",
	  "permissions": [
		"window:allow-start-dragging",
		{
		  "identifier": "window:create",
		  "allow": ["main", "settings"]
		}
	  ]
	}
  }
}
```

2. Runtime Checks
```rust
fn validate_window_access(
	app: &AppHandle,
	window_label: &str
) -> Result<(), WindowError> {
	if !app.runtime_authority()
		   .enable_window(window_label)
		   .is_ok() {
		return Err(WindowError::NotFound(
			window_label.to_string()
		));
	}
	Ok(())
}
```