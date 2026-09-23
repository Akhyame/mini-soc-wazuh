# Detection Rules

Rules below are **implementation- and evidence-tracked**. A successful `wazuh-logtest` match does not, by itself, prove a live dashboard alert or real attack.

| Scenario | Rule file / ID | Status | Actual test evidence |
| --- | --- | --- | --- |
| Repeated SSH failures from one source IP | [`ssh-repeated-failures.xml`](ssh-repeated-failures.xml), **custom 100100**, level 10 | Installed on lab manager; synthetic test **PASS**; bounded live loopback SSH test **PASS**; threshold tuning pending | Five local SSH failures in 35 seconds matched one alert **100100**, level 10. [Private Notion screenshots P04-01/02/04](../docs/evidence-index.md). |
| Failures followed by login success | Built-in **5760** (failure), **5715** (success) | Controlled failure → success sequence **observed**; automatic correlation **not implemented** | Ubuntu journal: 17:14:57 UTC failed SSH, 17:15:02 UTC accepted SSH, same source; both appeared as separate indexed Wazuh alerts. Private Notion evidence P04-05. |
| Login outside lab-defined working hours | [`ssh-off-hours.xml`](ssh-off-hours.xml), **custom 100101**, level 10 | Installed; synthetic and bounded live local SSH tests **PASS** | Lab policy 09:00–18:00 **UTC**; test `wazuh-logtest` before 18:00 matched built-in 5715 and after 18:00 matched 100101. At 18:05:13 UTC an authorized SSH success generated one matching live Wazuh rule 100101, level 10 alert (source port 58948). Private Notion P04-06/07. |
| Privileged action | [`sudo-root-use.xml`](sudo-root-use.xml), **custom 100102**, level 5 | Installed; bounded live sudo test **PASS** | Lab user invoked `sudo /usr/bin/true` at 18:14:37 UTC; Ubuntu journal recorded `USER=root` and the command, and Wazuh produced rule 100102, level 5 with matching `data.command`. This is authorized privilege use, **not** evidence of privilege escalation. Private Notion P04-08. |

## Repeated SSH failures — precise scope

The new rule uses `<if_matched_sid>5760</if_matched_sid>` to count events previously matching built-in SSH authentication failure rule **5760**, with the same decoded source IP, in a **120-second** window. The local XML file is in `/var/ossec/etc/rules/local_rules.xml` on the Wazuh manager; this repository contains **only the custom rule fragment**, not the full existing file. Back up the local rules file and insert this fragment *inside* its enclosing local `<group>`; do not overwrite default Wazuh files. On the lab manager, `sudo /var/ossec/bin/wazuh-analysisd -t` exited 0, then `wazuh-manager` was restarted and confirmed active.

In a single interactive `sudo /var/ossec/bin/wazuh-logtest` session, one simulated `Failed password` SSH syslog line was supplied five times (source loopback address on the lab endpoint). The first four test outputs reported rule 5760; the fifth reported custom rule 100100, level 10 and frequency 5. These are **five synthetic test inputs**, not five actual SSH connections. The custom rule was also validated against five actual, deliberately failed SSH logins to the endpoint's own loopback interface after both VMs were moved to the Host-only network and agent-to-manager ingestion was reconfirmed. The Ubuntu source journal recorded five `Failed password` events at 2026-09-22 16:58:15, 16:58:20, 16:58:26, 16:58:45 and 16:58:50 UTC. Wazuh Threat Hunting showed **one alert**, rule **100100**, level **10**; the alert's `full_log` matched the last journal event for the lab user and source port **37104**. This is a bounded authorized local test, not evidence of unauthorized intrusion. Alert behavior under other loads and false-positive rate remain untested; do not extrapolate from five attempts.

Potential false positives include a legitimate user entering an incorrect password repeatedly, or multiple users behind the same source IP. Match-only evidence is not evidence of intrusion.

For each future rule, document the source log, grouping keys, frequency/timeframe, implemented rule ID, test input, observed output, and limitations. Do not present a manual analyst query as automated detection.

## Off-hours SSH login — custom rule 100101

The rule extends Wazuh built-in SSH success rule **5715** with `<time>6 pm - 9 am</time>`. The lab-defined normal hours are **09:00–18:00 UTC** and are illustrative, not a real organization's policy. The manager's clock is UTC; both VM clocks were compared, but `timedatectl` reported `System clock synchronized: no`, so unattended time synchronization is not claimed. Rule XML syntax validation returned zero and manager was active after restart. An interactive logtest run before 18:00 UTC reported rule 5715 and a later run after 18:00 UTC reported rule 100101, level 10. On 2026-09-22 18:05:13 UTC, one actual authorized local SSH login produced a matching rule 100101, level 10 Wazuh alert (endpoint journal `Accepted password`, source loopback port 58948). The syslog timestamp typed into `wazuh-logtest` is not itself evidence of the time-condition evaluation; the manager's clock during each test mattered. Legitimate maintenance outside lab hours is a possible false positive. [Private evidence: P04-06/07](../docs/evidence-index.md).

## Sudo executed as root — custom rule 100102

Rule 100102 extends built-in rule **5402** and labels any successful `sudo` execution as `root` for analyst review. A no-op `sudo /usr/bin/true` command by the authorized lab user at 2026-09-22 18:14:37 UTC yielded a source journal entry with `USER=root ; COMMAND=/usr/bin/true` and a matching Wazuh rule 100102, level 5 alert; the expanded document also showed `data.command=/usr/bin/true` and `data.dstuser=root`. Normal administrative commands (including inspecting logs) also trigger this rule, so the alert is **not** proof of malicious escalation and its operational noise needs evaluation. [Private evidence: P04-08](../docs/evidence-index.md).

## Failure followed by success — manual correlation only

A controlled single SSH failure at 17:14:57 UTC followed by authorized SSH success at 17:15:02 UTC (same endpoint/source) was observed as two **separate** built-in Wazuh alerts, rules 5760 and 5715. No custom automated failed→success correlation or alert has been implemented or verified. This is a manual investigation example, not evidence of account compromise. [Private evidence: P04-05](../docs/evidence-index.md).
