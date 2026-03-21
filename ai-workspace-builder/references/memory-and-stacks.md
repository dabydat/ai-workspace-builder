# Memory & Stacks Guide

## MEMORY — 5 Files for Persistent State

### memory/STATE.md (THE most important file in the entire config)
```markdown
# STATE — Current Project State
> READ THIS FIRST. Every session starts here. Update at every session end.

## Active Project
- **Name:** [name]
- **Category:** [category]
- **Phase:** [0-5]
- **Last session:** [date]
- **Next action:** [single most important next step]

## Context (3 lines — don't re-explain, just read)
[Line 1: what exists]
[Line 2: what was just done]
[Line 3: what's next]

## Done
- [x] [completed items]

## Next (priority order)
1. [ ] [pending items]

## Workspace Variables
- **Stack:** [from stacks/active.md]
- **Location:** [user's location]
- **Monetization:** [payment methods available]
- **Constraint:** [limitations]
- **Goal:** [what they're trying to achieve]
```

Update template (at bottom of STATE.md):
```
<!-- Update: Phase, date, next action, move done items, 3-line context -->
```

### memory/CHANGELOG.md
```markdown
# CHANGELOG — Session Log
> Claude adds an entry at the END of every session.

## YYYY-MM-DD — Session N
**What done:** [1-2 lines]
**Files changed:** [list]
**Decisions:** [ADRs if any]
**Next:** [single next action]
```

### memory/DECISIONS.md
```markdown
# DECISIONS — Architecture Decision Records
> Check BEFORE making decisions that might contradict past ones.

| # | Decision | Date | Reason | Status |
|---|----------|------|--------|--------|
| 1 | [decision] | [date] | [reason] | Active/Reverted |
```

### memory/claude-capabilities.md
Source: platform.claude.com/docs/en/about-claude/models/overview
Must include:
- Model table: Opus 4.6, Sonnet 4.6, Haiku 4.5 with context window, max output, best for
- Thinking capabilities: adaptive vs extended, when to use each, effort parameter
- Tool use: web search, web fetch, code execution, bash, computer use, text editor
- Structured outputs: JSON schema enforcement via tools
- Prompt caching: ephemeral 5-min cache, 90% cost reduction
- Long context: documents FIRST, question LAST, use XML tags, quote before answering
- Vision: base64 images, 8000×8000px max, place images before text

### memory/token-strategy.md
Source: support.claude.com/en/articles/11647753-how-do-usage-and-length-limits-work
Must include:
- Usage limits (how many conversations) vs length limits (how much per conversation)
- Context window: 200K tokens (Pro/Team), 500K (Enterprise)
- Model cost comparison table: Opus/Sonnet/Haiku input/output per million tokens
- The 70% rule: compact or start new conversation at 70% context
- Do this / Don't do this lists for token efficiency
- Thinking mode strategy table: task type → adaptive/extended → effort level
- Batch API: 50% cheaper for non-urgent work

---

## STACKS — Stack Abstraction

### How it works
Dev agents (01-05) contain NO code examples. They say "Read stacks/active.md".
`stacks/active.md` contains ALL stack-specific patterns, code examples, and tools.
Changing the stack = replacing one file. Agents adapt automatically.

### stacks/active.md format (REQUIRED sections)
```markdown
# Stack: [Name]

## Language & Runtime
[Language, version, runtime, package manager]

## Framework
[Framework name + version, key features]

## Database / ORM
[DB, ORM, migration tool]

## Queue / Background Jobs
[Queue system if applicable]

## Code Patterns
[REAL code examples in the stack's language for:]
- Parallel execution pattern
- Error handling pattern
- Input validation pattern
- Repository/service pattern
- API route pattern

## Testing
[Test framework, runner, example test]

## CI/CD
[CI tool, pipeline stages, deploy target]

## Publishing / Distribution
[How to publish: npm, pip, docker, etc.]
```

### /setup-stack command behavior
When user says `/setup-stack [description]`:
1. Parse the technologies mentioned
2. Generate stacks/active.md with REAL code patterns (not pseudocode)
3. Save as stacks/[stack-name].md for reuse
4. Check if any existing agent can't cover this stack → create specialized agent if needed
5. Check if stack needs unique skills → create if needed
6. Update memory/STATE.md with stack info
7. NEVER touch universal files (CLAUDE.md, rules, non-dev agents, etc.)

### .claudeignore content
Always include:
```
node_modules/, .pnpm-store/, vendor/, __pycache__/, *.pyc, .venv/, venv/
dist/, build/, out/, .next/, .nuxt/, .output/
coverage/, .nyc_output/, *.lcov
.idea/, *.swp, *.swo, .DS_Store, Thumbs.db
*.log, npm-debug.log*, yarn-debug.log*
.env, .env.local, .env.production, .env.*.local
pnpm-lock.yaml, package-lock.json, yarn.lock, poetry.lock
*.min.js, *.min.css, *.map, *.chunk.js
*.png, *.jpg, *.jpeg, *.gif, *.svg, *.ico, *.mp4, *.mp3, *.woff*
*.sqlite, *.db, .code-review-graph/
*.csv, *.parquet
```

### CLAUDE.md format (generated LAST — references everything)
Must include these sections in order:
1. OPERATION PROTOCOL (who Claude is, self-management, session management)
2. Core Behavior Rules (10 rules including auto-plan, diagrams-first, auto-compact)
3. Agent Roster table (all agents with file paths)
4. Stack Abstraction (reference to /setup-stack and stacks/)
5. Model Selection (from memory/claude-capabilities.md)
6. Token Economy (from memory/token-strategy.md)
7. Quality Standards (all 6 rules with file paths)
8. Code Graph section (diagram-based, 3 mermaid files, zero dependencies)
9. Design Patterns Reference (refactoring.guru links)
10. Skills & Commands tables
11. Diagrams list with purpose
12. File Structure tree
