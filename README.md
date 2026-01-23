# Real Tools Marketplace

A Claude Code plugin marketplace providing tools for orchestrating complex projects through structured, repeatable workflows.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/Kir-STR/claude-real-tools/releases)

## Overview

**Real Tools** is a marketplace that hosts Claude Code plugins designed to enhance project orchestration capabilities. Each plugin provides a collection of components (skills, agents, commands, hooks) that work together to solve specific workflow challenges.

## Available Plugins

### Real Supervisor

A powerful orchestration plugin that manages complex, multi-step projects through a structured, resumable workflow with specialized worker agents.

**Components:**
- 1 skill: `real-supervisor` (14-step workflow orchestration)
- 6 custom agents: Analyst, Designer, Implementer, Writer, Critique, Tester
- 2 commands: `/supervisor`, `/rs` (alias)
- 3 hooks: SubagentStop, SessionStart, PostToolUse

**Documentation:** [plugins/real-supervisor/README.md](./plugins/real-supervisor/README.md)

**Skill Guide:** [plugins/real-supervisor/skills/real-supervisor/README.md](./plugins/real-supervisor/skills/real-supervisor/README.md)

## Installation

### Quick Install (Recommended)

```bash
# Add the marketplace to Claude Code
/plugin marketplace add Kir-STR/claude-real-tools

# Install a specific plugin
/plugin install real-supervisor@real-tools

# Verify installation
/plugins
```

### Team Configuration

Add to your project's `.claude/settings.json` for automatic marketplace availability:

```json
{
  "extraKnownMarketplaces": {
    "real-tools": {
      "source": {
        "source": "github",
        "repo": "Kir-STR/claude-real-tools"
      }
    }
  }
}
```

## Repository Structure

```
claude-real-tools/                    # Marketplace root
├── .claude-plugin/
│   └── marketplace.json              # Marketplace configuration
├── plugins/
│   └── real-supervisor/              # Real Supervisor plugin
│       ├── .claude-plugin/
│       │   └── plugin.json           # Plugin manifest
│       ├── agents/                   # Worker agent definitions
│       ├── commands/                 # Command implementations
│       ├── hooks/                    # Plugin hooks
│       ├── skills/                   # Skill definitions
│       │   └── real-supervisor/      # Real Supervisor skill
│       │       ├── SKILL.md          # Skill instructions for Claude
│       │       ├── examples.md       # Usage examples
│       │       └── README.md         # Skill user guide
│       └── README.md                 # Plugin documentation
├── CLAUDE.md                         # Instructions for Claude Code
├── LICENSE                           # MIT License
└── README.md                         # This file
```

## Documentation Hierarchy

- **Marketplace README** (this file) - Marketplace overview and plugin list
- **Plugin README** - Plugin components and installation
- **Skill README** - Detailed skill functionality and usage
- **Examples** - Usage patterns and templates

## Version Information

- **Marketplace Version**: 1.0.0
- **Repository**: https://github.com/Kir-STR/claude-real-tools

## License

MIT License - see [LICENSE](./LICENSE) file for details.

## Support

For issues, questions, or feature requests:
- Open an issue on [GitHub](https://github.com/Kir-STR/claude-real-tools/issues)
- Check existing issues for solutions
- Review plugin-specific documentation in `plugins/*/README.md`

## Author

**Kirill Bogdanov**

Marketplace: `real-tools`

---

**Quick Reference:**
- Add marketplace: `/plugin marketplace add Kir-STR/claude-real-tools`
- Install plugin: `/plugin install real-supervisor@real-tools`
- Plugin docs: [plugins/real-supervisor/README.md](./plugins/real-supervisor/README.md)
- Skill guide: [plugins/real-supervisor/skills/real-supervisor/README.md](./plugins/real-supervisor/skills/real-supervisor/README.md)
