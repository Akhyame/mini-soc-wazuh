# Mini SOC — lab operations notes

**Scope:** A local, authorized training lab; timestamps below are source-system UTC unless explicitly labelled as Dashboard display time. Findings and full case details: [public evidence index](evidence-index.md), [incident report](../incident-report/ssh-repeated-failures-lab-case.md).

## 22 September 2026 — implementation and rule verification

- Deployed Wazuh 4.14.7 central VM and enrolled monitored Ubuntu agent `mini-soc-agent`; verified journald source-to-alert ingestion.
- Moved two dedicated VMware VMs from initial NAT to Host-only VMnet1; updated agent manager address; ping 3/3 and fresh agent log ingestion verified. Agent route table had no default route when checked, but the VMware host remained reachable.
- After an initial Manager service timeout at boot, extended its systemd start wait to 180 seconds; Manager subsequently reported active.
- Custom rule **100100** matched five synthetic logtest inputs (on the fifth) and a bounded live endpoint-loopback SSH-failure sequence.
- Custom **100101** matched an authorized SSH login at 18:05:13 UTC, outside illustrative lab hours 09:00–18:00 UTC.
- Custom **100102** matched authorized `sudo /usr/bin/true` at 18:14:37 UTC. This monitors root-level privilege use, not demonstrated malicious privilege escalation.
- A failed SSH event followed by a successful login was observed as two separate alerts, built-in 5760 and 5715. Automatic correlation was not implemented.

## 23 September 2026 — restart, analyst exercise and dashboard

- On reboot, `wazuh-indexer` reached its existing 3-minute systemd start timeout; a service drop-in set `TimeoutStartSec=600`. After starting the service, `is-active` reported active and the web dashboard reopened. This does not prove why startup was slow.
- An initial incorrect local SSH password preceded five intentionally wrong local SSH passwords. The endpoint journal listed five failures between **09:36:50 and 09:37:09 UTC**. Custom 100100, level 10, matched the **09:37:04** event (source port **35652**) and showed frequency 5. The earlier initial failure can contribute to the rolling count.
- One authorized failed login at **09:46:46 UTC** followed by one successful login at **09:47:17 UTC**; separate built-in alerts 5760 and 5715 appeared.
- An ordinary SSH success during defined lab hours was executed, but the **absence** of custom 100101 was not checked in Wazuh; therefore negative-control PASS is not claimed.
- Saved `Mini SOC - Security Overview` with four widgets, filter `agent.name:"mini-soc-agent"`, rolling Last 7 days. It showed **412 alert documents** at the screenshot-capture time, not 412 attacks.

## Decision and publication scope

The additional 23 Sep SSH tests repeated patterns already validated on 22 Sep; later project phases reuse evidence rather than rerunning tests or duplicating screenshots. The public repository includes source-log excerpts, rule XML, dashboard configuration and an analyst report. A curated set of six portfolio image files was subsequently uploaded and verified under [screenshots/](../screenshots/README.md). It includes one designed explanatory infographic and five selected Wazuh/Ubuntu views, not every originally captured image. Never publish passwords or secrets.
