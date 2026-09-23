# Controlled lab simulations — completed scenarios

**These tests were performed on owned VMware Ubuntu VMs; no external host was targeted.** All SSH traffic described below targeted `127.0.0.1` on the monitored Ubuntu endpoint itself. No new tests are required to document these established observations.

| Test | Observed result | Public trace |
| --- | --- | --- |
| Repeated wrong SSH passwords | Custom 100100, level 10; five synthetic inputs matched on fifth; real 22 Sep and 23 Sep endpoint-loopback tests also generated matching alerts | [Source excerpts and event fields](../docs/evidence-index.md) · [Exact rule XML](../detection-rules/ssh-repeated-failures.xml) |
| Failed SSH then successful SSH | Separate built-in alerts 5760 and 5715; manual analyst comparison, **no automatic correlation** | [Source log and timestamps](../docs/evidence-index.md) |
| SSH success after illustrative lab hours | Custom 100101, level 10; authorized success at 18:05:13 UTC on 22 Sep produced matching alert | [Observed result](../docs/evidence-index.md) · [Exact rule XML](../detection-rules/ssh-off-hours.xml) |
| Successful `sudo` as root | Custom 100102, level 5; authorized `sudo /usr/bin/true` produced matching alert | [Observed result](../docs/evidence-index.md) · [Exact rule XML](../detection-rules/sudo-root-use.xml) |

**Limitations:** These are bounded exercises, not evidence of real attacks, unauthorized escalation or account compromise. An in-hours SSH success was executed as a potential negative control, but the absence of 100101 in Wazuh was **not checked**. No Windows or network-port-scan activity is claimed. The complete [analyst report](../incident-report/ssh-repeated-failures-lab-case.md) documents source-to-alert reasoning and benign-test disposition.

**Visual evidence:** Not yet published as files in the public repository; [screenshot upload plan](../screenshots/README.md). Text observations and technical artifacts above are available without private notebook access.
