# Git Conventions

## Branch Strategy
```
main        ← production, protected, only QA merges here
dev         ← integration branch, QA merges feature branches here
feat/*      ← feature branches (created by dev agents)
fix/*       ← bugfix branches (created by dev agents)
```

## Branch Naming
```
feat/front-001-login-screen
feat/back-001-auth-setup
fix/front-003-keyboard-overlap
fix/back-002-rls-policy
```

Format: `<type>/<scope>-<TASK-ID>-<short-description>`

## Commit Format
```
<type>(<scope>): <TASK-ID> <description>
```

### Types
- `feat` — new feature
- `fix` — bug fix
- `docs` — documentation only
- `style` — formatting, no code change
- `refactor` — code restructuring, no behavior change
- `test` — adding or fixing tests
- `chore` — tooling, config, dependencies

### Scopes
- `front` — frontend code
- `back` — backend code (migrations, functions)
- `qa` — QA updates (KANBAN, CHANGELOG)
- `content` — marketing content
- `project` — project-level changes (CLAUDE.md, configs)

### Examples
```
feat(front): FRONT-001 add login screen with email/password
feat(back): BACK-001 create users and profiles tables with RLS
fix(front): FRONT-001 fix keyboard covering input on small screens
docs(qa): BACK-001 update KANBAN, add CHANGELOG entry
refactor(front): FRONT-003 extract form validation into custom hook
chore(project): update dependencies
content: add App Store listing FR/EN
```

## Merge Rules
1. Dev agents push to their feature/fix branches
2. Dev agents update KANBAN.md → move task to IN REVIEW
3. QA agent reviews the branch
4. QA agent merges into `dev` if approved
5. When all P0 tasks are DONE, QA merges `dev` into `main`
6. No force pushes, no rebasing shared branches

## Agent Branch Permissions
- `@dev-front` → `feat/front-*`, `fix/front-*`
- `@dev-back` → `feat/back-*`, `fix/back-*`
- `@qa` → merges into `dev` and `main`
- `@pm` → no code branches, only updates specs/ and KANBAN.md
- `@content` → only `content/` folder
