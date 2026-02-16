# Context: $ARGUMENTS

Gather and display the current project state. Use this before starting any work to understand where the project stands.

If $ARGUMENTS specifies a scope (e.g., "frontend", "backend", "task FRONT-003"), focus on that area. Otherwise, give a full overview.

---

## 1. PROJECT OVERVIEW

- [ ] Read `CLAUDE.md` — note the tech stack and key rules
- [ ] Read `architecture.md` — note system design
- [ ] Read `PRD.md` — note product goals

Output:

```
## Project: [name from CLAUDE.md or architecture.md]
Stack: [tech stack summary]
Platform: mobile / web / both
Status: [planning / in-dev / testing / ready]
```

---

## 2. KANBAN STATUS

- [ ] Read `KANBAN.md`
- [ ] Count tasks by status

Output:

```
## Task Status
- Backlog: X tasks
- TODO: X tasks
- In Progress: X tasks (list them with agent assignments)
- In Review: X tasks (list them)
- Done: X tasks

## Blockers
- [TASK-ID] blocked by [TASK-ID] — reason
```

---

## 3. CODEBASE STATE

```bash
# Branch status
git branch -a
git status
git log --oneline -10

# File structure
find src/ -type f -name "*.ts" -o -name "*.tsx" 2>/dev/null | head -30
find supabase/migrations/ -type f 2>/dev/null | sort
ls types/ 2>/dev/null
ls specs/contracts/ 2>/dev/null
```

Output:

```
## Codebase
- Current branch: [branch]
- Open branches: [list]
- Recent commits: [last 5]
- Frontend files: [count]
- Migrations: [count]
- Contracts: [list]
- Types: [up to date / outdated / missing]
```

---

## 4. SCOPE-SPECIFIC CONTEXT (if $ARGUMENTS provided)

### If "frontend" or specific FRONT task:

- [ ] List all frontend components
- [ ] Check which screens/pages exist
- [ ] Check stores and hooks
- [ ] Check for TODO/FIXME comments
- [ ] Check type errors: `npx tsc --noEmit 2>&1 | tail -20`

### If "backend" or specific BACK task:

- [ ] List all migrations in order
- [ ] List Edge Functions
- [ ] Check RLS policies: read migration files
- [ ] Verify types are generated and current
- [ ] Check `supabase db reset` works

### If specific TASK-ID:

- [ ] Read the task spec in `specs/`
- [ ] Read the API contract if referenced
- [ ] Check if dependencies are met (other tasks DONE)
- [ ] Check if a branch already exists for this task
- [ ] Show relevant existing code

---

## 5. SUMMARY

Output a concise brief that any agent can read to get up to speed:

```
## Context Summary — [date]

**Project**: [name] — [one-line description]
**Platform**: [mobile/web/both]
**Progress**: X/Y tasks done (Z%)

**Current focus**: [what's being worked on now]
**Next up**: [what should be picked up next]
**Blockers**: [any blockers or none]

**Key files to read**:
- specs/[relevant].md
- specs/contracts/[relevant].md
- [any other critical files]
```
