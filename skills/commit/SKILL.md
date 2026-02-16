---
name: commit
description: "Atomic conventional commits with auto-push. Use when the user says /commit, wants to commit changes, push code, or save progress."
---

# Commit

Atomic conventional commit workflow for the current project repo.

## Rules

- Commit messages in English, conventional commit format: `type(scope): description`
- Types: `feat`, `fix`, `refactor`, `style`, `docs`, `test`, `perf`, `ci`, `build`, `chore`
- Scopes: `front`, `back`, `qa`, `content`, `project` (or omit for general)
- Never add co-author trailers or signed-off-by lines
- One logical change per commit (atomic)
- Always push after committing

## Workflow

### 1. Analyze changes

```bash
git status
git diff --stat
git diff --staged --stat
git diff
git diff --staged
```

If nothing to commit, report and stop.

### 2. Group atomic commits

Split unrelated changes into atomic groups. Each group = one commit. Present proposed groups:

```
Proposed commits:
  1. feat(front): FRONT-001 add login screen with email/password
     Files: src/app/(auth)/login.tsx, src/components/features/login-form.tsx
  2. fix(back): BACK-003 correct RLS policy on profiles table
     Files: supabase/migrations/20260216_fix_profiles_rls.sql
  3. docs(project): update KANBAN and CHANGELOG
     Files: KANBAN.md, CHANGELOG.md
```

Wait for user approval before proceeding.

### 3. Commit and push

For each approved group:

```bash
git add <files>
git commit -m "type(scope): description"
```

After all commits:

```bash
git push origin HEAD
```

### 4. Summary

Show final summary with commit hashes, branch name, and files changed.
