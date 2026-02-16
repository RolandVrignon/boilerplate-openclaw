# Update Docs: $ARGUMENTS

Update project documentation to reflect recent changes. $ARGUMENTS can be:

- A TASK-ID → update docs related to that task
- "all" → full documentation sync
- A file path → update docs affected by changes in that file
- empty → auto-detect what needs updating from recent commits

---

## 1. DETECT WHAT CHANGED

If $ARGUMENTS is empty or "all", scan recent changes:

```bash
# Recent commits not yet reflected in docs
git log --oneline -20

# Files changed since last docs update
git log --all --format="" --name-only -- '*.ts' '*.tsx' '*.sql' | sort -u | head -30

# Last docs commit
git log --oneline --all -- 'KANBAN.md' 'CHANGELOG.md' 'specs/' 'architecture.md' 'README.md' | head -5
```

---

## 2. DOCUMENTATION CHECKLIST

For each change detected, check if these documents need updating:

### KANBAN.md

- [ ] Task statuses match reality (branch merged = DONE, PR open = IN REVIEW)
- [ ] No tasks stuck in wrong status
- [ ] New tasks added if scope expanded
- [ ] Dependencies still accurate

### CHANGELOG.md

- [ ] All merged features have entries
- [ ] All merged fixes have entries
- [ ] Entries are under correct version heading
- [ ] Format follows Keep a Changelog

### architecture.md

- [ ] New tables reflected in data model
- [ ] New screens/pages added to screen list
- [ ] New Edge Functions documented
- [ ] Auth flow still accurate
- [ ] External services list up to date

### specs/

- [ ] Completed task specs marked as done
- [ ] Spec acceptance criteria match implementation
- [ ] No orphan specs (task cancelled but spec remains)

### specs/contracts/

- [ ] Contract tables match actual migrations
- [ ] Contract types match `types/database.ts`
- [ ] Contract API calls match actual implementation
- [ ] No missing contracts for new features

### types/database.ts

- [ ] Run `supabase gen types typescript --local > types/database.ts`
- [ ] Compare with existing: any new tables/columns?
- [ ] Frontend code uses latest types

### README.md

- [ ] Setup instructions still work
- [ ] Environment variables list complete
- [ ] Available scripts documented
- [ ] Tech stack section accurate

---

## 3. APPLY UPDATES

For each document that needs changes:

- [ ] Make the minimal necessary update
- [ ] Preserve existing formatting and style
- [ ] Don't remove useful information
- [ ] Add dates to entries where relevant

---

## 4. VERIFY CONSISTENCY

Cross-check:

- [ ] KANBAN task count matches actual branch/PR count
- [ ] CHANGELOG entries match KANBAN DONE tasks
- [ ] Architecture doc matches actual file structure
- [ ] Contract types match `types/database.ts`
- [ ] All acceptance criteria in specs are testable

---

## 5. COMMIT

```bash
git add KANBAN.md CHANGELOG.md architecture.md specs/ README.md types/
git commit -m "docs(qa): sync documentation with latest changes"
```

**Output:**

```
## Docs Updated
- Files modified: [list]
- KANBAN: [X tasks updated]
- CHANGELOG: [X entries added]
- Architecture: [updated / no changes]
- Contracts: [updated / no changes]
- Types: [regenerated / up to date]
- README: [updated / no changes]

## Inconsistencies Found
- [any remaining issues that need human decision]
```
