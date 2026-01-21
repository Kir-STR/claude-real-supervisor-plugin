# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

This repository contains the **Real Supervisor (RS) Plugin** - a Claude Code plugin that orchestrates complex, multi-step projects through specialized worker agents following a structured, resumable workflow.

### Project Type
Claude Code plugin with embedded skill (no build process required)

### Primary Language
Markdown (documentation and skill instructions)

## Repository Structure

```
rs-plugin/
├── CLAUDE.md                                    # This file (dev guide)
├── README.md                                    # User-facing documentation (GitHub)
├── LICENSE                                      # MIT License
├── .gitignore                                   # Git ignore rules
├── .claude/                                     # Claude Code plugin configuration
│   ├── settings.json                            # Plugin settings and hooks
│   └── skills/
│       └── real-supervisor/                     # The supervisor skill
│           ├── SKILL.md                         # Core supervisor agent instructions
│           ├── agents.md                        # Custom worker agent definitions
│           └── examples/                        # Example files and templates
│               ├── example_prd.md               # Sample PRD
│               ├── example_state.json           # State structure example
│               ├── example_session_log.json     # Session logging example
│               ├── example_spec_schema.md       # Specification schema template
│               └── workflow_diagram.md          # Visual workflow documentation
├── .claude-plugin/                              # Plugin manifest
│   └── plugin.json                              # Plugin metadata and configuration
└── supervisor-skill/                            # Original skill files (deprecated, for reference)
```

## Architecture Overview

### Design Pattern: Supervisor Pattern
The plugin implements a supervisor-worker architecture where:
- **Supervisor** (SKILL.md): Orchestrates the workflow, manages state, and coordinates agents
- **Workers** (agents.md): Specialized agents that perform specific tasks (analysis, design, implementation, etc.)

### Workflow State Machine
14-step workflow across 10 phases:
1. **Initialization** (Steps 1-2): State checking and session setup
2. **Explore** (Step 3): PRD analysis using Explore agent
3. **Plan** (Steps 4-5): Execution planning and schema definition
4. **Critique & Approval** (Steps 6-7): Plan review and user approval (HIL)
5. **Execute Specification** (Step 8): Structured spec generation
6. **Checkpoint Initial** (Step 9): First rollback point
7. **Execute Draft** (Step 10): Draft deliverable creation
8. **Review Draft** (Step 11): User feedback collection (HIL)
9. **Checkpoint Draft** (Step 12): Second rollback point
10. **Execute Final** (Step 13): Final deliverable with feedback
11. **Completion** (Step 14): Session archival and delivery

### Key Features
- **File-Based Operations**: All inputs/outputs are files for transparency
- **Resumable Workflows**: State persistence in `.supervisor/state.json`
- **Spec-Driven Communication**: Contract-based agent handoffs
- **Human-in-the-Loop**: Two mandatory approval gates (steps 7 & 11)
- **Checkpointing**: Integration with Claude Code's `/rewind` command
- **Session Logging**: Complete audit trail in `.supervisor/session_log.json`

## Key Technologies

- **Claude Code CLI**: The runtime environment for the skill
- **Task Tool**: Used to spawn and manage sub-agents
- **Markdown**: All documentation and skill instructions
- **JSON**: State management and session logging

## Development Workflow

### Installing the Plugin for Development

1. **Clone the repository**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/claude-real-supervisor-plugin.git
   cd claude-real-supervisor-plugin
   ```

2. **Install locally**:
   ```bash
   # Linux/Mac
   ln -s $(pwd) ~/.config/claude-code/plugins/real-supervisor

   # Windows (PowerShell, as Administrator)
   New-Item -ItemType SymbolicLink -Path "$env:APPDATA\claude-code\plugins\real-supervisor" -Target "$(Get-Location)"
   ```

3. **Reload plugins**:
   ```
   /reload-plugins
   ```

4. **Test with example PRD**:
   ```
   rs .claude/skills/real-supervisor/examples/example_prd.md
   ```

### Modifying the Plugin

#### Update Plugin Configuration
Edit `.claude-plugin/plugin.json` to:
- Update plugin metadata
- Modify version number
- Change plugin dependencies

#### Update Core Workflow
Edit `.claude/skills/real-supervisor/SKILL.md` to:
- Modify workflow steps or phases
- Change state management logic
- Adjust HIL approval gate behavior
- Update checkpoint creation logic

#### Add/Modify Worker Agents
Edit `.claude/skills/real-supervisor/agents.md` to:
- Add new specialized agents
- Modify existing agent instructions
- Change agent selection criteria

#### Update Hooks
Edit `.claude/settings.json` to:
- Modify command aliases
- Add new hooks or event handlers
- Adjust plugin behavior

#### Update Documentation
- `README.md`: User-facing documentation for GitHub
- `.claude/skills/real-supervisor/examples/workflow_diagram.md`: Workflow visualization

### Testing Changes

After modifications:
1. Save your changes
2. Run `/reload-plugins` in Claude Code
3. Test with: `rs .claude/skills/real-supervisor/examples/example_prd.md`
4. Verify state files are created in `.supervisor/`
5. Test resumption by interrupting and re-invoking
6. Test hooks by using the `rs` command alias

### Debugging

Check these files when troubleshooting:
- `.supervisor/state.json`: Current workflow state and artifact paths
- `.supervisor/session_log.json`: Complete agent execution history
- `.supervisor/output/`: All generated artifacts

## Important Implementation Notes

### State Management
- State is saved after EVERY step completion
- `state.json` structure includes: `current_phase`, `current_step`, `artifacts`, `checkpoints`, `completed_steps`
- State must be updated atomically to prevent corruption

### Agent Invocation
- Built-in agents: Use Task tool with appropriate `subagent_type` (e.g., "Explore", "Plan")
- Custom agents: Read instructions from `agents.md`, invoke with `subagent_type: "general-purpose"`
- Always update session_log.json after agent completion

### Error Handling
- Never advance to next step if current step fails
- Log failures in session_log.json with `status: "failed"`
- Prompt user for action: retry, modify inputs, or cancel

### HIL Gates (Human-in-the-Loop)
- Step 7: Plan approval - User must approve before execution begins
- Step 11: Draft review - User provides feedback before final version
- Use AskUserQuestion tool for approval prompts

### Checkpointing
- Step 9: Initial checkpoint (before draft creation)
- Step 12: Draft checkpoint (before final execution)
- Notify user when checkpoints are created
- User can `/rewind` to return to checkpoint state

## File Conventions

### PRD Files
- Must be markdown format
- Should include: Overview, Objectives, Functional Requirements, Non-Functional Requirements, Deliverables
- See `.claude/skills/real-supervisor/examples/example_prd.md` for template

### State Files
- `.supervisor/state.json`: JSON format, updated after every step
- `.supervisor/session_log.json`: JSON array of agent execution records
- Both files are critical for session resumption

### Output Artifacts
All generated in `.supervisor/output/`:
- `exploration_report.md`: PRD analysis (Step 3)
- `plan.md`: Execution plan (Step 4)
- `spec_schema.md`: Specification schema (Step 5)
- `plan_critique.md`: Plan review (Step 6)
- `task_spec.md`: Detailed specification (Step 8)
- `draft_*`: Draft deliverable (Step 10)
- `feedback.md`: User feedback (Step 11)
- `final_*`: Final deliverable (Step 13)

## Scope Limitations

The supervisor explicitly AVOIDS:
- Creating extensive documentation beyond specifications
- Performing complex analytics unless explicitly requested in PRD
- Creating database migrations unless specified
- Creating automated test suites unless specified
- Creating deployment pipelines unless specified

Focus is on core task execution from PRD to deliverable.

## Common Modification Patterns

### Adding a New Workflow Step
1. Update the phase/step numbering in SKILL.md
2. Add state management logic for the new step
3. Update session logging
4. Update workflow_diagram.md
5. Test resumption after the new step

### Creating a New Agent Type
1. Add agent definition to agents.md with role, expertise, and instructions
2. Update agent selection guide table
3. Update SKILL.md to reference the new agent where appropriate
4. Test agent invocation and output validation

### Customizing the Specification Schema
1. Create a new schema template in `examples/`
2. Document required sections and validation criteria
3. Update analyst agent instructions to handle new schema
4. Test specification generation and validation

## Version Information

- **Plugin Version**: 1.0.0
- **Claude Code Compatibility**: Requires Claude Code with Task tool support
- **Last Updated**: 2026-01-21

## Additional Resources

- User documentation: `README.md` (root)
- Plugin manifest: `.claude-plugin/plugin.json`
- Skill instructions: `.claude/skills/real-supervisor/SKILL.md`
- Workflow visualization: `.claude/skills/real-supervisor/examples/workflow_diagram.md`
- Example PRD: `.claude/skills/real-supervisor/examples/example_prd.md`

## Contributing Guidelines

When modifying this plugin:
1. Test all workflow steps thoroughly
2. Verify state management and resumption work correctly
3. Ensure session logging captures all agent activity
4. Update documentation to reflect changes
5. Test error handling and recovery scenarios
6. Verify HIL gates function properly
