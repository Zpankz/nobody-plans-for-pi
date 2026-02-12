---
name: nobody-executes-plans
description: Use when you have a written implementation plan to execute with review checkpoints
---

# Executing Plans

## Overview

Execute implementation plans in batches with human checkpoints. Each batch is a logical group of tasks that can be reviewed together.

**Announce at start:** "I'm using the nobody-executes-plans skill to work through the plan."

## Process

1. **Load plan** — Read the plan file, confirm scope with user
2. **Execute in batches** — Group 2–5 related tasks per batch
3. **Checkpoint after each batch** — Show what was done, run tests, ask to continue
4. **Track progress** — Update plan file with completion marks

## Batch Execution

For each batch:

```
### Batch N: [Description]

Tasks: [list from plan]

1. Execute each task following plan exactly
2. Run tests after each task
3. Commit after each task passes

Checkpoint:
- Tasks completed: [list]
- Tests: [pass/fail count]
- Files changed: [list]
- Continue? [y/n]
```

## Rules

- **Follow the plan exactly** — don't improve, optimize, or "enhance"
- **TDD always** — write test, watch fail, implement, watch pass
- **Stop on failure** — if tests fail, report and wait for guidance
- **Commit frequently** — after each passing task
- **Never skip checkpoints** — user must approve each batch

## Progress Tracking

Update the plan file as you go:

```markdown
- [x] Task 1: Setup database schema ✅
- [x] Task 2: Add migration script ✅
- [ ] Task 3: Implement CRUD endpoints
- [ ] Task 4: Add validation
```

## When Things Go Wrong

| Problem | Action |
|---------|--------|
| Plan step is unclear | Ask user, don't guess |
| Tests fail unexpectedly | Report full output, wait |
| Plan needs modification | Propose change, get approval |
| Scope creep temptation | Stick to plan, note ideas for later |
