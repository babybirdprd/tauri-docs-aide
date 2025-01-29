# Linux Distribution

## Package Formats
1. DEB Package
```json
{
    "bundle": {
        "linux": {
            "deb": {
                "files": {
                    "/usr/share/file": "path"
                },
                "depends": ["libwebkit2gtk-4.1-0"]
            }
        }
    }
}
```

2. AppImage
     - Self-contained bundle
     - Cross-distro support
     - GStreamer integration
     - Custom file inclusion

3. Flatpak
```yaml
id: org.app.id
runtime: org.gnome.Platform
modules:
    - name: binary
        sources: [...]
```

4. Snap/RPM
     - Containerized apps
     - RedHat packaging
     - System integration
     - Dependency mgmt

## Implementation
1. Build Features
     - Auto dependency
     - Icon generation
     - Desktop entry
     - System tray

2. Distribution
     - Package repos
     - Store submission
     - Direct download
     - Auto-updates

## Best Practices
1. Package Selection
     - Target audience
     - Distro support
     - Size constraints
     - Update method

2. Configuration
     - Custom files
     - Dependencies
     - Permissions
     - Integration
