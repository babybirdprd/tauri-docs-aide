# Webview Compatibility

## Windows (WebView2)
- Based on Edge/Chromium
- Self-updating engine
- Win7+ support
- Win11 preinstalled
- Auto-installer included

## macOS/iOS (WebKit)
1. Version Format
   - System prefix (17=macOS12)
   - Major.Minor.Tiny.Micro.Nano
   - OS version linked
   - Auto-updates with OS

2. Platform Support
   - macOS 10.10+
   - Core OS component
   - Security updates
   - Version tracking

## Linux (WebKitGTK)
1. Distribution Versions
```table
Distro          | Version  | WebKit
----------------|----------|--------
Ubuntu 22.04    | 2.36     | 614.1.6
Debian 11       | 2.32     | 612.1.6
Ubuntu 20.04    | 2.28     | 610.1.1
```

2. Considerations
   - Distro specific
   - Package updates
   - Version variance
   - Manual tracking

## Best Practices
1. Version Support
   - Min OS versions
   - Feature detection
   - Fallback support
   - Update handling

2. Development
   - Cross-platform testing
   - Feature compatibility
   - Version checking
   - Update mechanisms