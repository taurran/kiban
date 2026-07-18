# Tool Routing Guide

When to use which workflow -- written in tool-agnostic terms so any AI
tool can interpret the intent. Claude Code is the primary reference
implementation, but the workflows apply to any capable AI tool.

## GSD (Get Shit Done)

Use for: structured project planning, multi-phase execution.

**Trigger:** Start a new project planning workflow, plan a phase, or execute a phase.
When: any new project, any phase requiring planning, resuming work sessions.

In Claude Code, this maps to /gsd-new-project, /gsd-plan-phase, /gsd-execute-phase.
Other tools: follow the GSD workflow manually or via equivalent planning features.

## Thinking and Problem-Solving Workflows

Use for: brainstorming, debugging, code review, writing plans.

Key workflows:

- **Brainstorming** -- use BEFORE any implementation to explore options
- **Systematic debugging** -- structured investigation for bug hunting
- **Writing plans** -- after brainstorm, before execution
- **Verification before completion** -- sanity check before closing tasks

In Claude Code, these map to superpowers:brainstorming, superpowers:systematic-debugging, etc.
Other tools: apply the workflow pattern regardless of tool-specific invocation.

## Agent Dispatch

Use parallel agents when: 3+ independent tasks, no shared state between them.
Use sequential agents when: tasks have dependencies or share files.

In Claude Code, this uses the dispatching-parallel-agents pattern.
Other tools: use built-in parallel execution or run tasks in separate sessions.

## Security Scanning

Always run after generating new first-party code in supported languages.
Workflow: scan code -> review findings -> fix issues -> rescan -> repeat until clean.

Primary tool: Snyk (for supported languages).
Secondary tool: Bandit (for Python/FastAPI projects specifically).
See configs/universal/security.md for the full scanning policy.

## Knowledge Vault Access (Local Tools)

Method: Direct file access (read, search, glob) -- NOT network-based.
Applies to: any AI tool running on the local machine.

Vault locations:

- **Personal:** ~/vaults/personal/ (personal tools only)
- **AI/Dev:** ~/vaults/ai-dev/ (all local tools)
- **Work:** ~/vaults/work/ (work tools only)

## Knowledge Vault Access (Remote/Cloud Tools)

Method: MCP server or equivalent remote file access.
Applies to: web-based AI tools, cloud-hosted assistants, mobile apps.
