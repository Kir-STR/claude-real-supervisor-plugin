# CLAUDE.md

## Project Type
Claude Code plugin marketplace **Real Tools**

## Repository Structure

```
claude-real-tools/                               # Marketplace root
├── .claude-plugin/
│   └── marketplace.json                         # Marketplace configuration
├── plugins/
│   └── real-supervisor/                         # Real Supervisor plugin
│       ├── .claude-plugin/
│       │   └── plugin.json                      # Plugin manifest
│       ├── skills/real-supervisor/              # Supervisor skill
│       │   ├── SKILL.md                         # Skill instructions for Claude
│       │   ├── examples.md                      # Usage examples and structure examples
│       │   └── README.md                        # Skill documentation for users
│       ├── agents/                              # Worker agent definitions (auto-discovered)
│       ├── commands/                            # 2 commands: /rs, /supervisor
│       ├── hooks/                               # 3 hooks (SubagentStop, SessionStart, PostToolUse)
│       └── README.md                            # Plugin documentation
├── README.md                                    # Marketplace documentation (for users)
├── CLAUDE.md                                    # This file (for Claude Code)
└── LICENSE

```

## What This Repository Is

**Real Tools Marketplace** - a Claude Code plugin marketplace containing the **Real Supervisor Plugin**.

The Real Supervisor plugin orchestrates complex, multi-step projects through a 14-step workflow with 6 specialized worker agents, state management, checkpointing, and human-in-the-loop approval gates.

### Plugin Components
- **1 skill**: real-supervisor (orchestration workflow)
- **6 agents**: Analyst, Designer, Implementer, Writer, Critique, Tester
- **2 commands**: /rs (alias), /supervisor (full)
- **3 hooks**: SubagentStop, SessionStart, PostToolUse

## Key Configuration Files

### Marketplace Level
- `.claude-plugin/marketplace.json` - Marketplace configuration listing available plugins
  - Source path: `"./plugins/real-supervisor"` points to the plugin directory

### Plugin Level
- `plugins/real-supervisor/.claude-plugin/plugin.json` - Plugin manifest
  - Defines skills, commands, and hooks paths relative to plugin root
  - Skills: `"./skills/"`
  - Commands: `"./commands/"`
  - Hooks: `"./hooks/hooks.json"`

## File Conventions

### Documentation Separation
- **For users**: `README.md` files (installation, usage, features)
- **For Claude**: Skill instructions in `.md` files under `skills/`, `CLAUDE.md` in project root

### Do Not Create
- Additional documentation files unless explicitly requested
- README files in every directory
- Extensive comments in configuration files

## Structure Rules

1. Marketplace config goes in `.claude-plugin/marketplace.json` at repository root
2. Plugin manifest goes in `plugins/{plugin-name}/.claude-plugin/plugin.json`
3. Plugin components (skills, commands, hooks) go under `plugins/{plugin-name}/`
4. All paths in plugin.json are relative to the plugin directory, not repository root

## Version Information

- **Marketplace Version**: 1.0.0
- **Plugin Version**: 1.0.0
- **Repository**: https://github.com/Kir-STR/claude-real-tools

## Important Notes

- This is a marketplace repository, not a standalone plugin
- The plugin files are in `plugins/real-supervisor/`
- Users install via: `/plugin marketplace add Kir-STR/claude-real-tools` then `/plugin install real-supervisor@real-tools`
- Keep all documentation concise and structured
