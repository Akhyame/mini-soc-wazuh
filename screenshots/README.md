# Screenshot evidence — public upload status

**As of 23 September 2026, this folder contains documentation only: no original screenshot image files have been committed to GitHub.** The private lab notebook originally stored the screenshots, but public portfolio visitors should not need access to that notebook. The [public evidence index](../docs/evidence-index.md) and [SOC case](../incident-report/ssh-repeated-failures-lab-case.md) already include observed log excerpts, rule IDs and test limitations.

## Images to export and upload once (no new tests needed)

| Suggested repo filename | Screenshot already captured in lab | What it proves |
| --- | --- | --- |
| `P03-01-agents-connected.png` | Monitored Ubuntu Agent Active | Agent enrollment and monitoring state |
| `P03-02-ubuntu-source-log.png` | Original local SSH authentication journal | Source event before Wazuh ingestion |
| `P03-03-wazuh-ingested-event.png` | Matching built-in SSH success (5715) | Source-to-SIEM event path |
| `P04-01-rule-definition.png` | Custom 100100 rule definition | Exact installed threshold and parent rule |
| `P04-02-rule-test.png` | Five-input interactive logtest | **Synthetic** rule match only |
| `P04-04-live-ssh-alert.png` | Live custom 100100 alert and details | **Real authorized lab** alert; source log match |
| `P04-05-failed-then-success-events.png` | Separate 5760/5715 events | Manual comparison, **not** automated correlation |
| `P04-06-off-hours-ssh-logtest.png` | Custom 100101 synthetic match | **Synthetic** time-condition test |
| `P04-07-off-hours-ssh-live-alert.png` | Custom 100101 live alert and details | Authorized after-hours login observed in Wazuh |
| `P04-08-sudo-root-live-alert.png` | Custom 100102 alert and details | Authorized sudo-to-root use detected |
| `P06-01-soc-dashboard.png` | Four-widget saved filtered Dashboard | Global agent query, rolling Last 7 days; 412 alert docs at capture time |

**Export the existing original screenshots, not screenshots of this README or fabricated substitutes.** When a proof requires two different existing views, either use separate descriptive filenames or arrange both in one legible image without altering the evidence. Upload only images that actually show the claimed values. Lab RFC1918 IPs can remain; remove any passwords, session tokens, private keys, unrelated personal identifiers or sensitive browser/account details. Be careful that a temporary Dashboard time window may no longer display exactly 412 if you take a *new* screenshot.

After uploading, update this index to link the **actual image paths** and check that each GitHub URL renders for a visitor without private access. Until then, the screenshot gallery is **pending**, not completed. Do not duplicate these images in unrelated phase folders.
