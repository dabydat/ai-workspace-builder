# Structure Reference — Every File and Its Purpose

## Complete File Tree (70+ files)
```
claude-config/
├── CLAUDE.md              ← Master brain. Auto-loaded by Claude Code. Generated LAST.
├── README.md              ← Setup guide for humans/GitHub
├── .claudeignore          ← Files Claude must skip (node_modules, build, media, locks)
├── settings.json          ← Team-shared settings (permissions, statusLine, spinnerTips, attribution, hooks)
├── statusline-command.sh  ← Status bar (model, agent, turns, tokens, cost, git, context %, active file)
│
├── agents/ (17)           ← One file per specialist role
│   ├── 00-orchestrator    ← Coordinates all agents (Mediator pattern). UNIVERSAL.
│   ├── 01-architect       ← System design, API contracts, patterns. Reads stacks/active.md.
│   ├── 02-backend-dev     ← Server code. Reads stacks/active.md. STACK-AGNOSTIC.
│   ├── 03-frontend-dev    ← Browser code. Reads stacks/active.md. STACK-AGNOSTIC.
│   ├── 04-qa-engineer     ← Tests, security. Reads stacks/active.md. STACK-AGNOSTIC.
│   ├── 05-devops          ← CI/CD, deploy. Reads stacks/active.md. STACK-AGNOSTIC.
│   ├── 06-product-manager ← PRDs, scope, pricing. UNIVERSAL.
│   ├── 07-project-manager ← Sprints, timelines, blockers. UNIVERSAL.
│   ├── 08-ux-designer     ← Wireframes, design system. UNIVERSAL.
│   ├── 09-growth-marketer ← Launch, SEO, channels. UNIVERSAL.
│   ├── 10-content-strategist ← Copy, prompts, voice. UNIVERSAL.
│   ├── 11-business-analyst ← Economics, KPIs, pricing. UNIVERSAL.
│   ├── 12-dx-engineer     ← CX/DX/UX/UI, onboarding. UNIVERSAL.
│   ├── 13-systems-analyst ← Use cases, user stories, process flows, sequence diagrams. UNIVERSAL.
│   ├── 14-dba             ← ERD, schema, data dictionary, data integrity. UNIVERSAL.
│   ├── 15-ceo             ← Vision, strategy, OKRs, PMF, business model, investor narrative. UNIVERSAL.
│   └── 16-coo             ← Operations, processes, resource allocation, scaling, risk management. UNIVERSAL.
│
├── rules/ (6)             ← Non-negotiable quality standards
│   ├── coding.md          ← SOLID, design patterns (refactoring.guru), code smells, thresholds
│   ├── testing.md         ← Test pyramid, coverage, security tests
│   ├── design.md          ← Mobile-first, accessibility, conversion
│   ├── collaboration.md   ← PLAN → BRAINSTORM → EXECUTE protocol
│   ├── graph-thinking.md  ← Think in nodes+edges, blast radius via mermaid diagrams
│   └── self-management.md ← Auto-plan, auto-sweep, auto-compact, auto-save
│
├── skills/ (14)           ← Directory format: skills/<name>/SKILL.md (auto-discovered)
│   ├── design-patterns/SKILL.md    ← 22 GoF patterns (preloaded into architect)
│   ├── refactoring-techniques/SKILL.md ← Code smells + fixes (preloaded into devs)
│   ├── code-quality/SKILL.md       ← Quality checklist (preloaded into qa-engineer)
│   ├── business-analysis/SKILL.md  ← BA templates: use cases, user stories, ERD, flows (preloaded into systems-analyst + dba)
│   ├── brainstorm/SKILL.md         ← Multi-perspective ideation (effort: high, context: fork)
│   ├── code-review/SKILL.md        ← Code audit checklist
│   ├── plan/SKILL.md               ← Structured planning (effort: high)
│   ├── run-tests/SKILL.md          ← Test execution + report
│   ├── deploy/SKILL.md             ← Deployment checklist (manual only)
│   ├── aitmpl-catalog/SKILL.md     ← 1000+ components reference (from aitmpl.com)
│   ├── prompt-engineering/SKILL.md ← Optimal prompts (preloaded reference)
│   ├── token-optimization/SKILL.md ← Token efficiency rules (preloaded reference)
│   └── ai-workspace-builder/SKILL.md ← Builds claude-config for any project (+ 5 reference files)
│
├── stacks/ (3+ templates + active)
│   ├── README.md          ← How stacks work, how to create new ones
│   ├── active.md          ← CURRENT stack (dev agents read this)
│   ├── typescript-node.md ← TS + Node template
│   ├── react-nextjs.md    ← React + Next.js template
│   └── python-fastapi.md  ← Python + FastAPI template
│
├── memory/ (5)            ← Persistent state across sessions
│   ├── STATE.md           ← Current project state (Claude reads FIRST, updates LAST)
│   ├── CHANGELOG.md       ← Session-by-session work log
│   ├── DECISIONS.md       ← Architecture Decision Records
│   ├── claude-capabilities.md ← Claude models, features, limits reference
│   └── token-strategy.md  ← Token costs, model selection, context management
│
├── diagrams/ (8 universal + 3 code graphs built by /build-graph)
│   ├── context-map.mermaid       ← WHAT to read per task (most important)
│   ├── agent-routing.mermaid     ← WHO handles what
│   ├── workflow.mermaid          ← HOW work flows (idea → ship)
│   ├── decision-tree.mermaid     ← WHICH design pattern to use
│   ├── blast-radius.mermaid      ← WHAT a code change affects
│   ├── agents-communication.mermaid ← HOW agents coordinate
│   ├── monetization.mermaid      ← User-to-payment funnel
│   ├── session-lifecycle.mermaid ← /start → work → /end protocol
│   ├── code-graph.mermaid        ← (generated by /build-graph) codebase map
│   ├── code-deps.mermaid         ← (generated by /build-graph) file dependencies
│   └── code-tests.mermaid        ← (generated by /build-graph) test coverage
│
├── commands/ (12)         ← Slash commands for Claude Code
│   ├── start.md           ← Read STATE.md, output 5-line status (Haiku)
│   ├── end.md             ← Save STATE.md + CHANGELOG.md (Haiku)
│   ├── activate.md        ← Activate agent role for session (lists all 17 agents)
│   ├── setup-stack.md     ← Generate stacks/active.md from description
│   ├── create-spec.md     ← architect validates → create spec + flow diagram
│   ├── implement.md       ← architect+backend+frontend parallel → qa sequential
│   ├── plan-sprint.md     ← product-manager+project-manager parallel → orchestrator
│   ├── review.md          ← qa-engineer reviews against quality standards
│   ├── review-impact.md   ← architect(blast-radius)+qa-engineer parallel
│   ├── create-docs.md     ← scaffold docs/ → systems-analyst+dba parallel
│   ├── build-graph.md     ← Build codebase map as mermaid diagrams
│   └── blast-radius.md    ← Trace change impact from graphs
│
├── prompts/CATALOG.md     ← Copy-paste prompt templates for every task type
└── specs/TEMPLATE.md      ← Template for feature specifications
```

## Generation Order
Generate files in dependency order. A file can only reference files generated before it.

Phase 0: settings.json (with hooks), statusline-command.sh (standalone, no deps)
Phase 1: .claudeignore, memory/*
Phase 2: rules/*
Phase 3: skills/* (directory format: skills/<name>/SKILL.md)
         Mandatory: skills/business-analysis/SKILL.md (preloaded into agents 13 + 14)
Phase 4: diagrams/*
Phase 5: agents/* (reference rules + skills, include maxTurns/color/memory/skills/permissionMode)
         agents/13-systems-analyst.md (maxTurns:15, color:cyan, skills:[business-analysis])
         agents/14-dba.md             (maxTurns:15, color:blue, skills:[business-analysis])
         agents/15-ceo.md             (maxTurns:10, color:magenta)
         agents/16-coo.md             (maxTurns:10)
Phase 6: stacks/*
Phase 7: commands/* (include model:haiku for lightweight, argument-hint, disable-model-invocation)
         commands/create-docs.md      (argument-hint:"[project-name]", parallel agents inside)
Phase 8: prompts/*, specs/*, CLAUDE.md, README.md
