# Mini SOC — verified milestone status

**As of 23 September 2026:** Local authorized Wazuh lab. The statuses below refer to the **agreed limited Ubuntu scope**; they do not claim a production SOC. All linked results are available in this public repository.

| Phase | Implemented result | Status / public evidence |
| --- | --- | --- |
| 01 — Lab design | Two dedicated VMware Ubuntu VMs, subsequently configured on Host-only VMnet1 | **COMPLETE for selected topology** — [architecture](architecture.md) |
| 02 — Wazuh stack | Central Wazuh Manager/Indexer/Dashboard 4.14.7, authenticated Dashboard access and verified service health | **COMPLETE** — [operational checks](phase-07-technical-handover.md) |
| 03 — Endpoint and ingestion | Ubuntu agent Active, original SSH journal events matched indexed Wazuh alerts 5715/5760 | **COMPLETE for Ubuntu** — [endpoint collection](phase-03-endpoint-log-collection.md) |
| 04 — Detection engineering | Three custom rules 100100/100101/100102 each matched live authorized lab activity; failed→success reviewed manually | **COMPLETE for agreed scenarios** — [rules](../detection-rules/README.md), [evidence](evidence-index.md) |
| 05 — SOC simulation and analysis | Authorized bounded SSH exercises; analyst documented a benign-test case without claiming compromise | **COMPLETE for analysis scope** — [incident report](../incident-report/ssh-repeated-failures-lab-case.md) |
| 06 — Monitoring dashboard | Four saved visualizations; agent filter; Last 7 days; 412 alert documents at one capture-time snapshot | **COMPLETE for selected dashboard** — [dashboard details](dashboard.md) |
| 07 — Technical report | Implemented-architecture handover, case report and evidence-to-claim index | **COMPLETE** — [handover](phase-07-technical-handover.md), [case](../incident-report/ssh-repeated-failures-lab-case.md), [index](evidence-index.md) |
| 08 — Public repository audit | Public text and GitHub-first evidence are in place; six portfolio images committed and indexed | **COMPLETE for agreed six-image publication set** — [README](../README.md), [public gallery](../screenshots/README.md) |
| 09 — Screenshot index | Six public portfolio images (one architecture illustration and five selected Wazuh/Ubuntu views) are committed and linked. Original live 100101 and source-to-SIEM pair are not in the six-image set | **COMPLETE for selected public gallery; optional extra evidence images pending** — [screenshots](../screenshots/README.md) |
| 10 — Public portfolio / LinkedIn write-up | Final case study not yet published | **PENDING** |

**Not implemented or verified:** Windows agent, scan telemetry, automatic chained failed→success alert, quantified false-positive rate, externally sourced attack, successful account compromise, independent negative-control PASS or independent clock-synchronization confirmation.

**Data handling:** This GitHub repository is the public source of truth. All six linked gallery image paths were checked against the public repository tree. No private work-note access is needed to read these results; additional original lab captures are not represented as uploaded.
