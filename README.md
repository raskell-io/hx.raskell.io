# hx.raskell.io

Website for [hx](https://github.com/raskell-io/hx) — an extremely fast Haskell package and project manager, written in Rust.

**Live site:** https://hx.raskell.io

## Overview

This repository contains the source for the hx product website. Built with [Zola](https://www.getzola.org/) using the `purple-haze` theme.

## Development

### Prerequisites

- [Zola](https://www.getzola.org/documentation/getting-started/installation/) (0.18+)
- Or use [mise](https://mise.jdx.dev/) to manage the environment:
  ```bash
  mise install
  ```

### Local Development

```bash
# Start development server with live reload
zola serve

# Build for production
zola build

# Check for errors
zola check
```

The development server runs at `http://127.0.0.1:1111` by default.

## Project Structure

```
.
├── config.toml          # Zola configuration
├── content/             # Markdown content pages
├── static/              # Static assets (images, fonts, etc.)
├── templates/           # HTML templates (if overriding theme)
└── themes/
    └── purple-haze/     # Custom theme
        ├── sass/        # SCSS stylesheets
        └── templates/   # Theme templates
```

## Deployment

The site is automatically deployed on push to the main branch.

## License

MIT
