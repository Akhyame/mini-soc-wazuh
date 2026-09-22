# Mini SOC Lab — Roadmap

**Status:** Planning. This is a controlled local lab, not a production security operations center.

| Phase | Scope | Status | Evidence |
| --- | --- | --- | --- |
| 01 | Host resources, isolated architecture and network plan | Not started | Pending |
| 02 | Wazuh manager, indexer and dashboard deployment | Not started | Pending |
| 03 | Ubuntu agent connection and SSH log collection | Not started | Pending |
| 04 | Baseline and custom detection rules | Not started | Pending |
| 05 | Controlled attack simulation and detection validation | Not started | Pending |
| 06 | Dashboard creation and analyst investigation | Not started | Pending |
| 07 | Simulated incident report and technical documentation | Not started | Pending |
| 08 | GitHub publication and versioned artifacts | Repository created; scaffold in progress | Repository link |
| 09 | Evidence gallery and screenshot index | Not started | Pending |
| 10 | Portfolio / LinkedIn case study | Not started | Pending |

## Planned detection scenarios
- Repeated failed SSH login attempts from the same source (threshold to be calibrated).
- Failed logins followed by successful login (automatic correlation only if technically validated; otherwise manual analyst investigation).
- Successful login outside **defined lab working hours** (anomaly, not proof of compromise).
- Authorized lab sudo activity or privileged-group modification.
- Windows agent and port-scan/network telemetry are optional enhancements, not completed features.

## Definition of done
A real, traceable path from a controlled Ubuntu event to a matching Wazuh event, tested rule, generated alert, dashboard investigation and honest simulated-incident report.

Documentation hub: https://app.notion.com/p/3e37554fee9d81c5aed4f152c448a2a6
