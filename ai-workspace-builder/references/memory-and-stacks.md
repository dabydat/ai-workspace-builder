# Memory & Stacks Guide

## MEMORY — Two-Tier Architecture

**Rule:** `claude-config/memory/` is for workspace config only. Project data lives in `[project]/memory/`.

```
claude-config/memory/
├── STATE.md          ← Workspace state: active project pointer + project list
├── DECISIONS.md      ← Workspace-level ADRs (apply across all projects)
├── CHANGELOG.md      ← Config changelog (versions, not project sessions)
├── claude-capabilities.md
└── token-strategy.md

[project]/memory/     ← One per project. Created at project start.
├── STATE.md          ← Project phase, last session, next action
├── DECISIONS.md      ← Project-specific ADRs
└── CHANGELOG.md      ← Project session log
```

**Session protocol:**
```
/start → read claude-config/memory/STATE.md → find active project → read [project]/memory/STATE.md
/end   → write to [project]/memory/STATE.md + [project]/memory/CHANGELOG.md only
```

---

## 5 Config Files in claude-config/memory/

### memory/STATE.md (workspace pointer — read first, then follow to project)
```markdown
# WORKSPACE STATE
> READ THIS FIRST. Then read the active project's memory.

## Workspace
- **Category:** [category]
- **Stack default:** [stack]
- **Location:** [location]
- **Monetization:** [payment methods]

## Active Project
- **Name:** [project-name]
- **Folder:** [project-folder]/
- **Read next:** [project-folder]/memory/STATE.md

## All Projects
| Project | Folder | Status |
|---------|--------|--------|
| [name] | [folder]/ | [Active/Done] |
```

### [project]/memory/STATE.md (project-specific state)
```markdown
# [Project Name] — Project State
> Read after claude-config/memory/STATE.md. Update at every session end.

## Status
- **Phase:** [current phase]
- **Last session:** [date]
- **Next action:** [single most important next step]

## Context
- Stack: [tech stack]
- [2-3 key facts about the project]

## Done
- [x] [completed items]

## Next (priority order)
1. [ ] [pending items]
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

## Terminal Commands
[Exact commands with flags for common dev tasks:]
```bash
# Dev
[command to start dev server / watch mode]

# Build
[command to build for production]

# Test
[command to run tests, with common flags]

# Lint / Format
[command to lint and format]

# Deploy
[command to deploy]
```

## Testing
[Test framework, runner, example test]

## CI/CD
[CI tool, pipeline stages, deploy target]

## Publishing / Distribution
[How to publish: npm, pip, docker, etc.]

## Naming Overrides
[Stack-specific naming conventions that override rules/coding.md defaults]
[e.g., Python: snake_case for functions/variables, Go: short variable names]
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

### settings.json format (generated in Phase 0 — copy to .claude/settings.json)
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "statusLine": {
    "type": "command",
    "command": "bash .claude/statusline-command.sh"
  },
  "enableAllProjectMcpServers": true,
  "respectGitignore": true,
  "includeGitInstructions": true,
  "env": {
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "60"
  },
  "permissions": {
    "allow": [
      "Edit(*)", "Write(*)",
      "Bash(npm run *)", "Bash(npm test *)", "Bash(npx vitest *)", "Bash(npx tsup *)",
      "Bash(git status)", "Bash(git log *)", "Bash(git diff *)", "Bash(git branch *)",
      "Bash(git pull *)",
      "Bash(gh pr view *)", "Bash(gh pr list *)", "Bash(gh pr checks *)",
      "Bash(gh issue view *)", "Bash(gh issue list *)",
      "Bash(ls *)"
    ],
    "ask": [
      "Bash(git push *)", "Bash(git commit *)",
      "Bash(gh pr create *)", "Bash(gh issue create *)", "Bash(gh api *)",
      "Bash(npm publish *)", "Bash(rm *)", "Bash(rmdir *)"
    ],
    "read": {
      "deny": [
        "**/.env*",
        "**/*.pem",
        "**/*.key",
        "**/secrets/**",
        "**/credentials/**",
        "**/.aws/**",
        "**/.ssh/**"
      ]
    },
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)"
    ]
  },
  "attribution": { "commit": "", "pr": "" },
  "spinnerTipsOverride": [
    "Tip: Use /start and /end to save 500+ tokens per session",
    "Tip: /brainstorm before deciding — multiple perspectives beat one",
    "Tip: /blast-radius [file] shows what a change will impact",
    "Tip: Diagrams save 5x tokens vs text explanations",
    "Tip: /compact at ~50% context — don't wait for overflow",
    "Tip: Read stacks/active.md for stack-specific patterns",
    "Tip: specs/ before code — no spec = no implementation",
    "Tip: Dev agents preload quality skills automatically",
    "Tip: /review [file] audits code against coding.md standards",
    "Tip: Verify your work — tests + typecheck 2-3x quality"
  ]
}
```

Key settings:
- `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE: "60"` — Compact earlier (default ~95%)
- `git pull *` auto-allowed (safe), `git push *` requires confirmation (risky)
- `gh` read operations auto-allowed, write operations require confirmation
- `attribution` disabled — no Co-Authored-By on commits or PR footer
- `spinnerTipsOverride` — project-specific tips replace generic spinner text

### statusline-command.sh (generated in Phase 0)
Bash script that shows a colored status bar. **Requires:** `jq` installed, script must be `chmod +x`.

```
Opus | in:15.2k out:4.5k | $0.05 | my-project | main (clean) | ██░░░░░░░░ 18% of 200k
```

**Segments and JSON source fields:**
| Segment | Color | JSON field |
|---------|-------|------------|
| Model | magenta bold | `model.display_name` or `model.id` |
| Token I/O | cyan | `context_window.total_input_tokens` / `total_output_tokens` |
| Cost | dim | `cost.total_cost_usd` |
| Folder | blue bold | `workspace.current_dir` or `cwd` |
| Git branch | green/yellow/red (by state) | live `git status` on `cwd` |
| Context bar | adaptive (green < 50%, yellow < 75%, red 75%+) | `context_window.used_percentage` / `context_window_size` |

**Script requirements:**
- Add `export PATH="$HOME/bin:$PATH"` at top (ensures `jq` is found)
- Handle `null` values gracefully (fields are null before first API call)
- Use `printf "%b"` for ANSI color output
- Must be marked executable: `chmod +x .claude/statusline-command.sh`

### CLAUDE.md format (generated LAST — references everything)
Must include these sections in order:
1. OPERATION PROTOCOL (who Claude is, self-management, session management)
2. Core Behavior Rules (11 rules including auto-plan, diagrams-first, auto-compact, **verify your work**)
3. `<important>` conditional tags for critical rules:
   - `<important if="writing or modifying code">` — read before edit, run tests, quality gate
   - `<important if="context is above 50% or session is ending">` — save state before compact/end
4. Agent Roster table (all agents with file paths)
5. Stack Abstraction (reference to /setup-stack and stacks/)
6. Model Selection (from memory/claude-capabilities.md)
7. Token Economy (from memory/token-strategy.md)
8. Quality Standards (all 6 rules with file paths)
9. Code Graph section (diagram-based, 3 mermaid files, zero dependencies)
10. Design Patterns Reference (refactoring.guru links)
11. **Orchestration Pattern: Command → Agent → Skill**
    - Commands are entry points (user interaction)
    - Agents fetch/process using preloaded skills (domain knowledge)
    - Skills with `user-invocable: false` are preloaded via `skills:` frontmatter
    - Skills with `context: fork` run in isolated subagent context
12. **Workflow Best Practices**
    - Manual `/compact` at ~50% context
    - Break subtasks < 50% context
    - Start with plan mode for complex multi-file tasks
    - Agents have `maxTurns` to prevent runaway execution
    - Dev agents preload quality skills; business agents stay lightweight
    - **Parallel worktrees:** `claude --worktree feature/branch` for isolated parallel sessions
13. Skills & Commands tables
14. Settings reference (settings.json → copy to .claude/settings.json)
15. Diagrams list with purpose
16. File Structure tree
