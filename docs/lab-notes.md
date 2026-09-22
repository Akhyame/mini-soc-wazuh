# Lab Notes

Record **observed** outputs, not hypothetical results. Replace TBD only when verified.

## Session 01 — kickoff
- Date and timezone: 2026-09-22, Africa/Casablanca (time TBD)
- GitHub repository: https://github.com/Akhyame/mini-soc-wazuh
- Notion project: https://app.notion.com/p/3e37554fee9d81c5aed4f152c448a2a6
- Action: Repository created and initial documentation scaffold added.
- Environment / SIEM installation: Not started.
- Alerts / detections tested: None.
- Next step: Measure host CPU, RAM, free disk and virtualization before fixing the topology.

## Session 02 — Host resource diagnostic
- Date/timezone: 2026-09-22, Africa/Casablanca (exact time not provided).
- Source: User-supplied Windows PowerShell read-only CIM/Docker/WSL output; no remote host access.
- OS: Windows 11 Pro, 64-bit, version 10.0.26200.
- CPU: Intel Core i7-8850H, 6 physical cores / 12 logical processors.
- RAM: 31.66 GiB installed, 22.81 GiB available at measurement.
- C: drive: 237.52 GiB total, **18.4 GiB free**. Other suitable disk volumes not confirmed.
- Virtualization: HypervisorPresent=True; firmware-enabled virtualization not separately confirmed because it did not appear in the formatted output.
- Docker: CLI v29.7.2; Docker Engine version output blank. Engine health not confirmed.
- WSL: WSL 2; default distribution docker-desktop.
- Official sizing references: https://documentation.wazuh.com/current/quickstart.html and https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html (single-node guidance: 50 GB disk capacity, 4 CPU cores and 8 GB RAM).
- Assessment: CPU/RAM appear compatible with a small lab but C: free disk is below official single-node guidance. **Do not install Wazuh or create VM disks on C: yet.** Existing lab data must not be deleted without explicit review.
- Next read-only checks: enumerate other fixed volumes and available space; check Docker daemon connectivity/error; then select safe install location and topology.
- Screenshot P01-01: NOT CAPTURED. Wazuh deployment / rules / alert testing: NOT STARTED.


## Template for future sessions
### Date / timezone
TBD

### Goal
TBD

### Commands and configuration (sanitized)
TBD

### Actual outputs / rule IDs / timestamps
TBD

### Validation outcome
NOT RUN (PASS / FAIL / PARTIAL only after a real test)

### Evidence
TBD (screenshot name and matching Notion phase)

### Blockers, fixes and next action
TBD
