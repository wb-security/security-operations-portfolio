# Device Lifecycle Management

**Author:** Warren Bowen | Tier II NOC/SOC Engineer

---

## Overview

Device lifecycle management covers two primary workflows: onboarding new devices into monitoring and decommissioning devices being removed from the environment. Gaps in either direction create monitoring blind spots or ghost alerts on devices that no longer exist.

---

## Device Decommission Workflow

### Step 1 — REQs Spreadsheet

The REQs spreadsheet is the master record of all monitored devices per client. Before touching any monitoring system:

- Locate the device across all tabs — devices sometimes appear in multiple places
- Move the record to the Decommissioned section
- Note the decommission date and requestor name

This step happens first because it is the authoritative record. If the REQs spreadsheet is not updated, the device remains in scope for monitoring from a contractual standpoint.

### Step 2 — OpsRamp

1. Verify SNMP polling is active on the device
2. Remove the device from the OpsRamp console
3. Remove the device from any downtime schedules — failing to do this leaves orphaned rules that cause confusion on future maintenance windows

### Step 3 — Buddy Check

**All decommissions require a buddy check before closing.** The completed checklist is reviewed by a second engineer to confirm all steps were executed. The buddy check is documented in the ticket before closure.

This is a standing requirement with no exceptions — the consequences of an incomplete decommission (ghost alerts, missed billing updates, contractual gaps) are disproportionate to the time cost of a second set of eyes.

---

## Why Lifecycle Discipline Matters

In a large managed services portfolio, monitoring drift accumulates quietly. Devices decommissioned without full removal generate false alerts. Devices onboarded without complete configuration create coverage gaps. Neither problem is immediately obvious — both become apparent at the worst time, when something is actually wrong and the noise-to-signal ratio is too high to act quickly.

Consistent execution of lifecycle procedures is the operational discipline that keeps monitoring accurate over time.
