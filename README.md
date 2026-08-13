# Cloudora Executive Account Takeover — Incident Response Report

**Report ID:** CLD-IR-0001  
**Engagement:** Cloudora Security Operations (fictional client — MyFirstHack training)  
**Severity:** P1  

## Scenario

Investigated an executive account takeover during a simulated client engagement: password spray -> ATO -> MFA persistence -> BEC staging.  
Tools: KQL (Microsoft Sentinel / Azure Data Explorer).  
Mapped to MITRE ATT&CK: T1110.003, T1078, T1098.005, T1564.008.  

## Summary

Between Aug 8–10, 2026, an external attacker ran a low-and-slow password spray against 26 Cloudora staff accounts from three Lagos, Nigeria IPs. Two accounts were compromised: the CEO (Daniel Reeve) and a staff member (Priya Nair). On the CEO account, the attacker registered a rogue MFA device for persistence and created a hidden inbox rule to intercept finance/invoice emails ahead of a business email compromise (BEC) fraud attempt. The incident was flagged via an impossible-travel alert, investigated, and fully contained the same day.

## Attack chain

1. **Password spraying** — low-volume guessing across 26 accounts to avoid lockouts
2. **Initial access** — successful sign-in to CEO account from an unfamiliar device/location
3. **MFA persistence** — attacker registered their own authenticator device
4. **Defense evasion / BEC staging** — hidden inbox rule to hide finance/invoice email from the account owner
5. **Lateral compromise** — second account (Priya Nair) breached the same way

## MITRE ATT&CK mapping

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force: Password Spraying | T1110.003 |
| Initial Access | Valid Accounts | T1078 |
| Persistence | Device Registration (MFA) | T1098.005 |
| Defense Evasion | Hide Artifacts: Email Hiding Rules | T1564.008 |

## Contents

- `CLD-IR-0001.pdf` — full incident report
- `kql-queries-0001.kql` — KQL detection/hunting queries used during the investigation

## Response actions taken

- Revoked active sessions on compromised accounts
- Reset credentials for both compromised accounts
- Removed attacker-registered MFA device
- Deleted malicious inbox rule
- Blocked attacker source IPs
- Reset passwords for all 24 targeted-but-not-breached accounts as a precaution

## Key recommendations

- Alert on password spray patterns (one IP hitting many accounts in a short window)
- Enforce MFA on all accounts
- Alert on new MFA device registrations and inbox rule creation
- Block sign-ins from countries with no business presence
