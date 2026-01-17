# Purple Haze

A Haskell-inspired Zola theme with Catppuccin colors, designed for product landing pages and documentation.

## Features

- **Haskell-inspired aesthetics**: Clean white backgrounds with purple accents, honoring the Haskell brand
- **Catppuccin color scheme**: Full Latte (light) and Mocha (dark) theme support
- **Product mode**: Beautiful landing pages for developer tools
- **Documentation mode**: Clean, readable docs with versioning support
- **No blog**: Focused on what matters for tooling projects

## Usage

Add as a git submodule:

```bash
cd your-zola-site
git submodule add https://github.com/raskell-io/purple-haze themes/purple-haze
```

In your `config.toml`:

```toml
theme = "purple-haze"

[extra]
mode = "product"  # or "docs"
```

## Color Palette

Purple Haze uses Catppuccin colors with purple emphasis:

- **Primary**: Haskell Purple (`#5e5086`)
- **Accent**: Catppuccin Mauve
- **Links**: Catppuccin Lavender
- **Default theme**: Light (clean white background)

## Modes

### Product Mode

For tool landing pages like hx.raskell.io:

```toml
[extra]
mode = "product"

[extra.product]
tagline = "Next-generation Haskell tooling"
install_command = "curl -fsSL https://get.raskell.io/hx | sh"
```

### Docs Mode

For documentation sites:

```toml
[extra]
mode = "docs"

[extra.docs]
versioned = true
default_version = "latest"
```

## Built for

- [hx](https://github.com/raskell-io/hx) - Extremely fast Haskell package manager
- [bhc](https://github.com/raskell-io/bhc) - Basel Haskell Compiler

## License

MIT
