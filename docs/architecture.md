# Architecture (provisional)

> This is a logical design, **not** a claim that the infrastructure has been deployed. Final topology depends on measured PC resources.

```text
Controlled lab-only test source
           |
           | authorized test traffic
           v
Ubuntu SSH endpoint (Wazuh agent)
           |
           | authentication events / agent telemetry
           v
Wazuh manager -> indexer -> dashboard
           |
           v
Rules -> alerts -> analyst investigation -> incident report

[Optional] Windows endpoint + Wazuh agent
```

## Phase 01 inventory
| Role | OS / deployment | Hostname | IP / network | State |
| --- | --- | --- | --- | --- |
| Wazuh server | TBD after host capacity check | TBD | TBD | Not deployed |
| Ubuntu monitored endpoint | TBD | TBD | TBD | Not deployed |
| Test source | Host or authorized lab VM, TBD | TBD | TBD | Not selected |
| Windows endpoint (optional) | TBD | TBD | TBD | Not deployed |

## Deployment decisions (pending)
- Select a feasible storage location and supported deployment method before installing lab components.
- Record the **actual lab** network isolation, monitored agents and component versions after setup.

## Scope and safety
Only target machines owned/controlled inside this isolated lab. Do not expose management services or SSH to the public Internet. Redact secrets before publishing. A successful detection demonstrates an observed signal; it does not automatically prove a real compromise.
