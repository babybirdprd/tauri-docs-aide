# App Store Distribution

## Requirements
1. Base Setup
   - Apple Developer Program
   - Code signing setup
   - App Store Connect reg
   - Bundle ID match

2. Platform Config
   - macOS: App Bundle
   - iOS: Xcode project
   - Universal binary opt
   - System version reqs

## Configuration
1. App Store Setup
```json
{
  "bundle": {
	"category": "Utility",
	"macOS": {
	  "entitlements": "./Entitlements.plist",
	  "files": {
		"embedded.provisionprofile": "path/to/profile"
	  }
	}
  }
}
```

2. Required Files
   - Info.plist: Encryption
   - Entitlements.plist: Sandbox
   - Provisioning profiles
   - App icons

## Build Process
1. macOS
   - Build universal binary
   - Create signed .pkg
   - Upload via altool
   - TestFlight validation

2. iOS
   - Build IPA file
   - Export for App Store
   - Upload package
   - Store validation

## Authentication
- App Store Connect API
- API Key management
- Key file placement
- Upload credentials