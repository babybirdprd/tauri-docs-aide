# Resource Management

## Configuration
```json
{
	"bundle": {
		"resources": [
			"path/to/file.txt",
			"dir/**/*",
			{
				"src/path": "dest/path"
			}
		]
	}
}
```

## Path Patterns
1. File Access
	 - dir/file.txt: Single file
	 - dir/: Recursive all
	 - dir/*: Non-recursive
	 - dir/**/*: Recursive files
	 - $RESOURCE scope required

2. Implementation
```rust
// Rust Access
app.path().resolve("path", BaseDirectory::Resource)?;

// JS Access
import { resolveResource } from '@tauri-apps/api/path';
await resolveResource('path');
```

## Security
1. Permissions
	 - fs:allow-read-text-file
	 - fs:allow-resource-read
	 - Explicit paths only
	 - Scope restrictions

2. Best Practices
	 - Use relative paths
	 - Explicit permissions
	 - Resource scoping
	 - Path validation
```