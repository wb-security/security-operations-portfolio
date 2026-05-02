# Case Study 03 — Microsoft Teams Social Engineering / IT Helpdesk Impersonation

**Environment:** Regional home improvement services organization (~200 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** Suspicious Microsoft Teams Activity — Foreign Tenant Chat Initiation
**MITRE ATT&CK:** T1566 (Phishing), T1566.004 (Spearphishing via Service), T1534 (Internal Spearphishing)
**Severity:** Medium
**Outcome:** Confirmed social engineering attempt — client notified, watchlist entry created

---

## Initial Alert

Rapid7 flagged suspicious Microsoft Teams activity showing a foreign tenant account
initiating direct chat requests to multiple internal users within a one-minute window.

The external account details:
- **Display name:** "HelpDesk | Corporate IT Service (Internal)"
- **Tenant:** Default .onmicrosoft.com domain with no custom vanity domain
- **Activity:** ChatCreated events targeting two internal users in rapid succession

---

## Investigation

**Tenant Analysis:**

The source tenant used a default .onmicrosoft.com domain. Legitimate enterprise
organizations at any significant scale almost universally configure a custom vanity
domain for Microsoft 365. A default .onmicrosoft.com domain as the only identifier
is a strong indicator of a purpose-built malicious tenant created specifically for an
attack campaign rather than an established organization with a legitimate business
relationship.

**Display Name Analysis:**

The account presented as "HelpDesk | Corporate IT Service (Internal)." Three elements
of this display name were specifically engineered to deceive:

- "HelpDesk" — impersonates the IT support function users trust for credential resets
- "Corporate IT Service" — implies internal branding
- "(Internal)" — explicitly claims to be internal despite being an external tenant account

This is a textbook IT helpdesk impersonation pattern. The goal is to establish apparent
legitimacy so a user responds and either provides credentials, installs remote access
tooling, or follows instructions that facilitate further compromise.

**Activity Pattern Analysis:**

Two ChatCreated events targeting two different internal users within one minute. This
rapid sequential targeting is consistent with an automated or semi-automated campaign.
The timing suggests the attacker had a target list and was working through it
systematically — not a single opportunistic contact.

**Engagement Assessment:**

Available logs showed ChatCreated events — the foreign account initiated the chat.
No MessageSent, MessageRead, or response activity from the internal users was visible.
The threat was confirmed real but engagement was unconfirmed. The appropriate response
was to notify the client and ask them to validate whether either targeted user had
interacted with the chat.

**MITRE ATT&CK Mapping:**
- **T1566 — Phishing:** Social engineering via electronic communication
- **T1566.004 — Spearphishing via Service:** Using Microsoft Teams as the vector,
  bypassing email security controls entirely
- **T1534 — Internal Spearphishing:** Impersonating an internal resource to establish
  trust with the target

---

## Why Teams as an Attack Vector

Teams-based social engineering bypasses traditional email security controls. Organizations
with mature email security — safe links, attachment scanning, anti-phishing policies —
have no equivalent controls on Teams external chat by default. An attacker initiating
a Teams chat has a direct channel that:

- Bypasses email filtering entirely
- Appears in the same interface used for legitimate internal communication
- Can display a display name that looks internal even when the account is external
- Allows file sharing, link sharing, and voice/video calls

Microsoft 365 does not prevent external tenant accounts from setting display names that
impersonate internal resources. The only reliable indicator of external origin is the
email domain suffix — which is not prominently displayed in the Teams UI by default.

---

## Client Communication

Client notified via email with:
- Full account details of the malicious tenant
- Names of both targeted internal users
- Log evidence showing chat initiation without confirmed engagement
- Recommendation to block the source tenant via Teams external access settings
- Guidance that if engagement occurred, immediate password reset and session revocation
  were warranted

Watchlist entry created in Rapid7 for the malicious tenant domain.

---

## Key Takeaways

**On Teams as an attack surface:** Organizations with mature email security frequently
have a blind spot on Teams. Restricting external access to approved domains — or
disabling it unless a business need exists — is a high-value, low-disruption control.

**On engagement vs. initiation:** The investigation correctly separated confirmed
(chat initiation) from unconfirmed (user engagement). Escalating to full incident
response without confirmed engagement would have been disproportionate. Closing
without notification because no engagement was confirmed would have been a missed
obligation. Notify with full context and ask the client to validate.

**On watchlist entries:** Even when an investigation closes without confirmed compromise,
creating a watchlist entry for the malicious tenant domain ensures any future contact
from the same source triggers immediate recognition rather than a fresh triage cycle.
