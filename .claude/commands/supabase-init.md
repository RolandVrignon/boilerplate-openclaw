# Supabase Init: $ARGUMENTS

Initialize or connect a Supabase project for this repo. Follow every step in order.

---

## 1. CHECK PREREQUISITES

- [ ] Verify `supabase` CLI is installed: `supabase --version`
- [ ] Verify logged in: `supabase projects list`
- [ ] Read `CLAUDE.md` for project conventions
- [ ] Read `.claude/conventions/backend.md` for Supabase patterns

If not installed:

```bash
npm install -g supabase
supabase login
```

---

## 2. DECIDE: NEW OR EXISTING PROJECT

If $ARGUMENTS contains a project ref or URL → link to existing project.
If $ARGUMENTS says "new" or is a project name → create new project.

### New Project

```bash
supabase init
supabase projects create "$PROJECT_NAME" --region eu-west-1
supabase link --project-ref <ref_from_output>
```

### Existing Project

```bash
supabase init
supabase link --project-ref <ref>
```

---

## 3. CREATE BASE STRUCTURE

Create the following directories:

```
supabase/
├── migrations/
├── functions/
└── seed.sql
types/
└── (will be generated)
```

---

## 4. INITIAL MIGRATION: Auth Trigger + Profiles

Create the first migration — this is standard for every project:

```bash
supabase migration new create_profiles
```

Write migration following the pattern in `.claude/conventions/backend.md`:

- `profiles` table linked to `auth.users`
- RLS enabled with policies for own-data access
- `updated_at` trigger
- Index on commonly queried fields

---

## 5. GENERATE TYPES

```bash
supabase gen types typescript --local > types/database.ts
```

---

## 6. CREATE SUPABASE CLIENT

### Mobile (React Native)

Create `src/lib/supabase.ts`:

```typescript
import "react-native-url-polyfill/auto";
import AsyncStorage from "@react-native-async-storage/async-storage";
import { createClient } from "@supabase/supabase-js";
import { Database } from "@/types/database";

const supabaseUrl = process.env.EXPO_PUBLIC_SUPABASE_URL!;
const supabaseAnonKey = process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!;

export const supabase = createClient<Database>(supabaseUrl, supabaseAnonKey, {
  auth: {
    storage: AsyncStorage,
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: false,
  },
});
```

### Web (Next.js)

Create `src/lib/supabase/client.ts` (browser) and `src/lib/supabase/server.ts` (server).

---

## 7. ENV FILE

Create `.env.local` (gitignored):

```
# Mobile
EXPO_PUBLIC_SUPABASE_URL=https://<ref>.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=<anon_key>

# Web
NEXT_PUBLIC_SUPABASE_URL=https://<ref>.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=<anon_key>
```

Add to `.env.example` (committed, no values):

```
EXPO_PUBLIC_SUPABASE_URL=
EXPO_PUBLIC_SUPABASE_ANON_KEY=
```

---

## 8. VERIFY

- [ ] `supabase db reset` runs without errors
- [ ] `supabase gen types typescript --local` generates valid types
- [ ] Supabase client imports correctly
- [ ] `.env.local` is in `.gitignore`

---

## 9. COMMIT

```bash
git add -A
git commit -m "chore(back): initialize Supabase project with profiles migration"
```

**Output:**

```
## Supabase Init Complete
- Project: [name] ([ref])
- Region: [region]
- Migration: create_profiles
- Types: types/database.ts generated
- Client: src/lib/supabase.ts created
- Env: .env.local configured
```
