# Phase 03 — Ubuntu Endpoint and SSH Log Collection

**Status: verified for the Ubuntu endpoint (22 September 2026).** This is a local, authorized test lab, not a production SOC. Phase 04 custom rules, brute-force simulation, and network isolation testing have **not** been completed.

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

## Evidence and publication privacy

The private [Notion Phase 03 evidence page](https://app.notion.com/p/3e37554fee9d815e9aa0d6c4bbb05ba7) contains the original screenshots and captions:
- `P03-01-agents-connected.png` — enrolled active Ubuntu agent.
- `P03-02-ubuntu-source-log.png` — original SSH success in Ubuntu journald.
- `P03-03-wazuh-ingested-event.png` — matching indexed Wazuh success.
- `P03-05-failed-ssh-alert.png` — matching indexed Wazuh failure.

**No screenshots are committed to GitHub yet.** Before exporting a *copy* from private Notion to this public repository, cover all real lab IPv4/IPv6 addresses (including address bar, agent IP and source-IP fields) and unrelated personal/browser identifiers with opaque masks. Keep the original private evidence intact and never publish passwords, tokens, private keys, or identifying local paths. `P03-04` (Windows monitoring) is optional and was not implemented.

## Remaining work outside Phase 03

Review/configure lab network isolation before multi-attempt SSH simulations or network-origin attack testing. Create and validate custom detection rules in the later detection-rule phase; do not label rule 5715 or 5760 as custom.
