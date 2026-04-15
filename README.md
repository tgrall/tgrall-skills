# tgrall-skills

A collection of custom skills for GitHub Copilot CLI.

## Skills

### write-prd

Create a Product Requirements Document through a structured interview process. Walk through your product idea block by block — from problem framing to user stories — and produce a planning-ready markdown PRD.

**When to use:** You have a rough product idea and want to turn it into a complete, structured PRD.

### prd-to-issues

Turn a completed PRD into a GitHub issue hierarchy with a main epic, user-story issues, and supporting delivery issues. Preserves priorities, scope boundaries, and dependencies from the original PRD.

**When to use:** You have a finished PRD and want to operationalize it into trackable GitHub issues.

## Usage

These skills are designed for use with [GitHub Copilot CLI](https://githubnext.com/projects/copilot-cli). Each skill folder contains:

- `SKILL.md` — Skill definition and behavior instructions
- `references/` — Supporting templates, guides, and taxonomies
- `evals/` — Evaluation cases for testing skill quality
