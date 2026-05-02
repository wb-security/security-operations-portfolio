# NOC Alert Triage Workflow

**Author:** Warren Bowen | Tier II NOC/SOC Engineer

---

## Overview

NOC triage is about speed and accuracy in parallel. The goal is to correctly classify every alert — real outage, false positive, or monitoring anomaly — as fast as possible, then take the right action.

---

## Alert Classification

| Classification | Definition | Action |
|---|---|---|
| Active Outage | Device/service confirmed down, no maintenance window | Escalate per client SOP |
| Planned Maintenance | Device/service down within confirmed maintenance window | Acknowledge, monitor, no escalation |
| Flapping | Intermittent up/down pattern, no sustained outage | Investigate trend, hold 15-30 min before escalating |
| Threshold Alert | Metric crossing alert threshold (disk, CPU, bandwidth) | Investigate trend data before escalating |
| False Positive | Alert triggered by known benign condition | Document and clear |
| Agent/Monitoring Issue | Monitoring tool connectivity issue | Verify via alternate source before escalating |

---

## Phase 1 — Verification

Before contacting the client, I verify the alert independently.

**Verification steps:**
- Cross-reference across monitoring platforms — do SolarWinds, OpsRamp, and Statseeker agree?
- Check if the device has a history of false positive alerts
- Verify whether a maintenance window is active for this device or client
- For network devices, check upstream dependencies — is the router down? That explains everything behind it alerting
- For threshold alerts, pull trend data — sudden spike or slow-building trend?

The upstream dependency check is critical for multi-device alerts. When an access point goes down at the same time as the router it connects through, the root cause is the router. Escalating every device separately when a single upstream failure explains all of them creates confusion and makes the client's response harder.

---

## Phase 2 — Client Communication

**Subject line:** Always includes the SNOW ticket number, client name, and brief description.

**Multi-device alerts:** All affected devices are grouped into a single email. This gives the client a complete picture of impact in one communication.

**After-hours:** Client-specific call windows determine whether phone escalation accompanies the email. Client procedures take precedence over general judgment.

---

## Phase 3 — OpsRamp Documentation

Every action taken is documented in the ticket:
- What was verified and how
- What was sent and to whom
- Current status and next step

Documentation does not restate alert details — it captures what was done, not what triggered it.

---

## Phase 4 — Follow-Through

Alerts are not closed until:
- Client confirms the issue is resolved or was planned
- Device/service returns to normal and client is notified
- Alert is confirmed as a false positive with documentation
