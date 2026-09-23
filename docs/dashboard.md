# Mini SOC — saved security dashboard

**Observed 23 September 2026:** A custom Wazuh 4.14.7 Dashboard named `Mini SOC - Security Overview` was created and saved. Four visualization objects are present and showed data:

| Widget | Implemented aggregation | Interpretation |
| --- | --- | --- |
| Mini SOC - Total Alerts | Metric → Count in `wazuh-alerts-*` | Number of **alert documents**, not number of attacks |
| Mini SOC - Alerts by Rule Level | Pie → Count; Terms on `rule.level` (Size 4) | Selected most frequent numeric rule levels, **not a complete severity distribution** |
| Mini SOC - Alerts Over Time | Line → Count by Date Histogram on event timestamp, Auto interval | Event-volume trend for the selected range |
| Mini SOC - Top 10 Alert Rules | Vertical Bar → Count; Terms on `rule.id`, Size 10, descending by Count | Frequent rule IDs are not necessarily the most dangerous |

**Saved shared scope:** DQL `agent.name:"mini-soc-agent"`; **Last 7 days**, a **rolling** time period. At screenshot capture, the total was **412 Wazuh alert documents**. The dashboard contains routine endpoint activity and controlled test alerts. The number may change with time or new indexed events.

**Screenshot state:** A final four-widget dashboard image was captured during the lab session but has **not yet been uploaded to the public repository**. Do not claim a GitHub screenshot exists until the actual file is added and verified in [screenshots/](../screenshots/README.md).

The source/alert relationship and rule outcomes are transcribed in [the public evidence index](evidence-index.md). For actual incident interpretation, see [the case report](../incident-report/ssh-repeated-failures-lab-case.md).
