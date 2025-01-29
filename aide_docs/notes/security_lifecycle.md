# Application Lifecycle Security

## Upstream Threats
1. Dependencies
   - Keep Tauri updated
   - Audit npm/cargo deps
   - Use cargo-vet/crev
   - Pin git revisions
   - Monitor supply chain

2. Dev Environment
   - Secure dev server
   - Network isolation
   - mTLS for untrusted nets
   - System hardening
   - Access control

## Build Security
1. CI/CD
   - Trusted systems
   - Pinned versions
   - Binary signing
   - Hardware tokens
   - Reproducible builds

2. Distribution
   - Secure updates
   - Trusted hosting
   - Manifest control
   - Build verification

## Runtime Protection
1. Webview Security
   - CSP enforcement
   - Capability limits
   - API restrictions
   - Content isolation

2. Best Practices
   - Min admin access
   - No prod secrets
   - Hardware security
   - Version control
   - Vuln reporting