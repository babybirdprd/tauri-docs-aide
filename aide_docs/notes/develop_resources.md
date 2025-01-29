# Resource Management

## Resource Configuration
1. Basic Setup
```json
{
	"tauri": {
		"bundle": {
			"resources": [
				"resources/config/*",
				"assets/images/*.{png,jpg}",
				"data/default.json",
				{
					"path": "locales/*.json",
					"target": "i18n"
				}
			]
		}
	}
}
```

2. Directory Structure
```plaintext
src-tauri/
├── resources/
│   ├── config/
│   │   ├── default.json
│   │   └── production.json
│   ├── assets/
│   │   └── images/
│   └── locales/
│       ├── en.json
│       └── es.json
```

## Implementation
1. Resource Access
```rust
use tauri::api::path::{BaseDirectory, resolve_path};

// Basic resource loading
#[tauri::command]
async fn load_config(
		app: AppHandle,
		name: String
) -> Result<Value, Error> {
		let path = app.path_resolver()
				.resolve_resource(format!("config/{}.json", name))
				.ok_or(Error::ResourceNotFound)?;
		
		let content = fs::read_to_string(path)?;
		serde_json::from_str(&content).map_err(Error::from)
}

// Binary resource handling
#[tauri::command]
async fn load_image(
		app: AppHandle,
		path: String
) -> Result<Vec<u8>, Error> {
		let resource_path = app.path_resolver()
				.resolve_resource(format!("assets/images/{}", path))
				.ok_or(Error::ResourceNotFound)?;
				
		fs::read(resource_path).map_err(Error::from)
}
```

2. Resource Manager
```rust
#[derive(Default)]
struct ResourceManager {
		cache: Arc<RwLock<HashMap<String, Vec<u8>>>>
}

impl ResourceManager {
		async fn get_resource(
				&self,
				app: &AppHandle,
				path: &str
		) -> Result<Vec<u8>, Error> {
				// Check cache first
				if let Some(data) = self.cache.read().await.get(path) {
						return Ok(data.clone());
				}
				
				// Load from filesystem
				let resource_path = app.path_resolver()
						.resolve_resource(path)
						.ok_or(Error::ResourceNotFound)?;
						
				let data = fs::read(resource_path)?;
				
				// Cache the resource
				self.cache.write().await
						.insert(path.to_string(), data.clone());
						
				Ok(data)
		}
		
		async fn clear_cache(&self) {
				self.cache.write().await.clear();
		}
}
```

## Security Implementation
1. Path Validation
```rust
fn validate_resource_path(path: &str) -> Result<(), Error> {
		// Check for path traversal
		if path.contains("..") || path.starts_with('/') {
				return Err(Error::InvalidPath(
						"Path traversal not allowed".into()
				));
		}
		
		// Validate extension
		let extension = Path::new(path)
				.extension()
				.and_then(OsStr::to_str)
				.unwrap_or("");
				
		if !ALLOWED_EXTENSIONS.contains(&extension) {
				return Err(Error::InvalidExtension(
						extension.to_string()
				));
		}
		
		Ok(())
}

#[tauri::command(permit_list = ["fs:read-resource"])]
async fn secure_resource_access(
		app: AppHandle,
		path: String
) -> Result<Vec<u8>, Error> {
		validate_resource_path(&path)?;
		
		let resource_path = app.path_resolver()
				.resolve_resource(&path)
				.ok_or(Error::ResourceNotFound)?;
				
		fs::read(resource_path).map_err(Error::from)
}
```

2. Permission Management
```rust
fn setup_resource_permissions<R: Runtime>(
		app: &mut App<R>
) -> Result<(), Error> {
		// Configure resource access
		app.runtime_authority()
				.enable_permission(
						"fs:read-resource",
						Some("resources/*")
				)?;
				
		Ok(())
}
```

## Error Handling
```rust
#[derive(Debug, thiserror::Error, Serialize)]
enum ResourceError {
		#[error("Resource not found: {0}")]
		NotFound(String),
		#[error("Invalid path: {0}")]
		InvalidPath(String),
		#[error("Invalid extension: {0}")]
		InvalidExtension(String),
		#[error("Permission denied")]
		PermissionDenied,
		#[error(transparent)]
		Io(#[from] std::io::Error)
}

impl From<ResourceError> for String {
		fn from(err: ResourceError) -> Self {
				err.to_string()
		}
}
```

## Frontend Integration
```typescript
// Resource loading in frontend
import { resolveResource } from '@tauri-apps/api/path';
import { invoke } from '@tauri-apps/api/core';

async function loadConfig(name: string): Promise<any> {
		return invoke('load_config', { name });
}

async function loadImage(path: string): Promise<Uint8Array> {
		const data = await invoke('load_image', { path });
		return new Uint8Array(data as number[]);
}

// With resource path resolution
async function getResourcePath(path: string): Promise<string> {
		return resolveResource(path);
}
```

## Best Practices
1. Resource Loading
- Use async loading
- Implement caching
- Validate paths
- Handle missing resources

2. Performance
- Cache frequently used resources
- Lazy load large resources
- Use appropriate buffer sizes
- Clean up unused resources

3. Security
- Validate all paths
- Restrict resource access
- Use permission system
- Handle errors gracefully
```