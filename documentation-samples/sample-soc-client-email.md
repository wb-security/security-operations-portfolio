# Sample — SOC Client Email

> This is a representative example of my standardized SOC client email format. All identifying details are fictional. This format is applied consistently across all client escalations.
>
> Format notes: Headers are ALL CAPS with underscore dividers. No Unicode characters. Pasted into Outlook using Keep Text Only, font set to Calibri Size 11, key terms bolded manually.

---

**Subject:** [HIGH] Security Alert — Anomalous Authentication & Inbox Forwarding Rule Detected | [CLIENT NAME] | [DATE]

---

Dear [CLIENT SECURITY CONTACT],

Our Security Operations Center has identified suspicious activity on a user account in your environment that requires your immediate attention. We have taken initial containment steps per your incident response procedures and are providing full details below.

_______________________________________________
KEY FINDINGS
_______________________________________________

- Successful authentication from a foreign IP address via legacy protocol, bypassing MFA enforcement
- Inbox forwarding rule created immediately following the anomalous authentication, redirecting all inbound mail to an external address
- Simultaneous active sessions confirmed from two geographically separated locations
- Affected account: [USER.ACCOUNT]
- Anomalous session origin: [CITY, COUNTRY] — [ASN / HOSTING PROVIDER TYPE]

_______________________________________________
TECHNICAL BREAKDOWN
_______________________________________________

At [TIME] CST, a successful authentication was recorded on account **[USER.ACCOUNT]** via **IMAP** (legacy authentication protocol) originating from **[FOREIGN IP]**, geolocating to **[CITY, COUNTRY]**. The source ASN belongs to a hosting provider with no prior association with this account or your broader environment.

This session bypassed **multi-factor authentication** enforcement. Legacy protocols such as IMAP operate outside modern conditional access policy scope and do not require MFA challenge.

Eight minutes following this authentication, an **inbox forwarding rule** was created on the account redirecting all inbound mail to **[EXTERNAL ADDRESS]** — an address with no organizational relationship to your domain.

The user's legitimate session (authenticated from [US CITY] via normal baseline) was active concurrently, confirming the anomalous session represents unauthorized third-party access.

**MITRE ATT&CK Mapping:**
- **T1078 — Valid Accounts:** Adversary authenticated using legitimate credentials via a protocol that bypassed MFA enforcement
- **T1114.003 — Email Forwarding Rule:** Server-side forwarding rule established as a persistent collection mechanism

_______________________________________________
SUMMARY OF FINDINGS
_______________________________________________

Account **[USER.ACCOUNT]** shows indicators consistent with active account compromise. An unauthorized party authenticated via a legacy protocol that bypassed MFA, then immediately established a persistent email forwarding rule to an external address. This technique allows continued email access even after a password reset, unless the forwarding rule is also removed.

_______________________________________________
ACTIONS TAKEN
_______________________________________________

The following containment steps have been executed per your incident response procedures:

1. **Password reset** completed on-premises Active Directory — [TIMESTAMP]
2. **Account disabled** on-premises Active Directory — [TIMESTAMP]
3. **All active sessions revoked** in Microsoft Entra ID — [TIMESTAMP]
4. **Source IP range blocked** via conditional access named locations policy — [TIMESTAMP]

The inbox forwarding rule has been documented but not removed pending your confirmation.

_______________________________________________
ANALYST ASSESSMENT
_______________________________________________

The combination of legacy authentication bypass, foreign-origin session, and immediate post-authentication rule creation is consistent with a deliberate account takeover. The forwarding rule suggests the intent was persistent access to communications. The simultaneous legitimate session confirms this was not the user operating from an unusual location.

_______________________________________________
RECOMMENDED ACTIONS
_______________________________________________

1. **Remove the inbox forwarding rule** on [USER.ACCOUNT] immediately
2. **Verify no other forwarding rules** exist on accounts in your environment
3. **Audit legacy authentication access** — consider disabling IMAP, POP3, and SMTP AUTH org-wide
4. **Review MFA conditional access policy** to enforce modern authentication only
5. **Review sent items and recent email activity** for the prior 30 days
6. **Notify the user** so they are aware of the compromise

_______________________________________________
RISK ASSESSMENT
_______________________________________________

**Current Risk Level: HIGH**

Containment steps have reduced immediate risk. However, the active forwarding rule represents continued data exposure. Until removed, all inbound mail is being copied to an external address outside your control.

_______________________________________________
CURRENT STATUS
_______________________________________________

Initial containment complete. Investigation remains open pending your confirmation of forwarding rule removal and completion of recommended remediation steps.

_______________________________________________
RAPID7 LOG ENTRY
_______________________________________________

[Raw log data attached separately]
