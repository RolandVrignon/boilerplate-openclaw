---
name: pr
description: "Create a pull request on GitHub. Use when the user says /pr, wants to create a PR, submit for review, or merge a feature branch."
---

# Pull Request

Create a GitHub Pull Request for the current branch.

## Workflow

### 1. Gather context

```bash
git branch --show-current
git log dev..HEAD --oneline 2>/dev/null || git log main..HEAD --oneline
```

Determine:
- **Source branch**: current branch
- **Target branch**: `dev` by default, `main` if explicitly requested or if current branch is `dev`
- **Commits**: list of commits to include

If on `main` or `dev` with no feature branch, warn and stop.

### 2. Build PR content

- **Title**: derive from branch name and commits (e.g., `feat(front): FRONT-001 add login screen`)
- **Body**: auto-generate from commits + task spec if available in `specs/`
  - Summary of changes
  - Related task IDs
  - Checklist: type-check, lint, tested, KANBAN updated

Template:

```markdown
## Summary
[What this PR does]

## Related Tasks
- TASK-ID: [title]

## Changes
- [list of key changes]

## Checklist
- [ ] Type-check passes (`npx tsc --noEmit`)
- [ ] Lint passes (`npx eslint .`)
- [ ] KANBAN.md updated (task → IN REVIEW)
- [ ] Tested locally
```

### 3. Ensure pushed

```bash
git push origin HEAD
```

### 4. Create PR

```bash
gh pr create --base <target> --title "<title>" --body "<body>"
```

### 5. Report

Show PR URL, title, and base branch. Update KANBAN.md to move the task to IN REVIEW if applicable.
