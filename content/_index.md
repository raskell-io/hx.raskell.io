+++
title = "hx"
template = "landing.html"
+++

## Why hx?

Haskell's existing build tools are powerful but slow. **hx** brings Rust-level performance to Haskell package management.

### Features

- **Fast dependency resolution** — Parallel downloads and caching
- **Drop-in replacement** — Works with existing `.cabal` files
- **Project management** — Create, build, and manage Haskell projects
- **Modern CLI** — Clean interface with helpful error messages

## Quick Start

```bash
# Install hx
curl -fsSL https://get.raskell.io/hx | sh

# Create a new project
hx new my-project
cd my-project

# Build and run
hx build
hx run
```

## Comparison

| Feature | hx | cabal | stack |
|---------|-----|-------|-------|
| Written in | Rust | Haskell | Haskell |
| Parallel downloads | ✓ | ✓ | ✓ |
| Incremental builds | ✓ | ✓ | ✓ |
| Startup time | ~5ms | ~200ms | ~300ms |

## Part of the raskell.io ecosystem

hx is designed to work seamlessly with [bhc](https://github.com/raskell-io/bhc), the Basel Haskell Compiler — a next-generation Haskell compiler focused on predictable performance and modern concurrency.
