# Project Rules

## Overview
This project is built and maintained by an AI agent team managed via OpenClaw. Every agent MUST read and follow these rules before starting any work.

## Owner Preferences

### Stack Defaults
- **Package manager**: pnpm (always)
- **Language**: TypeScript strict mode, code in English
- **ORM**: Prisma
- **Auth**: NextAuth (Auth.js)
- **State**: zustand
- **Forms**: react-hook-form + zod
- **Styling**: Tailwind CSS v4 + shadcn/ui — NO custom CSS
- **Fonts**: dual font system — expressive for headlines, clean for body text (picked per project)
- **Theme**: light mode default
- **DB**: Docker PostgreSQL 16 (always for projects needing a DB)
- **CI/CD**: GitHub Actions
- **Deploy**: Coolify (self-hosted)
- **Structure**: feature-based (`features/auth/`, `features/dashboard/`)

### Project Types
- **Fullstack**: Next.js (App Router) with API routes — when front + back can be merged
- **Split**: Next.js frontend + NestJS backend in 2 separate repos — when front/back need separation

### Architecture Decisions
- Feature-based folder structure (not layer-based)
- Docker Compose for local dev (PostgreSQL)
- Prisma for all DB interactions
- Makefile at project root for common commands (`make dev`, `make build`, etc.)
- `.env.example` committed, `.env.local` gitignored

## Tech Stack

### Frontend (Next.js)
- Next.js + TypeScript (App Router, Turbopack)
- Styling: Tailwind CSS v4 — MANDATORY
- UI Components: shadcn/ui
- State: zustand
- Forms: react-hook-form + zod
- Auth: NextAuth (Auth.js)
- Fonts: dual (headline + body, chosen per project)
- Deployment: Coolify (Docker)

### Backend (NestJS — split projects only)
- NestJS + TypeScript
- ORM: Prisma
- DB: PostgreSQL (Docker)
- Validation: class-validator + class-transformer
- Deployment: Coolify (Docker)

### Backend (Next.js API routes — fullstack projects)
- Next.js API routes / Server Actions
- ORM: Prisma
- DB: PostgreSQL (Docker)

## Code Conventions
- TypeScript strict mode everywhere — no `any` unless justified with a comment
- Absolute imports configured in tsconfig (`@/`)
- No custom CSS — Tailwind only
- All components are functional (no class components)
- One component per file, named exports (not default)
- File names: kebab-case for files, PascalCase for components
- Code and variables in English
- Feature-based structure: `features/<name>/components/`, `features/<name>/hooks/`, etc.

## Git Conventions

### Branches
```
main                    ← production, protected
dev                     ← integration branch
feat/front-XXX          ← frontend feature branches
feat/back-XXX           ← backend feature branches
fix/front-XXX           ← frontend bugfixes
fix/back-XXX            ← backend bugfixes
```

### Commits
Format: `<type>(<scope>): <TASK-ID> <description>`

Types: feat, fix, docs, style, refactor, test, chore, perf, ci, build
Scopes: front, back, qa, content, project

Examples:
```
feat(front): FRONT-001 add login screen with NextAuth
feat(back): BACK-003 create profiles table with Prisma
fix(front): FRONT-001 fix keyboard overlap on login form
docs(qa): update KANBAN and CHANGELOG for BACK-003
```

### Branch Rules
- Agents work ONLY on their own branches
- No direct commits to `main` or `dev`
- All merges go through QA review
- Dev-front branches: `feat/front-*` or `fix/front-*`
- Dev-back branches: `feat/back-*` or `fix/back-*`

## Project Structure (Fullstack Next.js)
```
├── src/
│   ├── app/                    # Next.js App Router
│   ├── features/               # Feature-based modules
│   │   └── auth/
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── stores/
│   │       └── types.ts
│   ├── components/
│   │   └── ui/                 # shadcn/ui components
│   ├── lib/
│   │   ├── prisma.ts           # Prisma client singleton
│   │   ├── auth.ts             # NextAuth config
│   │   └── utils.ts            # cn() helper
│   └── types/
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
├── skills/                     # Agent skills
├── specs/
│   ├── contracts/              # API contracts (front↔back)
│   └── *.md                    # Task specs
├── docker-compose.yml
├── Dockerfile
├── Makefile
├── .github/workflows/ci.yml
├── PRD.md
├── KANBAN.md
├── CHANGELOG.md
├── CLAUDE.md
└── README.md
```

## Workflow
1. Human creates PRD.md with the product idea
2. PM reads PRD.md → produces architecture.md, KANBAN.md, specs/, specs/contracts/
3. Human validates PM output
4. Dev agents execute tasks following EPCT workflow (.claude/commands/epct.md)
5. QA reviews, merges, updates KANBAN.md
6. Content agent generates marketing assets when MVP is stable

## Skills
Available skills in `skills/`:
- `/commit` — atomic conventional commits + push
- `/pr` — create GitHub pull requests
- `/makefile` — generate project Makefile
- `/init-project` — bootstrap new project from scratch
- `/skill-creator` — create new skills
