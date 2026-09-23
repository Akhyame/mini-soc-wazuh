# Mini SOC — public visual evidence

**Uploaded and verified in GitHub, 23 September 2026:** six image files in this directory. Five are selected visual captures of the authorized Wazuh/Ubuntu lab; one is a **designed architecture infographic**, not an original forensic screenshot. The images were supplied as a curated ZIP for portfolio publication; refer to the original source logs and independent Wazuh observations in the [public evidence index](../docs/evidence-index.md) when verifying a claim. No private Notion access is required to view these files.

| Public image | Meaning and limitation |
| --- | --- |
| [Architecture infographic](mini-soc-architecture.png) | Explanatory design of the Host-only Ubuntu Agent → Wazuh Manager → Indexer → Dashboard flow; **illustration, not raw log or incident proof**. For the observed topology and constraints read [architecture](../docs/architecture.md). |
| [Monitored Ubuntu Agent](P03-01-agents-connected.png) | Wazuh shows `mini-soc-agent` enrolled/active; that status alone is not proof of current ingestion. |
| [Repeated failed SSH — rule 100100](P04-04-live-ssh-alert.png) | Curated real lab alert capture: five bounded SSH password failures from the endpoint's own loopback were observed, and custom **100100**, level 10, matched. [Source/alert details](../docs/evidence-index.md). |
| [Failed SSH followed by success](P04-05-failed-then-success-events.png) | Separate built-in rule **5760** and **5715** event views; chronological comparison was **manual**. No automated chained rule was implemented. |
| [Sudo as root — rule 100102](P04-08-sudo-root-live-alert.png) | Authorized `sudo /usr/bin/true` activity matched custom **100102**, level 5. Privileged **use** is not proof of malicious escalation. |
| [Security Overview dashboard](P06-01-soc-dashboard.png) | Four saved visualizations; filter `agent.name:"mini-soc-agent"`; rolling Last 7 days. **412 alert documents** at capture time, not 412 attacks or a permanent number. [Dashboard details](../docs/dashboard.md). |

**Not in this uploaded set:** an original GitHub image showing live off-hours SSH custom **100101**, an original full journal-to-matching-Wazuh ingestion pair and the interactive synthetic `wazuh-logtest` result. The observed outcomes are documented in [the text evidence index](../docs/evidence-index.md), but those **additional image artifacts are not claimed as publicly uploaded**. The canonical lab originals remain in the private work notes. This six-image portfolio selection is intentionally compact; missing images do not require rerunning tests.

All activity described here is an **authorized local lab test**, not a confirmed attack. Private VMware RFC1918 addresses shown for reproducibility are not outside targets. Do not publish credentials, bearer tokens, private keys or unrelated personal data.
