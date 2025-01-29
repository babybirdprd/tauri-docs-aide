# Tauri Security Model

## Trust Boundaries
- Core (Rust) vs Frontend (WebView)
- IPC layer bridges trust groups
- Core: Full sys access
- WebView: Limited access via IPC

## Key Components
1. Access Control
   - Permissions system
   - Capability configs
   - Runtime authority
   - Scoped access

2. Frontend Security
   - CSP implementation
   - Isolation pattern
   - Custom stack support
   - Attack surface control

3. WebView Strategy
   - Uses OS webview
   - No bundled webview
   - Faster security patches
   - Reduced vulnerability window

## Security Features
- IPC boundary enforcement
- Command restrictions
- Resource access control
- Fine-grained perms
- OS-level isolation

## Best Practices
- Validate boundary data
- Limit exposed resources
- Use isolation pattern
- Monitor dependencies
- Follow disclosure policy

## Vulnerability Reporting
- Github Security Advisory
- security@tauri.app
- No public disclosure
- Coordinated response