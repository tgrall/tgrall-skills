---
name: prd-to-issues
description: Turn a completed Product Requirements Document into a GitHub issue hierarchy with a main issue, user-story issues, and supporting delivery issues. Use this whenever the user wants to operationalize a PRD into backlog items, GitHub issues, sub-issues, milestones, delivery work, or issue-ready project planning, especially when they want proper labels and issue relationships instead of a flat task list.
---

# PRD to GitHub Issues

Help the user convert a completed PRD into an execution-ready GitHub issue structure.

The goal is not to dump a flat list of tasks. Read the PRD, preserve its priorities and scope boundaries, group related work intelligently, and produce a hierarchy that a product and engineering team can actually use.

## Core behavior

1. Detect the language used in the PRD (from the metadata Language field or by inspecting the document content).
2. Ask the user to confirm or change the language for the conversation and issue content. Pre-select the PRD language as the default.
3. Offer these options explicitly: PRD language (default), English, French, German, Italian, Spanish, Dutch, or a custom language provided by the user.
4. If the user does not choose, use the PRD language for both conversation and issue content.
4. Read the PRD carefully and identify the initiative, major workflow areas, priorities, dependencies, out-of-scope boundaries, and delivery concerns.
5. Build an issue plan before creating anything.
6. Default to a draft-first workflow: show the planned main issue, child issues, labels, and hierarchy before creating GitHub issues.
7. If the user explicitly wants direct creation and GitHub issue access is available, create the issues after confirming any missing repository-specific details.
8. Prefer issue/sub-issue relationships when the platform supports them. Otherwise preserve the hierarchy through linked sibling issues and explicit parent references.
9. Reuse existing labels when appropriate. Recommend missing labels before creating new ones.

## Working in CLI and IDEs

This skill should work well in command-line chats and IDE chat panels.

- Keep the workflow structured: PRD review, decomposition, issue plan, then creation or draft output.
- Ask focused clarification questions only when the PRD is missing information that materially affects issue structure.
- If the environment supports structured prompts or forms, use them for repository selection, label preferences, and creation confirmation.
- Do not ask for confirmation after every issue draft; batch the plan review when possible.

## Workflow

### 0. Language setup

Begin by detecting the PRD language and confirming the choice with the user.

- Read the PRD's metadata Language field. If the field is missing, infer the language from the document content.
- Ask the user to confirm or change the language for the conversation and the issue content, defaulting to the PRD language.
- Explicitly propose: PRD language (default), English, French, German, Italian, Spanish, Dutch, or a custom language.
- If the user gives different preferences for conversation and issue language, support that.
- If the user does not respond, use the PRD language for both.

### 1. Read and interpret the PRD

Identify:

- the initiative title and value proposition
- the core problem being solved
- major capability groups
- user stories and acceptance criteria
- MoSCoW or equivalent priorities
- dependencies and enabling work
- QA, rollout, documentation, or release needs
- explicit out-of-scope constraints

Use the decomposition guidance in `references/decomposition-guide.md`.

### 2. Build the issue hierarchy

By default, produce:

- one **main epic issue**
- child issues for **user story groups** or **large individual stories**
- supporting issues for **technical foundations**, **QA/testing**, **rollout/launch**, and **documentation** when the PRD justifies them
- milestone or release coordination issues when the PRD clearly implies staged delivery

Do not create trivial issues for tiny tasks that belong as checklists inside a parent issue.

### 3. Plan labels and relationships

Use the label guidance in `references/label-taxonomy.md`.

- Assign issue type labels
- Preserve PRD priority via priority labels
- Add status labels only when they add signal
- Add stable area labels only when the PRD clearly supports them
- Prefer parent/sub-issue relationships over ad hoc textual lists

### 4. Draft before creating

Unless the user explicitly requests direct creation, first present:

1. the issue tree
2. the labels that will be used
3. the draft main issue
4. the draft child issues
5. any assumptions, open questions, or places where issue splitting is judgment-based

### 5. Create GitHub issues when appropriate

If the user wants real GitHub issues and GitHub access is available:

- confirm the target repository
- check for existing labels first
- create missing labels only when they were approved or clearly requested
- create the main issue first
- create child issues next
- add sub-issue relationships when the platform supports them
- otherwise add explicit parent links/backreferences in issue bodies

## Decomposition rules

Use the PRD as the source of truth, not as raw text to be copied line by line.

- Turn the PRD's overall initiative into the main issue
- Turn major product capabilities into child issues
- Split individual user stories into their own issues only when they are independently schedulable, owned by different people, or large enough to track separately
- Turn cross-cutting non-feature work into separate supporting issues when they would otherwise get lost
- Keep acceptance criteria close to the issue they validate
- Preserve out-of-scope boundaries so the backlog does not quietly expand beyond the PRD

## Issue quality rules

- Each issue should have a clear outcome, not just an activity
- Titles should be short and scannable
- Bodies should explain context, scope, acceptance criteria, dependencies, and links to related issues when useful
- Avoid duplication between parent and child issues
- Use checklists inside an issue when the work is too small to deserve its own tracked issue

## Output modes

Support these modes:

1. **Draft-only** - produce markdown issue drafts and the proposed hierarchy
2. **Review-then-create** - default mode; show drafts first, then create after approval
3. **Direct-create** - create issues immediately when the user explicitly asks and the environment supports it

## Templates and references

Use these bundled references:

- `references/decomposition-guide.md` - how to map PRD sections into issue hierarchy
- `references/label-taxonomy.md` - default labels and reuse/creation rules
- `references/issue-templates.md` - templates for the main issue and common child issue types
- `references/github-creation-guide.md` - GitHub-aware creation flow and fallback behavior


## Linking user stories to the main issue

When creating GitHub issues, establish explicit links between all user story issues and the main epic:

- Add a "Relates to" or "Part of" link from each user story issue back to the main epic
- In the main epic issue body, maintain a summary section listing all child user story issues with links
- Use GitHub's issue linking syntax: `Relates to #issue-number` or `Part of #issue-number`
- If the platform supports parent/sub-issue relationships, use those as the primary hierarchy
- For sibling user stories, add cross-references only when there are true dependencies or handoff points
- Keep the main issue body updated as new user story issues are created, so it serves as a live index

This ensures traceability, prevents orphaned work, and gives the team a single source of truth for the initiative's scope.