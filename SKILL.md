---
name: agent-loops
displayName: Agent Loops | OpenClaw Skill
description: Multi-agent workflow orchestrator. Use when the user asks to ship a feature, fix a bug, review code, research a topic, refactor code, or publish a skill. Routes each step to the right agent (PM, Dev, Tester, Reviewer, Editor, Researcher, Architect) and chains their outputs.
version: 2.0.0
---

# Agent Loops

Prebuilt multi-agent workflows that chain sequential and parallel steps, with real output passing between agents.

## Description

Agent Loops orchestrates multi-step agent pipelines. Each workflow defines a sequence of steps; each step runs via `claude -p` with a role-specific system prompt and agent-swarm model routing. Outputs chain between steps so each agent builds on the previous one's work.

Use when you need to:
- Ship a feature end-to-end (scope → implement → document)
- Diagnose and fix a bug with verification
- Run parallel code review and security audit
- Research a topic and produce a polished report
- Plan and execute a refactoring safely
- Test, review, and publish a skill to ClawHub

## Installation

```bash
clawhub install agent-loops
```

Or clone into your skills directory:

```bash
git clone https://github.com/OpenClaw/agent-loops.git workspace/skills/agent-loops
```

Requires PyYAML for YAML workflows:

```bash
pip install pyyaml
```

## Usage

**Workflow selection — match the user's intent to a workflow:**

| User says something like... | Workflow | Command |
|------------------------------|----------|---------|
| "ship this feature", "build X", "add Y" | `ship_feature` | `python3 workspace/skills/agent-loops/scripts/run_workflow.py ship_feature "<input>" --apply` |
| "fix this bug", "debug X", "why is Y broken" | `bug_fix` | `python3 workspace/skills/agent-loops/scripts/run_workflow.py bug_fix "<input>" --apply` |
| "review this code", "audit X", "check for bugs" | `code_review` | `python3 workspace/skills/agent-loops/scripts/run_workflow.py code_review "<input>" --apply` |
| "research X", "compare A vs B", "write a report on" | `research_report` | `python3 workspace/skills/agent-loops/scripts/run_workflow.py research_report "<input>" --apply` |
| "refactor X", "clean up Y", "restructure Z" | `refactor` | `python3 workspace/skills/agent-loops/scripts/run_workflow.py refactor "<input>" --apply` |
| "publish this skill", "push to ClawHub" | `skill_publish` | `python3 workspace/skills/agent-loops/scripts/run_workflow.py skill_publish "<input>" --apply` |

Pass the user's natural language request as `<input>`. The workflow handles scoping, delegation, and output chaining automatically.

## Examples

**Example 1: Ship a feature**
*Scenario:* You want to scope, implement, and document a new feature.
*Action:* `python3 workspace/skills/agent-loops/scripts/run_workflow.py ship_feature "Add dark mode toggle to settings" --apply`
*Outcome:* PM scopes tasks, Dev implements with tests, Editor writes docs and changelog.

**Example 2: Fix a bug**
*Scenario:* A bug needs diagnosis, a fix, and regression tests.
*Action:* `python3 workspace/skills/agent-loops/scripts/run_workflow.py bug_fix "Login page crashes on empty password" --apply`
*Outcome:* Dev diagnoses root cause, Dev implements fix, Tester writes regression test, Editor documents the change.

**Example 3: Parallel code review**
*Scenario:* You want a code review and security audit run simultaneously.
*Action:* `python3 workspace/skills/agent-loops/scripts/run_workflow.py code_review "Review the auth module" --apply`
*Outcome:* Reviewer and Security auditor run in parallel, then Editor synthesizes a unified summary.

**Example 4: Override model**
*Scenario:* You want all steps to use a specific model.
*Action:* `python3 workspace/skills/agent-loops/scripts/run_workflow.py research_report "Compare REST vs GraphQL" --apply --model sonnet`
*Outcome:* All steps run with the specified model instead of agent-swarm routing.

## Commands

```bash
python3 workspace/skills/agent-loops/scripts/run_workflow.py <workflow> "<input>"              # Dry-run a workflow
python3 workspace/skills/agent-loops/scripts/run_workflow.py <workflow> "<input>" --apply       # Run agents for real
python3 workspace/skills/agent-loops/scripts/run_workflow.py --list                             # List available workflows
python3 workspace/skills/agent-loops/scripts/run_workflow.py --list --json                      # List workflows as JSON
python3 workspace/skills/agent-loops/scripts/run_workflow.py <workflow> "<input>" --apply --json # Output results as JSON
python3 workspace/skills/agent-loops/scripts/run_workflow.py <workflow> "<input>" --model sonnet # Override model for all steps
python3 workspace/skills/agent-loops/scripts/run_workflow.py <workflow> "<input>" --timeout 900  # Set per-step timeout (seconds)
python3 workspace/skills/agent-loops/scripts/run_workflow.py <workflow> "<input>" -v             # Verbose output
```

- **`<workflow>`** — Workflow id: `ship_feature`, `bug_fix`, `code_review`, `research_report`, `refactor`, `skill_publish`
- **`--apply`** — Actually spawn agents (default is dry-run)
- **`--list`** — List all available workflows
- **`--json`** — Output results as JSON for programmatic use
- **`--model`** — Override agent-swarm routing with a specific model
- **`--timeout`** — Per-step timeout in seconds (default: 600)
- **`-v`** — Show full task text per step
