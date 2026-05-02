# Case Study 02 — Impossible Travel / Confirmed Suspicious — Privileged Account

**Environment:** Regional distribution company (~300 users)
**Alert Source:** Rapid7 InsightIDR
**Alert Type:** Suspicious Authentication — Multiple Country / Impossible Travel
**MITRE ATT&CK:** T1078 (Valid Accounts), T1078.002 (Valid Accounts: Domain Accounts)
**Severity:** High
**Outcome:** Confirmed suspicious — account disable requested, investigation escalated

---

## Initial Alert

Rapid7 fired on a privileged admin account showing two successful authentications within
a 26-minute window from geographically impossible locations:

- **Session A:** Authentication from a corporate/business ISP, United States
- **Session B:** VPN authentication from a residential ISP in Mexico — 26 minutes later

Physical travel between these locations in 26 minutes is not possible. One session was
not the legitimate account holder.

---

## Investigation

**Account Context:**

The affected account was a privileged administrator account — not a standard user.
Admin-level accounts represent elevated risk in impossible travel scenarios because a
threat actor with these credentials has significantly broader access than a compromised
standard user. This raised the severity weighting of the investigation immediately.

**Session A Analysis:**

Authentication from a corporate ISP. Device naming convention consistent with known
managed infrastructure. User agent indicated Azure AD authentication provider — consistent
with automated or infrastructure-level authentication rather than interactive session.
Assessed as the likely legitimate session based on ISP type, device naming, and
authentication pattern.

**Session B Analysis:**

VPN authentication from a residential ISP in Mexico. No prior appearance of this ASN
or geolocation in the account's authentication history. The VPN authentication was
critical — this meant a threat actor, if the session was unauthorized, had active
access to the internal network, not just cloud services.

The residential ISP classification was significant. This was not a corporate network,
not a known VPN provider, and not infrastructure consistent with legitimate multi-site
business operations.

**Whitelist Check:**

The client maintained a pre-approved list of users authorized for cross-border
authentication. The affected account was not on this list. This absence confirmed
the Mexico authentication did not fit any approved access pattern.

**MITRE ATT&CK Mapping:**
- **T1078 — Valid Accounts:** Adversary authenticated using legitimate admin credentials
- **T1078.002 — Valid Accounts: Domain Accounts:** Privileged domain account targeted,
  with VPN authentication granting internal network access

---

## Comparison: Why This Escalated When Case 01 Did Not

| Factor | Case 01 (Benign) | Case 02 (Suspicious) |
|---|---|---|
| Account type | Standard user | Privileged admin |
| Geographic distance | 15 miles | Intercontinental |
| Time window | 55 minutes | 26 minutes |
| Foreign session ASN | Apple relay infrastructure | Residential ISP |
| Whitelist status | N/A | Not approved |
| VPN / internal network access | No | Yes |

Every factor that pointed to benign in Case 01 pointed the opposite direction here.

---

## Actions Taken

Per client incident response procedure:
- ServiceNow ticket created and assigned to client IT Service Desk for immediate
  account disable
- Client security team notified via email with full technical findings and timeline
- Investigation documented in Rapid7 with full analysis

Account disable was requested before client email response because the active VPN
session meant potential internal network access was ongoing. The risk of allowing
continued internal network access while waiting for email response outweighed the
risk of a false positive account disable on a privileged account.

---

## Key Takeaways

**On privileged account impossible travel:** The risk calculus changes when the account
is privileged. A threat actor with admin credentials authenticated via VPN has
substantially broader internal network access — the response timeline compresses
accordingly.

**On residential ISP vs. Apple relay:** Case 01 resolved benign partly because the
foreign session IP belonged to Apple infrastructure — a known, explainable source.
This case showed a residential ISP in a foreign country with no prior history and no
approved access pattern. ASN classification is a fast, high-signal data point.

**On whitelists as investigation tools:** The client's pre-approved cross-border
authentication list was a functional investigation tool, not just a compliance artifact.
Checking it took under two minutes and produced a definitive answer. Organizations
without this baseline make impossible travel investigations significantly harder.

**On the value of pairing these two cases:** Cases 01 and 02 illustrate why impossible
travel alerts cannot be handled with a fixed response playbook. The same alert type,
the same MITRE technique, completely different outcomes based on account type,
geographic context, ASN classification, and whitelist status. The analytical skill
is pattern recognition across these variables — not the alert type itself.
