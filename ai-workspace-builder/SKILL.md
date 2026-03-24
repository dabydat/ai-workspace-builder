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
7. **Domain terminology** — Any project-specific terms Claude should know? (e.g., "workspace" means X, "tenant" means Y). If yes, add a `## Domain Glossary` section to CLAUDE.md.
8. **Business context** — Who are the main users? What are the top 3 things they must do? Is it multi-tenant? What are the main data entities?

**CRITICAL:** If they mention a stack, ask which specific technologies.
"TypeScript project" is too vague. "TypeScript + Fastify + Prisma + PostgreSQL" is actionable.

### Step 1.5: Business Analysis Layer

**Read `references/business-analysis.md`** for the full methodology.

Every project config must include the business analysis layer. Generate in this order:

1. **skills/business-analysis/SKILL.md** — templates for all BA artifacts (Phase 3)
2. **agents/13-systems-analyst.md** — use cases, user stories, process flows, sequence diagrams (Phase 5)
3. **agents/14-dba.md** — ERD, schema, data dictionary, integrity rules (Phase 5)
4. **commands/create-docs.md** — scaffolds the docs/ folder at project start (Phase 7)

These four files are **non-optional**. Every project needs a Systems Analyst and a DBA even if those roles are filled by the same person or by AI.

**Add to `rules/collaboration.md`:** A Pre-Development Phase rule that mandates `/create-docs` → systems-analyst → dba → architect BEFORE any development agent starts work.

**Add to `CLAUDE.md` Agent Roster:**
| Task | Agent | File |
|---|---|---|
| Requirements, use cases, user stories, process flows | Systems Analyst | agents/13-systems-analyst.md |
| ERD, database schema, data dictionary, data integrity | DBA | agents/14-dba.md |

### Step 2: Generate the Configuration

Read `references/structure.md` for the complete file tree and generation order.

Generate files in this EXACT order (dependencies first):

```
Phase 0 — Settings (standalone, no dependencies):
  settings.json (permissions, statusLine, spinnerTips, attribution, autocompact, hooks)
  statusline-command.sh (model, agent name, turns, tokens, cost, git, context %, active file)

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
  skills/business-analysis/SKILL.md   (user-invocable: false — preloaded into systems-analyst + dba)
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
  agents/13-systems-analyst.md    (maxTurns: 15, color: cyan, skills: [business-analysis])
  agents/14-dba.md                (maxTurns: 15, color: blue, skills: [business-analysis])
  agents/15-ceo.md                (maxTurns: 10, color: magenta)
  agents/16-coo.md                (maxTurns: 10)

Phase 6 — Stacks (referenced by dev agents):
  stacks/README.md
  stacks/active.md (set to user's stack)
  stacks/[user-stack].md

Phase 7 — Commands (include model/argument-hint/disable-model-invocation):
  commands/start.md           (model: haiku — lightweight, saves tokens)
  commands/end.md             (model: haiku — lightweight, saves tokens)
  commands/activate.md        (argument-hint: "[agent-name]" — lists all 17 agents)
  commands/implement.md       (disable-model-invocation: true, argument-hint: "[spec-name]" — parallel: architect+backend+frontend → qa)
  commands/create-spec.md     (argument-hint: "[name]" — architect validates first)
  commands/plan-sprint.md     (parallel: product-manager+project-manager → orchestrator)
  commands/review.md          (argument-hint: "[file-path]" — qa-engineer agent)
  commands/review-impact.md   (argument-hint: "[file-path]" — parallel: architect+qa-engineer)
  commands/build-graph.md
  commands/blast-radius.md    (argument-hint: "[target]")
  commands/setup-stack.md     (argument-hint: "[description]")
  commands/create-docs.md     (argument-hint: "[project-name]" — parallel: systems-analyst+dba)

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

### Parallel Agent Execution (MANDATORY in commands)
Commands MUST invoke agents via the Agent tool — not just give text instructions. The pattern:

```markdown
## Parallel agents (launch in a SINGLE response)
**Agent 1 — Architect (background):**
- subagent_type: `architect`
- run_in_background: `true`
- prompt: `"[specific task with file paths]"`

**Agent 2 — Backend Dev (background):**
- subagent_type: `backend-dev`
- run_in_background: `true`
- prompt: `"[specific task with file paths]"`

Wait for both agents to complete, then proceed.
```

Rules:
- Independent tasks → ALWAYS parallel (`run_in_background: true`)
- Dependent tasks → sequential (wait, then invoke next agent)
- Long-running builds/tests → always background
- Integration/review after parallel work → foreground (`run_in_background: false`)
- Each prompt must include exact file paths — agents don't share context

Commands with parallel agents:
- `/implement` → architect + backend-dev + frontend-dev parallel → qa-engineer sequential
- `/plan-sprint` → product-manager + project-manager parallel → orchestrator sequential
- `/review-impact` → architect (blast-radius) + qa-engineer parallel
- `/create-docs` → systems-analyst + dba parallel (after scaffold)
- `/create-spec` → architect sequential (validates before writing spec)

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

### Settings, Hooks & Status Line
`settings.json` provides team-shared configuration:
- Git permissions: pull auto-allowed, push requires confirmation
- GitHub CLI: read operations auto, write operations ask
- Status line: colored bar showing model, agent name, turn count, tokens, cost, git, context %, active file
- Attribution: disabled by default (no Co-Authored-By on commits)
- Spinner tips: 10 project-specific tips during operations
- **Hooks:** `PreToolUse` on Read/Edit/Write writes active file path to `/tmp/.claude-active-file`

**Hook for active file tracking (ALWAYS include in settings.json):**
```json
"hooks": {
  "PreToolUse": [
    {
      "matcher": "Read|Edit|Write",
      "hooks": [
        {
          "type": "command",
          "command": "bash -c 'INPUT=$(cat); FILE=$(echo \"$INPUT\" | jq -r \".file_path // .path // empty\" 2>/dev/null); [ -n \"$FILE\" ] && [ \"$FILE\" != \"null\" ] && printf \"{\\\"file\\\":\\\"%s\\\",\\\"ts\\\":%s}\" \"$FILE\" \"$(date +%s)\" > /tmp/.claude-active-file; exit 0'"
        }
      ]
    }
  ]
}
```

**Status line shows (in order):** `Model [agent] t{turns} │ ↑in ↓out │ $cost │ folder │ branch status │ ctx% │ tool file/path`
The `active file` segment only shows if the file was accessed within the last 30 seconds.
Icon prefix: `r` = Read, `e` = Edit (yellow), `w` = Write (green).

### Sources
- Design patterns: https://refactoring.guru/design-patterns/catalog
- Code smells: https://refactoring.guru/refactoring/smells
- Claude Code Templates: https://www.aitmpl.com/
- Claude Platform Docs: https://platform.claude.com/docs/en/intro
- Graph concept: https://github.com/tirth8205/code-review-graph
- Best practices: https://github.com/shanraisshan/claude-code-best-practice

---

## Checklist Before Delivery

- [ ] settings.json exists with permissions, statusLine, spinnerTips, attribution, autocompact, **and hooks**
- [ ] settings.json hooks: PreToolUse on Read|Edit|Write → writes to /tmp/.claude-active-file
- [ ] statusline-command.sh exists, is `chmod +x`, requires `jq`, shows model/agent/turns/tokens/cost/git/context/active-file
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
- [ ] rules/coding.md includes Conventional Commits format and naming conventions
- [ ] stacks/active.md includes Terminal Commands section with exact flags
- [ ] stacks/active.md includes Naming Overrides section (stack-specific conventions)
- [ ] If domain terminology exists, CLAUDE.md has a Domain Glossary section
- [ ] README.md or GUIDE.md explains setup in under 2 minutes of reading
- [ ] Commands with multi-agent tasks use Agent tool invocations (not just text instructions)
- [ ] Parallel lanes identified: independent tasks use run_in_background=true
- [ ] activate.md has complete and accurate agent list (all 17 agents)
- [ ] All files copied to .claude/ (settings.json, agents/, skills/, commands/, statusline-command.sh)
