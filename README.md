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

Each agent has a recommended model **role**, not a hardcoded model. Swap freely based on your provider.

| Agent | Default Model | Role | What Matters |
|-------|---------------|------|--------------|
| `scout` | claude-haiku-4-5 | **Fast recon** | Speed, structured output, good at grep/find |
| `planner` | claude-sonnet-4-5 | **Reasoning** | Structured plans, step-by-step logic, TDD awareness |
| `reviewer` | claude-sonnet-4-5 | **Quality gate** | Finding bugs, security issues, edge cases |
| `worker` | claude-sonnet-4-5 | **Implementation** | Code quality, test writing, following plans precisely |

#### Choosing Models

Connect your preferred provider through pi's setup — API key, Codex, Claude Code, Antigravity, or any supported backend. Then pick models that match each agent's **role**:

| Agent | What to optimize for |
|-------|---------------------|
| **scout** | **Speed** — fastest available model; needs to be good at grep/find/read and structured output, not deep reasoning |
| **planner** | **Reasoning** — mid-to-high tier; must produce well-organized step-by-step plans with TDD awareness |
| **reviewer** | **Quality gate** — strongest available reasoning model; this is where you catch bugs, security issues, and edge cases |
| **worker** | **Coding ability** — strong code generation with good tool use; balance quality with cost since this runs most often |

> **Rule of thumb:** Scout should be your fastest model. Reviewer should be your strongest. Worker and planner are your best balance of quality and cost.

#### Changing Agent Models

Edit the `model:` field in the agent definition files. After installing, they live at:

- **Global:** `~/.pi/agent/git/github.com/HashWarlock/nobody-plans-for-pi/agents/*.md`
- **Or override per-project:** copy to `.pi/agents/` and edit

```yaml
# Example: switch scout to GPT-4.1-mini
---
name: scout
model: gpt-4.1-mini
---
```

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
| `nobody-brainstorms` | Before any creative/design work |
| `nobody-writes-plans` | When you have specs, before coding |
| `nobody-executes-plans` | When executing a written plan |
| `nobody-uses-tdd` | Before writing any implementation code |
| `nobody-debugs` | Any bug, test failure, or unexpected behavior |
| `nobody-verifies-before-completion` | Before claiming work is done |
| `nobody-requests-code-review` | Before merging or declaring complete |
| `nobody-receives-code-review` | When processing review feedback |
| `nobody-uses-git-worktrees` | Starting feature work needing isolation |
| `nobody-finishes-a-development-branch` | When tasks are complete, deciding how to integrate |
| `nobody-writes-skills` | Creating or editing skills |
| `using-plans-for-nobody` | Every conversation (establishes skill discipline) |

## Usage

### Automatic

Skills activate when the agent recognizes a matching task. The `using-plans-for-nobody` skill establishes discipline to check for relevant skills before acting.

### Explicit

```
/skill:nobody-brainstorms help me design a caching layer
/skill:nobody-debugs tests are failing intermittently
/skill:nobody-uses-tdd

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
