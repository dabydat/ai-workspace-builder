# Changelog

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
