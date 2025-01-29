# Code Signing

## Windows Signing
1. Certificate Types
   - OV (Organization Validated)
   - EV (Extended Validation)
   - SmartScreen reputation
   - Store requirements

2. Implementation
```json
{
  "windows": {
    "certificateThumbprint": "hash",
    "digestAlgorithm": "sha256",
    "timestampUrl": "url"
  }
}
```

## macOS Signing
1. Requirements
   - Apple Developer account
   - Code signing certificate
   - Keychain access
   - Notarization setup

2. Authentication
   - API key method:
     - APPLE_API_ISSUER
     - APPLE_API_KEY
     - APPLE_API_KEY_PATH
   - Apple ID method:
     - APPLE_ID
     - APPLE_PASSWORD
     - APPLE_TEAM_ID

## Azure Options
1. Key Vault
   - Certificate storage
   - Secret management
   - Role assignments
   - Relic integration

2. Code Signing
```yaml
signCommand: "trusted-signing-cli -e url -a account -c profile %1"
```

## CI/CD Integration
1. GitHub Actions
   - Base64 certificates
   - Secret management
   - Automated signing
   - Cross-platform

2. Environment Setup
   - Certificate import
   - Key management
   - Auth credentials
   - Build pipeline

## Best Practices
- Secure certificate storage
- Environment variables
- Platform-specific flows
- Automated notarization
```