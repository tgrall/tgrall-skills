# PRD to issue decomposition guide

Use this guide to convert a structured PRD into an issue hierarchy that is useful for planning and execution.

## 1. Identify the parent issue

Create one main issue for the initiative.

The main issue should capture:

- feature or initiative name
- short problem/value summary
- scope overview
- expected child issues
- major dependencies
- assumptions or open questions that affect execution

Use the PRD's executive summary, problem statement, goals, and prioritization sections to shape this issue.

## 2. Decide how to split user work

Use the PRD's user story groupings as the first signal for issue boundaries.

Prefer a separate child issue when:

- a story group represents a meaningful user-facing capability
- a story is large enough to be planned or delivered independently
- different owners or disciplines are likely to handle the work
- acceptance criteria are substantial enough to justify dedicated tracking

Prefer a checklist inside a parent issue when:

- the item is a small implementation step
- the work is tightly coupled and not independently schedulable
- creating another issue would create tracking noise

## 3. Create supporting work only when it matters

Many PRDs imply work that is not itself a user story. Create supporting issues when the PRD clearly calls for them.

Common categories:

- **Technical foundation** - schema changes, infrastructure, platform groundwork, integrations, permissions, migration paths
- **QA/testing** - test plans, acceptance test coverage, regression validation, cross-platform validation
- **Rollout/launch** - feature flags, phased release, internal enablement, communications, migration support
- **Documentation** - user documentation, admin documentation, internal runbooks, enablement material
- **Release coordination** - milestone tracking, dependency coordination, release readiness

Avoid creating empty placeholder issues for categories that the PRD does not meaningfully imply.

## 4. Respect prioritization

Use the PRD's prioritization model to guide issue structure.

- Must-have items should be explicit in the issue tree
- Should-have and could-have items may remain as lower-priority child issues or later-phase issues
- Won't-have or out-of-scope items should not appear as active delivery issues

If the PRD uses MoSCoW, map it directly to priority labels and issue ordering.

## 5. Keep acceptance criteria close to work

Acceptance criteria belong with the issue they validate.

- For a capability issue, include capability-level acceptance criteria
- For a large single-story issue, include that story's criteria directly
- For technical or rollout issues, replace user-facing criteria with completion conditions that still describe observable outcomes

## 6. Manage dependencies deliberately

Use dependencies when they help execution clarity.

Good dependency examples:

- technical foundation must land before user-facing capability work
- rollout work depends on feature completion
- documentation depends on final workflow decisions

Do not create a dense mesh of weak dependencies that makes the backlog harder to use.

## 7. Suggested default hierarchy

Use this as the default when the PRD is substantial enough:

1. Main epic issue
2. User-facing capability issues
3. Technical foundation issue(s), if needed
4. QA/testing issue
5. Rollout/documentation issue(s), if needed
6. Release coordination issue, if needed

Collapse levels when the PRD is small; expand them when the PRD is broad and clearly multi-stream.
