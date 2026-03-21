---
name: ai-workspace-builder
description: "Use this skill whenever the user wants to configure their AI workspace, set up agents, create a config for their AI coding assistant, organize development agents, set up their project for AI-assisted development, define quality rules, create slash commands, or prepare their AI team to work on any project. Triggers include: 'configure my workspace', 'setup my AI team', 'create my config', 'set up agents', 'create my workspace', 'configure workspace', 'prepare my AI for this project', 'create workspace configuration', 'I want my AI to self-manage', 'self-managing workspace', 'configure mi equipo', 'configure mi IA', 'ai-workspace-builder'. Also trigger when the user says things like 'I want automatic planning', 'make my AI remember between sessions', 'reduce my token usage', 'optimize my workflow', 'make my AI work like a team', or 'set up config builder'. Do NOT use for creating code, building projects, or tasks unrelated to the AI workspace configuration."
---

# Claude Config Builder

## Overview

This skill builds a complete, self-managing Claude configuration for ANY project.
The output is a `claude-config/` folder with agents, rules, skills, memory, diagrams,
commands, prompts, specs, and stacks — all configured so Claude auto-plans, auto-saves,
auto-compacts, and never loses context between sessions.

**Before starting, read the reference files relevant to the user's needs:**
- `references/structure.md` — Complete file tree, what each file does, generation order
- `references/agents-guide.md` — How to write each agent type (universal vs dev vs specialized)
- `references/rules-and-skills.md` — Quality standards, self-management, and skill content
- `references/diagrams-and-commands.md` — Mermaid diagrams format + slash command format
- `references/memory-and-stacks.md` — STATE.md, CHANGELOG, DECISIONS, stack abstraction

---

## Quick Decision Framework

| User Request | Action |
|---|---|
| "Configure Claude for my project" | Full build: gather project info → generate all files → deliver |
| "Set up Claude for [stack]" | Full build + run /setup-stack with their technology |
| "Add agents to my Claude config" | Read existing config → add missing agents → update CLAUDE.md |
| "Optimize my Claude tokens" | Add memory/, diagrams/, self-management rule, .claudeignore |
| "Make Claude remember between sessions" | Add memory/STATE.md + CHANGELOG.md + /start + /end commands |
| "I already have a claude-config, improve it" | Audit existing → identify gaps → add what's missing |

---

## Core Process

### Step 1: Gather Context

Before generating anything, understand:

1. **Project type** — What are they building? (web app, CLI, API, mobile, library)
2. **Stack** — What technologies? (language, framework, DB, hosting)
3. **Team size** — Solo dev, small team, or organization?
4. **Monetization** — How will they make money? (SaaS, open source, templates, API)
5. **Constraints** — Region, financial access, specific tools required?
6. **What they already have** — Existing claude-config? Existing CLAUDE.md? Start from scratch?

**CRITICAL:** If they mention a stack, ask which specific technologies.
"TypeScript project" is too vague. "TypeScript + Fastify + Prisma + PostgreSQL" is actionable.

### Step 2: Generate the Configuration

Read `references/structure.md` for the complete file tree and generation order.

Generate files in this EXACT order (dependencies first):

```
Phase 1 — Foundation (no dependencies):
  .claudeignore
  memory/STATE.md
  memory/CHANGELOG.md
  memory/DECISIONS.md
  memory/claude-capabilities.md
  memory/token-strategy.md

Phase 2 — Rules (referenced by agents):
  rules/coding.md
  rules/testing.md
  rules/design.md
  rules/collaboration.md
  rules/graph-thinking.md
  rules/self-management.md

Phase 3 — Skills (referenced by agents and commands):
  skills/design-patterns.md
  skills/refactoring-techniques.md
  skills/code-quality.md
  skills/brainstorm.md
  skills/code-review.md
  skills/plan.md
  skills/run-tests.md
  skills/deploy.md
  skills/aitmpl-catalog.md
  skills/prompt-engineering.md
  skills/token-optimization.md

Phase 4 — Diagrams (referenced by commands and agents):
  diagrams/context-map.mermaid
  diagrams/agent-routing.mermaid
  diagrams/workflow.mermaid
  diagrams/decision-tree.mermaid
  diagrams/blast-radius.mermaid
  diagrams/agents-communication.mermaid
  diagrams/monetization.mermaid
  diagrams/session-lifecycle.mermaid

Phase 5 — Agents (reference rules + skills):
  agents/00-orchestrator.md
  agents/01-architect.md
  agents/02-backend-dev.md (reads stacks/active.md)
  agents/03-frontend-dev.md (reads stacks/active.md)
  agents/04-qa-engineer.md (reads stacks/active.md)
  agents/05-devops.md (reads stacks/active.md)
  agents/06-product-manager.md
  agents/07-project-manager.md
  agents/08-ux-designer.md
  agents/09-growth-marketer.md
  agents/10-content-strategist.md
  agents/11-business-analyst.md
  agents/12-dx-engineer.md

Phase 6 — Stacks (referenced by dev agents):
  stacks/README.md
  stacks/active.md (set to user's stack)
  stacks/[user-stack].md

Phase 7 — Commands (reference everything above):
  commands/start.md
  commands/end.md
  commands/activate.md
  commands/implement.md
  commands/create-spec.md
  commands/plan.md
  commands/plan-sprint.md
  commands/review.md
  commands/review-impact.md
  commands/build-graph.md
  commands/blast-radius.md
  commands/setup-stack.md

Phase 8 — Orchestration (references everything):
  prompts/CATALOG.md
  specs/TEMPLATE.md
  CLAUDE.md (master config — generated LAST because it references all above)
  README.md
```

### Step 3: Customize to Project

After generating the base config, customize:

1. **stacks/active.md** — Fill with the user's actual stack, real code patterns, real framework idioms
2. **memory/STATE.md** — Set project name, phase, stack, constraints, goals
3. **memory/DECISIONS.md** — Record the stack choice and any decisions made during setup
4. **CLAUDE.md** — Ensure the agent roster matches what was generated
5. **diagrams/context-map.mermaid** — Adjust "what to read per task" to match the actual file structure

### Step 4: Deliver

Package as a zip or create files directly in the project.
Show the user:
- Total file count
- Brief description of each category
- How to start using it (`/start`)
- Remind them: `/end` at session close is mandatory for memory persistence

---

## Key Principles

### Agent Abstraction
Dev agents (01-05) are STACK-AGNOSTIC. They read `stacks/active.md` for patterns.
Non-dev agents (00, 06-12) are UNIVERSAL — they never change between projects.
Only create specialized agents if NO existing agent covers the role (e.g., 13-mobile-dev for Flutter).

### Graph-Based Thinking
Claude maintains 3 code graph diagrams (code-graph, code-deps, code-tests).
These are mermaid files Claude reads/writes directly — no MCP server, no external tools.
Commands /build-graph, /blast-radius, /review-impact use these diagrams.

### Self-Management
Claude auto-plans, auto-sweeps, auto-compacts, auto-saves.
This behavior comes from `rules/self-management.md` — it's ALWAYS included.
The user never needs to say "plan first" or "save your state."

### Token Efficiency
- .claudeignore prevents reading irrelevant files
- Diagrams replace text explanations (300 tokens vs 1500)
- memory/STATE.md prevents re-discovering context (500 tokens vs 2000+)
- Commands use file references, not inline content

### Sources
- Design patterns: https://refactoring.guru/design-patterns/catalog
- Code smells: https://refactoring.guru/refactoring/smells
- Claude Code Templates: https://www.aitmpl.com/
- Claude Platform Docs: https://platform.claude.com/docs/en/intro
- Graph concept: https://github.com/tirth8205/code-review-graph

---

## Checklist Before Delivery

- [ ] .claudeignore exists and covers node_modules, build, media, locks
- [ ] memory/STATE.md has project name, stack, phase, goals
- [ ] rules/self-management.md is present (auto-plan, auto-compact, auto-save)
- [ ] stacks/active.md is populated with the user's actual stack and real code patterns
- [ ] CLAUDE.md references all agents, rules, skills, diagrams, commands correctly
- [ ] All commands reference the correct file paths
- [ ] diagrams/context-map.mermaid matches the actual file structure
- [ ] The agent roster in CLAUDE.md matches the agents/ folder
- [ ] prompts/CATALOG.md has templates for all commands
- [ ] README.md or GUIDE.md explains setup in under 2 minutes of reading
