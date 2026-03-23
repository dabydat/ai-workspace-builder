# Rules & Skills Guide — What Each File Must Contain

## RULES (non-negotiable, always present, all projects)

### rules/coding.md
Source: https://refactoring.guru/design-patterns/catalog + /refactoring/smells
Must include:
- SOLID principles table (S/O/L/I/D with violation example + fix)
- Thresholds: functions 30 lines, files 200 lines, params 3 max, nesting 3 levels, complexity 10
- Code smells table: Bloaters, OO Abusers, Change Preventers, Dispensables, Couplers
  Each smell → refactoring technique to fix it
- Design patterns: Creational (Factory, Builder, Singleton), Structural (Adapter, Facade, Decorator, Proxy),
  Behavioral (Strategy, Observer, Command, State, Chain of Responsibility, Template Method)
  Each pattern → when to use + 1-line example
- Conventional Commits: type(scope): description — feat, fix, docs, style, refactor, test, chore, perf
- Naming conventions table: camelCase (vars/functions), PascalCase (classes/types), UPPER_SNAKE_CASE (constants), kebab-case (files/dirs)
  Note: stack-specific overrides (e.g., snake_case for Python) go in stacks/active.md
- Pre-merge checklist: no magic values, no nesting, no duplicates, no dead code, no `any`, functions ≤30 lines

### rules/testing.md
Must include:
- Test pyramid: Unit 60-70%, Integration 20-30%, E2E 5-10%
- What to test at each level (with code example format)
- Security tests: webhook signatures, idempotency, SQL injection, XSS, auth, rate limiting
- Coverage requirements: business logic 90%, handlers 80%, overall 80%
- Launch gate checklist: zero failures, coverage above threshold, no test.skip

### rules/design.md
Must include:
- Typography scale (12px-48px), spacing system (4px base), color system with semantic meanings
- Accessibility: WCAG AA contrast ratios, touch targets 44×44px, keyboard navigation, alt text
- Component states: default, hover, focus, active, disabled, loading, error, success
- Landing page structure: nav → hero → how it works → benefits → social proof → pricing → FAQ → CTA → footer
- Form UX: max 5 fields, inline validation, progress indicator, never disable submit silently

### rules/collaboration.md
Must include:
- PLAN → BRAINSTORM → EXECUTE protocol with triggers (>1 file, architectural change, >2 hours)
- Plan format: Current State → Desired State → Approach → Files → Agents → Dependencies → Risks
- Brainstorm format: 3+ perspectives from different agents → trade-off matrix → recommendation
- Conflict resolution: both write positions → orchestrator reviews → data-driven decision → document
- Standup: DONE | DOING | BLOCKED (async, 5 min)
- **Parallel worktrees:** `claude --worktree feature/branch-name` for isolated parallel work
  - Each worktree is a separate physical directory under `.claude/worktrees/<branch>`
  - Same git history, independent working tree — agents cannot interfere with each other
  - When to use: 2+ features with overlapping files, long-running tasks, risky refactors
  - Agents with `isolation: worktree` in frontmatter auto-run in worktrees
  - Merge through normal git workflows (PR per branch)

### rules/graph-thinking.md
Must include:
- Node types: FILE, CLASS, FUNCTION, TYPE, TEST (with properties)
- Edge types: CALLS, IMPORTS_FROM, INHERITS, IMPLEMENTS, CONTAINS, TESTED_BY, DEPENDS_ON
- 3 code graph diagrams Claude maintains: code-graph.mermaid, code-deps.mermaid, code-tests.mermaid
- When to use each: implementing (before + after), reviewing (instead of reading all files), planning
- Token math: 3 diagrams ~1K tokens vs scanning 200 files ~150K tokens

### rules/self-management.md (CRITICAL — this is what makes Claude auto-manage)
Must include 7 rules:
1. Auto-plan: RECEIVE → READ STATE → CLASSIFY trivial/non-trivial → plan if non-trivial → execute
2. Context sweep: silent checklist before each task (STATE read? stack known? graphs exist?)
3. Auto-compact: when to compact (after major task, before unrelated task, when repeating)
4. Never lose context: mandatory save at session end (STATE + CHANGELOG + DECISIONS + graphs)
5. Token-efficient reading: diagrams → signatures → targeted → full file (in that priority order)
6. Self-diagnosis table: signal → action (repeating = compact, lost = read DECISIONS, etc.)
7. Conversation start: read STATE.md silently, never say "let me check my memory"

---

## SKILLS (directory format: `skills/<name>/SKILL.md`)

Skills use directory format for auto-discovery. Each skill lives in `skills/<name>/SKILL.md`.
Supporting files (examples, templates) go in the same directory as reference material.

### Skill Frontmatter Fields (11 available)
```yaml
---
name: skill-name                    # Required: kebab-case identifier
description: "Activates when..."    # Required: trigger-oriented description
effort: high                        # Optional: low|medium|high — thinking depth
context: fork                       # Optional: "fork" runs in isolated subagent context
agent: general-purpose              # Optional: which agent type runs this skill
user-invocable: false               # Optional: false = preloaded into agents, not user-callable
allowed-tools: Read, Grep, Glob     # Optional: restrict which tools the skill can use
disable-model-invocation: true      # Optional: prevents Claude from auto-invoking this skill
model: haiku                        # Optional: model override
argument-hint: "[file-path]"        # Optional: hint for arguments
hooks: {}                           # Optional: skill-specific hooks
---
```

**Description best practice:** Write as trigger ("Activates when..."), not imperative ("Run when...").
This helps Claude's auto-invocation resolution: Skill (inline) > Agent (separate context) > Command (never auto-invoked).

### Two Skill Patterns
1. **Agent Skills** (`user-invocable: false`) — Preloaded via `skills:` field in agent frontmatter. Full SKILL.md content injected at agent startup. Used for reference knowledge (design-patterns, code-quality, refactoring-techniques).
2. **Invocable Skills** — Called via Skill tool or `/slash-command`. Used for actions (brainstorm, code-review, deploy).

### skills/design-patterns/SKILL.md
Source: https://refactoring.guru/design-patterns/catalog
`user-invocable: false` — preloaded into architect agent
Must include: all 22 GoF patterns in 3 tables (Creational, Structural, Behavioral)
Each row: Pattern | Use when... | Real example | URL
Plus: quick decision tree (text format matching diagrams/decision-tree.mermaid)

### skills/refactoring-techniques/SKILL.md
Source: https://refactoring.guru/refactoring/smells
`user-invocable: false` — preloaded into backend-dev and frontend-dev agents
Must include: smell → signal → cure → URL for each of the 5 smell categories

### skills/code-quality/SKILL.md
`user-invocable: false` — preloaded into qa-engineer agent
10 universal rules + pre-delivery checklist (checkbox format)

### skills/brainstorm/SKILL.md (`effort: high`, `context: fork`, `agent: general-purpose`)
### skills/plan/SKILL.md (`effort: high`)
### skills/code-review/SKILL.md (`allowed-tools: Read, Grep, Glob`)
### skills/run-tests/SKILL.md (`allowed-tools: Bash(npm test *), Bash(npx vitest *), Read`)
### skills/deploy/SKILL.md (`disable-model-invocation: true`, `argument-hint: [staging|production]`)
These are SLASH COMMAND SKILLS — they define the output format for their respective commands.
Each has frontmatter (name, description, + fields above) and a structured output template.

### skills/aitmpl-catalog/SKILL.md
`user-invocable: false`
Reference to https://www.aitmpl.com/ with categories (Skills, Agents, Commands, MCPs, Settings, Hooks)
and install command: `npx claude-code-templates@latest`

### skills/prompt-engineering/SKILL.md
`user-invocable: false`
Source: platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
Principles: be clear, use XML tags, give examples, specify format, role + context + task
Anti-patterns table: vague vs specific prompts with token savings

### skills/token-optimization/SKILL.md
`user-invocable: false`
Source: support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work
7 rules: never repeat, diagrams > text, short prompts, update STATE always, one task per prompt,
new conversation at 70%, explicit output format

### skills/business-analysis/SKILL.md
`user-invocable: false` — preloaded into agents 13 (systems-analyst) and 14 (dba)
Source: BABOK v3 adapted for AI-assisted development
Must include 12 sections (template library for all BA artifacts):
1. Actor identification table (Primary / Secondary / AI Agent / External System)
2. Use case diagram template (Mermaid `graph LR`)
3. Use case specification template (9-field table: ID, Name, Actor, Preconditions, Trigger, Main Flow, Alternative Flow, Exception Flow, Postconditions, Business Rules)
4. User story format (As a / I want / So that + Given/When/Then acceptance criteria)
5. BPMN-style process flow template (Mermaid `flowchart TD` with swimlanes)
6. Sequence diagram template (Mermaid `sequenceDiagram` with actor/participant/alt/loop)
7. ERD template (Mermaid `erDiagram` with cardinality notation)
8. Data dictionary row standard (field | type | nullable | description | business rules)
9. Requirements document structure (FR, NFR, Constraints, Out of Scope, Assumptions)
10. Team RACI matrix template (Role | Artifact | R/A/C/I)
11. docs/ folder structure (01-requirements through 06-team)
12. Quality gate checklist (10 checks before handoff to dev team)
