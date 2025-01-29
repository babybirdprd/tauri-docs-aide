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
- Granular control
- Module isolation
- Default bundles
- Explicit denials
- Command mapping