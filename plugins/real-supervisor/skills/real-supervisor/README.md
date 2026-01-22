# Real Supervisor Skill

The Real Supervisor orchestrates complex, multi-step projects through a structured, resumable workflow with specialized worker agents.

## Overview

The supervisor implements a **supervisor-worker pattern** where:
- **Supervisor** manages the workflow, coordinates agents, and handles state
- **Workers** are specialized agents that perform specific tasks

## Workflow

The supervisor executes a **14-step workflow** across **10 phases**:

### Phase 0: Initialization (Steps 1-2)
Checks for existing session state and initializes or resumes the session.

### Phase 1: Explore (Step 3)
Analyzes the PRD using the Explore agent, producing an exploration report.

### Phase 2: Plan (Steps 4-5)
Creates an execution plan and defines a specification schema for structured communication.

### Phase 3: Critique & Approval (Steps 6-7)
Reviews the plan with a Critique agent, then presents it to you for approval (**HIL Gate 1**).

### Phase 4: Execute Specification (Step 8)
The Analyst agent generates a detailed specification conforming to the schema.

### Phase 5: Checkpoint Initial (Step 9)
Creates the first checkpoint - you can `/rewind` to this point later.

### Phase 6: Execute Draft (Step 10)
A specialized worker agent (Designer/Implementer/Writer) creates a draft deliverable.

### Phase 7: Review Draft (Step 11)
Presents the draft to you for review and feedback (**HIL Gate 2**).

### Phase 8: Checkpoint Draft (Step 12)
Creates the second checkpoint before final execution.

### Phase 9: Execute Final (Step 13)
Incorporates your feedback to produce the final deliverable.

### Phase 10: Completion (Step 14)
Archives the session and presents all deliverables.

## Worker Agents

The supervisor delegates to **6 specialized agents**:

1. **Analyst** - Requirements analysis and specification generation
2. **Designer** - UI/UX designs, architecture diagrams, system design
3. **Implementer** - Code implementation and technical development
4. **Writer** - Documentation, technical writing, content creation
5. **Critique** - Critical review and constructive feedback
6. **Tester** - Testing strategies, test cases, quality assurance

The supervisor selects the appropriate agent based on deliverable type.

## Workflow Diagram

```
┌─────────────────────────────────────────┐
│  USER: /rs path/to/prd.md               │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 0: Initialization (Steps 1-2)   │
│  Check state → Resume or Start Fresh    │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 1: Explore (Step 3)              │
│  Explore agent → exploration_report.md  │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 2: Plan (Steps 4-5)              │
│  Create plan.md & spec_schema.md        │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 3: Critique & Approval (6-7)     │
│  Critique → plan_critique.md            │
│  HIL: User approves plan ◄──┐           │
│           └─ Changes? ───────┘           │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 4: Execute Spec (Step 8)         │
│  task_spec.md (follows schema)          │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 5: Checkpoint Initial (Step 9)   │
│  Create checkpoint (can /rewind here)   │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 6: Execute Draft (Step 10)       │
│  Worker agent → draft_[type].[ext]      │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 7: Review Draft (Step 11)        │
│  HIL: User reviews ◄──┐                 │
│       └─ Feedback? → feedback.md        │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 8: Checkpoint Draft (Step 12)    │
│  Create checkpoint (can /rewind here)   │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 9: Execute Final (Step 13)       │
│  Worker agent → final_[type].[ext]      │
│  (incorporates feedback)                │
└──────────────────┬──────────────────────┘
                   ▼
┌─────────────────────────────────────────┐
│  PHASE 10: Completion (Step 14)         │
│  Archive & present deliverables         │
└──────────────────┬──────────────────────┘
                   ▼
               ┌─────────┐
               │ SUCCESS │
               └─────────┘
```

## State Management

All session state is stored in `.supervisor/`:

```
.supervisor/
├── state.json                    # Current workflow state
├── session_log.json              # Complete agent execution log
└── output/
    ├── exploration_report.md     # Step 3
    ├── plan.md                   # Step 4
    ├── spec_schema.md            # Step 5
    ├── plan_critique.md          # Step 6
    ├── task_spec.md              # Step 8
    ├── draft_[type].[ext]        # Step 10
    ├── feedback.md               # Step 11 (if provided)
    └── final_[type].[ext]        # Step 13
```

### state.json Structure

```json
{
  "session_id": "timestamp-based-id",
  "current_phase": "phase_name",
  "current_step": 1,
  "prd_path": "path/to/prd.md",
  "artifacts": {
    "prd": "path/to/prd.md",
    "exploration_report": ".supervisor/output/exploration_report.md",
    "plan": ".supervisor/output/plan.md",
    "spec_schema": ".supervisor/output/spec_schema.md",
    "task_spec": ".supervisor/output/task_spec.md",
    "draft": ".supervisor/output/draft_*.ext",
    "feedback": ".supervisor/output/feedback.md",
    "final": ".supervisor/output/final_*.ext"
  },
  "checkpoints": {
    "initial": 9,
    "draft": 12
  },
  "completed_steps": [1, 2, 3, ...]
}
```

## Resumption

If a session is interrupted, simply re-run the command:

```bash
/rs path/to/prd.md
```

The supervisor:
1. Detects existing `state.json`
2. Reads the last completed step
3. Asks if you want to resume or start fresh
4. If resuming, continues from the next step

## Human-in-the-Loop (HIL) Gates

Two mandatory approval points:

### HIL Gate 1: Plan Approval (Step 7)
Review and approve the execution plan before work begins.

**Options:**
- **Approve** → Continue to specification
- **Request Changes** → Provide feedback, plan is revised, re-presented
- **Cancel** → Abort the task

### HIL Gate 2: Draft Review (Step 11)
Review the draft deliverable and provide feedback.

**Options:**
- **Approve** → Continue to final version (feedback: "approved")
- **Provide Feedback** → Save feedback, incorporated in final version
- **Reject** → Return to draft creation with rejection reasons

## Checkpoints

Two checkpoints enable safe rollback via `/rewind`:

1. **Initial Checkpoint (Step 9)** - Before draft creation
   - Use to change the specification or approach

2. **Draft Checkpoint (Step 12)** - Before final execution
   - Use to revise based on different feedback

## PRD Requirements

Your PRD should include:

```markdown
# Project Title

## Overview
Brief project description

## Objectives
What you want to achieve

## Functional Requirements
- Requirement 1
- Requirement 2
- ...

## Non-Functional Requirements (Optional)
- Performance requirements
- Security requirements
- ...

## Deliverable
What the final output should be (code, design, documentation, etc.)
```

See `examples/example_prd.md` for a complete template.

## Usage Examples

### Example 1: Build a REST API

```bash
/rs api_requirements.md
```

The supervisor will:
1. Analyze API requirements
2. Create implementation plan
3. Get your approval
4. Generate detailed specification
5. Create checkpoint
6. Implement draft API code
7. Present draft for review
8. Incorporate your feedback
9. Deliver final API implementation

### Example 2: Design System Architecture

```bash
/supervisor architecture_prd.md
```

The supervisor selects the **Designer** agent to create comprehensive architecture documentation with diagrams.

### Example 3: Resume Interrupted Session

```bash
/rs project_prd.md
```

If `.supervisor/state.json` exists, you'll be asked whether to resume or start fresh.

## Error Handling

If any step fails:
1. Error is logged in `session_log.json` with `status: "failed"`
2. Workflow does NOT advance to next step
3. You are notified of the failure
4. Options: Retry step, Modify inputs, or Cancel

## Best Practices

1. **Write detailed PRDs** - Better requirements lead to better results
2. **Review plans carefully** at HIL Gate 1
3. **Provide specific feedback** during draft review
4. **Use `/rewind`** if you need to go back to a checkpoint
5. **Check `session_log.json`** for complete audit trail

## Components

The Real Supervisor skill consists of:

- **SKILL.md** - Core supervisor instructions for Claude Code
- **examples.md** - Usage examples, common patterns, and structure examples
- **README.md** - This file (skill documentation for users)

Worker agents are defined in `../../agents/` directory (6 specialized agents).

## Architecture

The supervisor implements these principles:

1. **File-Based Operations** - All inputs/outputs are files for transparency
2. **Supervisor Pattern** - Decompose, delegate, aggregate
3. **Spec-Driven Communication** - Structured schemas for agent handoffs
4. **State Persistence** - Resume after interruptions
5. **Human-in-the-Loop** - User approval at critical decision points
