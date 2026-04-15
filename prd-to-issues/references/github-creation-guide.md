# GitHub creation guide

Use this guide when moving from issue drafts to real GitHub issues.

## Default mode

Default to **review-then-create**:

1. read the PRD
2. draft the issue hierarchy
3. propose labels
4. show the plan to the user
5. create issues only after approval

This reduces the risk of creating a noisy backlog from an ambiguous PRD.

## Before creating issues

Confirm:

- target repository
- whether draft-only or direct creation is desired
- whether new labels may be created
- whether the user wants one issue per story group or one issue per large story

If the PRD is ambiguous enough to change issue count or hierarchy materially, ask for clarification before creating anything.

## Creation order

When creating issues:

1. inspect existing labels
2. create approved missing labels if needed
3. create the main issue first
4. create child issues next
5. add parent/sub-issue relationships when supported
6. otherwise edit issue bodies to include parent references and related issue links

## Relationship fallback

If formal sub-issues are not available, preserve the structure in issue bodies:

- main issue lists all child issues
- each child issue references the main issue
- supporting issues also reference any directly related capability issues when that helps

## Draft output format

If the skill is operating in draft-only mode, present:

- the issue tree
- proposed labels
- issue titles
- issue bodies in markdown
- notes about assumptions, omitted work, and places where the PRD was too vague for stronger decomposition
