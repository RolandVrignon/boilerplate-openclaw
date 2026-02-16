# New Doc: $ARGUMENTS

Create a new documentation or spec file. $ARGUMENTS should describe what the document is about.

---

## 1. DETERMINE DOCUMENT TYPE

Based on $ARGUMENTS, decide which type to create:

| Keyword                | Type                         | Location                  | Template                            |
| ---------------------- | ---------------------------- | ------------------------- | ----------------------------------- |
| "spec", "task"         | Task specification           | `specs/TASK-ID.md`        | Task Spec                           |
| "contract", "api"      | API contract                 | `specs/contracts/NAME.md` | Use `/api-contract` command instead |
| "arch", "architecture" | Architecture doc             | `architecture.md`         | Architecture                        |
| "adr", "decision"      | Architecture Decision Record | `specs/adr/ADR-NNN.md`    | ADR                                 |
| anything else          | General doc                  | `docs/NAME.md`            | General                             |

---

## 2. TEMPLATES

### Task Spec

```markdown
# TASK-ID: [Title]

## Agent: @[dev-front / dev-back]

## Priority: [P0 / P1 / P2]

## Depends on: [TASK-ID or none]

## Contract: [specs/contracts/NAME.md or none]

## Description

[What needs to be built]

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Technical Notes

[Any implementation hints, patterns to follow, gotchas]

## Files Expected

- `path/to/file.ts` — [purpose]
- `path/to/other.ts` — [purpose]

## Out of Scope

- [What this task does NOT include]
```

### Architecture

```markdown
# Architecture: [Project Name]

## System Overview

[High-level description and diagram]

## Platform

- [ ] Mobile (React Native + Expo)
- [ ] Web (Next.js)
- [ ] Both (monorepo)

## Data Model

[Entity relationship description]

## Auth Flow

[How authentication works]

## Key Screens / Pages

| Screen | Route | Description |
| ------ | ----- | ----------- |
| ...    | ...   | ...         |

## External Services

| Service  | Purpose | SDK/API               |
| -------- | ------- | --------------------- |
| Supabase | Backend | @supabase/supabase-js |
| ...      | ...     | ...                   |

## Infrastructure

- Hosting: [EAS / Vercel / both]
- Database: Supabase PostgreSQL
- Storage: Supabase Storage
- Edge Functions: Supabase (Deno)
```

### ADR (Architecture Decision Record)

```markdown
# ADR-NNN: [Title]

## Status: [proposed / accepted / deprecated / superseded]

## Date: [date]

## Decided by: [human / @pm]

## Context

[What is the issue that we're seeing that is motivating this decision?]

## Decision

[What is the change that we're proposing and/or doing?]

## Alternatives Considered

1. **[Option A]** — [pros] / [cons]
2. **[Option B]** — [pros] / [cons]

## Consequences

- [What becomes easier]
- [What becomes harder]
- [What we need to watch out for]
```

### General Doc

```markdown
# [Title]

## Purpose

[Why this document exists]

## Content

[Main content]

## References

- [links or related docs]
```

---

## 3. CREATE AND COMMIT

- [ ] Create the file at the appropriate location
- [ ] Ensure directory exists: `mkdir -p [dir]`
- [ ] Fill in all sections based on $ARGUMENTS and project context
- [ ] Read existing docs to maintain consistency

```bash
git add [file]
git commit -m "docs(project): add [type] for [subject]"
```

**Output:**

```
## Document Created
- File: [path]
- Type: [task spec / architecture / ADR / general]
- Status: ready for review
```
