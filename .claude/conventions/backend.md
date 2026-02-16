# Backend Conventions

## File Structure
```
supabase/
├── migrations/
│   ├── 20260215100000_create_profiles.sql
│   ├── 20260215100100_create_posts.sql
│   └── ...
├── functions/
│   ├── hello-world/
│   │   └── index.ts
│   └── process-webhook/
│       └── index.ts
├── seed.sql                # development seed data
└── config.toml             # Supabase local config
types/
└── database.ts             # auto-generated: supabase gen types typescript
```

## Migration Pattern
```sql
-- migrations/20260215100000_create_profiles.sql

-- Create table
CREATE TABLE IF NOT EXISTS public.profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  username TEXT NOT NULL,
  avatar_url TEXT,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Enable RLS
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;

-- RLS Policies
CREATE POLICY "Users can read own profile"
  ON public.profiles FOR SELECT
  USING (auth.uid() = id);

CREATE POLICY "Users can update own profile"
  ON public.profiles FOR UPDATE
  USING (auth.uid() = id)
  WITH CHECK (auth.uid() = id);

CREATE POLICY "Users can insert own profile"
  ON public.profiles FOR INSERT
  WITH CHECK (auth.uid() = id);

-- Indexes
CREATE INDEX IF NOT EXISTS idx_profiles_username ON public.profiles(username);

-- Updated_at trigger
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = now();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER profiles_updated_at
  BEFORE UPDATE ON public.profiles
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();
```

## Edge Function Pattern
```typescript
// functions/hello-world/index.ts
import { serve } from "https://deno.land/std@0.168.0/http/server.ts";
import { createClient } from "https://esm.sh/@supabase/supabase-js@2";

serve(async (req: Request) => {
  try {
    // Auth check
    const authHeader = req.headers.get("Authorization");
    if (!authHeader) {
      return new Response(JSON.stringify({ error: "Missing auth" }), {
        status: 401,
        headers: { "Content-Type": "application/json" },
      });
    }

    // Create Supabase client with user's token
    const supabase = createClient(
      Deno.env.get("SUPABASE_URL")!,
      Deno.env.get("SUPABASE_ANON_KEY")!,
      { global: { headers: { Authorization: authHeader } } }
    );

    // Business logic here

    return new Response(JSON.stringify({ data: "ok" }), {
      status: 200,
      headers: { "Content-Type": "application/json" },
    });
  } catch (error) {
    return new Response(JSON.stringify({ error: error.message }), {
      status: 500,
      headers: { "Content-Type": "application/json" },
    });
  }
});
```

## Rules
- Every table has RLS enabled — no exceptions
- Use `TIMESTAMPTZ` not `TIMESTAMP` for all dates
- Use `gen_random_uuid()` for UUID defaults
- Add `created_at` and `updated_at` to every table
- Add `updated_at` trigger to every table
- Use `IF NOT EXISTS` / `IF EXISTS` guards in migrations
- Never modify an existing migration — create a new one
- Add indexes on foreign keys and frequently queried columns
- After schema changes: `supabase gen types typescript --local > types/database.ts`
- Edge Functions: always validate input, always check auth, always return proper error codes
