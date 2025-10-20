# Claude Code Plugins for Developers

A curated list of custom Claude Code plugins, agents, and skills for developers.

## 🎯 Overview

This Claude Code plugin marketplace provides plugins that extend Claude Code's capabilities, focusing on developer productivity and automation.

## 📦 Available Plugins

| Plugin | Description | Commands | Agents | Skills | Version |
|--------|-------------|----------|--------|--------|---------|
| [ai-ado](plugins/ai-ado/) | AI-powered Azure DevOps integration with MCP support | `/ado-init`, `/ado-create-feature`, `/ado-create-story`, `/ado-create-task` | - | - | 1.0.0 |
| [ai-git](plugins/ai-git/) | AI-powered git automation and workflow streamlining | `/git-init`, `/git-commit-push` | - | - | 1.1.0 |
| [ai-performance](plugins/ai-performance/) | AI-powered performance optimization and bottleneck detection | `/performance-audit` | `performance-optimizer` | - | 1.0.0 |
| [ai-plugins](plugins/ai-plugins/) | AI-powered plugin scaffolding and generation tools | `/plugins-scaffold` | - | - | 1.1.0 |
| [ai-security](plugins/ai-security/) | AI-powered security auditing with reproducible reports | `/security-init`, `/security-audit` | `security-auditor` | - | 1.1.0 |

## 🚀 Quick Start

### Prerequisites

**New to Claude Code?** Claude Code is an AI-powered CLI tool that helps with software development tasks.

👉 [Download and install Claude Code](https://www.claude.com/product/claude-code)

### Installation

1. Add this marketplace to Claude Code:

```bash
/plugin marketplace add charlesjones-dev/claude-code-plugins-dev
```

2. Install plugins (see **Available Plugins** table above for all options):

```bash
# Install any plugin from this marketplace
/plugin install <plugin-name>@claude-code-plugins-dev

# Examples:
/plugin install ai-ado@claude-code-plugins-dev
/plugin install ai-git@claude-code-plugins-dev
/plugin install ai-security@claude-code-plugins-dev
```

### Usage

Once installed, plugins add slash commands directly to Claude Code. Use any command from the **Available Plugins** table above:

```bash
# Examples:
/git-init              # Initialize .gitignore for project
/security-init         # Initialize security settings
/ado-init              # Initialize Azure DevOps + MCP server configuration
```

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

## 👤 Author

**Charles Jones**

- Website: [charlesjones.dev](https://charlesjones.dev)
- GitHub: [@charlesjones-dev](https://github.com/charlesjones-dev)

## 🔗 Links

- [This Repository](https://github.com/charlesjones-dev/claude-code-plugins-dev)
- [Claude Code Plugins Documentation](https://docs.claude.com/en/docs/claude-code/plugins)

---

**Built with ❤️ for the Claude Code community**
