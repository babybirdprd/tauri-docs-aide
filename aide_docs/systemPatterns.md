# System Patterns

## Core Architecture
1. Multi-Process Architecture
   - Main process (Rust)
   - WebView process (Frontend)
   - Optional sidecar processes

2. IPC System
   - Command pattern for frontend-to-rust
   - Event system for bidirectional communication
   - Isolation boundaries for security

3. Plugin Architecture
   - Core plugins with capability-based permissions
   - Custom plugin development support
   - Mobile-specific plugin adaptations

## Security Patterns
1. Permission System
   - Capability-based security model
   - Runtime authority checks
   - Plugin-specific permissions

2. Content Security
   - CSP implementation
   - HTTP security headers
   - Secure lifecycle management

## Development Patterns
1. Configuration Management
   - JSON-based config files
   - Environment variables
   - Build-time vs runtime config

2. State Management
   - Global state patterns
   - Persistent storage
   - State synchronization

3. Resource Handling
   - Asset bundling
   - Dynamic resource loading
   - Platform-specific resources

## Distribution Patterns
1. Build System
   - Platform-specific packaging
   - Code signing
   - Update system

2. Store Distribution
   - App Store requirements
   - Microsoft Store packaging
   - Linux distribution channels

## Testing Patterns
1. Integration Testing
   - WebDriver support
   - Mocking system
   - CI/CD integration

2. Debug Tooling
   - DevTools integration
   - VSCode debugging
   - Logging system