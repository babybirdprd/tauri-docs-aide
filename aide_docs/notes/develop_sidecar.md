# Sidecar System

## Configuration
```json
{
	"bundle": {
		"externalBin": [
			"binaries/sidecar",
			"../path/to/bin"
		]
	}
}
```

## Implementation
1. Binary Structure
	 - Target-specific bins
	 - Platform suffixes
	 - Architecture matching
	 - Extension handling

2. Permissions
```json
{
	"identifier": "shell:allow-execute",
	"allow": [{
		"name": "bin/app",
		"sidecar": true,
		"args": ["static", {"validator": "\\S+"}]
	}]
}
```

## Usage
1. Rust API
```rust
app.shell().sidecar("name")
	 .args([...])
	 .spawn()?;
```

2. JS API
```js
Command.sidecar('bin/name')
	.execute();
```

## Best Practices
- Target triple naming
- Explicit permissions
- Argument validation
- Event handling
- Path resolution