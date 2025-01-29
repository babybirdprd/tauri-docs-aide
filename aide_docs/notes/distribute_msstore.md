# Microsoft Store Distribution

## Requirements
1. Base Setup
   - Microsoft account
   - Developer enrollment
   - App registration
   - Code signing

2. App Config
   - Unique app name
   - Publisher setup
   - Icon generation
   - Bundle identifier

## Configuration
```json
{
  "bundle": {
	"publisher": "Company Name",
	"windows": {
	  "webviewInstallMode": {
		"type": "offlineInstaller"
	  }
	}
  }
}
```

## Key Features
1. Installer Options
   - EXE/MSI support
   - Offline WebView2
   - Auto-updates
   - Store linking

2. Publishing Process
   - Build installer
   - Sign package
   - Upload binary
   - Store submission

## Best Practices
- Separate store config
- Unique publisher name
- Handle updates
- Offline installer
- Icon management