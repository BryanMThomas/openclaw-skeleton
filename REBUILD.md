# REBUILD — bootstrap a fresh fleet from this skeleton

This skeleton is declarative: the whole fleet is **config + markdown personas**. To stand up a working system, you supply a host, OpenClaw, secrets, and (optionally) restored memories.

## 1. Host + OpenClaw

```bash
# On a fresh Linux host:
# install OpenClaw per https://github.com/openclaw/openclaw
mkdir -p ~/.openclaw
```

## 2. Lay down the structure

```bash
# copy this skeleton into place
cp -r agents ~/.openclaw/agents
cp openclaw.example.json ~/.openclaw/openclaw.json
```

For each agent, create the runtime dirs the skeleton intentionally omits:

```bash
for a in ~/.openclaw/agents/*/; do
  mkdir -p "$a/agent" "$a/sessions" "$a/workspace"
done
```

## 3. Wire up secrets (never commit these)

- Put Slack bot/app tokens, OpenRouter key, and any tool credentials under `~/.openclaw/credentials/`.
- Reference them by ref in `openclaw.json` (see `*TokenRef` fields) — never inline.

## 4. Personalize

- Edit each agent's `IDENTITY.md` + `SOUL.md` to define who it is and what it owns.
- Set up `shared/` (team-wide) and `shared-groups/<domain>/` directories; grant each agent access in its `AGENTS.md`.
- Fill in `openclaw.json`: real agent `id`s, models, channel routing, cron schedules.

## 5. Restore memories (optional)

If rebuilding after a failure, drop your backed-up `MEMORY.md` files into each `agents/<name>/agent/MEMORY.md` and shared-group contents back into `shared-groups/`.

## 6. Start the gateway

```bash
# start the OpenClaw gateway (user systemd unit recommended)
# verify agents respond in their Slack channels
```

## Notes

- The gateway can OOM on very large sessions — bump `gateway.heapMB` if so.
- Keep each agent **single-purpose**; add new ones by copying `agents/_TEMPLATE/`.
- Tune cost by assigning cheaper models to high-frequency, low-complexity agents.
