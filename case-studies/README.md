# Case Studies

## Anonymization Notice

All case studies in this directory are reconstructed from real investigations conducted
during my work as a Tier II NOC/SOC Engineer. All client-identifying information has been
removed and replaced with fictional industry verticals and generic organization descriptors.
Usernames, IP addresses, asset names, email addresses, and any other identifying details
have been redacted or replaced with representative placeholders.

The technical methodology, decision logic, MITRE ATT&CK mappings, escalation decisions,
and analytical reasoning in each case study are genuine — they reflect how I actually
conducted these investigations in production managed service environments.

---

## Case Index

| File | Alert Type | MITRE Technique | Outcome |
|---|---|---|---|
| case-01-impossible-travel-benign.md | Impossible Travel — iCloud Private Relay FP | T1078 | Confirmed benign — relay infrastructure identified |
| case-02-impossible-travel-suspicious.md | Impossible Travel — Privileged Account | T1078, T1078.002 | Confirmed suspicious — account disable requested |
| case-03-teams-social-engineering.md | Teams Foreign Tenant / IT Impersonation | T1566, T1566.004, T1534 | Confirmed social engineering — client notified |
| case-04-crowdstrike-anomalous-upload.md | CrowdStrike Anomalous Data Upload | T1567 | Escalated — pending authorization confirmation |

---

## Reading These Together

Cases 01 and 02 are intentionally paired. Both are impossible travel alerts. Both involve
the same MITRE technique. They produced completely opposite outcomes based on account type,
geographic distance, ASN classification, and whitelist status. The pairing illustrates that
alert type does not determine response — context does.
