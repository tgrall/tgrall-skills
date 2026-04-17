# Next.js stack guide

Opinionated defaults and conventions for the MVP architecture.

## Next.js App Router

Use the App Router (not Pages Router). Key conventions:

- All routes live under `src/app/[locale]/`
- Use route groups `(group)` to organize related routes without affecting the URL
- Use `layout.tsx` at each level for shared UI
- Use `loading.tsx` for streaming/suspense boundaries
- Use `error.tsx` for route-level error boundaries
- Use `not-found.tsx` for 404 handling
- Prefer React Server Components by default; add `"use client"` only when the component needs interactivity, browser APIs, or hooks

## ShadCN/ui

ShadCN components are copied into the project, not installed as a dependency.

Setup:

```bash
pnpm dlx shadcn@latest init
```

Conventions:

- ShadCN components live in `src/components/ui/`
- Do not modify ShadCN components directly for feature logic — wrap them in feature-specific components
- Add new ShadCN components as needed:

```bash
pnpm dlx shadcn@latest add button card dialog
```

Commonly needed components for typical PRDs:

- `Button`, `Card`, `Dialog`, `DropdownMenu`, `Input`, `Label`, `Select`, `Tabs`, `Toast`, `Tooltip`, `Sheet` (mobile sidebar), `Badge`, `Checkbox`, `Table`

## Tailwind CSS

Use Tailwind CSS 4+ with the default configuration extended for theming.

Key config patterns:

```ts
// tailwind.config.ts
import type { Config } from "tailwindcss";

const config: Config = {
  darkMode: "class",
  content: ["./src/**/*.{ts,tsx}"],
  theme: {
    extend: {
      colors: {
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
        },
        border: "hsl(var(--border))",
        ring: "hsl(var(--ring))",
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
};

export default config;
```

## State management

Default approach:

1. **Server state** — React Server Components fetch data directly; no client-side data fetching library needed for initial load
2. **URL state** — use `useSearchParams` and route params for state that should be shareable/bookmarkable
3. **Form state** — use React `useActionState` with Server Actions
4. **Client state** — use React context for simple cross-component state (theme, sidebar open/closed); escalate to Zustand only when the state graph is complex

Avoid Redux, MobX, or other heavy state management unless the PRD implies complex real-time collaborative features.

## Folder conventions

| Folder | Purpose | Import alias |
|---|---|---|
| `src/app/` | Routes and layouts | — |
| `src/components/ui/` | ShadCN primitives | `@/components/ui` |
| `src/components/shared/` | App-wide reusable components | `@/components/shared` |
| `src/components/[feature]/` | Feature-specific components | `@/components/[feature]` |
| `src/lib/` | Utilities, data access, services | `@/lib` |
| `src/hooks/` | Custom React hooks | `@/hooks` |
| `src/types/` | TypeScript type definitions | `@/types` |
| `src/config/` | App configuration (themes, i18n) | `@/config` |

## Import aliases

Configure in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

## Error handling

- Use `error.tsx` boundary files for route-level errors
- Use try/catch in Server Actions and Route Handlers
- Return structured error objects from API routes (see architecture template)
- Use toast notifications for user-facing errors via ShadCN Toast

## Performance defaults

- Use `next/image` for all images
- Use `next/font` for font loading
- Use dynamic imports (`next/dynamic`) for heavy client components
- Enable ISR or static generation where the PRD implies read-heavy, low-update pages
