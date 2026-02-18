# TuxBox Public Registry

> Community-maintained catalog of tools for [TuxBox](https://github.com/disoardi/tuxbox)

[![Tools](https://img.shields.io/badge/tools-2-blue)](#available-tools)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## What is this?

This is the **public registry** for TuxBox — a CLI tool that manages personal tools
distributed across Git repositories. This registry acts as a catalog: it tells TuxBox
where to find each tool and how to run it.

## Quick Start

```bash
# Install TuxBox
curl -fsSL https://raw.githubusercontent.com/disoardi/tuxbox/main/install.sh | sh

# Initialize with this registry
tbox init https://github.com/disoardi/tuxbox-registry-public

# List available tools
tbox list

# Run a tool (auto-clones on first use)
tbox run sshmenuc
tbox run cert_checker -- check --host google.com
```

## Available Tools

| Tool | Description | Type | Author |
|------|-------------|------|--------|
| [sshmenuc](https://github.com/disoardi/sshmenuc) | SSH connection manager with interactive TUI menu | Python | [@disoardi](https://github.com/disoardi) |
| [cert_checker](https://github.com/disoardi/cert-checker) | SSL/TLS certificate management and verification | Python + Docker | [@disoardi](https://github.com/disoardi) |

## Want to add your tool?

See [CONTRIBUTING.md](CONTRIBUTING.md) — it takes about 5 minutes.

## License

MIT — see [LICENSE](LICENSE)
