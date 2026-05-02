# Case Study 01 — Inbox Forwarding Rule / Account Compromise

**Environment:** Mid-size regional healthcare organization (~400 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** Suspicious inbox rule creation + anomalous authentication
**MITRE ATT&CK:** T1114.003 (Email Forwarding Rule), T1078 (Valid Accounts)
**Severity:** High
**Outcome:** Confirmed compromise — four-step remediation executed prior to client notification

---

## Initial Alert

The SIEM fired on two correlated events within a 12-minute window:

1. Successful authentication from an Eastern European IP address via IMAP, bypassing MFA
2. Inbox forwarding rule created on the same account, forwarding all inbound mail to an external Gmail address

---

## Investigation

**Authentication Analysis:**

The source IP resolved to a Ukrainian hosting provider with no prior appearance in this user's login history. The authentication method used was IMAP — a legacy protocol that does not support modern MFA enforcement in most configurations. This is a well-documented technique for bypassing conditional access policies that only apply to modern authentication flows.

The user's normal login pattern showed consistent authentication from a US-based residential ISP using a Windows device and the Outlook desktop client. The IMAP session represented a complete deviation from baseline in three dimensions simultaneously: geography, ASN type, and authentication protocol.

**Forwarding Rule Analysis:**

The rule was created approximately 8 minutes after the anomalous IMAP session authenticated. The rule was configured to forward all inbound mail to an external Gmail address with no organizational relationship to the company.

This combination — legacy auth bypass followed immediately by forwarding rule creation — is a textbook account takeover pattern. The adversary authenticated via a protocol that bypassed MFA, then immediately established a persistent collection mechanism so that even after a password reset, inbound mail would continue to route to their controlled address.

**MITRE ATT&CK Mapping:**
- **T1078 — Valid Accounts:** Adversary used legitimate credentials, avoiding detection mechanisms that look for failed logins
- **T1114.003 — Email Forwarding Rule:** Established persistent email collection via server-side rule, surviving password resets until the rule itself is removed

---

## Remediation (Pre-Notification)

Per this client's aggressive compromise response SOP, four steps were executed before client notification:

1. **Password reset** on the affected account via on-premises Active Directory
2. **Account disabled** on-premises to prevent re-authentication during investigation
3. **All active sessions revoked** in Microsoft Entra ID — invalidating any existing OAuth tokens including the active IMAP session
4. **Source IP blocked** via conditional access policy in Entra

The forwarding rule was documented but not removed prior to client notification — rule removal was left to the client's internal team to confirm scope.

---

## Key Takeaways

**For detection:** Correlating anomalous authentication with immediate post-authentication activity is far more signal-rich than treating each event in isolation. Neither event alone was necessarily conclusive — together they told a clear story.

**For response:** A password reset alone is insufficient if an IMAP session is still active. Session revocation is required. A password reset is also insufficient if a forwarding rule persists — the collection mechanism survives the credential change.

**For prevention:** Legacy authentication protocols (IMAP, POP3, SMTP AUTH) should be disabled organization-wide unless a specific business justification exists.
