---
description: Orchestrates complex, multi-step projects through structured, resumable workflow
---

The Real Supervisor implements a supervisor-worker pattern to orchestrate complex projects from requirements to final deliverables.

## Usage

```
/supervisor <prd_file_path>
```

Or use the shorter alias:
```
/rs <prd_file_path>
```

## Arguments

- `prd_file_path` (required): Path to Product Requirements Document (markdown file)

## Workflow

14-step workflow across 10 phases:
1. Initialization - State checking
2. Explore - PRD analysis
3. Plan - Execution planning
4. Critique & Approval - User approval (HIL gate)
5. Execute Specification - Spec generation
6. Checkpoint Initial - First rollback point
7. Execute Draft - Draft creation
8. Review Draft - User feedback (HIL gate)
9. Checkpoint Draft - Second rollback point
10. Execute Final - Final deliverable

## State Management

All session state stored in `.supervisor/`:
- `state.json` - Current workflow state
- `session_log.json` - Complete audit trail
- `output/` - Generated artifacts

## Resumption

Re-run the command to resume interrupted sessions. The supervisor detects existing state and prompts for resumption.
