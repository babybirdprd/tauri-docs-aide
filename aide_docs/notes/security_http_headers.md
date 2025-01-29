# HTTP Security Headers

## Supported Headers
- Access-Control-* (CORS)
- Cross-Origin-* (COEP/COOP)
- Permissions-Policy
- Timing-Allow-Origin
- X-Content-Type-Options
- Custom Tauri Header

## Config Format
```json
{
	"security": {
		"headers": {
			"Cross-Origin-Opener-Policy": "same-origin",
			"Cross-Origin-Embedder-Policy": "require-corp"
		}
	}
}
```

## Implementation
- Response headers only
- No IPC/error responses
- Value types:
	- String
	- String[]
	- Key-value
	- Null (ignore)

## Framework Support
1. Vite-based:
	 - Headers in vite.config
	 - Dev server config
	 - Framework specific

2. Others:
	 - Next: next.config.js
	 - Angular: angular.json
	 - Rust: Trunk.toml

## Best Practices
- Enable CORP/COEP
- Configure CSP separately
- Dev/prod parity
- Minimal exposure