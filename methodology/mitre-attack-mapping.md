# MITRE ATT&CK Mapping Approach

**Author:** Warren Bowen | Tier II NOC/SOC Engineer

---

## Overview

The MITRE ATT&CK framework is a living knowledge base of adversary tactics, techniques, and procedures based on real-world observations. I use it as a precision tool to force accurate characterization of observed behavior and to contextualize findings for both technical and non-technical audiences.

---

## Why Map to ATT&CK

Vague alert descriptions like "suspicious login" or "unusual file activity" are not useful. Mapping to ATT&CK forces specificity:

- "Suspicious login" becomes **T1078 — Valid Accounts** under Initial Access or Defense Evasion
- "Unusual file activity" becomes **T1074 — Data Staged** or **T1485 — Data Destruction** depending on what the data shows
- "Inbox rule created" becomes **T1114.003 — Email Forwarding Rule** under Collection

That precision matters for documentation, client communication, and building pattern awareness over time.

---

## Common Alert Type Mappings

### Initial Access
| Alert Type | Technique | ID |
|---|---|---|
| Credential stuffing / brute force | Valid Accounts | T1078 |
| Phishing link clicked | Spearphishing Link | T1566.002 |
| MFA fatigue / push bombing | MFA Request Generation | T1621 |

### Persistence
| Alert Type | Technique | ID |
|---|---|---|
| Inbox forwarding rule created | Email Forwarding Rule | T1114.003 |
| New admin account created | Create Account | T1136 |
| Scheduled task created | Scheduled Task/Job | T1053 |

### Privilege Escalation
| Alert Type | Technique | ID |
|---|---|---|
| User added to privileged group | Account Manipulation | T1098 |
| First-time admin action | Valid Accounts: Domain Accounts | T1078.002 |

### Defense Evasion
| Alert Type | Technique | ID |
|---|---|---|
| Login from anonymizing proxy/VPN | Valid Accounts | T1078 |
| Log clearing activity | Indicator Removal | T1070 |

### Lateral Movement
| Alert Type | Technique | ID |
|---|---|---|
| Auth to multiple hosts in short window | Remote Services | T1021 |
| Pass-the-hash indicators | Use Alternate Authentication Material | T1550 |

### Collection
| Alert Type | Technique | ID |
|---|---|---|
| Large volume file access | Data from Local System | T1005 |
| Inbox forwarding to external address | Email Forwarding Rule | T1114.003 |

### Exfiltration
| Alert Type | Technique | ID |
|---|---|---|
| Large outbound transfer to cloud storage | Exfiltration to Cloud Storage | T1567.002 |
| Data sent to personal email | Exfiltration Over Web Service | T1567 |

---

## Application in Documentation

In Rapid7 investigation notes, ATT&CK references precisely classify observed behavior and link it to known adversary methodology.

In client emails, the technical classification is translated into business-relevant language while retaining the ATT&CK reference for clients with internal security teams.

**Example translation:**
- Technical: Activity consistent with T1114.003 — adversary establishing persistent collection mechanism against compromised mailbox
- Client-facing: A mail forwarding rule was identified redirecting all inbound email to an external address. This is a common technique used to maintain access to communications after an account compromise is detected and the password is reset.

---

## Framework Reference

Full ATT&CK framework: https://attack.mitre.org
Enterprise Matrix: https://attack.mitre.org/matrices/enterprise/
