# Byron's Skills

Personal Codex skills for reusable engineering workflows.

## Structure

```text
skills/
└── skill-name/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    ├── references/
    ├── scripts/
    └── assets/
```

Only `SKILL.md` is required. Add `agents/openai.yaml` for UI metadata, and add `references/`, `scripts/`, or `assets/` only when the skill genuinely needs them.

## Conventions

- Skill folder names use lowercase hyphen-case.
- `SKILL.md` frontmatter contains only `name` and `description`.
- Keep `SKILL.md` concise; move bulky optional material into `references/`.
- Prefer small, composable skills over broad process frameworks.
- Do not add placeholder resource directories.

