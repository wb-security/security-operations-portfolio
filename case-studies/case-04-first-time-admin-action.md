# Case Study 04 — First Time Admin Action / Internal Group Add

**Environment:** Regional distribution company (~300 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** First Time Admin Action — user account added to privileged security group by internal admin
**MITRE ATT&CK:** T1098 (Account Manipulation), T1078.002 (Valid Accounts: Domain Accounts)
**Severity:** Medium
**Outcome:** Confirmed authorized — closed per client SOC documentation without notification

---

## Initial Alert

The SIEM generated a First Time Admin Action alert when a standard user account was added to a domain security group with elevated permissions by an internal IT administrator account.

---

## Investigation

**Account Context:**

The adding account was a known internal IT administrator with a consistent history of legitimate administrative actions. The target account belonged to a finance department user. The group they were added to had permissions scoped to a specific finance application — not broad domain admin rights, not access to security infrastructure.

Action occurred at 10:23 AM on a Tuesday — normal business hours, consistent with routine IT operations. No anomalous activity associated with either account in the surrounding time window.

**What Would Have Changed This Assessment:**
- Adding account was a service account, external identity, or newly created account
- Target group had domain admin, enterprise admin, or security infrastructure access
- Action occurred outside business hours with no corresponding change request
- Multiple accounts added to privileged groups in a short window

None of these were present.

**MITRE ATT&CK Mapping:**
- **T1098 — Account Manipulation:** Accurately describes the observed action. The technique describes the mechanism — authorized privilege assignment uses the same mechanism as adversarial privilege escalation.
- **T1078.002 — Valid Accounts: Domain Accounts:** The elevated group membership would allow domain-level permissions within the application scope.

---

## Closure Decision

This client's SOC documentation explicitly authorized closure without client notification for First Time Admin Action alerts where the action was performed by a known internal administrator and the target group did not grant domain-wide privileged access. Both conditions were met.

**This is an important distinction:** The alert was not ignored. It was fully investigated, evidence evaluated against closure criteria, criteria met, and closure documented. The difference between a skipped alert and an authorized self-closure is the investigation record.

---

## Key Takeaways

**On privilege escalation alerts:** The key variables are who made the change, what permissions were granted, when it happened, and whether it fits the pattern of normal IT activity in that environment.

**On SOC contract compliance:** Knowing when you are authorized to close without escalation is as important as knowing when to escalate. Self-closure without authorization is a compliance failure. Self-closure with authorization and documentation is correct procedure.

**On ATT&CK mapping in benign findings:** I map regardless of outcome. The mapping documents the technique observed — which is real — and the investigation record explains why it was authorized. This creates a richer record and supports pattern analysis over time.
