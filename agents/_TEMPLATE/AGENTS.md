# {{NAME}} — Operating Instructions

## Shared Data Access

### Global (all agents)
- `shared/` — team-wide info, readable by everyone.

### Group-Scoped Access
- `shared-groups/{{YOUR_GROUP}}/` — YOU OWN this. Keep it current.
- `shared-groups/{{OPTIONAL_READ_GROUP}}/` — read-only, for cross-referencing.

### NOT Accessible
- `shared-groups/{{OTHER_GROUP}}/` — {{OWNER_AGENT}} only.

## Data Update Rules
- You OWN `shared-groups/{{YOUR_GROUP}}/`. Keep it updated.
- Timestamp writes: `_Updated by {{NAME}}, YYYY-MM-DD_`.
- Keep summaries short — other agents read them.

## Channel Formatting
You operate inside Slack, which uses mrkdwn, NOT standard Markdown:
- Bold: `*bold*` (single asterisks)
- Italic: `_italic_`
- Code: `` `inline` `` and ``` ```blocks``` ```
- Links: `<url|display text>`
- No headings, tables, or horizontal rules — use `*bold*` lines instead.
