# Public evidence index — Mini SOC / Wazuh

This index is **self-contained and public**. It separates (a) results transcribed from the observed Ubuntu/Wazuh outputs from (b) original screenshot files. **No original screenshots have been committed to this repository as of 23 September 2026.** For the current screenshots, see the [upload plan](../screenshots/README.md). A private project notebook is *not* required to understand the tests below.

## Endpoint ingestion — 22 September 2026

| UTC source event | Observed Ubuntu / Wazuh outcome | Scope |
| --- | --- | --- |
| 14:42:19 | Authorized `Accepted password` SSH event for lab account `agentadmin`; matching Wazuh built-in **5715**, level 3, `location=journald` | Real local endpoint event and matching SIEM alert |
| 14:56:54 | One deliberately incorrect SSH password; matching Wazuh built-in **5760**, level 5, `location=journald` | One failure is **not** brute force |

Details: [Phase 03 endpoint collection](phase-03-endpoint-log-collection.md).

## Repeated SSH failures — two bounded live tests

**22 September 2026:** Five deliberately wrong passwords to the Ubuntu endpoint's **own loopback** `127.0.0.1` produced `Failed password` events at **16:58:15, :20, :26, :45 and :50 UTC**. Wazuh Threat Hunting showed **one** matching custom **100100**, level **10** alert; `full_log` matched the last source event (source port **37104**). Separate interactive synthetic `wazuh-logtest` used five repeated SSH samples: first four matched built-in 5760, and the fifth matched 100100 (frequency 5). The synthetic inputs were **not** five actual network logins.

**23 September 2026:** The operator performed one initial failed SSH attempt before a five-attempt loop. The Ubuntu agent's actual `sudo journalctl -u ssh --since "2026-09-23 09:36:40" --until "2026-09-23 09:37:15" --no-pager | grep "Failed password"` output included:

```text
Sep 23 09:36:50 mini-soc-agent sshd[2814]: Failed password for agentadmin from 127.0.0.1 port 43270 ssh2
Sep 23 09:36:54 mini-soc-agent sshd[2818]: Failed password for agentadmin from 127.0.0.1 port 48086 ssh2
Sep 23 09:36:59 mini-soc-agent sshd[2822]: Failed password for agentadmin from 127.0.0.1 port 48094 ssh2
Sep 23 09:37:04 mini-soc-agent sshd[2825]: Failed password for agentadmin from 127.0.0.1 port 35652 ssh2
Sep 23 09:37:09 mini-soc-agent sshd[2828]: Failed password for agentadmin from 127.0.0.1 port 35660 ssh2
```

Wazuh showed a **new** rule **100100**, level **10**, `rule.frequency=5`, `data.srcip=127.0.0.1`, `data.dstuser=agentadmin`, `data.srcport=35652` and `location=journald`. Its `full_log` matched the **fourth** journal line at **09:37:04 UTC** (port 35652), **not the fifth**. The initial prior failure may have contributed to the rule's rolling count. The Wazuh UI displayed the alert at approximately **10:37:05**; the UI's configured timezone was not independently checked. A separate result from 22 September belonged to the *earlier* test.

Custom rule XML: [100100](../detection-rules/ssh-repeated-failures.xml). The 23 September incident's [public analyst report](../incident-report/ssh-repeated-failures-lab-case.md) cites this extract. These were authorized tests, **not** external attacks.

## Failed SSH followed by successful SSH — manual analyst correlation

**22 September:** failed SSH at **17:14:57 UTC**, then successful SSH at **17:15:02 UTC**. Wazuh indexed two separate built-in alerts: **5760** (failure) and **5715** (success).

**23 September source journal:**

```text
Sep 23 09:46:46 mini-soc-agent sshd[2906]: Failed password for agentadmin from 127.0.0.1 port 39844 ssh2
Sep 23 09:47:17 mini-soc-agent sshd[2922]: Accepted password for agentadmin from 127.0.0.1 port 54736 ssh2
```

Wazuh showed rule **5760**, level **5**, near 10:46:47 UI time and rule **5715**, level **3**, near 10:47:19 UI time. **No automatic failed→success correlation rule was installed or verified.** Both connections were deliberately made by the authorized lab operator.

## Off-hours SSH success — 22 September

Illustrative permitted lab hours: **09:00–18:00 UTC**. Custom rule **100101**, level **10**, extends SSH success rule **5715** and uses the Wazuh Manager clock to evaluate its `<time>6 pm - 9 am</time>` condition. Synthetic logtest before 18:00 matched 5715; after 18:00 the same sample matched 100101. A real authorized local SSH success at **18:05:13 UTC** (source `127.0.0.1`, port **58948**) produced a **100101** live alert. This is a timing signal, not proof of an unauthorized login. Clock synchronization was not independently confirmed. [Rule XML](../detection-rules/ssh-off-hours.xml).

## Sudo executed as root — 22 September

The Ubuntu endpoint recorded:

```text
Sep 22 18:14:37 mini-soc-agent sudo[5188]: agentadmin : TTY=pts/1 ; PWD=/home/agentadmin ; USER=root ; COMMAND=/usr/bin/true
```

Wazuh showed custom rule **100102**, level **5**, with `data.command=/usr/bin/true`, `data.dstuser=root` and `location=journald`. An ordinary `sudo journalctl` command also generated this rule; such monitoring has expected benign alerts. **This is authorized root-level command use, not evidence of malicious privilege escalation.** [Rule XML](../detection-rules/sudo-root-use.xml).

## Dashboard snapshot — 23 September

The saved **Mini SOC - Security Overview** showed four widgets (Total Alerts, Alerts by Rule Level, Alerts Over Time, Top 10 Alert Rules) using the DQL filter `agent.name:"mini-soc-agent"` and rolling **Last 7 days**. The filtered total was **412 alert documents** at capture time. This snapshot is not a fixed count and not 412 attacks. [Dashboard documentation](dashboard.md).

## Evidence boundaries

Only Ubuntu endpoint SSH and sudo use were monitored in the implemented scenarios; no Windows endpoint, port-scan telemetry, external attacker, real account compromise or verified false-positive rate. The successful SSH test during normal lab hours returned without CLI error, but a missing 100101 alert was **not independently established**, so a negative-control PASS is **not** claimed. Original image exports are still pending: the textual observations above are public, while images should not be represented as already available on GitHub.
