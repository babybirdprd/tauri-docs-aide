# Content Security Policy

## Implementation
- Config-based CSP
- Script hash validation
- Nonce-based auth
- Auto nonce/hash append
- Compile-time checks

## Key Directives
```json
{
	"default-src": "'self' customprotocol: asset:",
	"connect-src": "ipc: http://ipc.localhost",
	"img-src": "'self' asset: http://asset.localhost blob: data:",
	"style-src": "'unsafe-inline' 'self'"
}
```

## Security Rules
- Avoid remote CDNs
- Restrict trusted hosts
- Local asset priority
- Minimize inline code
- Validate all sources

## Protection
- XSS prevention
- Asset validation
- Load restrictions
- Protocol control
```