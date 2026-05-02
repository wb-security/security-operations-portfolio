# Case Study 04 — CrowdStrike Anomalous Data Upload / Potential Exfiltration

**Environment:** Regional manufacturing and sales organization (~150 users)
**Alert Source:** Rapid7 InsightIDR (CrowdStrike Falcon Data Protection integration)
**Alert Type:** CrowdStrike Falcon Data Protection — Anomalous File Upload to Web Destination
**MITRE ATT&CK:** T1567 (Exfiltration Over Web Service)
**Severity:** Medium
**Outcome:** Escalated to client — investigation held open pending authorization confirmation

---

## Initial Alert

CrowdStrike Falcon Data Protection generated an anomalous upload alert on a sales
department endpoint:

- **Volume:** 1.86 GiB across 15 files
- **Destination:** WordPress staging platform subdomain — path: /wp-admin/plugin-install.php
- **Falcon anomaly metric:** 140x the user's established self-baseline for upload volume

---

## Investigation

**Volume Analysis:**

The 140x self-baseline metric was the first critical data point. CrowdStrike Falcon Data
Protection establishes a behavioral baseline for each user's normal upload activity and
flags deviations. A 140x deviation is not marginal — it represents upload volume two
orders of magnitude above this user's normal behavior.

1.86 GiB across 15 files — approximately 127 MB per file on average — is not consistent
with casual browsing, routine document uploads, or normal SaaS application behavior.
This volume suggests either deliberate bulk transfer or an automated process.

**Destination Analysis:**

The destination resolved to a WordPress staging environment hosted on a legitimate
managed WordPress hosting platform. The specific path — /wp-admin/plugin-install.php
— is the WordPress admin interface for installing plugins.

This destination produced two competing hypotheses:

**Hypothesis A (Exfiltration):** A threat actor or malicious insider is uploading
company data to an externally controlled WordPress installation as a staging point.
The /wp-admin path would be unusual for a non-developer unless they controlled the
WordPress installation.

**Hypothesis B (Authorized work):** The user is performing authorized WordPress
development or staging work — uploading assets, themes, or plugin files to a company
or client installation.

**User Role Profiling:**

The affected user was in the sales department. A standard sales role has no typical
business reason to upload 1.86 GiB to a WordPress admin plugin installation interface.
This role-activity mismatch increased the weight of Hypothesis A without resolving it —
sales organizations sometimes have hybrid roles, and the user's actual responsibilities
were not fully known from alert data alone.

**Destination Legitimacy as a Mixed Signal:**

The hosting platform was legitimate — not a bulletproof host, Tor exit node, or
paste site. This increased the probability of Hypothesis B. However, legitimate platforms
are frequently used as exfiltration staging points because their traffic blends with
normal web activity and is less likely to be blocked. Destination legitimacy informed
the severity assessment but did not resolve the investigation.

**MITRE ATT&CK Mapping:**
- **T1567 — Exfiltration Over Web Service:** Large volume transfer to a web-based
  destination. Technique surface confirmed regardless of intent determination.

---

## Escalation Decision

The combination of 140x behavioral deviation, 1.86 GiB volume, sales department user,
and WordPress admin path warranted client notification rather than self-closure. The
legitimate hosting platform prevented high-confidence malicious classification, but
the behavioral anomaly was too significant to close without client confirmation.

Medium severity, Possible malicious assessment — email escalation per client procedure.
Phone escalation not warranted at this confidence level.

---

## Client Communication

Client notified via email including:
- Full alert details — user, endpoint, destination, volume, file count, Falcon baseline metric
- Both hypotheses presented transparently
- Specific ask: confirm whether the user has a legitimate business reason to upload
  1.86 GiB to the identified WordPress staging environment via the plugin install path
- Note that if activity was not recognized as authorized, immediate endpoint and account
  investigation was recommended

Investigation held open pending client response.

---

## Key Takeaways

**On behavioral baseline metrics:** The self-baseline deviation multiple is more useful
than raw volume thresholds because it contextualizes activity relative to that specific
user's normal behavior. A 140x deviation on a user who normally uploads 10 MB/day is
meaningfully different from a 2x deviation on a user who routinely works with large files.
Always look at the baseline multiple, not just the raw volume.

**On destination legitimacy as a mixed signal:** A legitimate hosting platform as the
upload destination is evidence against simple exfiltration but not exculpatory. Legitimate
platforms are used as staging points precisely because their traffic blends with normal
web activity. Destination legitimacy informs severity assessment — it does not close
the investigation.

**On user role profiling:** Role-based context is a fast, high-value filter for data
protection alerts. When activity fits the role, the bar for confirmation is lower. When
it does not fit — sales employee uploading to WordPress admin — confirmation is required.

**On holding open vs. closing:** This alert could not be closed as benign without
client confirmation because the behavioral anomaly was real and significant. It also
could not be escalated as confirmed exfiltration because an authorized work explanation
was plausible. When evidence is genuinely ambiguous, hold open with a clear client
ask — not a premature close or an overconfident escalation.
