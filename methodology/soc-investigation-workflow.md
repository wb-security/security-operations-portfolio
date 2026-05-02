# SOC Investigation Workflow

**Author:** Warren Bowen | Tier II NOC/SOC Engineer
**Tooling:** Rapid7 InsightIDR, CrowdStrike Falcon, SentinelOne, Varonis DatAdvantage

---

## Overview

This document describes my personal investigation methodology for SOC alert triage. Applied consistently across all alert types and client environments. The goal is to move from raw alert data to a defensible, documented conclusion as efficiently as possible without skipping steps that matter.

---

## Phase 1 — Initial Alert Intake

When an alert fires, the first step is context, not action.

**What I establish immediately:**
- Alert type and tier classification (T1 vs T2)
- Asset involved — endpoint, identity, or network?
- User account associated — privileged or standard?
- Timestamp and time zone — business hours or off-hours?
- Client environment context — what does normal look like here?

The single most common mistake in SOC triage is reacting to the alert before understanding the environment. An after-hours login from an unfamiliar country means something very different for a company with global remote workers than it does for a single-site regional business.

---

## Phase 2 — Data Collection

Before forming any hypothesis, I pull all available data points.

**Identity and Authentication:**
- Source IP and geolocation
- ASN — residential ISP, VPN provider, hosting provider, or Tor exit node?
- User agent string — does it match the user's known devices and OS?
- Authentication method — MFA present? What factor type?
- Prior login history — is this pattern new?

**Endpoint (when applicable):**
- Process chain — what spawned what?
- Parent/child process relationships
- Hash reputation via CrowdStrike or SentinelOne
- File system activity — any writes, deletes, or renames?
- Network connections initiated from the endpoint

**Identity/Directory:**
- Recent account changes — password reset, group membership, MFA modification?
- Admin role assignments — when were they made and by whom?
- Inbox rules — any forwarding rules created recently?

---

## Phase 3 — MITRE ATT&CK Mapping

Every alert gets mapped to the ATT&CK framework before I write a single word of documentation. This forces precision — mapping to a specific technique requires understanding the mechanism, not just describing what happened in vague terms.

---

## Phase 4 — Hypothesis and Validation

Based on collected data, I form a primary hypothesis and at least one alternative:

- **Primary:** What is the most likely explanation for this activity?
- **Alternative:** What else could explain these exact data points?
- **Malicious threshold:** What would I need to see to confirm malicious intent?
- **Benign threshold:** What would I need to see to close this as a false positive?

I do not close alerts on gut feel. Every closure requires evidence that meets the benign threshold, or explicit client authorization per their SOC contract.

---

## Phase 5 — Escalation Decision

The escalation decision is made before any client communication is drafted.

| Condition | Action |
|---|---|
| Confirmed malicious activity | Immediate escalation — phone + email |
| Probable malicious activity | Email escalation, phone based on client SOP |
| Unconfirmed — awaiting evidence | Hold investigation open, document status |
| Confirmed benign with authorization | Close per client SOC tab guidance |
| Confirmed benign, no explicit authorization | Email client for confirmation before closing |

---

## Phase 6 — Documentation

Every investigation produces three deliverables regardless of outcome:

1. **Rapid7 Investigation Note** — technical findings, data points reviewed, conclusion, and current status. Written for the next analyst, not the client.
2. **Client Email** — structured communication translating findings to business-relevant language.
3. **Time Entry** — accurate work log capturing time spent and actions taken.

Documentation discipline is not administrative overhead. It is the difference between an investigation that stands up to review and one that does not.

---

## Key Principles

- Context before action — understand the environment before interpreting the alert
- Evidence before conclusion — form hypotheses, then validate them
- Document everything — if it is not written down, it did not happen
- Escalate rather than assume — a false positive escalation costs less than a missed incident
- Client-specific procedures are not optional
