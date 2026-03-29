# Gateway Host Inventory

| Host | IP | CT | SSH Key | Agent | Bot | Port |
|------|----|----|---------|-------|-----|------|
| V-OC-Demigrious | 127.0.0.1 (local) | CT113 | n/a | Sofia (default) | @Sofia Demigrious | 18789 |
| V-OC-ArdI | 192.168.1.70 | CT118 | /root/.ssh/openclaw_id | Ardy | @Ardy | 18789 |

## SSH Pattern

```bash
ssh -i /root/.ssh/openclaw_id -o StrictHostKeyChecking=no root@<IP> "<command>"
```

## Notes

- V-OC-Demigrious: 5 agents (sofia, sec, infra, finance, web)
- V-OC-ArdI: 1 agent (ardy), independent hive, V-LM as primary model
- V-OC-Caroline: CT117, currently stopped (was clone template for ArdI)
