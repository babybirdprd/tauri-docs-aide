# Node.js Sidecar Integration

## Setup Process
1. Project Structure
```
/sidecar-app/
	package.json
	index.js
/src-tauri/
	binaries/
	tauri.conf.json
```

2. Configuration
```json
{
	"bundle": {
		"externalBin": ["binaries/app"]
	}
}
```

## Implementation
1. Node.js App
```javascript
// Basic sidecar
const command = process.argv[2];
console.log(`response: ${command}`);
```

2. Integration
```rust
// Rust usage
Command::sidecar("app")
	.args([...])
	.execute()?;

// JS usage
Command.sidecar('binaries/app')
	.execute();
```

## Build Process
1. Packaging
	 - pkg bundler
	 - Target triples
	 - Platform bins
	 - Binary naming

2. Distribution
	 - Self-contained
	 - No Node.js req
	 - Cross-platform
	 - Binary assets

## Best Practices
1. Implementation
	 - IPC methods
	 - Error handling
	 - Process lifecycle
	 - Resource cleanup

2. Security
	 - Command validation
	 - Process isolation
	 - Input sanitization
	 - Output handling