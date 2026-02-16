---
name: skill-creator
description: "Guide for creating effective skills. Use when the user wants to create a new skill or update an existing skill that extends Claude's capabilities with specialized knowledge, workflows, or tool integrations."
---

# Skill Creator

Guide for creating effective skills in this project.

## Skill Structure

```
skill-name/
├── SKILL.md          (required — frontmatter + instructions)
├── scripts/          (optional — executable code)
├── references/       (optional — docs loaded on demand)
└── assets/           (optional — templates, icons, files used in output)
```

## SKILL.md Format

### Frontmatter (required)

```yaml
---
name: skill-name
description: "What the skill does and when to use it. Be specific about triggers."
---
```

- `name`: kebab-case skill name
- `description`: comprehensive — this is what triggers the skill. Include all "when to use" info here.

### Body (required)

Instructions for using the skill. Keep under 500 lines. Use progressive disclosure — reference files in `references/` for detailed content.

## Core Principles

1. **Concise is key** — only add context the AI doesn't already have
2. **Progressive disclosure** — metadata always loaded, SKILL.md on trigger, references on demand
3. **Match freedom to fragility** — tight scripts for fragile ops, loose guidance for creative tasks
4. **No extra docs** — no README, CHANGELOG, CONTRIBUTING. Just what the agent needs.

## Creation Process

1. **Understand** — gather concrete usage examples
2. **Plan** — identify scripts, references, assets needed
3. **Create** — `mkdir -p skills/<name>` and write SKILL.md
4. **Implement** — add scripts/references/assets
5. **Test** — use the skill on real tasks
6. **Iterate** — improve based on usage

## Tips

- Scripts: test them by running. Deterministic > regenerated each time.
- References: use for large docs (>100 lines). Add TOC for long files.
- Assets: templates, images, etc. Not loaded into context, just used in output.
- Keep references one level deep from SKILL.md.
- If SKILL.md approaches 500 lines, split into reference files.

## Skill Location

Place skills in `skills/` at the project root. They'll be available to all agents working on this project.
