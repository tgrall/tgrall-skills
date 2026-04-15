# Label taxonomy

Use labels to make the issue tree readable, not noisy.

## Default label groups

### Type labels

- `type:epic`
- `type:user-story`
- `type:technical`
- `type:qa`
- `type:rollout`
- `type:docs`
- `type:release`

### Priority labels

- `priority:must`
- `priority:should`
- `priority:could`
- `priority:wont`

### Status labels

- `status:ready`
- `status:blocked`
- `status:needs-clarification`

### Area labels

Create area labels only when the PRD has stable functional domains, for example:

- `area:search`
- `area:billing`
- `area:reporting`

Do not create area labels from one-off wording or weak signals.

## Reuse vs create

1. Reuse existing labels when they are semantically equivalent or close enough.
2. If the repository already has a strong taxonomy, align to it instead of forcing this default set.
3. Recommend missing labels before creating them.
4. Avoid duplicating labels that differ only in punctuation or minor wording.

## Minimal default assignment

Every created issue should usually receive:

- one type label
- one priority label when the PRD supports it

Status labels are optional and should be used only when they help the team's workflow.

## Suggested mapping from PRD

- Main issue -> `type:epic`
- User-facing capability issue -> `type:user-story`
- Enabler or foundation issue -> `type:technical`
- Test or validation issue -> `type:qa`
- Launch, migration, or comms issue -> `type:rollout`
- Docs or enablement issue -> `type:docs`
- Milestone coordination issue -> `type:release`

Map MoSCoW directly:

- Must -> `priority:must`
- Should -> `priority:should`
- Could -> `priority:could`
- Won't -> do not create an active issue unless the user explicitly wants a parking-lot or future-work issue
