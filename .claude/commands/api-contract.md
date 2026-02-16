# API Contract: $ARGUMENTS

Generate an API contract for the feature described in $ARGUMENTS. This contract defines the exact interface between frontend and backend agents.

---

## 1. UNDERSTAND THE FEATURE

- [ ] Read the task spec in `specs/` related to this feature
- [ ] Read `architecture.md` for system context
- [ ] Identify all data entities involved
- [ ] Identify all user actions that require backend interaction
- [ ] Check existing contracts in `specs/contracts/` to avoid duplication

---

## 2. GENERATE CONTRACT

Create file: `specs/contracts/$FEATURE_NAME.md`

Use this structure:

````markdown
# Contract: [Feature Name]

Created by: @pm
Date: [date]
Related tasks: FRONT-XXX, BACK-XXX

## Tables

### [table_name]

| Column | Type | Nullable | Default           | Description |
| ------ | ---- | -------- | ----------------- | ----------- |
| id     | UUID | NO       | gen_random_uuid() | Primary key |
| ...    | ...  | ...      | ...               | ...         |

**Constraints:**

- FK: column → other_table(column) ON DELETE CASCADE
- UNIQUE: column
- CHECK: condition

### RLS Policies

| Policy             | Operation | Using                | With Check           |
| ------------------ | --------- | -------------------- | -------------------- |
| "Users read own"   | SELECT    | auth.uid() = user_id | —                    |
| "Users insert own" | INSERT    | —                    | auth.uid() = user_id |
| ...                | ...       | ...                  | ...                  |

## Shared Types

```typescript
// types/[feature].ts — to be placed in types/database.ts after generation

export interface [Entity] {
  id: string;
  // ...
  created_at: string;
  updated_at: string;
}

export interface [Entity]Insert {
  // omit id, created_at, updated_at
}

export interface [Entity]Update {
  // all fields optional except id
}
```
````

## API Calls

### [Action Name]

- **Method**: supabase.from('[table]').select/insert/update/delete
- **Auth required**: yes/no
- **Input**: [describe params]
- **Output**: [describe return shape]
- **Errors**: [list possible errors]
- **Used by**: FRONT-XXX

### [Another Action]

...

## Edge Functions (if needed)

### [Function Name]

- **Path**: /functions/v1/[name]
- **Method**: POST
- **Auth**: Bearer token required
- **Input body**: { field: type, ... }
- **Output**: { field: type, ... }
- **Errors**: 400 (validation), 401 (auth), 500 (server)
- **Used by**: FRONT-XXX

## Realtime Subscriptions (if needed)

### [Subscription Name]

- **Table**: [table_name]
- **Event**: INSERT / UPDATE / DELETE / \*
- **Filter**: column=eq.value
- **Used by**: FRONT-XXX

````

---

## 3. VERIFY CONTRACT

- [ ] All frontend tasks have the API calls they need
- [ ] All backend tasks have tables and RLS defined
- [ ] Types are consistent between tables and TypeScript interfaces
- [ ] No missing error cases
- [ ] No circular dependencies

---

## 4. COMMIT

```bash
git add specs/contracts/$FEATURE_NAME.md
git commit -m "docs(project): add API contract for $FEATURE_NAME"
````

**Output:**

```
## Contract Generated
- File: specs/contracts/$FEATURE_NAME.md
- Tables: [list]
- API calls: [count]
- Edge Functions: [count or none]
- Realtime: [count or none]
- Ready for: BACK-XXX (implement), FRONT-XXX (consume)
```
