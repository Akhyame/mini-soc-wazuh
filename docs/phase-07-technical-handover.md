# Phase 07 — Mini SOC technical handover

**Scope:** Verified 22–23 September 2026; local, authorized training lab only. This is a description of the *observed setup*, not proof that another host can reproduce it without environment-specific adjustments.

## Architecture and log path

```text
Owned VMware Host-only VMnet1 lab
Ubuntu monitored endpoint: mini-soc-agent (192.168.80.129)
  SSH / sudo activity → systemd-journald → Wazuh agent 4.14.7
                                        → Wazuh manager (192.168.80.128)
                                        → custom/built-in rules → indexer → dashboard
```

The central Ubuntu VM (192.168.80.128) runs Wazuh Manager, Indexer and Dashboard, version 4.14.7. The Ubuntu endpoint (192.168.80.129) runs the Wazuh agent, an SSH service and a pre-existing `journald` localfile collector in `/var/ossec/etc/ossec.conf`; a second `auth.log` collector was **not** added. Authorized SSH test traffic targeted the endpoint's **own loopback address `127.0.0.1`**, not the host PC or outside machines.

Both VMs were switched from VMware NAT to Host-only and agent manager-address configuration was updated. The agent became Active and fresh journal events were verified in Wazuh. The endpoint route table showed no default route when checked, but the host is a reachable neighbor on the Host-only segment; this is **not** an absolute host-isolation guarantee.

## Local detections and reproducible verification

The custom XML files below are **rule fragments only**; the running Wazuh Manager stored them inside the existing local `<group>` in `/var/ossec/etc/rules/local_rules.xml`. Never overwrite that entire manager file with one fragment. Back up the existing file first, validate syntax with `sudo /var/ossec/bin/wazuh-analysisd -t`, then restart `wazuh-manager` if syntax is valid. These are maintenance steps, not instructions to run new attack simulations.

| Scenario | Rule and observed result | Proof |
| --- | --- | --- |
| Repeated local SSH password failures | Custom **100100**, level 10: five matching failed SSH events from the same source IP within 120 seconds. Five bounded local failures triggered a matching live alert; synthetic logtest also passed. | [Exact XML](../detection-rules/ssh-repeated-failures.xml) · [Phase 04 original screenshots and matrix](evidence-index.md) |
| Failed SSH followed by successful SSH | Built-in **5760** (failure) and **5715** (success) appeared as **separate** alerts. The analyst related them by source/account/time; **no automatic chained rule was implemented**. | [Phase 04 evidence](evidence-index.md) · [Phase 05 analyst notes](../incident-report/ssh-repeated-failures-lab-case.md) |
| SSH success outside illustrative lab hours (09:00–18:00 UTC) | Custom **100101**, level 10, extends built-in 5715; synthetic tests on each side of 18:00 UTC and authorized live off-hours login matched as documented. | [Exact XML](../detection-rules/ssh-off-hours.xml) · [Phase 04 original evidence](evidence-index.md) |
| Successful `sudo` execution as root | Custom **100102**, level 5, extends built-in 5402. A benign `sudo /usr/bin/true` produced a matching live alert. It detects **root privilege use**, not proven malicious escalation. | [Exact XML](../detection-rules/sudo-root-use.xml) · [Phase 04 original evidence](evidence-index.md) |

Detailed original tests and known false-positive cases: [Detection Rules README](../detection-rules/README.md). Do not label a simulated `wazuh-logtest` match as an endpoint-originated event.

## Operating checks and dashboard

After starting the two VMs, on the **central VM** check:

```bash
sudo systemctl is-active wazuh-manager wazuh-indexer wazuh-dashboard
```

On the monitored **endpoint**, check:

```bash
sudo systemctl is-active wazuh-agent ssh
```

Service state alone is not evidence of complete ingestion: inspect the agent's freshness and a recent, matching endpoint event under **Wazuh → Threat Hunting → Events**.

**Observed restart issue:** The Manager previously timed out during VM boot; its systemd start timeout was increased to **180 seconds**. On 23 September the Indexer reached its previous **3-minute** startup timeout. A drop-in at `/etc/systemd/system/wazuh-indexer.service.d/timeout.conf` set `TimeoutStartSec=600`; Indexer then reported `active` and the Dashboard reopened. These changes extend the waiting period; they do not by themselves establish why startup was slow.

The saved **Mini SOC - Security Overview** dashboard has four panels: Total Alerts, Alerts by Rule Level, Alerts Over Time, and Top 10 Alert Rules. It was saved with DQL `agent.name:"mini-soc-agent"` and **Last 7 days** selected. Its [single final Phase 06 screenshot](dashboard.md) displayed **412 alert documents at capture time**—not 412 attacks. Because Last 7 days is rolling, this is **not** a permanent metric. The pie/bar charts show selected top terms, not every rule or every level.

## Evidence handoff and limitations

- [Phase 03 — original Ubuntu source log and Wazuh ingestion](phase-03-endpoint-log-collection.md)
- [Phase 04 — original rule-test screenshots, XML and detection matrix](evidence-index.md)
- [Phase 05 — SOC analysis and simulated-case report](../incident-report/ssh-repeated-failures-lab-case.md)
- [Phase 06 — dashboard and its one original screenshot](dashboard.md)
- [Phase 07 — evidence-to-claim index](phase-07-technical-handover.md)

**Not implemented/verified:** Windows agent, port-scan telemetry, automated failure→success correlation, external-source attack, actual compromise, quantified false-positive rates or a confirmed in-hours SSH *negative-control PASS*. VM clock synchronization was not independently confirmed, so off-hours detections should not be used as proof of time accuracy. All tested suspicious-looking events described here were authorized local lab activity. Do not publish passwords, tokens, private keys or unrelated personal identifiers.
