+++
title = "Using the BHC Compiler Backend"
description = "Learn how to use the Basel Haskell Compiler (BHC) with hx for optimized numeric and tensor workloads"
date = 2026-01-25
updated = 2026-02-02
template = "page.html"

[taxonomies]
tags = ["bhc", "compiler", "tutorial"]

[extra]
author = "raskell.io"
+++

Starting with hx 0.4.2, you can install and use the Basel Haskell Compiler (BHC) directly through hx. BHC is an alternative Haskell compiler optimized for numeric computing and tensor operations.

**hx 0.5.0** deepens BHC integration significantly: full test and run support, `--backend` on init/new, dedicated numeric and server project templates, and BHC Platform curated snapshots.

## Installing BHC

Install BHC with a single command:

```bash
# Install the latest version
hx toolchain install --bhc latest

# Or install a specific version
hx toolchain install --bhc 0.2.0

# Install and set as active
hx toolchain install --bhc 0.2.0 --set
```

BHC binaries are installed to `~/.bhc/versions/<version>/`.

## Creating a BHC Project

The fastest way to get started is with a dedicated template:

```bash
# Numeric computing project (hmatrix, vector, massiv, statistics)
hx new numeric my-science

# Web server project (Servant, Warp, WAI)
hx new server my-api
```

These templates come pre-configured with the BHC backend, an optimized profile, and relevant dependencies.

You can also create any project type with BHC:

```bash
# New project with BHC from the start
hx init --backend bhc

# Standard templates with BHC
hx new webapp my-app --backend bhc
hx new cli my-tool --backend bhc
hx new library my-lib --backend bhc
```

## Configuring Your Project

To use BHC for your project, set the compiler backend in your `hx.toml`:

```toml
[project]
name = "my-numeric-project"
version = "0.1.0"

[compiler]
backend = "bhc"
version = "0.2.0"
```

## BHC-Specific Options

BHC provides several optimization options for different workloads. Configure them in the `[compiler.bhc]` section:

```toml
[compiler]
backend = "bhc"

[compiler.bhc]
profile = "numeric"
emit_kernel_report = true
tensor_fusion = true
```

### Profiles

BHC supports four optimization profiles:

| Profile | Use Case |
|---------|----------|
| `default` | Balanced optimizations for general workloads |
| `server` | Optimized for long-running server applications |
| `numeric` | Aggressive optimizations for numeric/scientific computing |
| `edge` | Optimized for resource-constrained environments |

### Options

- **`emit_kernel_report`**: When enabled, BHC generates a performance report for compute kernels after compilation
- **`tensor_fusion`**: Enables tensor fusion optimizations that combine multiple tensor operations into single kernels
- **`target`**: Cross-compilation target (e.g., `wasm32`, `aarch64-linux`)

## BHC Platform Snapshots

**New in hx 0.5.0.** BHC Platform provides curated package sets — the BHC equivalent of Stackage. Each snapshot contains ~70 packages verified to build together under BHC.

```bash
# See what's available
hx bhc-platform list

# Learn about a snapshot
hx bhc-platform info bhc-platform-2026.1

# See all packages
hx bhc-platform info bhc-platform-2026.1 --packages

# Set it for your project
hx bhc-platform set bhc-platform-2026.1
```

Or configure directly in `hx.toml`:

```toml
[bhc-platform]
snapshot = "bhc-platform-2026.1"
```

The initial `bhc-platform-2026.1` snapshot covers:

- **Core**: base, text, bytestring, containers, vector
- **Web**: servant, warp, wai, aeson
- **Numeric**: hmatrix, statistics, massiv
- **Testing**: hspec, QuickCheck, tasty, hedgehog
- **And more**: mtl, lens, megaparsec, conduit, optparse-applicative, ...

When you run `hx lock` with a BHC Platform configured, package versions from the snapshot are automatically pinned in the resolver. No manual version constraints needed.

If you need a package that's not in the snapshot:

```toml
[bhc-platform]
snapshot = "bhc-platform-2026.1"
allow_newer = true
extra_deps = { some-extra-package = "1.0.0" }
```

## Building, Testing, and Running with BHC

All major commands work with the BHC backend — build, test, and run:

```bash
# Build the project
hx build

# Run tests
hx test

# Run the project
hx run

# Pass arguments to your program
hx run -- --input data.csv --output results.json
```

You can also override the backend on the command line:

```bash
# Use BHC for this command only
hx build --backend bhc
hx test --backend bhc
hx run --backend bhc

# Use GHC for this command only
hx build --backend ghc
```

## Checking BHC Status

Use `hx doctor` to verify your BHC installation:

```bash
hx doctor
```

If BHC is not installed but your project requires it, `hx doctor` will show:

```
bhc: not installed
  fix: Run `hx toolchain install --bhc latest`
```

## Putting It All Together

Here's a complete example of a numeric project using BHC with a curated platform snapshot:

```toml
# hx.toml
[project]
name = "ml-pipeline"
version = "0.1.0"

[compiler]
backend = "bhc"

[compiler.bhc]
profile = "numeric"
tensor_fusion = true
emit_kernel_report = true

[bhc-platform]
snapshot = "bhc-platform-2026.1"

[build]
release = true
```

```bash
# Create the project
hx new numeric ml-pipeline
cd ml-pipeline

# Build
hx build

# Run tests
hx test

# Run
hx run

# Check optimization report
cat .hx/bhc-reports/kernel-report.txt
```

## When to Use BHC

BHC is particularly well-suited for:

- **Numeric computing**: Linear algebra, signal processing, scientific simulations
- **Machine learning**: Tensor operations, neural network training/inference
- **Web servers**: Long-running services with consistent latency
- **High-performance computing**: Parallel workloads, GPU offloading
- **Edge deployment**: Smaller binaries for embedded systems

For general-purpose Haskell development, GHC remains the recommended choice.

## Resources

- [Compiler Backends](/docs/features/compiler-backends) — Full GHC vs BHC comparison
- [BHC Platform](/docs/features/bhc-platform) — Curated snapshot guide
- [hx bhc-platform](/docs/commands/bhc-platform) — CLI command reference
- [hx new](/docs/commands/new) — Project templates
- [hx.toml Reference](/docs/configuration/hx-toml) — Configuration details
