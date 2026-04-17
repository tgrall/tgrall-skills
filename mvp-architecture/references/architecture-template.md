# MVP Architecture Document
## [Product or Feature Name]

| Field | Details |
|---|---|
| **Product Name** | [Name] |
| **Version** | [Version or milestone] |
| **Author** | [Owner or team] |
| **Date** | [YYYY-MM-DD] |
| **Status** | [Draft, In Review, Final] |
| **Language** | [Document language] |
| **PRD Reference** | [Link or filename of the companion PRD] |

---

## 1. Tech Stack Summary

| Layer | Technology | Version | Notes |
|---|---|---|---|
| **Framework** | Next.js (App Router) | 15+ | [Notes] |
| **UI Library** | ShadCN/ui | latest | [Notes] |
| **Styling** | Tailwind CSS | 4+ | [Notes] |
| **Theming** | CSS custom properties | — | Multi-color palettes, dark/light mode |
| **i18n** | next-intl | latest | [Notes] |
| **Unit/Integration Tests** | Vitest + React Testing Library | latest | [Notes] |
| **E2E Tests** | Playwright | latest | [Notes] |
| **State Management** | [Approach] | — | [Notes] |
| **Data Layer** | [Database/storage] | — | [Notes] |
| **Auth** | [Approach or N/A] | — | [Notes] |
| **Package Manager** | pnpm | latest | [Notes] |
| **Linting** | ESLint + Prettier | latest | [Notes] |
| **Deployment** | [Target platform] | — | [Notes] |

---

## 2. Project Structure

```
[product-name]/
├── public/
│   ├── locales/
│   │   ├── en/
│   │   │   └── common.json
│   │   └── [locale]/
│   │       └── common.json
│   └── [static assets]
├── src/
│   ├── app/
│   │   ├── [locale]/
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx
│   │   │   ├── [capability-route]/
│   │   │   │   └── page.tsx
│   │   │   └── [capability-route]/
│   │   │       └── page.tsx
│   │   ├── api/
│   │   │   └── [route-handler]/
│   │   │       └── route.ts
│   │   ├── layout.tsx
│   │   └── globals.css
│   ├── components/
│   │   ├── ui/                    # ShadCN components
│   │   ├── shared/                # App-wide shared components
│   │   └── [capability]/          # Feature-specific components
│   ├── lib/
│   │   ├── utils.ts
│   │   ├── [data-access].ts
│   │   └── [service].ts
│   ├── hooks/
│   │   └── [hook-name].ts
│   ├── types/
│   │   └── [entity].ts
│   ├── config/
│   │   ├── themes.ts
│   │   └── i18n.ts
│   └── __tests__/
│       ├── unit/
│       ├── integration/
│       └── fixtures/
├── e2e/
│   ├── [flow-name].spec.ts
│   ├── fixtures/
│   └── pages/                     # Page Object Models
│       └── [page-name].page.ts
├── tailwind.config.ts
├── next.config.ts
├── vitest.config.ts
├── playwright.config.ts
├── tsconfig.json
├── package.json
└── .env.example
```

### File naming conventions

- Components: `PascalCase.tsx` (e.g., `NoteCard.tsx`)
- Utilities and hooks: `camelCase.ts` (e.g., `useTheme.ts`)
- Test files: `[name].test.ts` or `[name].test.tsx` for unit/integration
- E2E test files: `[flow-name].spec.ts`
- Page Object Models: `[page-name].page.ts`
- Type files: `camelCase.ts` (e.g., `notebook.ts`)

### Route mapping from PRD

| PRD Capability | Route | Page Component |
|---|---|---|
| [Capability from PRD] | `/[locale]/[route]` | `[PageName]` |
| [Capability from PRD] | `/[locale]/[route]` | `[PageName]` |

---

## 3. Component Architecture

### Page components

| Page | Route | Key responsibilities | Server/Client |
|---|---|---|---|
| [PageName] | `/[route]` | [What this page does] | [Server or Client] |

### Shared components

| Component | Purpose | ShadCN base |
|---|---|---|
| [ComponentName] | [What it does] | [ShadCN component or custom] |

### Layout components

| Component | Purpose |
|---|---|
| [RootLayout] | [App shell, theme provider, i18n provider] |
| [AppLayout] | [Sidebar, header, main content area] |

### Component composition pattern

```
RootLayout (theme + i18n providers)
└── AppLayout (navigation + content shell)
    ├── Sidebar
    ├── Header
    └── MainContent
        └── [Page Component]
            ├── [Feature Component]
            └── [Feature Component]
```

---

## 4. Theming & Styling

### Color palette structure

```css
:root {
  --background: [value];
  --foreground: [value];
  --primary: [value];
  --primary-foreground: [value];
  --secondary: [value];
  --secondary-foreground: [value];
  --accent: [value];
  --accent-foreground: [value];
  --muted: [value];
  --muted-foreground: [value];
  --destructive: [value];
  --border: [value];
  --ring: [value];
  --radius: [value];
}

.dark {
  --background: [value];
  --foreground: [value];
  /* ... dark mode overrides */
}

[data-theme="blue"] {
  --primary: [value];
  --accent: [value];
  /* ... theme-specific overrides */
}
```

### Theme switching mechanism

[Describe how the theme toggle works: cookie/localStorage, CSS class toggle, ShadCN theme provider pattern]

### Available themes

| Theme name | Primary color | Accent color |
|---|---|---|
| [Default] | [Color] | [Color] |
| [Theme 2] | [Color] | [Color] |

---

## 5. Internationalization

### Configuration

| Setting | Value |
|---|---|
| **Library** | next-intl |
| **Default locale** | [e.g., en] |
| **Supported locales** | [e.g., en, fr, de] |
| **URL strategy** | [e.g., /[locale]/path] |
| **Fallback behavior** | [e.g., fall back to default locale] |

### Message file structure

```
public/locales/
├── en/
│   ├── common.json        # Shared UI strings
│   ├── [capability].json  # Feature-specific strings
│   └── errors.json        # Error messages
└── fr/
    ├── common.json
    ├── [capability].json
    └── errors.json
```

### Translation key conventions

- Flat keys with dot notation: `"capability.component.label"`
- Example: `"notebooks.sidebar.createButton"` → "New Notebook"

---

## 6. Data Model

### Entity definitions

| Entity | Key fields | Storage |
|---|---|---|
| [Entity] | [field1, field2, ...] | [localStorage / DB / API] |

### Entity relationships

```
[Entity A] 1──* [Entity B]
[Entity B] *──* [Entity C]
```

### Storage approach

[Describe whether data lives in localStorage, IndexedDB, a database via Prisma/Drizzle, or an external API. Include migration strategy if applicable.]

---

## 7. API Design

### Route handlers

| Method | Route | Purpose | Request body | Response |
|---|---|---|---|---|
| [GET/POST/...] | `/api/[resource]` | [What it does] | [Shape or N/A] | [Shape] |

### Error handling pattern

```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Human-readable description",
    "details": {}
  }
}
```

---

## 8. Testing Strategy

### Overview

| Test type | Tool | Location | Coverage target |
|---|---|---|---|
| **Unit** | Vitest + RTL | `src/__tests__/unit/` | [e.g., 80%] |
| **Integration** | Vitest | `src/__tests__/integration/` | [Key flows] |
| **E2E** | Playwright | `e2e/` | [Critical paths] |

### Unit test patterns

[Include example unit test from `references/testing-guide.md`]

### Integration test patterns

[Include example integration test]

### E2E test patterns

[Include example Playwright test with page object model]

### Critical user flows for e2e

| Flow | PRD stories | Test file |
|---|---|---|
| [Flow name] | [US-XX, US-YY] | `e2e/[flow].spec.ts` |

---

## 9. Environment & Tooling

### Required tools

| Tool | Version | Purpose |
|---|---|---|
| Node.js | [version] | Runtime |
| pnpm | [version] | Package manager |

### Environment variables

| Variable | Purpose | Required |
|---|---|---|
| `[VAR_NAME]` | [Purpose] | [Yes/No] |

### Scripts

| Script | Command | Purpose |
|---|---|---|
| `dev` | `pnpm dev` | Start development server |
| `build` | `pnpm build` | Production build |
| `test` | `pnpm test` | Run unit + integration tests |
| `test:e2e` | `pnpm test:e2e` | Run Playwright tests |
| `lint` | `pnpm lint` | Lint + format check |

---

## 10. Implementation Roadmap

Build in this order, derived from PRD priorities:

### Phase 1 — Foundation

| Step | What to build | PRD reference |
|---|---|---|
| 1 | Project scaffold, Tailwind + ShadCN setup, theme system | — |
| 2 | i18n setup with default + one additional locale | — |
| 3 | Root layout, app layout, navigation shell | — |

### Phase 2 — Must-have capabilities

| Step | What to build | PRD reference |
|---|---|---|
| [N] | [Capability] | [US-XX, US-YY] |

### Phase 3 — Should-have capabilities

| Step | What to build | PRD reference |
|---|---|---|
| [N] | [Capability] | [US-XX, US-YY] |

### Phase 4 — Polish and testing

| Step | What to build | PRD reference |
|---|---|---|
| [N] | E2E tests for critical flows | [US-XX, US-YY] |
| [N] | Accessibility review | — |
| [N] | Performance optimization | — |
