# Case Study 02 — Lateral Movement / Authorized Engineer Activity

**Environment:** Multi-site manufacturing organization (~250 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** Lateral Movement — authentication to multiple internal hosts in short time window
**MITRE ATT&CK:** T1021 (Remote Services)
**Severity:** Medium
**Outcome:** Confirmed authorized activity — Rapid7 investigation note only, no client escalation required

---

## Initial Alert

The SIEM generated a lateral movement alert based on a service account authenticating to fourteen internal hosts within a 22-minute window. The account had domain admin privileges. The activity occurred at 2:14 AM on a weekday.

On surface read: high-privilege account, unusual hours, rapid multi-host authentication — consistent with both legitimate administrative scripting and adversary lateral movement during off-hours.

---

## Investigation

**Account Context:**

The service account belonged to an authorized IT engineering firm contracted for managed services work. A review of the change management record confirmed a scheduled maintenance window for patch deployment across the affected host group — the same fourteen hosts that appeared in the alert.

The account's activity pattern during the alert window matched exactly what patch deployment tooling produces: sequential authentication to each host, brief session, disconnect, move to next host. No interactive logon sessions. No new processes spawned outside the patch management tool's known process chain. No data access outside system directories.

**What Would Have Changed This Assessment:**
- Interactive sessions (RDP, console logon) rather than tool-driven authentication
- Authentication to hosts outside the fourteen in the maintenance scope
- Process execution outside the known patch tool process chain
- Any outbound network connections from the authenticated hosts during the window
- No corresponding change record in the ticketing system

None of these were present.

**MITRE ATT&CK Mapping:**
- **T1021 — Remote Services:** The technique matches the observed behavior. The mapping stands regardless of whether the activity is malicious — ATT&CK describes the technique, not the intent.

---

## Key Takeaways

**On lateral movement alerts:** The same technique signature describes both an adversary moving through a network and an administrator deploying patches. The difference is entirely in surrounding context: change records, process chains, session types, and timing relative to documented activity.

**On change management:** This investigation closed cleanly because a change record existed and was findable. Organizations without disciplined change management processes create significant SOC overhead — every administrative action becomes an investigation rather than a quick verification.

**On documentation discipline:** Even when the conclusion is benign and no client email is sent, the investigation note matters. It creates the audit trail demonstrating the alert was reviewed and the closure was deliberate.
