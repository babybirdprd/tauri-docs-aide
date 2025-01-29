# Environment Variables

## CLI Variables
1. Core Settings
   - TAURI_CLI_CONFIG_DEPTH
   - TAURI_CLI_PORT
   - TAURI_CLI_NO_DEV_SERVER_WAIT
   - CI: Non-interactive mode

2. Build Config
   - TAURI_SKIP_SIDECAR_SIGNATURE_CHECK
   - TAURI_BUNDLER_WIX_FIPS_COMPLIANT
   - TAURI_LINUX_AYATANA_APPINDICATOR

## Signing Variables
1. General Signing
```bash
TAURI_SIGNING_PRIVATE_KEY="key/path"
TAURI_SIGNING_PRIVATE_KEY_PASSWORD="pass"
```

2. Platform Specific
```bash
# Apple
APPLE_CERTIFICATE="base64"
APPLE_ID="id"
APPLE_TEAM_ID="team"

# Windows
TAURI_WINDOWS_SIGNTOOL_PATH="path"
```

## Hook Variables
1. Build Context
   - TAURI_ENV_DEBUG
   - TAURI_ENV_TARGET_TRIPLE
   - TAURI_ENV_ARCH
   - TAURI_ENV_PLATFORM

2. Platform Info
   - TAURI_ENV_PLATFORM_VERSION
   - TAURI_ENV_FAMILY
   - Target specifics
   - Build context

## Best Practices
1. Usage
   - CLI flags priority
   - Platform specific
   - Build context
   - Security vars

2. Security
   - Key management
   - Path validation
   - Platform checks
   - Access control