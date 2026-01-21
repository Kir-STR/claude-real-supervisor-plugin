# Real Supervisor - Project Orchestration Command

The Real Supervisor orchestrates complex, multi-step projects through a structured, resumable workflow with specialized worker agents.

## Usage

```
/supervisor path/to/prd.md
```

Or use the shorter alias:
```
/rs path/to/prd.md
```

## Description

This command activates the Real Supervisor skill, which implements a sophisticated supervisor pattern to:

- Decompose complex tasks into manageable phases
- Delegate work to specialized agents (Analyst, Designer, Implementer, Writer, Tester, Critique)
- Manage complete lifecycle from requirements analysis to final delivery
- Maintain resumable state across sessions
- Create checkpoints for rollback capability
- Provide human-in-the-loop approval gates at critical decision points

## Workflow Phases

The supervisor follows a 14-step workflow across 10 phases:

1. **Initialization** - State checking and session setup
2. **Explore** - PRD analysis using Explore agent
3. **Plan** - Execution planning and schema definition
4. **Critique & Approval** - Plan review and user approval (HIL gate)
5. **Execute Specification** - Structured spec generation
6. **Checkpoint Initial** - First rollback point
7. **Execute Draft** - Draft deliverable creation
8. **Review Draft** - User feedback collection (HIL gate)
9. **Checkpoint Draft** - Second rollback point
10. **Execute Final** - Final deliverable with feedback
11. **Completion** - Session archival and delivery

## Arguments

- **prd_file_path** (required): Path to a Product Requirements Document (markdown file) containing:
  - Project overview
  - Objectives
  - Functional requirements
  - Non-functional requirements (optional)
  - Deliverable description

## State Management

All session state is stored in `.supervisor/`:
- `state.json` - Current workflow state
- `session_log.json` - Complete audit trail
- `output/` - All generated artifacts

## Resumption

If a session is interrupted, simply re-run the same command:
```
/supervisor path/to/prd.md
```

The supervisor will detect the existing state and ask if you want to resume from where you left off.

## Checkpointing

The supervisor creates two checkpoints:
1. **Initial checkpoint** (before draft creation) - Use `/rewind` to change specification
2. **Draft checkpoint** (before final version) - Use `/rewind` to revise based on different feedback

## Examples

### Example 1: Build a REST API

```
/supervisor api_requirements.md
```

The supervisor will analyze requirements, create implementation plan, generate specification, build draft code, collect your feedback, and deliver final implementation.

### Example 2: Design System Architecture

```
/supervisor architecture_prd.md
```

The supervisor will use the Designer agent to create comprehensive architecture documentation with diagrams.

### Example 3: Resume Interrupted Session

```
/supervisor project_prd.md
```

If `.supervisor/state.json` exists, you'll be asked whether to resume or start fresh.

## PRD Template

See `.claude/skills/real-supervisor/examples/example_prd.md` for a complete template.

Minimal PRD structure:
```markdown
# Project Title

## Overview
Brief description

## Objectives
What you want to achieve

## Functional Requirements
- Requirement 1
- Requirement 2

## Deliverable
What the final output should be
```

## Human-in-the-Loop Gates

The supervisor requires your approval at two critical points:

1. **Plan Approval (Step 7)**: Review and approve the execution plan before work begins
2. **Draft Review (Step 11)**: Provide feedback on the draft before final version

This ensures you maintain control over the approach and quality.

## Output Files

All artifacts are saved to `.supervisor/output/`:
- `exploration_report.md` - PRD analysis
- `plan.md` - Execution plan
- `spec_schema.md` - Specification schema
- `plan_critique.md` - Plan review
- `task_spec.md` - Detailed specification
- `draft_*` - Draft deliverable
- `feedback.md` - Your review feedback
- `final_*` - Final deliverable

## Specialized Agents

The supervisor delegates to these agents based on task type:

- **Analyst**: Requirements analysis, specification generation
- **Designer**: UI/UX design, architecture diagrams
- **Implementer**: Code implementation
- **Writer**: Documentation, technical writing
- **Tester**: Testing and quality assurance
- **Critique**: Critical review and feedback

## Best Practices

1. Write detailed PRDs for better results
2. Review plans carefully at the approval gate
3. Provide specific feedback during draft review
4. Use `/rewind` if you need to go back to a checkpoint
5. Check `.supervisor/session_log.json` for complete audit trail

## Related

- Quick alias: `/rs`
- Example PRDs: `.claude/skills/real-supervisor/examples/`
- Workflow diagram: `.claude/skills/real-supervisor/examples/workflow_diagram.md`
