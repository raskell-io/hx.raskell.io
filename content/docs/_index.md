+++
title = "Documentation"
description = "Learn how to use hx, the fast Haskell toolchain CLI"
template = "section.html"
sort_by = "weight"
+++

Welcome to the hx documentation. hx is a fast, opinionated, batteries-included toolchain for Haskell.

## Quick Links

- [Installation](/docs/installation) - Get hx installed on your system
- [Quick Start](/docs/quickstart) - Your first hx project
- [CLI Reference](/docs/commands) - All available commands
- [Configuration](/docs/configuration) - Customize hx with `hx.toml`
- [Guides](/docs/guides) - Tutorials and workflow guides

## What is hx?

hx is a unified CLI that wraps and orchestrates the Haskell toolchain. Instead of juggling `ghcup`, `cabal`, `stack`, `fourmolu`, and `hlint` separately, hx provides a single, consistent interface:

```bash
hx init         # Create a new project
hx build        # Build your project
hx test         # Run tests
hx fmt          # Format code
hx doctor       # Diagnose issues
```

## Key Features

- **Fast**: Native build mode for simple projects, optimized dependency fetching
- **Managed toolchains**: Automatic GHC installation and version switching
- **Deterministic builds**: TOML lockfiles with fingerprint verification
- **Excellent errors**: Actionable diagnostics with fix suggestions
- **Batteries included**: Build, test, format, lint, profile, and publish in one tool
