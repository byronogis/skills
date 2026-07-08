---
name: project-docs-charter
description: Initialize a project-specific agent documentation system.
disable-model-invocation: true
---

# Project Docs Charter

Use this skill as a one-time or occasional initializer. Its job is to help the
user draw governance boundaries, then write those decisions into the target
repository's own `AGENTS.md` and `docs/`. After initialization, the project
docs govern day-to-day agent behavior; this skill does not stay resident.

The leading word is **charter**: create a local documentation charter that says
which work types exist, what each work type reads, what it updates, and which
sources outrank others.

## Steps

1. Inspect the target repo.
   - Read existing `AGENTS.md` when present.
   - Inventory existing docs, READMEs, plans, handoffs, design notes, package
     manifests, scripts, CI, tests, schemas, migrations, generated contracts,
     config, source layout, and major runtime boundaries.
   - Do not ask the user for facts the repo can show.
   - Completion criterion: you can name the repo's discoverable docs, command
     surfaces, implementation evidence, and obvious boundaries.

2. Interview the user one decision at a time.
   - Ask exactly one question, give your recommended answer, and wait.
   - Ask about decisions, not discoverable facts.
   - Start from the default work types in this skill; let the user keep, rename,
     merge, remove, or add types.
   - Completion criterion: every work type has a confirmed purpose, required
     reading, update obligations, and escalation points.

3. Draft the local charter.
   - Keep `AGENTS.md` thin: first reads, trust order, hard boundaries, and the
     pointer to `docs/index.md`.
   - Put task routes in `docs/index.md`, organized by work type.
   - Create a thin `docs/context.md` for current situation and open questions.
   - Create `docs/specs/README.md`; add concrete specs only when repo evidence
     or user confirmation supports them.
   - Create a governance ADR by default.
   - Route old docs in place unless they are clearly historical or the user
     confirms they are historical.
   - Completion criterion: the draft has one current home for each rule and no
     lower-trust document acting as current guidance.

4. Write the files after the user accepts the draft.
   - Preserve existing project conventions and equivalent file names.
   - Prefer routing to existing docs over moving them.
   - Move only clearly historical material to `docs/history/`.
   - Completion criterion: the target repo has the agreed charter files, and
     unresolved decisions are recorded as open questions rather than invented
     rules.

5. Finish with a handoff.
   - Summarize created files, routed existing docs, moved historical material,
     inferred facts, user decisions, and unresolved questions.
   - Name the docs future agents should read first.
   - Completion criterion: a future agent can enter the repo by reading
     `AGENTS.md`, then follow `docs/index.md` without invoking this skill.

## Default Work Types

Use these as the initial interview frame. Keep the set small unless the user
has a real project-specific split.

1. **Orientation**: entering the project and understanding the current state.
2. **Feature Work**: adding features or changing behavior.
3. **Bug Fixing**: diagnosing and fixing broken behavior.
4. **Architecture / Data Changes**: changing architecture, data models,
   persistence, API contracts, or runtime boundaries.
5. **Operations / Commands**: setup, development, validation, deployment,
   migrations, and maintenance commands.
6. **Documentation Cleanup**: routing, migrating, downgrading, deleting, or
   consolidating old docs.

For each confirmed work type, capture:

- What triggers this work type.
- What the agent reads before acting.
- Which implementation evidence outranks docs.
- Which docs the agent may need to update before finishing.
- When the agent must ask the user rather than infer.

## Local Files To Create

Prefer this structure unless the target repo already has equivalent names:

```text
/
├── AGENTS.md
└── docs/
    ├── index.md
    ├── context.md
    ├── specs/
    │   └── README.md
    ├── adr/
    │   ├── README.md
    │   └── 0001-agent-docs-governance.md
    └── history/
        └── README.md
```

Add `docs/commands.md` only when commands or validation flow can be discovered.
Add `docs/roadmap.md` only when phases, current direction, or non-goals are
documented or confirmed.

Project-specific folders such as business rules, tool contracts, adapters,
generated docs, or product manuals belong outside this generic structure unless
the charter routes them.

## File Charters

### AGENTS.md

Thin project entry contract. Include:

- What to read first.
- Trust and fact priority.
- Hard project boundaries.
- Documentation synchronization rules.
- Pointer to `docs/index.md` for work-type routes.

Do not duplicate detailed routes from `docs/index.md`.

### docs/index.md

The route map. Organize it by confirmed work type.

Use reading levels:

- L1 default reading: `AGENTS.md`, `docs/context.md`, `docs/index.md`, and
  `docs/adr/README.md`.
- L2 work-type reading: relevant specs, commands, roadmap, existing docs, and
  directly related ADRs.
- L3 trace reading: `docs/history/`, old plans, handoff notes, and Git history.

Never tell agents to read all of `docs/` by default.

### docs/context.md

Thin current situation file. Include only:

- Current phase or focus.
- Short-lived facts that affect agent action.
- Open questions not yet promoted to specs or ADRs.
- Links to relevant roadmap, specs, and ADRs.

Do not use it as a work log, full plan, or long-term rule store. When a note
becomes durable, move it to a spec or ADR. When it expires, delete it or move
trace-worthy material to history.

### docs/specs/

Current effective rules. Create `docs/specs/README.md` by default. Create
specific specs only when the repo evidence or user confirmation supports them.

Specs may cover system boundaries, data model rules, runtime boundaries,
integration contracts, validation rules, scope exclusions, acceptance criteria,
and known gaps. Do not copy code-shaped facts into prose when source, schemas,
tests, generated contracts, or migrations are better evidence.

### docs/adr/

Durable decision rationale. Create a governance ADR by default because the
documentation charter shapes future agent behavior.

The governance ADR records:

- Why this repo uses an agent documentation charter.
- Why `AGENTS.md` is thin.
- Why routes are organized by work type.
- Which documents are current rules and which are historical.
- Which initialization questions remain open.

Use the repo's ADR style when present. Otherwise use:

```markdown
# 0001: Agent Docs Governance

## Status

Accepted

## Context

What forces made this documentation charter necessary?

## Decision

What structure and routing rules were chosen?

## Consequences

What becomes easier, harder, constrained, or intentionally deferred?
```

### docs/history/

Downgraded historical material. Create `docs/history/README.md` by default, but
move old docs into `history/` only when they are clearly historical or the user
confirms they are historical. Otherwise route them in place and record open
questions in `docs/context.md`.

History is not current guidance. If historical material is still valid, promote
it into specs or ADRs before using it as a rule.

## Trust Order To Install

Install this trust order into the local charter unless the user chooses a
project-specific variant:

1. Running code, migrations, schemas, tests, generated contracts, package
   scripts, CI, and config.
2. `docs/specs/`: current intended rules.
3. `docs/adr/`: durable decision rationale.
4. `docs/history/`: downgraded historical material.

When implementation and specs disagree, do not silently choose one. Decide
whether implementation is wrong or the spec is stale, then update the correct
side or record the unresolved question.

## Interview Defaults

Carry these defaults into the interview unless the user changes them:

- The skill is strongly interactive.
- Work-type boundaries come before document-role boundaries.
- The default work-type list starts the interview.
- `AGENTS.md` stays thin.
- A governance ADR is created by default.
- `docs/context.md` exists, stays thin, and has explicit cleanup rules.
- `docs/specs/README.md` is created before any concrete spec.
- Concrete specs require repo evidence or user confirmation.
- Old docs move only when clearly historical or confirmed historical.

## Pruning Rules

- Every document must have a reader and reading moment.
- Each current rule has one current home.
- Reasons live in ADRs.
- Short-lived status lives in `docs/context.md`.
- Stable phase plans live in `docs/roadmap.md` when that file exists.
- Historical material lives in `docs/history/` or stays routed as old material.
- Implementation facts are linked, not recopied, when code, schemas, tests,
  migrations, generated contracts, or config are better evidence.
