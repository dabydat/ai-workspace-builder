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
Phase 0 — Settings (standalone, no dependencies):
  settings.json (permissions, statusLine, spinnerTips, attribution, autocompact)
  statusline-command.sh (colored status bar: model, tokens, git, context %)

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

Phase 3 — Skills (directory format: skills/<name>/SKILL.md):
  skills/design-patterns/SKILL.md      (user-invocable: false — preloaded into architect)
  skills/refactoring-techniques/SKILL.md (user-invocable: false — preloaded into devs)
  skills/code-quality/SKILL.md         (user-invocable: false — preloaded into qa-engineer)
  skills/brainstorm/SKILL.md           (effort: high, context: fork, agent: general-purpose)
  skills/code-review/SKILL.md          (allowed-tools: Read, Grep, Glob)
  skills/plan/SKILL.md                 (effort: high)
  skills/run-tests/SKILL.md            (allowed-tools: Bash(npm test *), Read)
  skills/deploy/SKILL.md               (disable-model-invocation: true)
  skills/aitmpl-catalog/SKILL.md       (user-invocable: false)
  skills/prompt-engineering/SKILL.md   (user-invocable: false)
  skills/token-optimization/SKILL.md   (user-invocable: false)

Phase 4 — Diagrams (referenced by commands and agents):
  diagrams/context-map.mermaid
  diagrams/agent-routing.mermaid
  diagrams/workflow.mermaid
  diagrams/decision-tree.mermaid
  diagrams/blast-radius.mermaid
  diagrams/agents-communication.mermaid
  diagrams/monetization.mermaid
  diagrams/session-lifecycle.mermaid

Phase 5 — Agents (reference rules + skills, include full frontmatter):
  agents/00-orchestrator.md   (maxTurns: 25, color: magenta, memory: project)
  agents/01-architect.md      (maxTurns: 15, color: blue, memory: project, permissionMode: acceptEdits, skills: [design-patterns])
  agents/02-backend-dev.md    (maxTurns: 15, color: green, permissionMode: acceptEdits, skills: [refactoring-techniques])
  agents/03-frontend-dev.md   (maxTurns: 15, color: yellow, permissionMode: acceptEdits, skills: [refactoring-techniques])
  agents/04-qa-engineer.md    (maxTurns: 15, color: red, permissionMode: acceptEdits, skills: [code-quality])
  agents/05-devops.md         (maxTurns: 15, color: white, permissionMode: acceptEdits, skills: [deploy])
  agents/06-product-manager.md (maxTurns: 10, color: cyan)
  agents/07-project-manager.md (maxTurns: 10)
  agents/08-ux-designer.md     (maxTurns: 10)
  agents/09-growth-marketer.md (maxTurns: 10)
  agents/10-content-strategist.md (maxTurns: 10)
  agents/11-business-analyst.md   (maxTurns: 10)
  agents/12-dx-engineer.md        (maxTurns: 10, color: cyan)

Phase 6 — Stacks (referenced by dev agents):
  stacks/README.md
  stacks/active.md (set to user's stack)
  stacks/[user-stack].md

Phase 7 — Commands (include model/argument-hint/disable-model-invocation):
  commands/start.md           (model: haiku — lightweight, saves tokens)
  commands/end.md             (model: haiku — lightweight, saves tokens)
  commands/activate.md        (argument-hint: "[agent-name]")
  commands/implement.md       (disable-model-invocation: true, argument-hint: "[spec-name]")
  commands/create-spec.md     (argument-hint: "[name]")
  commands/plan.md
  commands/plan-sprint.md
  commands/review.md          (argument-hint: "[file-path]")
  commands/review-impact.md   (argument-hint: "[file-path]")
  commands/build-graph.md
  commands/blast-radius.md    (argument-hint: "[target]")
  commands/setup-stack.md     (argument-hint: "[description]")

Phase 8 — Orchestration (references everything):
  prompts/CATALOG.md
  specs/TEMPLATE.md
  CLAUDE.md (master config — generated LAST, includes <important> tags and orchestration pattern)
  README.md
```

### Step 3: Customize to Project

After generating the base config, customize:

1. **stacks/active.md** — Fill with the user's actual stack, real code patterns, real framework idioms
2. **memory/STATE.md** — Set project name, phase, stack, constraints, goals
3. **memory/DECISIONS.md** — Record the stack choice and any decisions made during setup
4. **CLAUDE.md** — Ensure the agent roster matches what was generated. Include:
   - 11 core behavior rules (rule 11: "Verify your work")
   - `<important if="writing or modifying code">` tag for read-before-write + quality gate
   - `<important if="context is above 50% or session is ending">` tag for state saving
   - Orchestration Pattern section: Command → Agent → Skill
   - Workflow Best Practices section
5. **diagrams/context-map.mermaid** — Adjust "what to read per task" to match the actual file structure
6. **settings.json** — Adjust permissions for the user's stack tools (e.g., Bash(python *), Bash(cargo *))

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
Dev agents use `permissionMode: acceptEdits` to auto-accept file changes (reduced friction).

### Orchestration Pattern: Command → Agent → Skill
- **Commands** are entry points (user interaction via /slash-commands)
- **Agents** run autonomously with preloaded skills (domain knowledge injected at startup)
- **Skills** with `user-invocable: false` are preloaded into agents via `skills:` frontmatter
- **Skills** with `context: fork` run in isolated subagent context
- Auto-invocation resolution: Skill (inline) > Agent (separate context) > Command (never auto-invoked)

### Graph-Based Thinking
Claude maintains 3 code graph diagrams (code-graph, code-deps, code-tests).
These are mermaid files Claude reads/writes directly — no MCP server, no external tools.
Commands /build-graph, /blast-radius, /review-impact use these diagrams.

### Self-Management
Claude auto-plans, auto-sweeps, auto-compacts, auto-saves.
This behavior comes from `rules/self-management.md` — it's ALWAYS included.
The user never needs to say "plan first" or "save your state."

### Verification (2-3x quality improvement)
Rule 11: "Verify your work — run tests, typecheck, lint, or read the output."
CLAUDE.md uses `<important>` tags to enforce critical rules that Claude must never skip.

### Token Efficiency
- .claudeignore prevents reading irrelevant files
- Diagrams replace text explanations (300 tokens vs 1500)
- memory/STATE.md prevents re-discovering context (500 tokens vs 2000+)
- Commands use file references, not inline content
- `model: haiku` on lightweight commands (start, end) saves tokens/cost
- `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE: "80"` compacts earlier (default ~95%)
- Spinner tips override replaces generic tips with project-specific ones

### Settings & Status Line
`settings.json` provides team-shared configuration:
- Git permissions: pull auto-allowed, push requires confirmation
- GitHub CLI: read operations auto, write operations ask
- Status line: colored bar showing model, tokens, git, context %
- Attribution: disabled by default (no Co-Authored-By on commits)
- Spinner tips: 10 project-specific tips during operations

### Sources
- Design patterns: https://refactoring.guru/design-patterns/catalog
- Code smells: https://refactoring.guru/refactoring/smells
- Claude Code Templates: https://www.aitmpl.com/
- Claude Platform Docs: https://platform.claude.com/docs/en/intro
- Graph concept: https://github.com/tirth8205/code-review-graph
- Best practices: https://github.com/shanraisshan/claude-code-best-practice

---

## Checklist Before Delivery

- [ ] settings.json exists with permissions, statusLine, spinnerTips, attribution, autocompact
- [ ] statusline-command.sh exists with colored segments (model, tokens, git, context)
- [ ] .claudeignore exists and covers node_modules, build, media, locks
- [ ] memory/STATE.md has project name, stack, phase, goals
- [ ] rules/self-management.md is present (auto-plan, auto-compact, auto-save)
- [ ] stacks/active.md is populated with the user's actual stack and real code patterns
- [ ] Skills use directory format: skills/<name>/SKILL.md
- [ ] All agents have full frontmatter: maxTurns, color, and skills (for dev agents: permissionMode)
- [ ] Dev agent skills preloaded: architect→design-patterns, devs→refactoring-techniques, qa→code-quality, devops→deploy
- [ ] CLAUDE.md has 11 rules (including rule 11: verify your work)
- [ ] CLAUDE.md has `<important>` tags for critical rules (read-before-write, save state)
- [ ] CLAUDE.md has Orchestration Pattern section (Command → Agent → Skill)
- [ ] CLAUDE.md references all agents, rules, skills, diagrams, commands correctly
- [ ] Lightweight commands (start, end) use `model: haiku`
- [ ] All commands reference the correct file paths (skills/<name>/SKILL.md format)
- [ ] diagrams/context-map.mermaid matches the actual file structure
- [ ] The agent roster in CLAUDE.md matches the agents/ folder
- [ ] prompts/CATALOG.md has templates for all commands
- [ ] README.md or GUIDE.md explains setup in under 2 minutes of reading
- [ ] All files copied to .claude/ (settings.json, agents/, skills/, commands/, statusline-command.sh)
