# Binary Size Optimization

## Cargo Profiles

### Stable Profile
```toml
[profile.dev]
incremental = true

[profile.release]
codegen-units = 1
lto = true
opt-level = "s"
panic = "abort"
strip = true
```

### Key Optimizations
- codegen-units=1: Better LLVM opts
- lto=true: Link-time opts
- opt-level="s": Size priority
- panic="abort": No panic handlers
- strip=true: Remove debug syms

## Advanced Options
- trim-paths: Remove path info
- rustflags: Compiler tweaks
- debuginfo=0: No debug syms
- threads=8: Better compile perf

## Size vs Speed
- opt-level="s": Size focus
- opt-level="3": Speed focus
- opt-level="z": Max size reduction