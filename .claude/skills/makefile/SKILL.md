---
name: makefile
description: "Generate a Makefile at project root. Use when initializing a new project, when the user says /makefile, or when a project needs a Makefile. Adapts to the detected stack (Next.js, Expo, Supabase, monorepo, etc.)."
---

# Makefile Generator

Generate a comprehensive Makefile at the project root, adapted to the detected tech stack.

## Workflow

### 1. Detect stack

Check for:
- `package.json` → Node.js project (read scripts section)
- `next.config.*` → Next.js
- `app.json` or `expo` in package.json → Expo / React Native
- `supabase/config.toml` → Supabase
- `docker-compose.yml` → Docker
- `pyproject.toml` / `requirements.txt` → Python
- `Cargo.toml` → Rust
- `go.mod` → Go

### 2. Generate Makefile

Always include these core targets:

```makefile
.PHONY: help dev build clean install lint typecheck test

help: ## Show available commands
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

install: ## Install dependencies
dev: ## Start development server
build: ## Build for production
clean: ## Clean build artifacts and caches
lint: ## Run linter
typecheck: ## Run type checker
test: ## Run tests
```

Then add stack-specific targets:

#### Next.js
```makefile
dev: ## Start Next.js dev server
	npm run dev

build: ## Build for production
	npm run build

start: ## Start production server
	npm run start

lint: ## Run ESLint
	npx eslint . --ext .ts,.tsx

typecheck: ## Run TypeScript check
	npx tsc --noEmit
```

#### Expo / React Native
```makefile
dev: ## Start Expo dev server
	npx expo start

dev-ios: ## Start on iOS simulator
	npx expo run:ios

dev-android: ## Start on Android emulator
	npx expo run:android

build-ios: ## Build iOS with EAS
	npx eas build --platform ios

build-android: ## Build Android with EAS
	npx eas build --platform android

build: build-ios build-android ## Build all platforms
```

#### Supabase
```makefile
db-start: ## Start local Supabase
	supabase start

db-stop: ## Stop local Supabase
	supabase stop

db-reset: ## Reset database (run all migrations)
	supabase db reset

db-migrate: ## Create a new migration
	@read -p "Migration name: " name; supabase migration new $$name

db-types: ## Regenerate TypeScript types
	supabase gen types typescript --local > types/database.ts

db-push: ## Push migrations to remote
	supabase db push

db-seed: ## Seed database
	psql $$(supabase status | grep 'DB URL' | awk '{print $$NF}') -f supabase/seed.sql
```

#### Docker
```makefile
up: ## Start all containers
	docker compose up -d

down: ## Stop all containers
	docker compose down

logs: ## Tail container logs
	docker compose logs -f

rebuild: ## Rebuild and restart containers
	docker compose up -d --build
```

#### Always add utility targets
```makefile
clean: ## Clean build artifacts
	rm -rf .next node_modules/.cache dist .expo

fresh: clean install ## Clean install from scratch

env: ## Copy .env.example to .env.local
	cp -n .env.example .env.local 2>/dev/null || echo ".env.local already exists"
```

### 3. Rules

- Use tabs for indentation (Makefile requirement)
- Every target MUST have a `## description` comment for `make help`
- `.PHONY` all targets that don't produce files
- Group targets by category with comment headers
- Default target is `help` so `make` alone shows available commands
- Use `@` prefix for commands that shouldn't echo
- Keep it practical — only include targets that will actually be used

### 4. Write and report

Write `Makefile` at project root. Show the user what was generated and how to use it:

```
✅ Makefile generated with X commands
Run `make` or `make help` to see all available commands.
```
