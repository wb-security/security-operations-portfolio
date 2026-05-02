# Case Study 03 — Impossible Travel / Concurrent Sessions

**Environment:** Regional professional services firm (~180 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** Impossible Travel — authentication from geographically separated locations within physically impossible time window
**MITRE ATT&CK:** T1078 (Valid Accounts), T1621 (Multi-Factor Authentication Request Generation)
**Severity:** High
**Outcome:** Escalated to client — investigation held open pending confirmation

---

## Initial Alert

Two successful authentications recorded on a finance department user account:

- **Session A:** Chicago, IL at 08:42 AM CST — recognized ASN, Windows device, Outlook client
- **Session B:** Lagos, Nigeria at 09:17 AM CST — 35 minutes later, unrecognized ASN, Android device, mobile browser

Physical travel between Chicago and Lagos in 35 minutes is not possible. One of these sessions was not the legitimate user.

---

## Investigation

**Session A:** Consistent with established baseline across every measurable dimension — ASN, device type, application, time of day. Normal post-authentication activity.

**Session B:** Complete deviation from baseline. The Nigerian mobile carrier ASN had no prior appearance in this user's history or the broader client environment. Post-authentication activity showed immediate navigation to sent items and inbox — behavior consistent with account reconnaissance rather than normal work activity.

**MFA Analysis:**

Both sessions successfully completed MFA. This means either the adversary had access to the MFA factor, or the MFA policy had a gap allowing Session B to satisfy authentication without a fresh challenge. MFA success on an anomalous session is not a reason to downgrade severity — it is an additional finding requiring explanation.

**MITRE ATT&CK Mapping:**
- **T1078 — Valid Accounts:** Adversary authenticated with legitimate credentials
- **T1621 — Multi-Factor Authentication Request Generation:** Flagged as possible given MFA was satisfied on the anomalous session

---

## Escalation Decision

High severity, probable malicious assessment. Client escalation procedures specified email for High alerts during business hours, with phone reserved for confirmed active compromise.

Session B had occurred 47 minutes prior to alert review — not confirmed active at investigation time. Decision: email escalation with explicit recommendation to verify whether Session B was still active and treat as active compromise requiring immediate session revocation if so.

The additional 8 minutes to draft a thorough email gave the client's security team better information than an immediate call without full findings prepared.

---

## Key Takeaways

**On impossible travel:** Not all impossible travel alerts are genuine compromise. VPN, split tunneling, and corporate proxy configurations can produce false signals. The differentiating factor is post-authentication behavior — an adversary in a compromised account often shows reconnaissance patterns immediately after authentication.

**On MFA success:** A successful MFA challenge on a session that looks like a compromise is a signal, not a reassurance.

**On escalation timing:** A thorough email sent quickly often serves the client better than an immediate call without full findings. The goal is giving the client everything they need to act.
