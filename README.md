# Mini SOC — Security Monitoring & Threat Detection Lab

> **Status: planning / repository initialized.** Wazuh has not yet been deployed, and no detection or incident simulation has been tested.

A small, authorized **local lab** to practice the SOC workflow: collect endpoint security logs, understand SIEM ingestion, engineer and validate detection rules, investigate alerts, and document findings using Wazuh.

## Planned architecture

```text
Controlled local test source
            |
            v
Ubuntu SSH endpoint + Wazuh agent
            |
            v
Wazuh manager -> indexer -> dashboard
            |
            v
Tested rules -> observed alerts -> SOC investigation -> incident report
```

The topology and deployment method will be finalized after measuring available host resources. Windows monitoring is optional.

## Milestones

1. Measure PC resources, design the isolated lab and choose a feasible topology.
2. Deploy Wazuh central components and verify service health.
3. Connect an Ubuntu agent and confirm matching SSH logs appear in Wazuh.
4. Implement and **test** selected detection rules: repeated SSH failures; failure-to-success correlation (if technically feasible); login outside defined lab hours; and privilege changes.
5. Simulate bounded authentication activity only against authorized lab hosts and validate real alerts.
6. Build a monitoring dashboard and document analyst triage of a simulated event.
7. Prepare an evidence-backed incident report and concise portfolio case study.

**No phase is marked complete based solely on a plan.** See [the roadmap](docs/roadmap.md) for current status and [lab notes](docs/lab-notes.md) for observed results.

## Repository structure

```text
.
├── README.md
├── .gitignore
├── docs/
│   ├── roadmap.md
│   ├── architecture.md
│   └── lab-notes.md
├── detection-rules/
│   └── README.md
├── attack-simulations/
│   └── README.md
├── screenshots/
│   └── README.md
└── incident-report/
    └── incident-template.md
```

Verified rule XML, configuration examples, safe screenshots and final incident report will be added to the relevant folders **only after they actually exist**.

## Safety and evidence standards

Only generate security test traffic against systems you own/control within an isolated lab. Do not expose Wazuh management components to the public Internet or commit passwords, API tokens, private keys, host-specific logs, or sensitive personal information. Each published detection result must have a real timestamped test and corresponding log/alert evidence. Do not treat a suspicious alert as proof of compromise.

## Working documentation

[Notion Mini SOC project hub](https://app.notion.com/p/3e37554fee9d81c5aed4f152c448a2a6) contains separate phase pages, checklists, screenshot targets, investigation notes and evidence references.
