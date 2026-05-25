# Shared groups — file-based access control

Agents don't share a database. They coordinate through a directory tree, and **access is granted by instruction**: each agent's `AGENTS.md` lists exactly which directories it may read and write. An agent simply never touches a group it isn't told about.

## Layout

```
shared/                     # team-wide; every agent may read
shared-groups/
  <domain-a>/               # owned by one agent; a second may have read access
  <domain-b>/
  <domain-c>/
```

## Ownership model

- Each shared group has **one owner** (read + write).
- The orchestrator typically gets **read** access to domain summaries so it can compile cross-domain briefs, without write access to domain internals.
- Sensitive domains (e.g. finance, health) are scoped to a minimal set of agents.

## Example grant (in an agent's `AGENTS.md`)

```markdown
### Group-Scoped Access
- `shared-groups/projects/` — YOU OWN this. Keep project status current.
- `shared-groups/finance/` — read-only; for cross-referencing budgets.

### NOT Accessible
- `shared-groups/health/` — Health agent + Orchestrator only.
```

## Conventions

- Timestamp every write: `_Updated by <Agent>, YYYY-MM-DD_`.
- Keep summaries short and current — other agents read them, not your full history.
- Memories that are personal to one agent live in its own `agent/MEMORY.md`, **not** in a shared group.
