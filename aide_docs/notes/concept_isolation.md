# Tauri Isolation Pattern

## Purpose
- Intercept/modify API msgs pre-Core
- Protect from malicious FE calls
- Counter dev dependency threats
- Secure msg validation layer

## Implementation
1. Core Mechanism
   - iframe sandbox
   - SubtleCrypto encryption
   - Runtime key gen
   - All IPC msgs intercepted

2. Message Flow
   - IPC → Isolation app
   - Hook runs (modify/validate)
   - AES-GCM encrypt
   - Pass to Core
   - Core decrypts

## Technical Details
- Uses browser SubtleCrypto
- AES-GCM encryption
- Per-session keys
- iframe sandbox security

## Limitations
- Win: ext files in sandbox
- Build-time script inline
- No ES modules support

## Best Practices
- Min dependencies
- Simple build steps
- Validate paths/origins
- Always use when possible