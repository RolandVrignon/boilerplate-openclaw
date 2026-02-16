# Frontend Conventions

## File Structure (Mobile — Expo)
```
src/
├── app/                    # expo-router pages
│   ├── (auth)/             # auth group layout
│   │   ├── login.tsx
│   │   └── register.tsx
│   ├── (tabs)/             # main tab layout
│   │   ├── _layout.tsx
│   │   ├── index.tsx
│   │   └── profile.tsx
│   └── _layout.tsx         # root layout
├── components/
│   ├── ui/                 # reusable UI primitives (Button, Input, Card)
│   └── features/           # feature-specific components
├── hooks/                  # custom hooks
├── lib/
│   ├── supabase.ts         # Supabase client init
│   └── utils.ts            # utility functions
├── stores/                 # zustand stores
└── constants/              # app constants (colors, config)
```

## File Structure (Web — Next.js)
```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   └── layout.tsx          # root layout
├── components/
│   ├── ui/                 # shadcn/ui components
│   └── features/
├── hooks/
├── lib/
│   ├── supabase/
│   │   ├── client.ts       # browser client
│   │   └── server.ts       # server client
│   └── utils.ts
├── stores/
└── constants/
```

## Component Pattern
```tsx
// components/features/user-avatar.tsx
import { View, Text } from "react-native"; // or from React for web
import { cn } from "@/lib/utils";

interface UserAvatarProps {
  url: string | null;
  size?: "sm" | "md" | "lg";
}

export function UserAvatar({ url, size = "md" }: UserAvatarProps) {
  // component logic
}
```

## Rules
- One component per file, named export (not default)
- Props interface defined in the same file, above the component
- Use `cn()` utility for conditional Tailwind classes
- All text in constants or i18n — no hardcoded strings in components
- Loading state: skeleton or spinner
- Error state: user-friendly message + retry action
- Empty state: illustration + description + CTA
