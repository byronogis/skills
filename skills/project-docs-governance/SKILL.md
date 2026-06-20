---
name: project-docs-governance
description: >
  Use when creating, reviewing, or maintaining an agent-readable project
  documentation governance system: AGENTS.md as the agent work contract,
  docs/index.md as the read router, docs/context.md as the current project
  snapshot, docs/specs as current rules, docs/adr as decision rationale,
  docs/history as downgraded historical material, plus commands and roadmap
  docs. Also use when bootstrapping or retrofitting this system into an
  existing repository, or before changing code in a repo that follows this
  structure, to decide what documentation to read or update.
---

# Project Docs Governance

Use this skill to work with a documentation system that controls what an agent reads, trusts, and updates while working in a codebase.

The goal is not more documentation. The goal is a small, reliable project memory with explicit trust levels and reading routes.

## Canonical Structure

Prefer this structure for reusable project documentation:

```text
/
├── AGENTS.md
└── docs/
    ├── index.md
    ├── context.md
    ├── commands.md
    ├── roadmap.md
    ├── specs/
    │   ├── README.md
    │   └── *.md
    ├── adr/
    │   ├── README.md
    │   └── 0001-*.md
    └── history/
        ├── README.md
        └── YYYY-MM-DD-*.md
```

If a repo already has equivalent names, follow the repo. Do not rename files just to match this structure unless the user asks.

Project-specific folders such as business skills, tool contracts, adapters, generated docs, or product-specific manuals are outside this generic structure. Let that project's specs define them.

## Trust Model

Use this priority order when documents and implementation disagree:

1. Running code, migrations, schemas, tests, and generated contracts are implementation facts.
2. `docs/specs/` is the current intended rule set.
3. `docs/adr/` explains durable decisions and tradeoffs.
4. `docs/history/` is historical context only.

When code and specs conflict, do not silently pick one. Decide whether the implementation is wrong or the spec is stale, then update the correct side.

Historical docs cannot directly justify implementation changes. If historical material is still valid, promote it into `docs/specs/` or `docs/adr/` first.

## File Roles

### AGENTS.md

The agent work contract. It should stay short and operational.

Include:

- What to read first.
- Trust and fact priority.
- Hard project boundaries.
- Documentation synchronization rules.
- Any repo-specific agent instructions.

Avoid duplicating the full read router. Link to `docs/index.md` for task-specific reading paths.

### docs/index.md

The documentation router. It tells agents what to read for a task and prevents broad context loading.

Use reading levels:

- L1 default reading: `AGENTS.md`, `docs/context.md`, `docs/index.md`, `docs/adr/README.md`.
- L2 task-specific reading: relevant specs, `docs/commands.md`, `docs/roadmap.md`, and directly related ADRs.
- L3 trace reading: `docs/history/`, old plans, handoff notes, and Git history.

Maintain a "read by task" section, for example:

```markdown
### Changing data model or migrations

Read:

- `docs/specs/data.md`
- `docs/specs/architecture.md`
- `docs/commands.md`
- Related ADRs
```

Never tell agents to read all of `docs/` by default.

### docs/context.md

The current project snapshot. It helps an agent enter the live situation quickly.

Good contents:

- Current phase or status.
- Current decisions that affect active work.
- Next step or current direction.
- Current implementation snapshot.
- Short-lived caveats that matter now.

Hard rule: `context.md` is not a work log. When information becomes long-lived, move it to specs or ADRs. When it becomes obsolete, delete it or move trace-worthy material to history.

### docs/specs/

The current effective rules.

Specs describe how the project should work now:

- System boundaries.
- Data model rules.
- Runtime boundaries.
- Integration contracts.
- Validation and error rules.
- Scope exclusions.
- Acceptance criteria.
- Known differences between intended and implemented behavior.

Specs should not become a second copy of the code. Prefer rules, constraints, contracts, and exceptions over function-by-function prose. If a fact is better proven by schema, tests, generated contracts, or source code, link to that source instead of duplicating it.

Each spec should distinguish current behavior, planned behavior, and known gaps when relevant.

### docs/adr/

Durable decision rationale.

Create or update an ADR when a change affects future judgment, such as:

- Architecture boundaries.
- Data model or storage semantics.
- Deletion or retention policy.
- Runtime integration boundaries.
- Deployment model.
- Major dependency or technology choices.

Do not create ADRs for routine bug fixes, small style changes, or "we completed X" status reports.

Prefer this format when the repo has no local ADR style:

```markdown
# 0001: Decision Title

## Status

Accepted

## Context

What constraints, forces, and alternatives made this decision necessary?

## Decision

What are we choosing?

## Consequences

What becomes easier, harder, constrained, or intentionally deferred?
```

When replacing a decision, create a new ADR and mark the old one as `Superseded`; do not rewrite the old rationale. Add explicit links in both directions when practical.

### docs/history/

Downgraded historical material.

Use for old plans, long discussion summaries, previous specs, handoff records, and evolution traces that may help future archaeology.

Rules:

- Do not read by default.
- Do not treat as current rules.
- Do not implement directly from history.
- If still valid, promote into specs or ADRs.

### docs/commands.md

Commands and verification.

Include setup, development, formatting, typecheck, tests, smoke checks, migrations, and maintenance commands. Update it whenever command names, required environment variables, or validation flow changes.

### docs/roadmap.md

Phase and scope management.

Use for current phase status, completed phase summary, future enhancements, and explicit non-goals. Keep it more stable than `context.md`; let `context.md` point at it instead of copying the whole plan.

## Update Rules

When code changes affect project rules:

- Update the relevant spec.
- Update `docs/commands.md` if commands or validation changed.
- Update `docs/context.md` and `docs/roadmap.md` if phase or current status changed.
- Add or update an ADR for durable decisions.
- Add history only when the evolution itself must be traceable.

When docs change:

- Check claims against code, migrations, tests, or contracts where practical.
- Keep current rules in specs.
- Keep reasons in ADRs.
- Keep old material in history.
- Update `docs/index.md` when files move, new specs are added, or reading routes change.

## Anti-Bloat Rules

- Do not create long-lived docs for one-off tasks.
- Do not record execution logs in `context.md`.
- Do not copy code facts into prose when code, tests, schema, or generated artifacts are better sources.
- Do not let `AGENTS.md` and `docs/index.md` duplicate detailed routing.
- Do not let `roadmap.md` and `context.md` drift into parallel status documents.
- Every doc must have a clear reader and reading moment. If it does not, merge it, delete it, or move it to history.

## Bootstrapping Into an Existing Repo

When asked to land this system in an existing repository, implement it yourself. Do not ask the user to classify documents manually.

First inventory the repo:

- Existing docs, READMEs, plans, handoff notes, and design files.
- Package manifests, scripts, task runners, and CI files.
- Tests, schemas, migrations, generated contracts, and config.
- Source directory layout and major runtime boundaries.

Then create the smallest useful governance layer:

- `AGENTS.md`
- `docs/index.md`
- `docs/context.md`
- `docs/specs/README.md`
- `docs/adr/README.md`
- `docs/history/README.md`

Add `docs/commands.md` only when commands or validation flow can be discovered. Add `docs/roadmap.md` only when current direction, phases, or non-goals are documented or strongly inferable.

Prefer routing to existing docs over moving them. Move or rewrite existing docs only when their new home is obvious and the change improves future navigation.

Create initial specs only for areas supported by repository evidence. Common first specs are architecture, data model, runtime boundaries, API contracts, or validation flow. Do not invent product strategy, unstated architecture intent, or future plans.

Create ADRs only for decisions evident from code, configs, tests, or existing docs. If a decision is plausible but not evidenced, record it as an open question in `docs/context.md`, not as an ADR.

Keep `docs/history/` empty except for migrated old plans or documents that are clearly historical. Historical material must remain downgraded.

Before finishing a bootstrap:

1. Ensure `docs/index.md` contains L1/L2/L3 reading levels.
2. Add task-specific reading routes for the parts of the repo you can identify.
3. Verify claims against code, scripts, tests, schemas, or existing docs where practical.
4. Summarize created files, inferred facts, routed existing docs, and unresolved questions.

## Working Procedure

Before changing code in a repo that follows this system:

1. Read `AGENTS.md`.
2. Read `docs/index.md`.
3. Read `docs/context.md`.
4. Read `docs/adr/README.md`.
5. Use `docs/index.md` to choose only the L2 docs relevant to the task.
6. Read history only when tracing evolution or resolving ambiguity.

Before finishing:

1. Decide whether the change affected specs, ADRs, commands, roadmap, or context.
2. Update only the necessary documentation layer.
3. Check that no historical material has become an unstated current rule.
4. Summarize which docs were read or updated.
