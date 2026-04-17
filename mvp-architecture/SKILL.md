---
name: mvp-architecture
description: Generate a prescriptive MVP architecture document from a completed PRD, optimized for vibe coding. Default stack is Next.js App Router, ShadCN, Tailwind CSS, multi-color theming, dark mode, and i18n. Includes a complete testing strategy with Vitest and Playwright. Use this whenever the user wants to define a technical architecture, project structure, component design, or implementation blueprint from a PRD — especially when the goal is to feed the PRD + architecture into an AI coding agent as a single implementation prompt.
---

# MVP Architecture from PRD

Help the user turn a completed PRD into a prescriptive MVP architecture document.

The architecture document should be detailed and opinionated enough that when combined with the PRD in a single prompt, an AI coding agent can implement the full MVP without needing further clarification on technology choices, project structure, component design, or testing approach.

## Core behavior

1. Detect the language used in the PRD (from the metadata Language field or by inspecting the document content).
2. Ask the user to confirm or change the language for the conversation and the architecture document. Pre-select the PRD language as the default.
3. Offer these options explicitly: PRD language (default), English, French, German, Italian, Spanish, Dutch, or a custom language provided by the user.
4. If the user does not choose, use the PRD language for both conversation and document.
5. Read the PRD carefully and extract capabilities, user stories, priorities, data entities, and scope boundaries.
6. Use the opinionated default stack (Next.js App Router, ShadCN, Tailwind CSS, multi-color theming, dark mode, i18n) unless the user wants to swap parts.
7. Walk through each architecture block, asking focused questions when the PRD does not provide enough information.
8. Produce a markdown architecture document that follows the template in `references/architecture-template.md`.

## Default tech stack

The skill defaults to this opinionated stack. The user can swap any part during the interview.

- **Framework:** Next.js 15+ with App Router
- **UI library:** ShadCN/ui components
- **Styling:** Tailwind CSS 4+
- **Theming:** CSS custom properties for multi-color palettes, dark/light mode toggle
- **Internationalization:** next-intl
- **Testing — unit/integration:** Vitest + React Testing Library
- **Testing — e2e:** Playwright
- **State management:** React Server Components + URL state; client state via React context or Zustand when needed
- **Package manager:** pnpm (or npm/yarn if the user prefers)
- **Linting:** ESLint + Prettier

If the user wants a different framework, UI library, or testing tool, adapt the architecture accordingly while preserving the same level of prescriptive detail.

## Working in CLI and IDEs

This skill should work well in command-line chats and IDE chat panels.

- Interview block by block, not all at once.
- Skip questions when the PRD or the default stack already provides the answer.
- When the user accepts the defaults, move fast — do not re-explain every choice.
- When the user wants to change something, explain trade-offs briefly and adapt.

## Workflow

### 0. Language setup

Detect the PRD language and confirm with the user.

- Read the PRD metadata Language field. If missing, infer from the document content.
- Ask the user to confirm or change the language, defaulting to the PRD language.
- Explicitly propose: PRD language (default), English, French, German, Italian, Spanish, Dutch, or a custom language.

### 1. Read and extract from the PRD

Identify:

- product name and initiative scope
- core capabilities and workflow areas
- user stories grouped by capability
- data entities and relationships implied by the stories
- priorities (must/should/could)
- non-functional requirements (performance, accessibility, offline, etc.)
- out-of-scope boundaries

### 2. Stack confirmation

Present the default stack and ask:

- Does the default stack work for you, or do you want to swap any part?
- Are there additional constraints (hosting platform, existing codebase, team familiarity)?
- Any specific versions or libraries you want to pin?

If the user accepts the defaults, move to the next block immediately.

### 3. Block-by-block architecture interview

For each architecture block, ask focused questions only when the PRD does not provide sufficient detail. Use the guidance in `references/interview-guide.md`.

Required blocks:

- Project structure and folder layout
- Component architecture (pages, shared components, layouts)
- Theming and styling (multi-color, dark mode)
- Internationalization
- Data model and storage
- API design (when applicable)
- Testing strategy (unit, integration, e2e)
- Environment and tooling

### 4. Codebase grounding

If a repo exists and it helps:

- inspect the current project structure, config files, and existing patterns
- use findings to align the architecture with what already exists
- avoid contradicting established conventions without good reason

### 5. Synthesis

After the interview:

1. Resolve any contradictions between the PRD and the chosen architecture.
2. Draft the architecture document using `references/architecture-template.md`.
3. Make the document prescriptive: include exact folder paths, file naming conventions, component patterns, config snippets, and test examples.
4. Include an implementation roadmap that maps PRD priorities to build order.

## What makes a good architecture document for vibe coding

The document should be specific enough that an AI coding agent reading PRD + architecture can:

- scaffold the project structure without asking follow-up questions
- create components with the right patterns and conventions
- set up theming, dark mode, and i18n correctly on the first pass
- write tests that match the prescribed strategy
- build features in the right order based on priorities

Avoid vague guidance like "use a good folder structure." Instead, prescribe the exact structure and explain why.

## Architecture block completeness

A block is ready when it is specific enough to implement without further clarification.

- **Project structure**: exact folder tree, naming conventions, route mapping from PRD
- **Component architecture**: page-level and shared components identified, composition patterns clear
- **Theming**: color palette structure, CSS variable naming, dark mode toggle mechanism
- **i18n**: message file structure, key naming convention, supported locales
- **Data model**: entities, fields, relationships, storage choice
- **API design**: route structure, payload shapes, error format
- **Testing**: test file locations, naming, example patterns for each test type, coverage targets
- **Environment**: tooling versions, config approach, CI considerations

## Output

Produce a single markdown architecture document following `references/architecture-template.md`.

The document should be self-contained: a reader (human or AI) should not need to look up external documentation to understand the prescribed architecture.

If the user asks for a file, save it as markdown. Otherwise, present it directly in the conversation.

## Bundled references

- `references/interview-guide.md` — block-by-block question guide
- `references/architecture-template.md` — output template for the architecture document
- `references/nextjs-stack-guide.md` — opinionated defaults for Next.js, ShadCN, Tailwind, and conventions
- `references/testing-guide.md` — Vitest and Playwright patterns, coverage targets, and templates
- `references/theming-i18n-guide.md` — multi-color theming, dark mode, and i18n implementation patterns
