# Phase 03 — Ubuntu Endpoint and SSH Log Collection

**Status: verified for the Ubuntu endpoint (22 September 2026).** This is a local, authorized test lab, not a production SOC. This page covers Phase 03 only; later rule tests and Host-only networking are documented in [Detection Rules](../detection-rules/README.md) and [the public evidence index](evidence-index.md).

## Scope and deployment

- Two dedicated VMware virtual machines: a central Ubuntu Wazuh server and a separate Ubuntu SSH endpoint.
- Central Wazuh manager, indexer and dashboard: installed package version `4.14.7-1`; all three systemd services returned `active`. Authenticated dashboard access was verified.
- Endpoint: Ubuntu 24.04.5 LTS as displayed in Wazuh Endpoints; Wazuh agent `4.14.7-1`, enrolled as `mini-soc-agent` (agent ID `001`, default group). The service returned `active` and the dashboard reported agent status **Active**.
- OpenSSH service on the endpoint returned `active`. Endpoint-to-manager ICMP reachability was verified (3/3 replies), but this does **not** establish network isolation.
- The agent's `/var/ossec/etc/ossec.conf` already included a `localfile` entry with `log_format=journald` and `location=journald`. No duplicate `/var/log/auth.log` collector was added.

## Reproducible verification (authorized local endpoint)

Commands shown use placeholders for all address information; do not substitute a non-owned target.

```bash
sudo systemctl is-active ssh
sudo systemctl is-active wazuh-agent
sudo journalctl -u ssh --no-pager -n 15

# Authorized successful login to the endpoint's own loopback SSH service:
ssh agentadmin@<ENDPOINT_LOOPBACK_IP>

# One incorrect password only; not a brute-force test:
ssh -o NumberOfPasswordPrompts=1 agentadmin@<ENDPOINT_LOOPBACK_IP>
sudo journalctl -u ssh --no-pager -n 10
```

In Wazuh **Threat Hunting → Events**, select a time range containing the events and filter by agent name plus `full_log` matching `Accepted password` or `Failed password`. Expand document details and compare the original `journald` entry with the indexed alert by timestamp, agent, account, source address, and port.

## Observed results

| Observed event (UTC) | Ubuntu source | Wazuh evidence |
| --- | --- | --- |
| 2026-09-22 14:42:19 | Authorized local SSH password login accepted for lab account `agentadmin` | `mini-soc-agent`, source type `journald`, built-in rule **5715**, level **3**, `sshd: authentication success`. The full log matched the original event. |
| 2026-09-22 14:56:54 | Exactly one incorrect password for lab account `agentadmin`; SSH denied access | `mini-soc-agent`, source type `journald`, built-in rule **5760**, level **5**, `sshd: authentication failed`. The full log matched the original event. |

**Interpretation:** These results demonstrate endpoint-to-SIEM event ingestion and *built-in* SSH authentication alerting for two controlled local events. They do **not** demonstrate a custom detection rule, brute-force activity, compromise, or externally sourced SSH testing.

## Public evidence and privacy

Original source-event timestamps and indexed-rule details are recorded above and in the [public evidence index](evidence-index.md). Screenshot files have **not yet been uploaded to GitHub**; see [the public screenshot checklist](../screenshots/README.md). Readers do not need a private notebook account to review the documented results. Retain private VMware lab addresses when useful for reproducibility; remove credentials, tokens and unrelated identifiers before publishing any screenshots. Windows agent monitoring was not implemented.

## Remaining work outside Phase 03

**Subsequent completed work:** the two VMs were moved to VMware Host-only before bounded SSH repetition tests and three custom rules (100100, 100101, 100102) were validated. See [Detection Rules](../detection-rules/README.md); built-in 5715 and 5760 remain built-in. No external-origin testing was performed.
