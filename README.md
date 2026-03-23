<p align="center">
  <h1 align="center">AI Workspace Builder for Claude</h1>
  <p align="center">
    <a href="https://buymeacoffee.com/daby"><img src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-support-yellow?style=for-the-badge&logo=buy-me-a-coffee&logoColor=white" alt="Buy Me a Coffee"></a>
    <img src="https://img.shields.io/badge/Claude_AI-Skill-blueviolet?style=for-the-badge&logo=anthropic&logoColor=white" alt="Claude AI Skill">
    <img src="https://img.shields.io/badge/Version-1.2.0-blue?style=for-the-badge" alt="Version">
    <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="MIT License">
  </p>
</p>

> A Claude Code skill that generates a complete, self-managing AI workspace configuration for any project — covering the full lifecycle from **business analysis to deployment**.

---

## What It Does

You say: **"Configure my workspace for TypeScript + Next.js + PostgreSQL"**

The skill generates **80+ files** that turn Claude into a self-managing development team with built-in business analysis, database design, and development best practices:

- **15 agents** — From Systems Analyst and DBA to Architect, Backend, Frontend, QA, DevOps, and 7 business roles
- **14 skills** — Including business-analysis templates (use cases, ERD, user stories, BPMN flows)
- **6 rules** — Coding (SOLID + refactoring.guru), Testing, Design, Collaboration, Graph Thinking, Self-Management
- **8 diagrams** — Mermaid graphs that replace text explanations (5× token savings)
- **13 slash commands** — Including `/create-docs` to scaffold project documentation
- **5 memory files** — Persistent state across sessions (Claude never forgets where it left off)
- **Stack templates** — TypeScript, React, Python (or generate any stack with `/setup-stack`)
- **docs/ folder** — Auto-generated business analysis documentation for every project
- **settings.json** — Permissions (git/gh auto-allow), spinner tips, attribution, autocompact
- **statusline-command.sh** — Colored status bar: model, tokens, git info, context usage %

---

## Full Project Workflow

This is the complete lifecycle every project follows — from first idea to shipped code:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    PROJECT LIFECYCLE                                │
├─────────────────────────────────────────────────────────────────────┤
│  PHASE 0: SETUP                                                     │
│  "Configure my workspace for [stack]"                               │
│  → Generates claude-config/ with all agents, rules, skills         │
│  → /setup-stack [technologies]  — sets stacks/active.md            │
├─────────────────────────────────────────────────────────────────────┤
│  PHASE 1: BUSINESS ANALYSIS  ← NEW in v1.2.0                       │
│  /create-docs [project-name]  — scaffolds docs/ folder             │
│  /activate systems-analyst    — use cases, user stories, flows     │
│  /activate dba                — ERD, schema, data dictionary        │
│  /activate architect          — system design, C4 diagrams, ADRs   │
│  /activate product-manager    — prioritize into sprint backlog      │
│                                                                     │
│  Output: docs/ with requirements, use cases, ERD, architecture     │
├─────────────────────────────────────────────────────────────────────┤
│  PHASE 2: PLANNING                                                  │
│  /plan [feature]              — structured plan for single task     │
│  /plan-sprint                 — sprint plan with gantt + agents     │
│  /create-spec [name]          — feature spec + flow diagram         │
├─────────────────────────────────────────────────────────────────────┤
│  PHASE 3: DEVELOPMENT                                               │
│  /start                       — read STATE.md, know where you are  │
│  /activate backend-dev        — APIs, DB, queues, AI pipelines      │
│  /activate frontend-dev       — UI, components, forms, routing      │
│  /implement [spec-name]       — implement from spec with graphs     │
│  /build-graph                 — map codebase as mermaid diagrams    │
├─────────────────────────────────────────────────────────────────────┤
│  PHASE 4: QUALITY                                                   │
│  /review [file]               — code review vs coding.md           │
│  /review-impact [file]        — review + blast radius from graphs  │
│  /blast-radius [target]       — trace what a change impacts         │
│  /activate qa-engineer        — test plan, execution, sign-off      │
├─────────────────────────────────────────────────────────────────────┤
│  PHASE 5: SHIP                                                      │
│  /activate devops             — CI/CD, deploy, monitoring           │
│  /end                         — save state for next session         │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Business Analysis Layer (v1.2.0)

Every project now starts with a proper analysis phase before any code is written.

### What gets documented automatically

```
docs/
├── 01-requirements/
│   └── REQUIREMENTS.md        ← Functional + non-functional requirements
├── 02-analysis/
│   ├── USE_CASES.md           ← Actor map, use case diagram, use case specs
│   └── USER_STORIES.md        ← All user stories with acceptance criteria
├── 03-database/
│   ├── ERD.md                 ← Mermaid erDiagram + relationship rules
│   ├── SCHEMA.md              ← Full SQL DDL: tables, views, RLS, constraints
│   └── DATA_DICTIONARY.md     ← Every field: type, nullable, business meaning
├── 04-architecture/
│   └── SYSTEM_CONTEXT.md      ← C4 diagrams (context, container, component)
├── 05-flows/
│   └── SEQUENCE_DIAGRAMS.md   ← Step-by-step interaction diagrams
└── 06-team/
    └── TEAM_ROLES.md          ← Roles, responsibilities, RACI matrix
```

### New agents for analysis

| Agent | Role | When to invoke |
|---|---|---|
| **13 — Systems Analyst** | Use cases, user stories, process flows, sequence diagrams | Before any feature development |
| **14 — DBA** | ERD, SQL schema, data dictionary, integrity rules | After entities defined by use cases |

### New skill: business-analysis
Preloaded into agents 13 and 14. Contains templates for every BA artifact:
- Actor map, use case diagram, use case specification
- User story format with Given/When/Then acceptance criteria
- BPMN-style process flow (Mermaid flowchart)
- Sequence diagram (Mermaid sequenceDiagram)
- ERD (Mermaid erDiagram), data dictionary row format
- Requirements document structure (FR, NFR, Constraints, Out of Scope)
- Team RACI matrix template, docs/ folder structure

### New command: /create-docs
```
/create-docs [project-name]
```
Scaffolds the complete docs/ folder in under 2 minutes. Asks 5 questions, generates 9 files with Mermaid diagram stubs, and outputs the next steps sequence.

---

## Key Features

### Self-Managing Claude
Claude auto-plans before executing, auto-saves state between sessions, auto-compacts context before overflow, and auto-diagnoses when it loses track. The user only needs to say `/start` and `/end`.

### Orchestration: Command → Agent → Skill
Commands are user entry points. Agents run autonomously with preloaded skills (domain knowledge injected at startup). Dev agents auto-accept edits (`permissionMode: acceptEdits`), have execution limits (`maxTurns`), and preload quality skills.

### Stack Abstraction
Dev agents contain zero framework-specific code. They read `stacks/active.md` for patterns. Change your entire stack by running `/setup-stack [new tech]` — all agents adapt automatically.

### Diagram-Based Code Graphs
Claude maintains your codebase as 3 mermaid diagrams (structure, dependencies, test coverage). No MCP server, no Python, no external tools. Claude reads a 300-token diagram instead of scanning 200 files.

### Token Efficiency
- `.claudeignore` blocks irrelevant files from context
- Diagrams replace text explanations (300 tokens vs 1500)
- `memory/STATE.md` prevents re-discovering context each session
- `model: haiku` on lightweight commands saves tokens/cost
- `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE: 80` compacts earlier

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

The skill activates, asks about your project (stack, users, main features, entities), and generates the full configuration including docs/ scaffolding.

### After Setup — Full Session Example

```bash
# Session start
/start                            # Claude reads memory, knows where it is

# Business analysis (do this first, before any code)
/create-docs my-saas-app          # Scaffold docs/ with all BA templates
/activate systems-analyst         # Write use cases, user stories, process flows
/activate dba                     # Design ERD, schema, data dictionary
/activate architect               # Design system architecture, C4 diagrams

# Planning
/plan-sprint                      # Sprint plan with gantt + agent assignments
/create-spec user-auth            # Create a feature specification

# Development
/activate backend-dev             # Implement APIs from spec
/implement user-auth              # Implement from spec using graphs
/build-graph                      # Map codebase as mermaid diagrams
/blast-radius src/auth.ts         # See what a change would impact

# Quality
/review-impact src/auth.ts        # Code review + blast radius
/activate qa-engineer             # Test plan and sign-off

# Ship & close
/activate devops                  # Deploy pipeline
/end                              # Save state for next session
```

---

## What Gets Generated

```
your-project/
└── claude-config/
    ├── CLAUDE.md                    ← Master brain (auto-loaded, <important> tags, 11 rules)
    ├── .claudeignore                ← Files Claude skips
    ├── settings.json                ← Permissions, statusLine, spinnerTips, attribution
    ├── statusline-command.sh        ← Colored status bar (model, tokens, git, context %)
    ├── agents/ (15)                 ← Full frontmatter: maxTurns, color, memory, skills
    │   ├── 00-orchestrator.md       ← Coordinates all agents
    │   ├── 01-architect.md          ← System design, API contracts
    │   ├── 02-backend-dev.md        ← APIs, DB, queues
    │   ├── 03-frontend-dev.md       ← UI, components, routing
    │   ├── 04-qa-engineer.md        ← Tests, security, sign-off
    │   ├── 05-devops.md             ← CI/CD, deploy, monitoring
    │   ├── 06-product-manager.md    ← PRDs, scope, pricing
    │   ├── 07-project-manager.md    ← Sprints, timelines, blockers
    │   ├── 08-ux-designer.md        ← Wireframes, design system
    │   ├── 09-growth-marketer.md    ← Launch, SEO, channels
    │   ├── 10-content-strategist.md ← Copy, prompts, brand voice
    │   ├── 11-business-analyst.md   ← Unit economics, KPIs
    │   ├── 12-dx-engineer.md        ← CX/DX/UX/UI, onboarding
    │   ├── 13-systems-analyst.md    ← Use cases, user stories, process flows ← NEW
    │   └── 14-dba.md                ← ERD, schema, data dictionary ← NEW
    ├── rules/ (6)                   ← Non-negotiable quality standards
    ├── skills/ (14)                 ← Directory format: skills/<name>/SKILL.md
    │   └── business-analysis/       ← BA templates preloaded into agents 13+14 ← NEW
    ├── stacks/                      ← Stack-specific patterns (swap per project)
    ├── memory/ (5)                  ← Persistent state across sessions
    ├── diagrams/ (8+3)              ← Visual context + code graphs
    ├── commands/ (13)               ← Slash commands
    │   └── create-docs.md           ← Scaffolds docs/ at project start ← NEW
    ├── prompts/CATALOG.md           ← Copy-paste prompt templates
    ├── specs/TEMPLATE.md            ← Feature spec template
    └── docs/                        ← Generated per project by /create-docs ← NEW
        ├── 01-requirements/
        ├── 02-analysis/
        ├── 03-database/
        ├── 04-architecture/
        ├── 05-flows/
        └── 06-team/
```

---

## How Self-Management Works

| What Claude does automatically | When |
|-------------------------------|------|
| Reads `memory/STATE.md` | Start of every conversation |
| Plans before executing | Every non-trivial task |
| Reads diagrams before source files | Before touching existing code |
| Verifies its work (tests, typecheck) | After implementing changes (Rule 11) |
| Compacts context proactively | When context reaches ~50% |
| Saves state to memory files | When work completes |
| Re-reads memory if it loses track | When it detects it's repeating itself |

---

## Changelog

### v1.2.0 — Business Analysis Layer
- Added `agents/13-systems-analyst.md` — use cases, user stories, process flows, sequence diagrams
- Added `agents/14-dba.md` — ERD, SQL schema, data dictionary, data integrity rules
- Added `skills/business-analysis/SKILL.md` — 12-section template library for all BA artifacts
- Added `commands/create-docs.md` — scaffolds complete docs/ folder at project start
- Added `references/business-analysis.md` — full methodology guide for the workspace builder
- Updated `SKILL.md` — business analysis phase now mandatory in workspace generation
- Updated `references/structure.md` — reflects new agents, skill, and command

### v1.1.0 — Best Practices Update
- Added conventional commits, naming conventions, terminal commands, domain glossary
- Added parallel worktrees documentation for isolated multi-agent work
- Fixed statusline dependencies (jq, chmod +x, correct JSON fields)

### v1.0.0 — Initial Release
- 13 agents, 11 skills, 12 commands, 6 rules, 8 diagrams, 5 memory files

---

## Built On

- [refactoring.guru](https://refactoring.guru/design-patterns/catalog) — Design patterns & code smells
- [aitmpl.com](https://www.aitmpl.com/) — 1000+ Claude Code templates
- [Claude Platform Docs](https://platform.claude.com/docs/en/intro) — Best practices & prompting
- [code-review-graph](https://github.com/tirth8205/code-review-graph) — Graph-based code review concept
- [claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) — Community best practices
- BABOK v3 — Business Analysis Body of Knowledge (methodology for business analysis layer)

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
</p>
