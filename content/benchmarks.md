+++
title = "Benchmarks"
description = "Performance benchmarks comparing hx native builds to cabal-based builds"
template = "page.html"
+++

# Build Performance Benchmarks

hx includes a native build mode that bypasses Cabal for simple projects, compiling directly with GHC. This results in significantly faster build times for projects without complex build requirements.

## Test Environment

| Property | Value |
|----------|-------|
| **hx version** | 0.3.6 |
| **GHC version** | 9.8.2 |
| **Platform** | macOS (Apple Silicon) |
| **Date** | January 17, 2026 |

**Test project:** Simple 3-module executable with Main.hs, Lib.hs, and Utils.hs. Single dependency on `base`.

---

## Native vs Cabal Build Performance

### Cold Build (Clean)

A fresh build after removing all build artifacts (`.hx/`, `dist-newstyle/`).

| Build Mode | Time | Speedup |
|------------|------|---------|
| **hx native** | 0.48s | — |
| cabal | 2.68s | — |
| **Improvement** | — | **5.6x faster** |

### Incremental Build (No Changes)

Subsequent build with no source file modifications.

| Build Mode | Time | Speedup |
|------------|------|---------|
| **hx native** | 0.05s | — |
| cabal | 0.39s | — |
| **Improvement** | — | **7.8x faster** |

---

## Why Native Builds Are Faster

Native builds skip Cabal's build orchestration entirely:

1. **Direct GHC invocation** — hx constructs the module graph and calls GHC directly
2. **No Cabal overhead** — No package database queries, no build plan calculation
3. **Aggressive caching** — Fingerprint-based caching with minimal I/O
4. **Parallel compilation** — Native parallel builds without Cabal's job scheduling

### When to Use Native Builds

Native builds work best for:
- Single-package projects
- Projects with only `base` dependencies (or no dependencies)
- Quick iteration during development
- CI pipelines where speed matters

For complex projects with many dependencies, hx falls back to Cabal-based builds which provide full dependency resolution and build planning.

---

## Preprocessor Performance

Preprocessors add minimal overhead to native builds:

| Preprocessor | File Type | Additional Time |
|--------------|-----------|-----------------|
| **alex** | `.x` | ~50ms |
| **happy** | `.y` | ~100ms |
| **hsc2hs** | `.hsc` | ~335ms |

---

## Reproducing These Benchmarks

You can reproduce these benchmarks on your own machine:

```bash
# Create test project
mkdir /tmp/hx-bench && cd /tmp/hx-bench

cat > hx.toml << 'EOF'
[project]
name = "hx-bench"

[toolchain]
ghc = "9.8.2"
EOF

cat > hx-bench.cabal << 'EOF'
cabal-version: 3.0
name: hx-bench
version: 0.1.0.0
build-type: Simple

executable hx-bench
    main-is: Main.hs
    other-modules: Lib, Utils
    hs-source-dirs: src
    default-language: GHC2021
    build-depends: base
EOF

mkdir src
echo 'module Main where; import Lib; import Utils; main = putStrLn (format greeting)' > src/Main.hs
echo 'module Lib (greeting) where; greeting = "Hello"' > src/Lib.hs
echo 'module Utils (format) where; format s = ">>> " ++ s ++ " <<<"' > src/Utils.hs

# Benchmark native build (cold)
rm -rf .hx dist-newstyle
time hx build --native

# Benchmark cabal build (cold)
rm -rf .hx dist-newstyle
time hx build

# Benchmark incremental (run again without changes)
time hx build --native
time hx build
```

---

## Historical Results

| Version | Date | Native Cold | Cabal Cold | Speedup |
|---------|------|-------------|------------|---------|
| 0.3.6 | 2026-01-17 | 0.48s | 2.68s | 5.6x |

---

## Methodology

- **Cold builds** are measured after completely removing `.hx/` and `dist-newstyle/` directories
- **Incremental builds** are measured on subsequent runs with no source changes
- Times are wall-clock time from `time` command
- Each measurement is the median of 3 runs
- Benchmarks run on a quiet system with minimal background processes
