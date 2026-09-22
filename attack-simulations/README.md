# Controlled Lab Simulations

Only conduct tests against endpoints you own/control in an isolated lab. Record authorization, network boundaries, source and destination addresses, date, timezone, test volume, and results.

## SSH authentication scenario (planned)
1. Confirm lab isolation, monitored endpoint, agent connectivity and source log collection.
2. Generate limited failed SSH login attempts against the lab Ubuntu endpoint.
3. Verify source authentication events before inspecting SIEM events.
4. Correlate timestamps, source IP, username and event fields with Wazuh rule IDs.
5. Record an alert only if it actually appears; otherwise document the failure and investigate.
6. Optionally compare an ordinary authorized login to avoid conflating all logins with malicious activity.

Actual commands, test inputs, event count, rule ID and screenshots: **TBD — NOT RUN**.
