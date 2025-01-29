# Tauri Configuration System

## Core Config Files
1. tauri.conf.json
   - App metadata
   - Build settings
   - Runtime behavior
   - Plugin configs
   - Window settings

2. Cargo.toml
   - Rust dependencies
   - Build deps
   - Features flags
   - Version control

3. package.json
   - Frontend deps
   - Build scripts
   - Dev commands
   - Tauri CLI/API

## Format Support
- JSON (default)
- JSON5 (feature flag)
- TOML (feature flag)

## Platform Configs
- tauri.[platform].conf.json
- Merges via RFC 7396
- Platform overrides
- Custom extensions

## Key Features
1. Build Config
   - Dev URL
   - Pre-commands
   - Resource bundling
   - Icon settings

2. Version Management
   - Semantic versioning
   - Lock files
   - Dependency sync
   - Feature flags

3. CLI Integration
   - Build hooks
   - Dev commands
   - Config merging
   - Custom configs