# Case Study 01 — Impossible Travel / iCloud Private Relay False Positive

**Environment:** Multi-site retail distribution organization (~500 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** Suspicious Authentication — Multiple Country / Impossible Travel
**MITRE ATT&CK:** T1078 (Valid Accounts)
**Severity:** High
**Outcome:** Confirmed benign — iCloud Private Relay identified as geographic discrepancy cause

---

## Initial Alert

Rapid7 fired a High priority impossible travel alert on a standard user account showing
two successful authentications within a 55-minute window:

- **Session A:** Corporate workstation authentication from the user's primary work location
  (US-domestic, Hybrid Azure AD joined, compliant managed device)
- **Session B:** Mobile authentication resolving to a geolocation approximately 15 miles
  from Session A but flagging as a different region in GeoIP databases

Both sessions completed MFA successfully. Both originated from US-domestic locations.

---

## Investigation

**Session A Analysis:**

Authentication from a compliant, managed, Hybrid Azure AD joined corporate workstation.
Device posture confirmed via Entra ID — IsCompliant: True, IsManaged: True. User agent
consistent with Windows desktop client. Authentication method: modern auth with MFA
satisfied. Session matched the user's established baseline in every dimension.

**Session B Analysis:**

Authentication from an iPhone. Source IP resolved to a geolocation approximately 15 miles
from Session A. MFA satisfied. The key finding: the source IP for Session B belonged to
an **iCloud Private Relay exit node**.

iCloud Private Relay routes traffic through Apple relay infrastructure before forwarding
to the destination, masking the device's true IP and replacing it with an Apple relay
exit node IP. These exit node IPs resolve to geolocations that do not reflect the user's
physical position. The exit node is Apple infrastructure — not the user's location.

**Conditional Access Evaluation:**

Azure AD evaluated all applicable conditional access policies on both sessions. Neither
session was blocked. No risk events were flagged. No impossible travel signal was generated
by Entra ID's own risk engine — the impossible travel flag came from the SIEM correlating
the two IP geolocations, not from the identity provider's risk evaluation.

**MITRE ATT&CK Mapping:**
- **T1078 — Valid Accounts:** Technique surface is present. Investigation determined
  this was legitimate use, not adversarial.

---

## Differentiating Factors — Benign vs. Suspicious

| Factor | This Investigation | Suspicious Indicator Would Be |
|---|---|---|
| Geographic distance | 15 miles | Intercontinental |
| Time window | 55 minutes | Under 30 minutes |
| Foreign session ASN | Apple relay infrastructure | Residential or hosting provider ISP |
| Entra ID risk events | None | Medium or High risk flagged |
| Device posture | Compliant managed devices | Unmanaged or non-compliant |
| Post-auth activity | Normal | Reconnaissance pattern |

---

## Client Communication

Client notified per escalation procedure with full technical details including the iCloud
Private Relay explanation. Client asked to confirm the user accessed their work PC and
then their iPhone within the identified time window. Investigation held open pending
client confirmation.

---

## Key Takeaways

**On iCloud Private Relay:** A recurring false positive source in Microsoft 365 environments.
The ASN lookup on the mobile session IP is the fastest identification method — Apple relay
exit nodes resolve to Apple Inc. ASN (AS714). Once identified, the investigation pivots
from impossible travel analysis to confirming the user's normal mobile access pattern.

**On alert fidelity vs. risk engine signals:** When a SIEM impossible travel alert fires
but the identity provider's own risk engine shows no risk events, that gap is significant
data. The SIEM correlates IPs; the identity provider evaluates full authentication context.
Disagreement between them warrants explanation, not automatic escalation.

**On documentation:** Even clearly benign alerts require full investigation documentation.
The iCloud Private Relay identification is the finding. That explanation lives in the
investigation note and protects both the client and the SOC if the pattern recurs.
