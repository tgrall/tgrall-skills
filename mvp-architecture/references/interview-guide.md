# Architecture interview guide

Use this guide to ask focused questions for each architecture block. Skip questions when the PRD or the default stack already provides the answer.

## 0. Language setup

- Detect the PRD language.
- Ask the user to confirm or change the language for the conversation and the architecture document.
- Default to the PRD language.

Completion check: language is confirmed.

## 1. Stack confirmation

Typical questions:

- Does the default stack (Next.js App Router, ShadCN, Tailwind, next-intl, Vitest, Playwright) work for you?
- Do you want to swap any part? (e.g., different UI library, CSS-in-JS instead of Tailwind, Jest instead of Vitest)
- Are there hosting or deployment constraints? (Vercel, self-hosted, Docker, etc.)
- Any existing codebase or monorepo to align with?
- Preferred package manager? (pnpm, npm, yarn)

Completion check: the tech stack is locked. Every major technology choice is resolved.

## 2. Project structure

Typical questions:

- How many top-level route groups do the PRD capabilities suggest?
- Should the project use a feature-based or layer-based folder structure?
- Are there shared libraries or packages to consider?
- Should the project support a monorepo layout?

Completion check: a specific folder tree can be written with exact paths and naming conventions.

## 3. Component architecture

Typical questions:

- Which PRD capabilities map to full pages?
- Which user stories share common UI patterns?
- Are there reusable layout elements (sidebar, header, navigation, modals)?
- Which ShadCN components are likely needed based on the PRD stories?
- Should components use server components by default, or are there heavy client interaction patterns?

Completion check: page components, shared components, and layout components are identified with clear boundaries.

## 4. Theming and styling

Typical questions:

- How many color themes should be supported? (2 default: light + dark, or more?)
- Should users be able to choose accent colors?
- Are there brand colors or a design system to follow?
- Should the theme toggle be in the header, settings, or both?

Completion check: color palette structure, CSS variable strategy, and toggle mechanism are defined.

## 5. Internationalization

Typical questions:

- Which locales should be supported at MVP?
- Should the URL include the locale? (e.g., /en/dashboard vs /dashboard)
- Are there right-to-left (RTL) locales to consider?
- How should the language switcher work?

Completion check: locales, message file layout, key convention, and switching UX are defined.

## 6. Data model

Typical questions:

- What entities does the PRD imply? (e.g., users, notes, projects, tasks)
- What are the key relationships between entities?
- Should data be stored locally (localStorage, IndexedDB), via an API, or in a database?
- If a database, which one? (PostgreSQL, SQLite, Supabase, Prisma, Drizzle)
- Is there an authentication requirement?

Completion check: entities, fields, relationships, and storage approach are defined concretely.

## 7. API design

Typical questions:

- Are there external APIs to integrate with?
- Should the app use Next.js Route Handlers, Server Actions, or an external API layer?
- What error handling pattern should be used?
- Are there real-time requirements (WebSocket, SSE)?

Completion check: route structure, payload shapes, and error format are defined — or the section is marked as not applicable.

## 8. Testing strategy

Typical questions:

- What coverage targets do you want? (e.g., 80% unit, key flows for e2e)
- Are there specific user flows from the PRD that must have e2e tests?
- Should tests run in CI? Which CI platform?
- Do you want visual regression testing?
- Are there accessibility testing requirements?

Completion check: test types, file structure, naming conventions, example patterns, Playwright config approach, and coverage targets are defined.

## 9. Environment and tooling

Typical questions:

- Which Node.js version?
- Any environment variables needed from the start? (API keys, feature flags)
- Should the project include Docker or devcontainer support?
- Any CI/CD pipeline preferences?

Completion check: tooling versions, config approach, and CI considerations are resolved.
