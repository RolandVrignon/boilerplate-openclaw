---
name: init-project
description: "Initialize a new project from scratch. Use when the user says /init, /new-project, wants to start a new side project, or bootstrap a new app. Detects project type (fullstack Next.js or separate Next.js + NestJS) and scaffolds everything."
---

# Init Project

Bootstrap a new project following Hervé's stack and best practices.

## Step 1: Determine project type

Ask the user:
1. **Project name** (kebab-case)
2. **Type**:
   - **`fullstack`** — Next.js with API routes (front + back merged)
   - **`split`** — Next.js frontend + NestJS backend (2 separate repos)
3. **Needs a database?** (default: yes → Docker PostgreSQL + Prisma)
4. **Brief description** (one line for README/package.json)

## Step 2: Scaffold based on type

### Common to all projects

- Package manager: **pnpm**
- Language: **TypeScript** (strict mode)
- Formatting: **Prettier + ESLint**
- Styling: **Tailwind CSS v4** + **shadcn/ui**
- Fonts: **dual font** — expressive headline + clean body (see `references/fonts.md` for pairings, propose 2-3 options to user)
- Theme: **light mode** default
- State: **zustand**
- Forms: **react-hook-form** + **zod**
- Auth: **NextAuth (Auth.js)**
- ORM: **Prisma** (when DB needed)
- DB dev: **Docker Compose** with PostgreSQL 16
- CI: **GitHub Actions**
- Deploy: **Coolify** (self-hosted)
- Code language: **English**
- Structure: **feature-based**

### Type: fullstack (Next.js)

Create a single repo:

```
<project-name>/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (auth)/
│   │   │   ├── login/page.tsx
│   │   │   └── register/page.tsx
│   │   ├── (dashboard)/
│   │   │   ├── layout.tsx
│   │   │   └── page.tsx
│   │   ├── api/                # API routes
│   │   │   └── auth/[...nextauth]/route.ts
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
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
│       └── index.ts
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
├── public/
├── docker-compose.yml
├── .env.example
├── .env.local                  # gitignored
├── .eslintrc.json
├── .prettierrc
├── .github/
│   └── workflows/
│       └── ci.yml
├── Dockerfile
├── Makefile
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── package.json
├── PRD.md
├── KANBAN.md
├── CHANGELOG.md
├── CLAUDE.md
└── README.md
```

### Type: split (Next.js + NestJS)

Create **two repos**:

**Frontend** (`<project-name>-front/`):
```
<project-name>-front/
├── src/
│   ├── app/                    # Next.js App Router
│   ├── features/               # Feature-based modules
│   ├── components/ui/          # shadcn/ui
│   ├── lib/
│   │   ├── api.ts              # API client (fetch wrapper or axios)
│   │   ├── auth.ts             # NextAuth config
│   │   └── utils.ts
│   └── types/
│       └── api.ts              # Shared API types (mirrored from back)
├── public/
├── .github/workflows/ci.yml
├── Dockerfile
├── Makefile
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

**Backend** (`<project-name>-back/`):
```
<project-name>-back/
├── src/
│   ├── app.module.ts
│   ├── main.ts
│   ├── features/               # Feature-based modules
│   │   └── auth/
│   │       ├── auth.module.ts
│   │       ├── auth.controller.ts
│   │       ├── auth.service.ts
│   │       └── dto/
│   ├── common/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   ├── decorators/
│   │   └── filters/
│   └── config/
│       └── configuration.ts
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
├── test/
├── docker-compose.yml
├── .env.example
├── .github/workflows/ci.yml
├── Dockerfile
├── Makefile
├── nest-cli.json
├── tsconfig.json
├── tsconfig.build.json
└── package.json
```

## Step 3: Key files content

### docker-compose.yml

```yaml
services:
  db:
    image: postgres:16-alpine
    restart: unless-stopped
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: ${PROJECT_NAME}
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### prisma/schema.prisma

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  image     String?
  createdAt DateTime @default(now()) @map("created_at")
  updatedAt DateTime @updatedAt @map("updated_at")

  @@map("users")
}
```

### .env.example

```
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/PROJECT_NAME?schema=public"
NEXTAUTH_SECRET="change-me"
NEXTAUTH_URL="http://localhost:3000"
```

### .github/workflows/ci.yml

```yaml
name: CI
on:
  push:
    branches: [main, dev]
  pull_request:
    branches: [main, dev]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
        with:
          version: 9
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm
      - run: pnpm install --frozen-lockfile
      - run: pnpm typecheck
      - run: pnpm lint
      - run: pnpm build
```

### Makefile (use makefile skill to generate — invoke it automatically)

### Dockerfile (Next.js)

```dockerfile
FROM node:20-alpine AS base
RUN corepack enable && corepack prepare pnpm@9 --activate

FROM base AS deps
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN pnpm build

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/public ./public
EXPOSE 3000
CMD ["node", "server.js"]
```

### Dockerfile (NestJS)

```dockerfile
FROM node:20-alpine AS base
RUN corepack enable && corepack prepare pnpm@9 --activate

FROM base AS deps
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN pnpm build

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package.json ./
EXPOSE 3001
CMD ["node", "dist/main.js"]
```

### package.json scripts (Next.js)

```json
{
  "scripts": {
    "dev": "next dev --turbopack",
    "build": "next build",
    "start": "next start",
    "lint": "eslint . --ext .ts,.tsx",
    "typecheck": "tsc --noEmit",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "db:generate": "prisma generate",
    "db:migrate": "prisma migrate dev",
    "db:push": "prisma db push",
    "db:seed": "prisma db seed",
    "db:studio": "prisma studio",
    "postinstall": "prisma generate"
  }
}
```

### lib/utils.ts

```typescript
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

### lib/prisma.ts

```typescript
import { PrismaClient } from "@prisma/client";

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient };

export const prisma = globalForPrisma.prisma ?? new PrismaClient();

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma;
```

## Step 4: Initialize

1. Create directory structure
2. Write all config files
3. Run `pnpm init` / `pnpm create next-app` or scaffold manually
4. Run `pnpm install`
5. Initialize git: `git init && git add -A && git commit -m "chore: initial project setup"`
6. Create GitHub repo: `gh repo create <name> --private --source .`
7. Push: `git push -u origin main`
8. Create `dev` branch: `git checkout -b dev && git push -u origin dev`
9. Invoke `/makefile` skill to generate Makefile
10. Start Docker: `docker compose up -d`
11. Run `pnpm db:migrate` to apply initial schema

## Step 5: Report

```
✅ Project <name> initialized!
- Type: fullstack / split
- Repos: <list>
- Stack: Next.js + Prisma + PostgreSQL + shadcn/ui + Tailwind v4
- DB: PostgreSQL running on Docker (localhost:5432)
- Run `make dev` to start developing
```
