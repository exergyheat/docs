# Exergy Documentation

Open-source documentation for hashrate heating with Home Assistant.

This repository contains the source files for [docs.exergyheat.com](https://docs.exergyheat.com) — guides for integrating bitcoin miners into your smart home heating system.



## What's Inside

- **DIY Guides** — Set up Home Assistant, install Exergy integrations, configure automations
- **Integration Docs** — Canaan Avalon miners, Ocean mining pool
- **Automation Blueprints** — Thermostat control, HVAC integration, time-of-use scheduling
- **Dashboard Templates** — Monitor and control your hashrate heating system

## Prerequisites

This site is built with [mdBook](https://rust-lang.github.io/mdBook/). You'll need:

- [Rust](https://www.rust-lang.org/tools/install) (to install mdBook)
- mdBook (`cargo install mdbook`)

Or install mdBook directly via Homebrew (macOS):

```bash
brew install mdbook
```

## Getting Started

### Clone the Repository

```bash
git clone https://github.com/exergyheat/docs.git
cd docs
```

### Run Locally

Start the development server with live reload:

```bash
mdbook serve
```

Open [http://localhost:3000](http://localhost:3000) in your browser. Changes to `.md` files auto-refresh.

### Build for Production

```bash
mdbook build
```

Output is generated in the `book/` directory.

## Project Structure

```
.
├── book.toml          # mdBook configuration
├── src/
│   ├── SUMMARY.md     # Navigation structure
│   ├── introduction.md
│   ├── diy/           # DIY guides
│   │   ├── brains/    # Home Assistant setup
│   │   ├── integrations/  # Miner integrations
│   │   ├── blueprints/    # Automation templates
│   │   └── dashboards/    # Dashboard templates
│   └── reference/     # Reference docs
└── theme/             # Custom styling
```

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

For bug reports and feature requests, open an issue on GitHub.

## Related Repositories

- [Exergy Canaan Integration](https://github.com/exergyheat) — Home Assistant integration for Canaan miners
- [Exergy Ocean Integration](https://github.com/exergyheat) — Home Assistant integration for Ocean pool

## License

MIT License — see [LICENSE](LICENSE) for details.

## Links

- **Website**: [exergyheat.com](https://exergyheat.com)
- **Documentation**: [docs.exergyheat.com](https://docs.exergyheat.com)
- **Community**: [support.exergyheat.com](https://support.exergyheat.com)
