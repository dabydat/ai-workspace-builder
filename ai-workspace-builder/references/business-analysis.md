# Business Analysis Reference — What to Generate

This reference defines the business analysis layer that every project config must include.
It covers agents, skills, commands, and the docs/ folder structure.

---

## Why Business Analysis Comes First

```
Business Goals
     ↓
Actor Identification (who uses the system)
     ↓
Use Cases (what actors do)          ← Systems Analyst
     ↓
User Stories (granular requirements)← Systems Analyst
     ↓
Process Flows (how business works)  ← Systems Analyst
     ↓
Sequence Diagrams (how components interact) ← Systems Analyst
     ↓
ERD + Schema (how data is structured) ← DBA
     ↓
Architecture Design                 ← Architect
     ↓
Development begins                  ← Backend + Frontend
```

Skipping this phase causes: scope creep, wrong data models, rework, missed requirements.

---

## New Agents to Generate

### agents/13-systems-analyst.md
**When to invoke:** Before any feature development. When requirements are unclear. When use cases are needed.

**Frontmatter:**
```yaml
---
name: systems-analyst
description: Use this agent to gather business requirements, define actors and use cases, write user stories, create process flow diagrams, and produce sequence diagrams. Invoke before any development begins on a new feature or project.
model: sonnet
tools: [Read, Write, Edit, Glob, Grep]
maxTurns: 15
color: cyan
---
```

**Content must include:**
- Identity: translates business needs into precise specs
- Core responsibilities: actors, use cases, user stories, process flows, sequence diagrams
- Non-negotiable: every feature has a use case before code is written
- Diagram standards: which Mermaid type for each artifact
- Output location: `docs/02-analysis/`
- Collaboration map: receives from PM, hands off to DBA and Architect

### agents/14-dba.md
**When to invoke:** After systems analyst defines entities, before backend development.

**Frontmatter:**
```yaml
---
name: dba
description: Use this agent to design database schemas, create ERDs, write SQL DDL, define indexes, write data dictionaries, and make data integrity decisions. Invoke after use cases define entities, before any backend development.
model: sonnet
tools: [Read, Write, Edit, Glob, Grep]
maxTurns: 15
color: blue
---
```

**Content must include:**
- Identity: owns the data model, schema is the contract
- Core responsibilities: ERD, SQL DDL, indexes, data dictionary, multi-tenancy strategy, partitioning
- Non-negotiable: every table has UUID PK + created_at; org-scoped tables have RLS; append-only tables have DB-level protection
- Output location: `docs/03-database/`
- Collaboration map: receives from Systems Analyst, hands off to Backend Dev

---

## New Skill to Generate

### skills/business-analysis/SKILL.md
```yaml
---
name: business-analysis
description: Provides templates and standards for all business analysis artifacts. Covers actor identification, use case specs, user stories, BPMN flows, sequence diagrams, ERD, data dictionary, requirements docs, and RACI matrix. Preloaded into systems-analyst and dba agents.
user-invocable: false
---
```

**Must include these sections:**
1. **Phase 0 mandate** — the analysis sequence that must happen before coding
2. **Actor identification** — actor types + Mermaid actor map template
3. **Use case diagram template** — `graph LR` with actors and use cases
4. **Use case specification template** — ID, Name, Actor, Preconditions, Trigger, Main Flow, Alternative Flow, Exception Flow, Postconditions, Business Rules
5. **User story template** — As a/I want/So that + Given/When/Then acceptance criteria + MoSCoW priority + story sizing
6. **Process flow template** — `flowchart TD` with swimlanes, decisions, events
7. **Sequence diagram template** — `sequenceDiagram` with actor/participant, alt/loop, solid/dashed arrows
8. **ERD template** — `erDiagram` with cardinality notation
9. **Data dictionary row standard** — Column/Type/Nullable/Default/Description/Business Rules
10. **Requirements document structure** — FR, NFR, Constraints, Assumptions, Out of Scope
11. **Team roles & RACI matrix** — all 10 standard roles + RACI table
12. **docs/ folder structure** — numbered folder layout with file purposes
13. **Quality gates checklist** — 10 checkboxes before handing to development

---

## New Command to Generate

### commands/create-docs.md
```yaml
---
name: create-docs
description: "Scaffolds the complete project documentation structure with all required files and Mermaid diagram templates. Run at the START of any new project."
argument-hint: "[project-name]"
---
```

**Behavior:**
1. Ask 5 questions: product type, users, top 3 actions, multi-tenant?, main entities
2. Create docs/ folder with 9 files populated from business-analysis skill templates
3. Pre-fill what's known, mark unknowns as [TODO]
4. Output summary of next steps (activate systems-analyst → dba → architect → product-manager)
5. Update memory/STATE.md: phase = "Analysis"

---

## docs/ Folder Structure (Generated for Every Project)

```
docs/
├── README.md                          ← Index + reading order + diagram quick-reference
├── 01-requirements/
│   └── REQUIREMENTS.md               ← Functional + non-functional + constraints + out-of-scope
├── 02-analysis/
│   ├── USE_CASES.md                  ← Actor map, use case diagram, use case specifications
│   └── USER_STORIES.md               ← All user stories with acceptance criteria
├── 03-database/
│   ├── ERD.md                        ← Mermaid erDiagram + relationship rules + design decisions
│   ├── SCHEMA.md                     ← Full SQL DDL: enums, tables, views, RLS, immutability
│   └── DATA_DICTIONARY.md            ← Every field: type, nullable, business meaning, rules
├── 04-architecture/
│   └── SYSTEM_CONTEXT.md             ← C4 diagrams (context + container + component), ADRs
├── 05-flows/
│   └── SEQUENCE_DIAGRAMS.md          ← Step-by-step interaction diagrams for critical workflows
└── 06-team/
    └── TEAM_ROLES.md                 ← Roles, responsibilities, RACI matrix
```

---

## Updated Agent Preloading

Add `business-analysis` skill preloading to the new agents:

```yaml
# agents/13-systems-analyst.md frontmatter:
skills:
  - business-analysis

# agents/14-dba.md frontmatter:
skills:
  - business-analysis
```

---

## Updated Generation Order

Insert into Phase 5 (Agents):
```
agents/13-systems-analyst.md  (maxTurns: 15, color: cyan, skills: [business-analysis])
agents/14-dba.md              (maxTurns: 15, color: blue, skills: [business-analysis])
```

Insert into Phase 3 (Skills):
```
skills/business-analysis/SKILL.md  (user-invocable: false — preloaded into agents 13 + 14)
```

Insert into Phase 7 (Commands):
```
commands/create-docs.md  (argument-hint: "[project-name]")
```

---

## Updated CLAUDE.md Agent Roster

Add these rows to the Agent Roster table:

| Task | Agent | File |
|---|---|---|
| Requirements, use cases, user stories, process flows | Systems Analyst | agents/13-systems-analyst.md |
| ERD, database schema, data dictionary, data integrity | DBA | agents/14-dba.md |

---

## Updated Collaboration Rule

Add this to `rules/collaboration.md`:

```
### Pre-Development Phase (MANDATORY for new projects and major features)
Before ANY code is written for a new project or significant feature:

1. /create-docs [project-name]  → scaffold documentation structure
2. /activate systems-analyst    → define actors, use cases, user stories, process flows
3. /activate dba               → design ERD, schema, data dictionary
4. /activate architect         → design system architecture from specs
5. /activate product-manager   → prioritize use cases into sprint backlog

This sequence is non-negotiable. Development agents (02-05) must NOT start
work until steps 1-4 are complete and reviewed.
```

---

## Diagrams to Add

Add these to the diagrams that /create-docs generates per project:
- `diagrams/business-flow.mermaid` — main business process from actor to outcome
- `diagrams/data-model.mermaid` — high-level ERD overview (not full schema)

These follow the same pattern as other diagrams: Mermaid format, < 40 lines, read by agents to save tokens.
