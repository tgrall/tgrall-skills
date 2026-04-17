# tgrall-skills

A collection of custom skills for GitHub Copilot CLI.

## Skills

### write-prd

Create a Product Requirements Document through a structured interview process. Walk through your product idea block by block — from problem framing to user stories — and produce a planning-ready markdown PRD.

**When to use:** You have a rough product idea and want to turn it into a complete, structured PRD.

### mvp-architecture

Generate a prescriptive MVP architecture document from a completed PRD, optimized for vibe coding. Defaults to Next.js App Router, ShadCN, Tailwind CSS, multi-color theming, dark mode, and i18n — with a complete testing strategy using Vitest and Playwright. The output is detailed enough that an AI coding agent can implement the full MVP without further clarification.

**When to use:** You have a finished PRD and want a technical architecture blueprint before handing it off to a coding agent.

### prd-to-issues

Turn a completed PRD into a GitHub issue hierarchy with a main epic, user-story issues, and supporting delivery issues. Preserves priorities, scope boundaries, and dependencies from the original PRD.

**When to use:** You have a finished PRD and want to operationalize it into trackable GitHub issues.

## Usage

These skills are designed for use with [GitHub Copilot CLI](https://githubnext.com/projects/copilot-cli). Each skill folder contains:

- `SKILL.md` — Skill definition and behavior instructions
- `references/` — Supporting templates, guides, and taxonomies
- `evals/` — Evaluation cases for testing skill quality
