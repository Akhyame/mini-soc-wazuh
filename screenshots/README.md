# Mini SOC — public visual evidence

**Uploaded and verified in GitHub, 23 September 2026:** eight image files in this directory. Seven are selected visual captures of the authorized Wazuh/Ubuntu lab, including two complementary original views for the live off-hours SSH rule 100101; one is a **designed architecture infographic**, not an original forensic screenshot. The images were supplied as a curated ZIP for portfolio publication; refer to the original source logs and independent Wazuh observations in the [public evidence index](../docs/evidence-index.md) when verifying a claim. No private Notion access is required to view these files.

| Public image | Meaning and limitation |
| --- | --- |
| [Architecture infographic](mini-soc-architecture.png) | Explanatory design of the Host-only Ubuntu Agent → Wazuh Manager → Indexer → Dashboard flow; **illustration, not raw log or incident proof**. For the observed topology and constraints read [architecture](../docs/architecture.md). |
| [Monitored Ubuntu Agent](P03-01-agents-connected.png) | Wazuh shows `mini-soc-agent` enrolled/active; that status alone is not proof of current ingestion. |
| [Repeated failed SSH — rule 100100](P04-04-live-ssh-alert.png) | Curated real lab alert capture: five bounded SSH password failures from the endpoint's own loopback were observed, and custom **100100**, level 10, matched. [Source/alert details](../docs/evidence-index.md). |
| [Failed SSH followed by success](P04-05-failed-then-success-events.png) | Separate built-in rule **5760** and **5715** event views; chronological comparison was **manual**. No automated chained rule was implemented. |
| [Off-hours SSH success — rule 100101 alert](P04-07-off-hours-ssh-live-alert.png) | Authorized local SSH success at **18:05:13 UTC** outside illustrative 09:00–18:00 UTC lab hours; Wazuh custom **100101**, level **10**, matched. |
| [Off-hours SSH success — matching event details](P04-07-off-hours-ssh-live-details.png) | Complementary original Wazuh Document Details for the same **100101** event: user `agentadmin`, source `127.0.0.1`, port `58948`, collected via `journald`. This is *not* a second test or second alert. |
| [Sudo as root — rule 100102](P04-08-sudo-root-live-alert.png) | Authorized `sudo /usr/bin/true` activity matched custom **100102**, level 5. Privileged **use** is not proof of malicious escalation. |
| [Security Overview dashboard](P06-01-soc-dashboard.png) | Four saved visualizations; filter `agent.name:"mini-soc-agent"`; rolling Last 7 days. **412 alert documents** at capture time, not 412 attacks or a permanent number. [Dashboard details](../docs/dashboard.md). |

**Not in this uploaded set:** an original full journal-to-matching-Wazuh ingestion pair and interactive synthetic `wazuh-logtest` screenshots. Their observed outcomes are documented in [the text evidence index](../docs/evidence-index.md). The canonical lab originals remain in private work notes; these optional additions do not require rerunning tests.

All activity described here is an **authorized local lab test**, not a confirmed attack. Private VMware RFC1918 addresses shown for reproducibility are not outside targets. Do not publish credentials, bearer tokens, private keys or unrelated personal data.
