# Changelog

## v1.3.0 — 2026-03-24

### Parallel Agent Execution + Hooks + Statusline Trace

**Core change:** Commands now invoke agents via the Agent tool with explicit `subagent_type` and `run_in_background` parameters. Previously, commands gave text instructions that Claude read as guidance but never triggered real agent execution. Now they are wired to spawn and coordinate agents in parallel.

**Updated Commands (parallel agent execution):**
- `commands/implement.md` — Launches `architect` + `backend-dev` + `frontend-dev` IN PARALLEL (background). After all complete, invokes `qa-engineer` sequentially for quality gate.
- `commands/plan-sprint.md` — Runs `product-manager` + `project-manager` IN PARALLEL (background), then `orchestrator` integrates both outputs into Gantt + agent swimlanes.
- `commands/review.md` — Invokes `qa-engineer` agent explicitly. Outputs PASS/FAIL table.
- `commands/review-impact.md` — Runs `architect` (blast-radius trace) + `qa-engineer` (code quality) IN PARALLEL.
- `commands/create-docs.md` — Scaffolds `docs/` folder, then runs `systems-analyst` + `dba` IN PARALLEL to complete use cases, ERD, and schema.
- `commands/create-spec.md` — Invokes `architect` to validate pattern + file list before writing spec.
- `commands/activate.md` — Fixed agent list to include all 17 agents with correct filenames.

**New: PreToolUse hook for active file tracking:**
- `settings.json` now includes a `PreToolUse` hook on `Read|Edit|Write`
- Hook writes `{"file": "/path", "tool": "Read", "ts": 1234}` to `/tmp/.claude-active-file`
- Statusline reads this file to show which file Claude is working on in real time

**Upgraded statusline (`statusline-command.sh`):**
- Added: agent name segment (visible when running as subagent, e.g. `[backend-dev]`)
- Added: turn counter (`t5` = 5th turn in session)
- Added: active file segment (last file accessed, fades after 30s, icon `r`/`e`/`w`)
- Improved: cost formatting (4 decimals if < $0.01, 2 decimals otherwise)
- Improved: git status uses compact symbols (`✓` clean, `3~+2` changed+untracked, `10!` many)
- Improved: separator changed to `│` for cleaner visual separation

**Updated skill documentation:**
- `SKILL.md` — New section "Parallel Agent Execution (MANDATORY in commands)" with pattern, rules, and command map
- `SKILL.md` — New section "Settings, Hooks & Status Line" with hook JSON template
- `SKILL.md` — Updated checklist: 3 new items for parallel agents + hooks
- `SKILL.md` — Phase 0: statusline description updated; Phase 5: added agents 15-ceo + 16-coo; Phase 7: removed non-existent `plan.md`
- `references/structure.md` — Fixed: removed `plan.md` from commands (count: 13→12); updated command descriptions with parallel agent info; updated Phase 0 descriptions
- `references/diagrams-and-commands.md` — Command Catalog now documents agent invocations per command; added 3 new Command Design Principles for parallel execution

**Breaking change:** `plan.md` command removed from spec — it never existed as a file. Use `plan-sprint.md` for sprint planning.

---

## v1.2.2 — 2026-03-23

## v1.2.1 — 2026-03-23

### C-Suite Agents

Added the two missing executive roles to complete the business team.

**New Agents:**
- `agents/15-ceo.md` — Vision, strategy, OKRs, product-market fit validation, pivot decisions, investor narrative, go-to-market strategy. Model: opus-4-6. Color: magenta.
- `agents/16-coo.md` — Operations, process design, resource allocation, scaling playbooks, risk management, KPI dashboard, weekly operations review. Model: sonnet-4-6. Color: yellow.

**Updated Files:**
- `references/agents-guide.md` — Behaviors for agents 15+16; added to UNIVERSAL list; updated "create new agent" example numbers
- `references/structure.md` — Agent count updated to 17; agents 15+16 added to file tree
- `README.md` — Agent count updated to 17; agents 15+16 in file tree

**Agent count:** 15 → 17 (5 dev + 12 universal)

---

## v1.2.0 — 2026-03-23

### Business Analysis Layer

Every project now starts with a mandatory analysis phase before any code is written.

**New Agents:**
- `agents/13-systems-analyst.md` — use cases, user stories, BPMN process flows, sequence diagrams. Preloads `business-analysis` skill. Color: cyan.
- `agents/14-dba.md` — ERD, SQL DDL schema, data dictionary, index strategy, multi-tenancy design, data integrity rules. Preloads `business-analysis` skill. Color: blue.

**New Skill:**
- `skills/business-analysis/SKILL.md` — 12-section template library covering: actor identification, use case diagrams, use case specifications, user stories (Given/When/Then), BPMN process flows, sequence diagrams, ERD, data dictionary row format, requirements doc structure, team RACI matrix, docs/ folder structure, quality gate checklist. `user-invocable: false` — preloaded into agents 13 and 14.

**New Command:**
- `commands/create-docs.md` — `/create-docs [project-name]` scaffolds the complete docs/ folder in one command. Asks 5 questions, generates 9 files with Mermaid diagram stubs, outputs the next-step sequence (systems-analyst → dba → architect → product-manager).

**New Reference:**
- `references/business-analysis.md` — Complete guide for the business analysis layer: agent frontmatter, skill content requirements, command behavior, docs/ folder structure, agent preloading, updated generation order, CLAUDE.md additions.

**Updated Files:**
- `SKILL.md` — Added Step 1.5 (business analysis gather), added agents 13+14 and business-analysis skill to generation order, added create-docs to Phase 7
- `references/structure.md` — Updated agent count (13→15), skill count (11→14), command count (12→13), added all new entries to file tree and generation order
- `README.md` — Full project lifecycle workflow section, business analysis layer section, updated counts throughout, changelog section

**docs/ folder structure (generated per project by /create-docs):**
```
docs/
├── 01-requirements/REQUIREMENTS.md
├── 02-analysis/USE_CASES.md + USER_STORIES.md
├── 03-database/ERD.md + SCHEMA.md + DATA_DICTIONARY.md
├── 04-architecture/SYSTEM_CONTEXT.md
├── 05-flows/SEQUENCE_DIAGRAMS.md
└── 06-team/TEAM_ROLES.md
```

**Methodology source:** BABOK v3 adapted for AI-assisted development.

## v1.1.0 — 2026-03-21

### Best Practices Update

Based on comprehensive analysis of [claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) (84 tips, 9 reports, hooks docs, skill/agent implementations).

**Agent Improvements:**
- All 13 agents now have full frontmatter: `maxTurns` (10-25), `color` (terminal visual ID), `memory` (project scope for orchestrator/architect)
- Dev agents (01-05) use `permissionMode: acceptEdits` — auto-accept file edits, reduced friction
- Dev agents preload quality skills: architect→design-patterns, backend/frontend→refactoring-techniques, qa→code-quality, devops→deploy
- Agent frontmatter fields documented (15 fields: name, description, model, tools, permissionMode, maxTurns, color, memory, skills, hooks, mcpServers, background, effort, isolation, disallowedTools)

**Skill Improvements:**
- Skills now use directory format: `skills/<name>/SKILL.md` (auto-discovered by Claude Code)
- Skill frontmatter fields documented (11 fields: name, description, effort, context, agent, user-invocable, allowed-tools, disable-model-invocation, model, argument-hint, hooks)
- `effort: high` on brainstorm and plan skills (deeper thinking)
- `context: fork` on brainstorm skill (isolated subagent context)
- Trigger-oriented descriptions ("Activates when..." not "Run when...")
- Two skill patterns documented: Agent Skills (preloaded) vs Invocable Skills

**Command Improvements:**
- `model: haiku` on lightweight commands (start, end) — saves tokens/cost
- `argument-hint` on commands that accept arguments
- `disable-model-invocation` on manual-only commands (implement, deploy)

**New Files Generated:**
- `settings.json` — Team-shared settings: permissions (git pull auto, push ask; gh read auto, write ask), `statusLine`, `spinnerTipsOverride` (10 project-specific tips), `attribution` (disabled), `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE: 80`
- `statusline-command.sh` — Colored status bar: model (magenta), tokens (cyan), folder (blue), git branch (green/yellow/red by state), context bar (adaptive color by usage %)

**CLAUDE.md Improvements:**
- Rule 11 added: "Verify your work — run tests, typecheck, lint. Verification 2-3x quality."
- `<important if="writing or modifying code">` tag — enforces read-before-write + quality gate
- `<important if="context is above 50% or session is ending">` tag — enforces state saving
- Orchestration Pattern section: Command → Agent → Skill (with auto-invocation resolution)
- Workflow Best Practices section (compact at 50%, subtasks < 50% context, maxTurns)

**Sources Added:**
- https://github.com/shanraisshan/claude-code-best-practice

## v1.0.0 — 2026-03-20

### Initial Release

- **13 agents**: Orchestrator, Architect, Backend Dev, Frontend Dev, QA Engineer, DevOps, Product Manager, Project Manager, UX Designer, Growth Marketer, Content Strategist, Business Analyst, DX Engineer
- **6 rules**: coding.md (SOLID + refactoring.guru), testing.md, design.md, collaboration.md (PLAN→BRAINSTORM→EXECUTE), graph-thinking.md, self-management.md (auto-plan, auto-compact, auto-save)
- **11 skills**: design-patterns, refactoring-techniques, code-quality, brainstorm, code-review, plan, run-tests, deploy, aitmpl-catalog, prompt-engineering, token-optimization
- **8 diagrams**: context-map, agent-routing, workflow, decision-tree, blast-radius, agents-communication, monetization, session-lifecycle
- **12 commands**: /start, /end, /activate, /setup-stack, /create-spec, /implement, /plan, /plan-sprint, /review, /review-impact, /build-graph, /blast-radius
- **5 memory files**: STATE.md, CHANGELOG.md, DECISIONS.md, claude-capabilities.md, token-strategy.md
- **3 stack templates**: typescript-node, react-nextjs, python-fastapi
- **Stack abstraction**: Dev agents read stacks/active.md, zero hardcoded framework code
- **Self-management**: Claude auto-plans, auto-saves, auto-compacts without user prompting
- **Diagram-based code graphs**: /build-graph creates mermaid maps, /blast-radius traces impact — no MCP server needed
