# Implemented architecture — Mini SOC / Wazuh

**Verified 22–23 September 2026.** This is an owned, authorized local training lab, not a production SOC.

```text
                         VMware Host-only VMnet1
           ┌───────────────────────────────────────────┐
           │ Ubuntu agent VM: 192.168.80.129           │
           │ mini-soc-agent — Wazuh Agent 4.14.7       │
           │                                           │
           │ Authorized SSH on 127.0.0.1; sudo events  │
           │       → systemd-journald                  │
           │       → Wazuh agent                       │
           │                      │                    │
           │                      ▼                    │
           │ Central Ubuntu VM: 192.168.80.128         │
           │ Manager 4.14.7 → rules → Indexer           │
           │                       → Wazuh Dashboard   │
           └───────────────────────────────────────────┘
                    Analyst triage → SOC report
```

| Role | Observed system | Scope |
| --- | --- | --- |
| Central Wazuh server | Own Ubuntu VM; Wazuh Manager, Indexer and Dashboard; `192.168.80.128` | Alert analysis, storage and visualisation |
| Monitored Ubuntu endpoint | Ubuntu 24.04.5 LTS as shown in Wazuh; SSH and Wazuh agent `mini-soc-agent`, ID `001`; `192.168.80.129` | SSH and sudo logs via existing `journald` collector |
| Authorized SSH source and target | Endpoint's own `127.0.0.1` | No outside/host-PC traffic in the described security tests |
| Optional Windows endpoint / scan sensor | **Not implemented** | No claims of Windows detections or port-scanning telemetry |

Both VMs were changed from an initial VMware NAT segment to Host-only VMnet1 before bounded multi-attempt tests. The endpoint's route table had no default route when checked; Host-only does **not** exclude access to the VMware host. Private lab IPs are included for reproducibility and are not public destination addresses.

**Implemented pipeline:** source SSH or sudo log → systemd journal → Wazuh Agent → Manager built-in/custom rule → Indexer → Dashboard and analyst. Agent status Active and source-log-to-indexed-event matching were verified. The dashboard was saved with four widgets and a rolling Last 7 days filter for `mini-soc-agent`.

**Original observations and exact rule XML:** [public evidence index](evidence-index.md) · [endpoint ingestion](phase-03-endpoint-log-collection.md) · [technical handover](phase-07-technical-handover.md) · [detection rules](../detection-rules/README.md) · [saved dashboard](dashboard.md).

**Limitations:** Host-only is an intentional local networking mode, not proof of absolute isolation. A matching alert does not prove unauthorized access. Endpoint clock synchronization, real-world noise rates and an in-hours rule negative control were not independently confirmed.
