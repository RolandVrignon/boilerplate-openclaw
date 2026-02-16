# PR Review: $ARGUMENTS

Review the branch or PR specified in $ARGUMENTS. Follow the full checklist before giving a verdict.

---

## 1. CONTEXT

- [ ] Identify the branch: `$ARGUMENTS` (e.g., `feat/front-001` or `feat/back-003`)
- [ ] Read the task spec: `specs/TASK-ID.md`
- [ ] Read the API contract if applicable: `specs/contracts/`
- [ ] Read the KANBAN entry for this task
- [ ] Understand acceptance criteria before looking at code

```bash
git diff dev...$BRANCH --stat
git log dev...$BRANCH --oneline
```

---

## 2. CODE REVIEW CHECKLIST

### General

- [ ] Changes match the task spec and acceptance criteria
- [ ] No unrelated changes or scope creep
- [ ] Commit messages follow convention: `<type>(<scope>): TASK-ID description`
- [ ] Branch name follows convention: `feat/<scope>-XXX` or `fix/<scope>-XXX`
- [ ] No hardcoded secrets, API keys, or sensitive data
- [ ] No `console.log` left in code (except intentional debug flags)
- [ ] No `any` types unless justified with comment

### Frontend (if `feat/front-*` or `fix/front-*`)

- [ ] TypeScript strict — no type errors
- [ ] Uses shared types from `types/database.ts` — never redefines
- [ ] Tailwind/NativeWind only — no custom CSS
- [ ] Loading state handled (skeleton/spinner)
- [ ] Error state handled (user-friendly message + retry)
- [ ] Empty state handled (illustration + CTA)
- [ ] Components are functional, one per file
- [ ] File names: kebab-case
- [ ] No direct Supabase queries in components (use hooks or stores)

### Backend (if `feat/back-*` or `fix/back-*`)

- [ ] Every new table has RLS enabled
- [ ] RLS policies cover SELECT, INSERT, UPDATE, DELETE as needed
- [ ] Migration uses `IF NOT EXISTS` / `IF EXISTS` guards
- [ ] Migration is incremental — no modification of existing migrations
- [ ] `created_at` and `updated_at` on every table
- [ ] `updated_at` trigger present
- [ ] Types regenerated: `types/database.ts` is up to date
- [ ] Edge Functions: input validated, auth checked, proper error codes
- [ ] Indexes on foreign keys and frequently queried columns

### EPCT Compliance

- [ ] Agent followed EPCT workflow (evidence of Explore → Plan → Code → Test)
- [ ] Plan was approved before coding (check conversation/commit history)
- [ ] Test results documented

---

## 3. AUTOMATED CHECKS

Run and report results:

```bash
npx tsc --noEmit                    # Type check
npx eslint . --ext .ts,.tsx          # Lint
npm test 2>/dev/null || echo "No test suite"  # Tests if they exist
```

For backend changes:

```bash
supabase db reset                    # Migrations run cleanly
supabase gen types typescript --local | diff - types/database.ts  # Types up to date
```

---

## 4. VERDICT

After completing all checks, output a structured review:

```
## PR Review: TASK-ID — [Title]
Branch: [branch name]
Agent: @[agent]
Spec: specs/[task].md

### Checklist Results
- General: X/X passed
- [Scope] specific: X/X passed
- EPCT compliance: X/X passed
- Automated checks: X/X passed

### Issues Found
1. [BLOCKER/WARNING] Description — file:line
2. [BLOCKER/WARNING] Description — file:line

### Verdict: APPROVED / CHANGES_REQUESTED

### Required Changes (if CHANGES_REQUESTED)
1. [ ] Fix description
2. [ ] Fix description
```

---

## 5. POST-REVIEW ACTIONS

### If APPROVED:

```bash
git checkout dev
git merge --no-ff $BRANCH -m "merge: TASK-ID [title]"
git push origin dev
```

Then update KANBAN.md: move task from IN REVIEW → DONE
Then update CHANGELOG.md: add entry under [Unreleased]

```bash
git add KANBAN.md CHANGELOG.md
git commit -m "docs(qa): TASK-ID update KANBAN and CHANGELOG"
git push origin dev
```

### If CHANGES_REQUESTED:

Do NOT merge. Report required changes. The responsible agent will fix and re-submit.
