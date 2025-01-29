# Permission System

## Core Structure
```toml
[[permission]]
identifier = "id"
description = "desc"
commands.allow = ["cmd"]

[[scope.allow]]
scope = "$PATH/*"

[[scope.deny]]
scope = "$PATH/secret"
```

## Key Concepts
1. Identifiers
   - Format: name:command-name
   - Max len: 116 chars
   - ASCII lowercase only
   - Plugin prefix auto-added

2. Permission Sets
   - Groupable permissions
   - Reusable configs
   - OS-specific bundles
   - Extendable defs

3. File Structure
   - Plugin: permissions/<id>.toml
   - App: src-tauri/permissions/
   - Capabilities: json/toml format
   - Default auto-included

## Implementation
- Command privileges
- Scope mapping
- Allow/deny rules
- Plugin integration
- Path restrictions

## Best Practices
- Explicit privileges
- Minimal access
- Clear descriptions
- Logical grouping
- Scope validation