+++
title = "hx 0.6.0: Zero-Config BHC Experience"
description = "hx 0.6.0 makes going from zero to a running BHC project effortless — auto-detected backends, bundled toolchain installs, and smarter doctor checks"
date = 2026-02-03
template = "page.html"

[taxonomies]
tags = ["bhc", "release", "toolchain"]

[extra]
author = "raskell.io"
+++

hx 0.6.0 focuses on a single goal: getting from nothing to a running BHC project with as few steps as possible. If BHC is your compiler, hx now handles the rest automatically.

## One Command to Rule Them All

Previously, setting up a BHC project required several manual steps — installing BHC, then GHC, then Cabal, then creating a project with the right flags. Now it's:

```bash
hx toolchain install --bhc latest
hx new numeric my-science
cd my-science
hx run
```

That first command does more than it used to. When you install BHC without specifying `--ghc` or `--cabal`, hx now automatically installs the matching GHC and Cabal versions required by the BHC Platform:

```
$ hx toolchain install --bhc latest
  Installing BHC 0.3.0
  Done BHC 0.3.0 installed
  Also installing GHC 9.10.1 (required by BHC Platform bhc-platform-2026.2)
  Done GHC 9.10.1 installed
  Also installing Cabal 3.12.1.0 (recommended for BHC Platform)
  Done Cabal 3.12.1.0 installed
```

No guessing which GHC version pairs with which BHC. The platform snapshot defines the compatibility matrix, and hx follows it.

## Smart Backend Detection

`hx new` now auto-detects which compiler backend to use. If BHC is installed and GHC is not — the typical state after a fresh `hx toolchain install --bhc latest` — new projects default to the BHC backend automatically:

```bash
# No --backend flag needed
hx new cli my-tool
hx new webapp my-app
hx new library my-lib
hx new numeric my-science
hx new server my-api
```

All of these will use BHC when it's the only compiler available. If both BHC and GHC are installed, hx keeps the existing GHC default. You can always override with `--backend bhc` or `--backend ghc`.

## Automatic Platform Snapshots in Templates

When a project is created with the BHC backend, hx now looks up the best matching BHC Platform snapshot for your installed version and writes it into `hx.toml`:

```toml
[project]
name = "my-science"
version = "0.1.0"

[compiler]
backend = "bhc"

[bhc-platform]
snapshot = "bhc-platform-2026.2"
```

No need to run `hx bhc-platform list` and `hx bhc-platform set` separately. The snapshot is selected using major.minor version matching — BHC `0.3.x` maps to `bhc-platform-2026.2`, BHC `0.2.x` maps to `bhc-platform-2026.1`.

## Smarter Doctor Checks

`hx doctor` now understands the BHC Platform and checks for compatibility issues:

```
$ hx doctor
  ✓ bhc: 0.3.0
  ✓ BHC Platform: bhc-platform-2026.2 (90 packages)
  ✓ ghc: 9.10.1
```

When something is off, doctor tells you exactly what to do:

- **GHC version mismatch**: If your GHC doesn't match what the platform expects, doctor warns you and suggests the right install command
- **No matching platform**: If your BHC version doesn't have a compatible snapshot, doctor suggests upgrading
- **Newer platform available**: If you're on an older BHC version and a newer platform exists, doctor lets you know
- **Project snapshot mismatch**: If your `hx.toml` references a snapshot that doesn't match your installed BHC, doctor flags it

## The Full Zero-Config Flow

Here's what going from zero to running looks like with hx 0.6.0:

```bash
# Install everything in one shot
hx toolchain install --bhc latest

# Create a project (BHC auto-detected, snapshot auto-selected)
hx new numeric my-ml-pipeline
cd my-ml-pipeline

# Verify everything is wired up
hx doctor

# Build and run
hx build
hx run
```

Five commands. No configuration files to edit, no version matrices to look up, no compatibility tables to cross-reference.

## Upgrading

```bash
# Self-update hx
hx self-update

# Or install fresh
curl -fsSL https://hx.raskell.io/install.sh | sh
```

## Resources

- [BHC Platform Snapshots](/docs/features/bhc-platform) — Curated package set guide
- [Compiler Backends](/docs/features/compiler-backends) — GHC vs BHC comparison
- [hx toolchain install](/docs/commands/toolchain) — Toolchain management reference
- [hx new](/docs/commands/new) — Project template reference
