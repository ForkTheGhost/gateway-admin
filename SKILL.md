---
name: gateway-admin
description: Manage OpenClaw gateway instances across hosts — restart, check status, view logs, update config on local or remote CTs via SSH. Use when restarting a gateway, checking if an agent is online, viewing gateway logs, or managing systemd services on remote OpenClaw hosts. Triggers on "restart gateway", "is X online", "check gateway status", "view gateway logs", "restart Ardy".
---

# Gateway Admin

Manage OpenClaw gateway instances on local or remote hosts.

## Host Registry

Load `references/hosts.md` for the current host inventory before any operation.

## Operations

### Status Check

```bash
# Local
systemctl --user is-active openclaw-gateway
ss -tlnp | grep 18789

# Remote
ssh -i <KEY> -o StrictHostKeyChecking=no root@<IP> \
  "systemctl --user is-active openclaw-gateway && ss -tlnp | grep 18789"
```

### Restart

```bash
# Local
systemctl --user restart openclaw-gateway

# Remote
ssh -i <KEY> -o StrictHostKeyChecking=no root@<IP> \
  "systemctl --user restart openclaw-gateway"
```

Wait 8-10 seconds after restart, then verify port is listening.

### View Logs

```bash
# Recent entries (last 20 lines, parsed)
ssh -i <KEY> root@<IP> "tail -20 /tmp/openclaw/openclaw-\$(date +%Y-%m-%d).log"
```

### Verify Discord Connection

Look for `discord client initialized as <bot_id>` in logs. If present, bot is connected.

## Rules

- Always verify port 18789 is listening after restart
- Use `loginctl enable-linger root` on remote CTs if gateway dies on SSH disconnect
- Never `pkill -f openclaw` on a remote host — it kills the gateway
- Wait at least 8 seconds after restart before checking status
- If systemd is unavailable (container), run gateway in foreground: `openclaw gateway --port 18789 &`

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Port not listening after restart | Process crashed | Check logs for config errors |
| Bot shows offline on Discord | Missing permissions or token | Check `openclaw doctor` output |
| Gateway dies on SSH disconnect | No linger enabled | `loginctl enable-linger root` |
| Config invalid after edit | Schema violation | Run `openclaw doctor --fix`, check `.bak` |
