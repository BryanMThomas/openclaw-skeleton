# Architecture

## The pieces

**Gateway.** A single OpenClaw process is the control plane: it owns sessions, routes inbound messages to the right agent, runs cron jobs, and emits events. It runs as a user systemd unit (not a system unit) and is the one thing that must stay up.

**Agents.** Each agent is a directory of markdown + a small JSON config entry. The markdown *is* the agent:

| File | Purpose |
|---|---|
| `IDENTITY.md` | One-glance card — name, role, vibe, emoji, channel |
| `SOUL.md` | Persona, purpose, and explicit **scope** (what it owns / what it defers) |
| `AGENTS.md` | Operating instructions — shared-data access, update rules, channel formatting |
| `HEARTBEAT.md` | Periodic self-checks (empty = no heartbeat) |
| `TOOLS.md` | Environment-specific notes (hosts, device names, voices) |
| `agent/MEMORY.md` | Persistent memory **(never committed)** |

**Models.** Every model call goes through OpenRouter, so any provider (Claude, GPT, DeepSeek, Gemini, …) is one config string away. Agents that fire often and reason lightly get cheap models; the orchestrator gets a strong one.

**Tools.** Agents call external capabilities over MCP. Credentials are referenced, never inlined.

## How a request flows

1. A message lands in a Slack channel (or DM).
2. The gateway matches it to an agent via `channels.slack.routing`.
3. The agent loads its persona + instructions, reads any shared-group files it's scoped to, and reasons with its assigned model.
4. It replies in-channel and, if relevant, updates the shared-group files it owns (timestamped).

## Design principles

- **One agent, one job.** Small, bounded responsibilities. You can understand and change one agent without touching the others.
- **Communicate through files, not shared mutable state.** Agents coordinate by reading/writing scoped directories — auditable and simple.
- **Least privilege.** An agent's `AGENTS.md` lists exactly which shared groups it may read/write; everything else is explicitly out of bounds. See [SHARED-GROUPS.md](SHARED-GROUPS.md).
- **Declarative + recoverable.** The fleet is config + markdown. Lose the host and you rebuild from this skeleton; only memories and credentials need restoring.
- **Cost-aware.** Per-agent model choice keeps the monthly bill proportional to value.
