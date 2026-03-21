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

## SKILLS (loaded on demand, not always in context)

### skills/design-patterns.md
Source: https://refactoring.guru/design-patterns/catalog
Must include: all 22 GoF patterns in 3 tables (Creational, Structural, Behavioral)
Each row: Pattern | Use when... | Real example | URL
Plus: quick decision tree (text format matching diagrams/decision-tree.mermaid)

### skills/refactoring-techniques.md
Source: https://refactoring.guru/refactoring/smells
Must include: smell → signal → cure → URL for each of the 5 smell categories

### skills/code-quality.md
10 universal rules + pre-delivery checklist (checkbox format)

### skills/brainstorm.md, plan.md, code-review.md, run-tests.md, deploy.md
These are SLASH COMMAND SKILLS — they define the output format for their respective commands.
Each has frontmatter (name, description) and a structured output template.

### skills/aitmpl-catalog.md
Reference to https://www.aitmpl.com/ with categories (Skills, Agents, Commands, MCPs, Settings, Hooks)
and install command: `npx claude-code-templates@latest`

### skills/prompt-engineering.md
Source: platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
Principles: be clear, use XML tags, give examples, specify format, role + context + task
Anti-patterns table: vague vs specific prompts with token savings

### skills/token-optimization.md
Source: support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work
7 rules: never repeat, diagrams > text, short prompts, update STATE always, one task per prompt,
new conversation at 70%, explicit output format
