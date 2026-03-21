<p align="center">
  <h1 align="center">AI Workspace Builder for Claude</h1>
  <p align="center">
    <a href="https://buymeacoffee.com/daby"><img src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-support-yellow?style=for-the-badge&logo=buy-me-a-coffee&logoColor=white" alt="Buy Me a Coffee"></a>
    <img src="https://img.shields.io/badge/Claude_AI-Skill-blueviolet?style=for-the-badge&logo=anthropic&logoColor=white" alt="Claude AI Skill">
    <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="MIT License">
  </p>
</p

> A Claude Code skill that generates a complete, self-managing AI workspace configuration for any project.

---

## What It Does

You say: **"Configure my workspace for TypeScript + Next.js + Supabase"**

The skill generates **68+ files** that turn Claude into a self-managing development team:

- **13 agents** — With full frontmatter: maxTurns, color, memory, skills preloading, permissionMode
- **6 rules** — Coding (SOLID + refactoring.guru), Testing, Design, Collaboration, Graph Thinking, Self-Management
- **11 skills** — Directory format (`skills/<name>/SKILL.md`), with effort/context/agent fields
- **8 diagrams** — Mermaid graphs that replace text explanations (5× token savings)
- **12 slash commands** — With model selection (haiku for lightweight), argument hints
- **5 memory files** — Persistent state across sessions (Claude never forgets where it left off)
- **Stack templates** — TypeScript, React, Python (or generate any stack with /setup-stack)
- **settings.json** — Permissions (git/gh auto-allow), spinner tips, attribution, autocompact
- **statusline-command.sh** — Colored status bar: model, tokens, git info, context usage %

## Key Features

### Self-Managing Claude
Claude auto-plans before executing, auto-saves state between sessions, auto-compacts context before overflow, and auto-diagnoses when it loses track. The user only needs to say `/start` and `/end`.

### Orchestration: Command → Agent → Skill
Commands are user entry points. Agents run autonomously with preloaded skills (domain knowledge injected at startup). Dev agents auto-accept edits (`permissionMode: acceptEdits`), have execution limits (`maxTurns`), and preload quality skills.

### Stack Abstraction
Dev agents contain zero framework-specific code. They read `stacks/active.md` for patterns. Change your entire stack by running `/setup-stack [new tech]` — all agents adapt automatically.

### Diagram-Based Code Graphs
Inspired by [code-review-graph](https://github.com/tirth8205/code-review-graph). Claude maintains your codebase as 3 mermaid diagrams (structure, dependencies, test coverage). No MCP server, no Python, no external tools. Claude reads a 300-token diagram instead of scanning 200 files.

### Colored Status Line
Real-time status bar showing model, token I/O, project folder, git branch/status, and context usage with a progress bar. Colors match the agent roster (magenta, blue, green, yellow, red, cyan).

### Smart Permissions & Git Integration
`settings.json` auto-allows safe operations (git pull, gh read) and prompts for risky ones (git push, gh create). Attribution disabled by default. Spinner shows project-specific tips.

### Token Efficiency
- `.claudeignore` blocks irrelevant files from context
- Diagrams replace text explanations (300 tokens vs 1500)
- `memory/STATE.md` prevents re-discovering context each session
- `model: haiku` on lightweight commands saves tokens/cost
- `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE: 80` compacts earlier
- Prompts catalog with copy-paste templates optimized for minimal tokens

---

## Install

### Option 1: Skill file (recommended)
```bash
# Download ai-workspace-builder.skill from Releases
claude skill install ai-workspace-builder.skill
```

### Option 2: Manual
```bash
git clone https://github.com/YOUR_USERNAME/ai-workspace-builder.git
mkdir -p ~/.claude/skills/ai-workspace-builder
cp -r ai-workspace-builder/ai-workspace-builder/* ~/.claude/skills/ai-workspace-builder/
```

### Option 3: Direct copy
```bash
mkdir -p ~/.claude/skills/ai-workspace-builder
cp -r ai-workspace-builder/ ~/.claude/skills/
```

---

## Usage

Open any project in Claude Code:

```bash
cd ~/my-project
claude
```

Then say any of these:

```
"Configure my workspace for TypeScript + Fastify + Prisma + PostgreSQL"
"Set up my AI team"
"Create my config"
"Prepare my AI for this project"
"I want automatic planning and self-management"
```

The skill activates, asks about your project, and generates the full configuration.

### After Setup

```
/start                          # Begin session (Claude reads its memory)
/setup-stack React + Next.js    # Configure for a specific stack
/create-spec user-auth          # Create a feature specification
/implement user-auth            # Implement from the spec
/build-graph                    # Map your codebase as diagrams
/blast-radius src/auth.ts       # See what a change would impact
/review-impact src/auth.ts      # Code review with blast radius
/end                            # Save state for next session
```

---

## What Gets Generated

```
your-project/
└── claude-config/
    ├── CLAUDE.md              ← Master brain (auto-loaded)
    ├── .claudeignore          ← Files Claude skips
    ├── agents/ (13)           ← Stack-agnostic specialists
    ├── rules/ (6)             ← Quality standards + self-management
    ├── skills/ (11)           ← How-to guides (refactoring.guru, aitmpl.com)
    ├── stacks/                ← Stack-specific patterns
    │   └── active.md          ← Current stack (agents read this)
    ├── memory/ (5)            ← Persistent state across sessions
    ├── diagrams/ (8+3)        ← Visual context + code graphs
    ├── commands/ (12)         ← Slash commands
    ├── prompts/CATALOG.md     ← Copy-paste prompt templates
    └── specs/TEMPLATE.md      ← Feature spec template
```

---

## How Self-Management Works

| What Claude does automatically | When |
|-------------------------------|------|
| Reads `memory/STATE.md` | Start of every conversation |
| Plans before executing | Every non-trivial task |
| Reads diagrams before source files | Before touching existing code |
| Compacts context proactively | When context is filling up |
| Saves state to memory files | When work completes |
| Re-reads memory if it loses track | When it detects it's repeating itself |

Defined in `rules/self-management.md`. The user never needs to say "plan first" or "save your state."

---

## Built On

- [refactoring.guru](https://refactoring.guru/design-patterns/catalog) — Design patterns & code smells
- [aitmpl.com](https://www.aitmpl.com/) — 1000+ Claude Code templates
- [Claude Platform Docs](https://platform.claude.com/docs/en/intro) — Best practices & prompting
- [code-review-graph](https://github.com/tirth8205/code-review-graph) — Graph-based code review concept

---

## License

MIT License — see [LICENSE](./LICENSE) for details.

---

## 💛 Support This Project

<p align="center">
    <a href="https://buymeacoffee.com/daby">
    <img src="https://img.shields.io/badge/☕_Buy_Me_a_Coffee-Support_This_Project-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" alt="Buy Me a Coffee" width="300">
    </a>
</p>

Your support helps maintain and improve this skill with new research and better examples.

<p align="center">
  <a href="https://buymeacoffee.com/daby"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me a Coffee" width="200"></a>
</p