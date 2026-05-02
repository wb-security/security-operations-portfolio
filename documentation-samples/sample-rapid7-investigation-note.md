# Sample — Rapid7 Investigation Note

> This is a representative example of my Rapid7 InsightIDR investigation note format. All identifying details are fictional. The structure, terminology, and documentation approach are genuine.

---

## Investigation Note — Anomalous Authentication / Inbox Rule Creation

**Investigation Title:** Anomalous Authentication + Inbox Forwarding Rule — [USER.ACCOUNT]
**Assigned Analyst:** W. Bowen
**Date/Time Opened:** [DATE] [TIME] CST
**Severity:** High
**Status:** Open — Awaiting Client Confirmation

---

**ALERT SUMMARY**

Two correlated events triggered within a 12-minute window on [USER.ACCOUNT]:

1. Successful IMAP authentication from [FOREIGN IP] — [COUNTRY], ASN: [HOSTING PROVIDER]
2. Inbox forwarding rule created redirecting all inbound mail to [EXTERNAL ADDRESS]

---

**INVESTIGATION FINDINGS**

Authentication event reviewed. Source IP [FOREIGN IP] geolocates to [CITY, COUNTRY]. ASN belongs to [HOSTING PROVIDER TYPE] — no residential or corporate classification consistent with legitimate user activity. IP has no prior appearance in client environment authentication logs.

Authentication method: IMAP (legacy protocol). User's established baseline shows exclusively modern authentication (Outlook desktop client, Windows device, [CORPORATE ISP ASN]). IMAP session bypassed MFA enforcement — conditional access policy does not apply to legacy auth protocols in current client configuration.

Forwarding rule created 8 minutes post-authentication. Rule scope: all inbound mail. Destination: [EXTERNAL GMAIL ADDRESS] — no organizational relationship to client domain.

User's concurrent Session A (authenticated from [US CITY] via normal baseline) showed no anomalous post-authentication activity. Session A and Session B were active simultaneously — confirming account access by two separate parties.

---

**MITRE ATT&CK**

- T1078 — Valid Accounts (Initial Access / Defense Evasion)
- T1114.003 — Email Forwarding Rule (Collection)

---

**REMEDIATION EXECUTED**

Per client incident response SOP:
1. Password reset executed on-premises AD — [TIMESTAMP]
2. Account disabled on-premises AD — [TIMESTAMP]
3. All active Entra sessions revoked — [TIMESTAMP]
4. Source IP range [FOREIGN IP/CIDR] blocked via conditional access named locations — [TIMESTAMP]

Forwarding rule documented — removal deferred to client confirmation per SOP.

---

**ACTIONS TAKEN**

Client notified via email — [TIMESTAMP]. Full technical findings, remediation steps taken, and recommended next steps included. Call not placed per client escalation procedure (email escalation for High severity, business hours).

---

**NEXT STEPS / OPEN ITEMS**

- Awaiting client confirmation of remediation completion
- Client to confirm forwarding rule removal
- Recommend client audit legacy authentication protocol access — disable IMAP/POP3 org-wide
- Recommend MFA policy review to enforce modern auth only

---

**STATUS:** Open — client email sent [TIMESTAMP], awaiting response
