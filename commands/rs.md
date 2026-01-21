# Real Supervisor (RS) - Quick Command

This is a shortcut command for the Real Supervisor skill.

When invoked with `/rs <prd_file_path>`, this command activates the Real Supervisor skill to orchestrate a complex, multi-step project based on the provided Product Requirements Document.

## Usage

```
/rs path/to/your_prd.md
```

This is equivalent to invoking `/real-supervisor path/to/your_prd.md` but shorter and more convenient.

## What This Does

The Real Supervisor will:
1. Check for existing session state and offer to resume if found
2. Analyze the PRD using the Explore agent
3. Create an execution plan and specification schema
4. Present the plan for your approval (HIL gate)
5. Generate a detailed specification
6. Create a checkpoint
7. Generate a draft deliverable
8. Present the draft for your review (HIL gate)
9. Create another checkpoint
10. Deliver the final version incorporating your feedback

## Arguments

- **prd_file_path** (required): Path to the Product Requirements Document markdown file

## Examples

Start a new project:
```
/rs ./project-specs/api_requirements.md
```

Resume an interrupted session:
```
/rs ./project-specs/api_requirements.md
```
(The supervisor will detect the existing state and ask if you want to resume)

## Related

- Full skill: `/real-supervisor`
- Documentation: See `.claude/skills/real-supervisor/examples/` for PRD templates and workflow details
