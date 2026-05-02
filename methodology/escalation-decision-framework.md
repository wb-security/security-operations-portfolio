# Escalation Decision Framework

**Author:** Warren Bowen | Tier II NOC/SOC Engineer

---

## Overview

The escalation decision — when to call, when to email, when to hold, when to close — is one of the highest-stakes judgment calls in SOC work. Getting this wrong in either direction has consequences: under-escalation misses real incidents; over-escalation without clear findings erodes client trust and creates alert fatigue.

---

## Step 1 — Severity and Confidence

**Severity:**
- Critical: Active compromise, ransomware indicators, data exfiltration in progress
- High: Probable compromise, privilege escalation, confirmed suspicious behavior
- Medium: Anomalous activity requiring investigation, no confirmed malicious indicator
- Low: Policy violation, benign anomaly, informational

**Confidence:**
- Confirmed: Evidence conclusively supports the conclusion
- Probable: Evidence strongly suggests the conclusion, alternative is unlikely
- Possible: Evidence is consistent with the conclusion but alternatives exist
- Unconfirmed: Insufficient data to form a conclusion

---

## Step 2 — Decision Matrix

| Severity | Confidence | Action |
|---|---|---|
| Critical | Any | Immediate phone + email |
| High | Confirmed / Probable | Phone + email |
| High | Possible / Unconfirmed | Email, hold investigation open |
| Medium | Confirmed malicious | Email escalation |
| Medium | Probable benign | Email client for confirmation |
| Medium | Confirmed benign | Close per client authorization |
| Low | Any | Email only, or close per SOC tab guidance |

---

## Step 3 — Client-Specific Procedures

The matrix above is the default. Every client SOC contract defines specific escalation requirements that take precedence:

- Call windows — some clients restrict calls to business hours or specific severity thresholds
- Self-closure authorization — some alert types are explicitly authorized for closure without notification
- Contact priority — clients define who to call first, second, and third
- Email-only clients — some clients have specified no phone escalations under any circumstances

**Client SOC procedures are not optional and are not overridden by personal assessment of urgency.**

---

## Step 4 — Communication Checklist

**Before calling:**
- Do I have a confirmed finding to report?
- Do I have the correct contact from the current client escalation sheet?
- Is the investigation documented in Rapid7 before I dial?

**Before sending email:**
- Does it follow the standardized SOC email format?
- Are all technical findings accurately represented?
- Is the MITRE ATT&CK mapping included where applicable?
- Is the correct contact list from the escalation sheet used — not inferred from memory?

---

## Step 5 — Post-Escalation Investigation Management

Investigations remain open until:
- Client confirms the activity and authorizes closure
- Client completes remediation and confirms resolution
- Client SOC tab explicitly authorizes self-closure for this alert type

**Investigations are never closed on analyst assumption alone.**

---

## Common Decision Errors and How I Avoid Them

- **Closing on familiarity** — past pattern does not substitute for present evidence
- **Contact inference** — contacts come exclusively from the current client escalation sheet
- **Urgency override** — document urgency, escalate internally if needed, follow the contract
- **Premature closure** — probable benign still requires client confirmation
