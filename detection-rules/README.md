# Detection Rules

Rules below are **implementation- and evidence-tracked**. A successful `wazuh-logtest` match does not, by itself, prove a live dashboard alert or real attack.

| Scenario | Rule file / ID | Status | Actual test evidence |
| --- | --- | --- | --- |
| Repeated SSH failures from one source IP | [`ssh-repeated-failures.xml`](ssh-repeated-failures.xml), **custom 100100**, level 10 | Installed on lab manager; syntax check and synthetic test **PASS**; live validation pending | Interactive `wazuh-logtest`: first four samples matched built-in rule **5760** (level 5); fifth matched **100100** (`frequency=5`, `timeframe=120`, `same_srcip`). [Private Notion screenshots P04-01/02](https://app.notion.com/p/3e37554fee9d81a6a57feee525dc272c). |
| Failures followed by login success | TBD | Planned; automatic correlation feasibility not verified | Pending |
| Login outside lab-defined working hours | TBD | Planned | Pending |
| Privileged action | TBD | Planned | Pending |

## Repeated SSH failures — precise scope

The new rule uses `<if_matched_sid>5760</if_matched_sid>` to count events previously matching built-in SSH authentication failure rule **5760**, with the same decoded source IP, in a **120-second** window. The local XML file is in `/var/ossec/etc/rules/local_rules.xml` on the Wazuh manager; this repository contains **only the custom rule fragment**, not the full existing file. Back up the local rules file and insert this fragment *inside* its enclosing local `<group>`; do not overwrite default Wazuh files. On the lab manager, `sudo /var/ossec/bin/wazuh-analysisd -t` exited 0, then `wazuh-manager` was restarted and confirmed active.

In a single interactive `sudo /var/ossec/bin/wazuh-logtest` session, one simulated `Failed password` SSH syslog line was supplied five times (source loopback address on the lab endpoint). The first four test outputs reported rule 5760; the fifth reported custom rule 100100, level 10 and frequency 5. These are **five synthetic test inputs**, not five actual SSH connections. The custom rule's live ingestion/dashboard behavior and false-positive rate are **not yet tested**; actual network attack simulation requires a separate lab isolation review.

Potential false positives include a legitimate user entering an incorrect password repeatedly, or multiple users behind the same source IP. Match-only evidence is not evidence of intrusion.

For each future rule, document the source log, grouping keys, frequency/timeframe, implemented rule ID, test input, observed output, and limitations. Do not present a manual analyst query as automated detection.
