+++
title = "Using the BHC Compiler Backend"
description = "Learn how to use the Basel Haskell Compiler (BHC) with hx for optimized numeric and tensor workloads"
date = 2026-01-25
template = "page.html"

[taxonomies]
tags = ["bhc", "compiler", "tutorial"]

[extra]
author = "raskell.io"
+++

Starting with hx 0.4.2, you can now install and use the Basel Haskell Compiler (BHC) directly through hx. BHC is an alternative Haskell compiler optimized for numeric computing and tensor operations.

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

## Building with BHC

Once configured, use the standard hx commands:

```bash
# Build the project
hx build

# Run the project
hx run

# Run tests
hx test
```

You can also override the backend on the command line:

```bash
# Use BHC for this build only
hx build --backend bhc

# Use GHC for this build only
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

## When to Use BHC

BHC is particularly well-suited for:

- **Numeric computing**: Linear algebra, signal processing, scientific simulations
- **Machine learning**: Tensor operations, neural network training/inference
- **High-performance computing**: Parallel workloads, GPU offloading
- **Edge deployment**: Smaller binaries for embedded systems

For general-purpose Haskell development, GHC remains the recommended choice.

## Resources

- [BHC Documentation](https://github.com/bhc-lang/bhc)
- [hx Configuration Reference](/docs/configuration/)
- [hx CLI Reference](/docs/cli/)
