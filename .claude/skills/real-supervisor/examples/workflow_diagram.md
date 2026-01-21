# Supervisor Skill Workflow Diagram

This document visualizes the complete workflow of the Real Supervisor Skill.

---

## High-Level Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                    USER INVOKES SUPERVISOR                       │
│                   supervisor path/to/prd.md                      │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                  PHASE 0: INITIALIZATION                         │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 1-2: Check for existing state.json             │       │
│  │           ├─ If found → Offer to resume              │       │
│  │           └─ If not → Initialize new session         │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                   PHASE 1: EXPLORE                               │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 3: Invoke Explore agent                        │       │
│  │          Read PRD, analyze requirements              │       │
│  │          Output: exploration_report.md               │       │
│  │          Save state.json, update session_log.json    │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                   PHASE 2: PLAN                                  │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 4: Create execution plan (CoT reasoning)       │       │
│  │          Output: plan.md                             │       │
│  │                                                       │       │
│  │  Step 5: Define specification schema                 │       │
│  │          Output: spec_schema.md                      │       │
│  │          Save state after each step                  │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│              PHASE 3: CRITIQUE & APPROVAL                        │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 6: Invoke Critique agent                       │       │
│  │          Review plan and schema                      │       │
│  │          Output: plan_critique.md                    │       │
│  │          Refine plan based on critique               │       │
│  │                                                       │       │
│  │  Step 7: Present plan to user (HIL) ◄──┐            │       │
│  │          └─ Approved? ──────────────────┘            │       │
│  │          └─ Changes? → Revise → Re-present           │       │
│  │          Save state                                  │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│            PHASE 4: EXECUTE SPECIFICATION                        │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 8: Invoke Analyst agent                        │       │
│  │          Input: PRD, exploration report, plan        │       │
│  │          Must conform to spec_schema.md              │       │
│  │          Output: task_spec.md                        │       │
│  │          Validate against schema                     │       │
│  │          Save state                                  │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│            PHASE 5: CHECKPOINT INITIAL                           │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 9: Create initial checkpoint                   │       │
│  │          Notify user about checkpoint                │       │
│  │          User can /rewind to this point later        │       │
│  │          Update state: checkpoints.initial = 9       │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│              PHASE 6: EXECUTE DRAFT                              │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 10: Invoke appropriate worker agent            │       │
│  │           - Designer (for design deliverables)       │       │
│  │           - Implementer (for code)                   │       │
│  │           - Writer (for documentation)               │       │
│  │           Input: task_spec.md                        │       │
│  │           Output: draft_[type].[ext]                 │       │
│  │           Save state                                 │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│              PHASE 7: REVIEW DRAFT                               │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 11: Present draft to user (HIL) ◄──┐          │       │
│  │           └─ Approved? ────────────────────┘         │       │
│  │           └─ Feedback? → Collect → feedback.md       │       │
│  │           └─ Reject? → Return to Step 10             │       │
│  │           Save state                                 │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│            PHASE 8: CHECKPOINT DRAFT                             │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 12: Create draft checkpoint                    │       │
│  │           Notify user about checkpoint               │       │
│  │           Update state: checkpoints.draft = 12       │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│              PHASE 9: EXECUTE FINAL                              │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 13: Invoke same agent type as Step 10          │       │
│  │           Input: draft + feedback.md + task_spec.md  │       │
│  │           Incorporate ALL user feedback              │       │
│  │           Output: final_[type].[ext]                 │       │
│  │           Save state                                 │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                  PHASE 10: COMPLETION                            │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  Step 14: Archive session_log.json                   │       │
│  │           Present all deliverables to user           │       │
│  │           - Final deliverable path highlighted       │       │
│  │           - List all artifacts in state              │       │
│  │           Mark state as completed                    │       │
│  │           Save final state                           │       │
│  └──────────────────────────────────────────────────────┘       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
                      ┌─────────────┐
                      │   SUCCESS   │
                      └─────────────┘
```

---

## State Persistence Points

State is saved after every step completion:

```
Step 1-2  → state.json created/loaded
Step 3    → state.json updated (exploration_report path added)
Step 4    → state.json updated (plan path added)
Step 5    → state.json updated (spec_schema path added)
Step 6    → state.json updated (critique path added)
Step 7    → state.json updated (approval recorded)
Step 8    → state.json updated (task_spec path added)
Step 9    → state.json updated (checkpoint.initial = 9)
Step 10   → state.json updated (draft path added)
Step 11   → state.json updated (feedback path added if applicable)
Step 12   → state.json updated (checkpoint.draft = 12)
Step 13   → state.json updated (final path added)
Step 14   → state.json updated (status = "completed")
```

---

## Agent Invocation Flow

```
┌──────────────┐
│  Supervisor  │ (SKILL.md instructions)
└──────┬───────┘
       │
       ├─► Step 3:  Explore agent (built-in)
       │            └─► exploration_report.md
       │
       ├─► Step 6:  Critique agent (general-purpose)
       │            └─► plan_critique.md
       │
       ├─► Step 8:  Analyst agent (custom from agents.md)
       │            └─► task_spec.md (conforms to spec_schema)
       │
       ├─► Step 10: Designer / Implementer / Writer agent
       │            (selected based on deliverable type)
       │            └─► draft_[type].[ext]
       │
       └─► Step 13: Same agent as Step 10
                    └─► final_[type].[ext]
```

---

## Resumption Scenarios

### Scenario 1: Interruption during Step 10
```
1. User runs: supervisor path/to/prd.md
2. Supervisor reads state.json → completed_steps = [1,2,3,4,5,6,7,8,9]
3. Supervisor detects last completed step = 9
4. Supervisor asks: "Resume from Step 10 (execute_draft)?"
5. User confirms
6. Supervisor loads all artifact paths from state.json
7. Supervisor continues from Step 10
```

### Scenario 2: Interruption after Draft Feedback
```
1. User runs: supervisor path/to/prd.md
2. Supervisor reads state.json → completed_steps = [1-11]
3. Supervisor detects checkpoints.draft is not set (Step 12 not complete)
4. Supervisor asks: "Resume from Step 12 (checkpoint_draft)?"
5. User confirms
6. Supervisor continues to create draft checkpoint and then execute final
```

---

## Human-in-the-Loop (HIL) Gates

Two mandatory approval points:

### HIL Gate 1: Plan Approval (Step 7)
```
Supervisor presents:
├─ Execution Plan (plan.md)
├─ Specification Schema (spec_schema.md)
└─ Critique Summary (plan_critique.md)

User options:
├─ Approve → Continue to Step 8
├─ Request Changes → Revise plan → Re-present Step 7
└─ Cancel → Abort task
```

### HIL Gate 2: Draft Review (Step 11)
```
Supervisor presents:
└─ Draft Deliverable (draft_[type].[ext])

User options:
├─ Approve → feedback = "approved" → Continue to Step 12
├─ Provide Feedback → Save to feedback.md → Continue to Step 12
└─ Reject → Return to Step 10 with rejection reasons
```

---

## Error Handling Flow

```
┌─────────────┐
│  Any Step   │
└──────┬──────┘
       │
       ▼
   ┌────────┐
   │ Error? │
   └───┬────┘
       │ Yes
       ▼
┌──────────────────┐
│ Log to session_  │
│ log with status  │
│ = "failed"       │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│ Do NOT advance   │
│ to next step     │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│ Notify user of   │
│ failure          │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│ Ask user:        │
│ ├─ Retry step    │
│ ├─ Modify inputs │
│ └─ Cancel        │
└──────────────────┘
```

---

## Files Generated During Workflow

```
.supervisor/
├── state.json                    (Created at Step 2, updated after every step)
├── session_log.json              (Created at Step 2, updated after agent invocations)
└── output/
    ├── exploration_report.md     (Step 3)
    ├── plan.md                   (Step 4)
    ├── spec_schema.md            (Step 5)
    ├── plan_critique.md          (Step 6)
    ├── task_spec.md              (Step 8)
    ├── draft_[type].[ext]        (Step 10)
    ├── feedback.md               (Step 11, if user provides feedback)
    └── final_[type].[ext]        (Step 13)
```

---

This workflow ensures robust, resumable task execution with clear checkpoints and user approval gates.
