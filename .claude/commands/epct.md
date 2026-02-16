# EPCT: $ARGUMENTS

Follow the EPCT workflow strictly. **Do NOT skip steps.**

---

## 1. EXPLORE

Understand the codebase and the task before doing anything:

- [ ] Read this project's `CLAUDE.md` for global rules
- [ ] Read the task spec in `specs/TASK-ID.md`
- [ ] Read the API contract in `specs/contracts/` if the task involves front↔back interaction
- [ ] Read shared types in `types/` if applicable
- [ ] Explore existing code patterns related to the task
- [ ] Identify all files that will be affected
- [ ] Check for similar implementations to follow as reference

**Output after EXPLORE:**
```
## Exploration Summary
- Task: [TASK-ID] [Title]
- Spec: specs/[TASK-ID].md
- Contract: specs/contracts/[name].md (if applicable)
- Shared types used: [list or N/A]
- Existing patterns found: [list]
- Files likely affected: [list]
- Dependencies status: [all met / blocked by TASK-ID]
```

---

## 2. PLAN

Create a detailed implementation plan:

- [ ] List all files to create or modify
- [ ] Define step-by-step implementation order
- [ ] Identify edge cases, error states, loading states
- [ ] Note any dependencies or prerequisites
- [ ] Estimate complexity (small / medium / large)

**Output after PLAN:**
```
## Implementation Plan

### Goal
[One sentence]

### Complexity: [small / medium / large]

### Files to change
- `path/file.ts` — [what changes]
- `path/new-file.ts` — [new file, purpose]

### Steps
1. ...
2. ...
3. ...

### Edge cases
- ...

### Error handling
- ...

### Risks / Uncertainties
- ...
```

**STOP HERE — Wait for approval before proceeding to CODE**

---

## 3. CODE

After approval, implement the solution:

- [ ] Create a new branch: `feat/[scope]-[TASK-ID]`
- [ ] Follow existing code patterns strictly
- [ ] Use shared types from `types/` — never redefine
- [ ] Handle loading, error, and empty states (frontend)
- [ ] Add RLS policies for every new table (backend)
- [ ] Add inline comments only for complex logic
- [ ] Make incremental, focused changes — one concern per commit

---

## 4. TEST

Verify the implementation works:

- [ ] Run type-check: `npx tsc --noEmit`
- [ ] Run lint: `npx eslint .`
- [ ] Run existing test suite if present
- [ ] Test edge cases identified in PLAN
- [ ] Verify no regressions in existing functionality
- [ ] Backend: verify migrations run cleanly (`supabase db reset`)
- [ ] Backend: verify RLS policies work (test as anon + authenticated)

**Output after TEST:**
```
## Test Results
- Type-check: pass/fail
- Lint: pass/fail
- Migrations: pass/fail/N/A
- RLS policies: pass/fail/N/A
- Edge cases tested: [list]
- Manual verification: [details]
- Regressions found: none / [details]
```

---

## 5. WRAP UP

- [ ] Commit with format: `<type>(<scope>): <TASK-ID> <description>`
- [ ] Push branch: `feat/[scope]-[TASK-ID]`
- [ ] Update `KANBAN.md`: move task to IN REVIEW
- [ ] Propose `CLAUDE.md` or `specs/` updates if needed

**Output after WRAP UP:**
```
## Summary
- Task: [TASK-ID] [Title]
- Branch: feat/[scope]-[TASK-ID]
- Commits: [count]
- Files changed: [count]
- Ready for QA review
```
