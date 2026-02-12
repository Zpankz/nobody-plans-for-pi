# nobody-plans-for-pi

Self-improving development workflows for the [pi coding agent](https://github.com/mariozechner/pi-coding-agent). Subagent orchestration, structured planning, TDD enforcement, and systematic debugging.

> **Adapted from [obra/superpowers](https://github.com/obra/superpowers)** by Jesse Vincent — the original structured development workflow system for coding agents. This package reimplements the superpowers methodology as a native pi package, using the pi SDK's subagent architecture and skill/prompt system. Full credit to Jesse for the ideas, methodology, and skill designs that this project builds on.

## What This Does

When installed, your pi agent gains:

- **Subagent orchestration** — Delegate tasks to specialized agents (scout, planner, reviewer, worker) running in isolated context windows
- **Workflow prompts** — `/implement`, `/scout-and-plan`, `/implement-and-review`, `/review`, `/scout` as chainable pipelines
- **Development skills** — Brainstorming, TDD, systematic debugging, code review, verification, planning — loaded automatically when relevant
- **Self-improvement loop** — The agent uses these tools to develop *any* codebase it works on, not just this one

## Install

```bash
pi install https://github.com/HashWarlock/nobody-plans-for-pi
```

That's it. Pi discovers all extensions, skills, and prompts automatically.

### Project-level install (shared with team)

```bash
pi install -l https://github.com/HashWarlock/nobody-plans-for-pi
```

## What's Inside

### Subagent Extension

The `subagent` tool lets the agent spawn isolated pi processes for focused work:

| Mode | Description |
|------|-------------|
| **Single** | `{ agent: "scout", task: "find auth code" }` — one agent, one task |
| **Parallel** | `{ tasks: [...] }` — up to 8 agents concurrently |
| **Chain** | `{ chain: [...] }` — sequential pipeline with `{previous}` output passing |

### Agents

| Agent | Model | Tools | Purpose |
|-------|-------|-------|---------|
| `scout` | Haiku | read, grep, find, ls, bash | Fast codebase recon, compressed context handoff |
| `planner` | Sonnet | read, grep, find, ls | Implementation plans from context + requirements |
| `reviewer` | Sonnet | read, grep, find, ls, bash | Code review for quality and security |
| `worker` | Sonnet | all | General-purpose implementation |

### Workflow Prompts

| Command | Flow | Use When |
|---------|------|----------|
| `/implement <task>` | scout → planner → worker | Building a feature end-to-end |
| `/scout-and-plan <task>` | scout → planner | Planning without implementing |
| `/implement-and-review <task>` | worker → reviewer → worker | Implement with quality gate |
| `/review <scope>` | reviewer | Reviewing recent changes |
| `/scout <query>` | scout | Quick codebase exploration |

### Skills

| Skill | Trigger |
|-------|---------|
| `brainstorming` | Before any creative/design work |
| `writing-plans` | When you have specs, before coding |
| `executing-plans` | When executing a written plan |
| `test-driven-development` | Before writing any implementation code |
| `systematic-debugging` | Any bug, test failure, or unexpected behavior |
| `verification-before-completion` | Before claiming work is done |
| `requesting-code-review` | Before merging or declaring complete |
| `receiving-code-review` | When processing review feedback |
| `using-git-worktrees` | Starting feature work needing isolation |
| `finishing-a-development-branch` | When tasks are complete, deciding how to integrate |
| `writing-skills` | Creating or editing skills |
| `using-nobody-plans` | Every conversation (establishes skill discipline) |

## Usage

### Automatic

Skills activate when the agent recognizes a matching task. The `using-nobody-plans` skill establishes discipline to check for relevant skills before acting.

### Explicit

```
/skill:brainstorming help me design a caching layer
/skill:systematic-debugging tests are failing intermittently
/skill:test-driven-development

/implement add Redis session caching
/scout-and-plan refactor the auth module
/review recent changes to the API layer
```

### Custom Agents

Add your own agents to `~/.pi/agent/agents/` (user-level) or `.pi/agents/` (project-level):

```markdown
---
name: my-agent
description: What this agent does
tools: read, grep, find, ls
model: claude-sonnet-4-5
---

System prompt for the agent.
```

## Update

```bash
pi update
```

## Uninstall

```bash
pi remove https://github.com/HashWarlock/nobody-plans-for-pi
```

## Philosophy

Adapted from [superpowers](https://github.com/obra/superpowers):

- **Test-Driven Development** — Write tests first, always
- **Systematic over ad-hoc** — Process over guessing
- **Complexity reduction** — Simplicity as primary goal
- **Evidence over claims** — Verify before declaring success
- **Delegated execution** — Isolated subagents with focused context produce better results than one overloaded context window

## Acknowledgments

This project is a pi-native adaptation of [obra/superpowers](https://github.com/obra/superpowers) by [Jesse Vincent](https://github.com/obra). The skills, methodology, workflow patterns, and development philosophy are directly derived from Jesse's work. The subagent extension is adapted from the [pi-coding-agent examples](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent/examples/extensions/subagent) by [Mario Zechner](https://github.com/badlogic).

If superpowers has helped you, consider [sponsoring Jesse's work](https://github.com/sponsors/obra).

## License

MIT
