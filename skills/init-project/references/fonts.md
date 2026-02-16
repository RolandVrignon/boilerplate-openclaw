# Font Pairings

Choose a pairing that matches the project's personality. Always use **two fonts**: one expressive for headlines, one clean for body text.

## Pairings by Vibe

### 🔥 Bold / Startup
- **Headlines**: Space Grotesk — geometric, techy, punchy
- **Body**: Inter — neutral, ultra-readable

### 🎨 Creative / Fun
- **Headlines**: Clash Display — sharp, characterful
- **Body**: Satoshi — soft geometric, friendly

### 💼 Pro / SaaS
- **Headlines**: Cabinet Grotesk — modern, confident
- **Body**: General Sans — clean, professional

### 🧪 Tech / Dev Tool
- **Headlines**: JetBrains Mono — monospace with personality
- **Body**: Geist Sans — Vercel's font, clean and technical

### 🪶 Elegant / Editorial
- **Headlines**: Playfair Display — serif, sophisticated
- **Body**: Source Sans 3 — humanist, comfortable reading

### 🚀 Modern / Minimal
- **Headlines**: Geist Sans (bold weights) — sharp, minimal
- **Body**: Geist Sans (regular weights) — consistent, clean

### 🎮 Playful / Consumer
- **Headlines**: Bricolage Grotesque — quirky, characterful
- **Body**: DM Sans — geometric, approachable

### 🏗️ Industrial / No-BS
- **Headlines**: Instrument Sans — condensed, strong
- **Body**: IBM Plex Sans — structured, reliable

## Implementation (Next.js)

### Google Fonts (next/font)

```typescript
// src/app/layout.tsx
import { Space_Grotesk, Inter } from "next/font/google";

const heading = Space_Grotesk({
  subsets: ["latin"],
  variable: "--font-heading",
});

const body = Inter({
  subsets: ["latin"],
  variable: "--font-body",
});

export default function RootLayout({ children }) {
  return (
    <html className={`${heading.variable} ${body.variable}`}>
      <body className="font-body">{children}</body>
    </html>
  );
}
```

### Tailwind v4 config

```css
/* globals.css */
@theme {
  --font-heading: var(--font-heading);
  --font-body: var(--font-body);
}
```

Usage:
```html
<h1 class="font-heading text-4xl font-bold">Headline</h1>
<p class="font-body text-base">Body text here</p>
```

### Fontsource (for non-Google fonts like Clash Display, Satoshi)

```bash
pnpm add @fontsource-variable/clash-display @fontsource-variable/satoshi
```

```typescript
import "@fontsource-variable/clash-display";
import "@fontsource-variable/satoshi";
```

## Rules

- Always pick a pairing that matches the project's personality
- Headlines: bold, characterful, expressive
- Body: readable, neutral, comfortable for long text
- Propose 2-3 pairings to the user during `/init-project`, let them pick
- Default fallback if no preference: Space Grotesk + Inter
