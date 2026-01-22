# Real Tools Marketplace

Official Claude Code plugin marketplace featuring the **Real Supervisor** plugin.

## What's Included

### Real Supervisor Plugin
A powerful orchestration plugin that manages complex, multi-step projects through structured, resumable workflows with specialized worker agents.

**Features:**
- 14-step workflow with human-in-the-loop approval gates
- 6 specialized worker agents (Analyst, Designer, Implementer, Writer, Critique, Tester)
- Resumable sessions with automatic state management
- Checkpoint support for safe rollback
- Complete audit trail with session logging

## Installation

### Quick Install (Recommended)

```bash
# Add the marketplace
/plugin marketplace add Kir-STR/claude-real-tools

# Install the plugin
/plugin install real-supervisor@real-tools
```

### Local Development

```bash
# Clone the repository
git clone https://github.com/Kir-STR/claude-real-tools.git
cd claude-real-tools

# Create symlink to plugin
# Linux/Mac
ln -s $(pwd)/plugins/real-supervisor ~/.config/claude-code/plugins/real-supervisor

# Windows (PowerShell, as Administrator)
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\claude-code\plugins\real-supervisor" -Target "$(Get-Location)\plugins\real-supervisor"

# Reload plugins
/reload-plugins
```

## Usage

After installation, invoke the supervisor with:

```bash
/rs path/to/your/prd.md
```

or

```bash
/supervisor path/to/your/prd.md
```

The plugin will orchestrate the entire workflow from PRD analysis to final deliverable creation.

## Updating

```bash
/plugin update real-supervisor@real-tools
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
│       ├── skills/real-supervisor/   # 1 skill
│       │   ├── SKILL.md              # Skill instructions
│       │   ├── examples.md           # Usage examples
│       │   └── README.md             # Skill documentation
│       ├── agents/                   # 6 worker agents
│       │   ├── analyst.md
│       │   ├── designer.md
│       │   ├── implementer.md
│       │   ├── writer.md
│       │   ├── critique.md
│       │   └── tester.md
│       ├── commands/                 # 2 commands: /rs and /supervisor
│       ├── hooks/                    # 3 hooks: SubagentStop, SessionStart, PostToolUse
│       └── README.md                 # Plugin documentation
├── CLAUDE.md                         # Instructions for Claude Code
├── LICENSE                           # MIT License
└── README.md                         # This file
```

## Team Configuration

Add to your project's `.claude/settings.json` for automatic installation:

```json
{
  "extraKnownMarketplaces": {
    "real-tools": {
      "source": {
        "source": "github",
        "repo": "Kir-STR/claude-real-tools"
      }
    }
  },
  "enabledPlugins": {
    "real-supervisor@real-tools": true
  }
}
```

## Documentation

- **Plugin Guide**: `plugins/real-supervisor/README.md`
- **Skill Guide**: `plugins/real-supervisor/skills/real-supervisor/README.md`
- **Usage Examples**: `plugins/real-supervisor/skills/real-supervisor/examples.md`

## Resources

- **Repository**: https://github.com/Kir-STR/claude-real-tools
- **Issues**: https://github.com/Kir-STR/claude-real-tools/issues
- **License**: MIT

## Version

Current version: **1.0.0**

---

**Author**: Kirill Bogdanov
**Marketplace**: real-tools
