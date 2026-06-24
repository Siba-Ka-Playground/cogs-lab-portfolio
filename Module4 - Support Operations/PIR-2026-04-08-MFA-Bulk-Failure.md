# Post-Incident Report (PIR)
**Incident ID:** INC-2026-0408-001
**Date:** 2026-04-08
**Severity:** P2
**Status:** Resolved

## Incident Summary
On April 8, 2026, at 09:15 IST, a bulk multi-factor authentication (MFA) OTP failure occurred, preventing 25 users at a major financial institution from logging into their accounts. The incident was detected through immediate customer support tickets and verified via Kaleyra gateway logs showing transaction rejections. The issue was resolved at 11:45 IST after L2 routing engineers identified a blocked Sender ID and programmatically switched traffic to a backup secondary telecommunications gateway path.

## Timeline
| Time (IST) | Event |
| :--- | :--- |
| 09:15 | Customer reported MFA failure for 25 users |
| 09:18 | Ticket #1001 created – priority P2 |
| 09:20 | First response sent to customer |
| 09:35 | Kaleyra checked – REJECTED codes confirmed |
| 10:30 | Escalated to L2 |
| 11:30 | L2 identified: Kaleyra sender ID flagged |
| 11:40 | Backup sender ID activated |
| 11:45 | Customer confirmed OTP working |

## Root Cause
The primary SMS gateway provider (Kaleyra) experienced an automated carrier-side spam filter trip, which incorrectly flagged and blacklisted our primary outbound production Sender ID. This resulted in hard `REJECTED` downstream delivery status errors for all outbound automated MFA One-Time Passwords (OTPs) sent to users on that specific carrier network. 

## Impact Assessment
- **Users affected:** 25 users
- **Duration:** 2 hours 30 minutes
- **Business impact:** External financial institution clients experienced systemic authentication blockages, halting critical financial transaction signing and administrative operations for the duration of the outage.

## Resolution Steps
1. Reviewed Kaleyra API error responses and identified consistent `REJECTED` codes on SMS transactions.
2. Escalated to L2 platform engineering to route outbound MFA traffic away from the blocked Sender ID and activate the pre-configured secondary backup Sender ID pool.

## Prevention Actions
- [ ] Action 1 – Implement an automated threshold alert in Grafana to flag SMS gateway delivery failure rates exceeding 5% within a 3-minute window. – Owner: DevOps Monitoring Team – Due: 2026-04-15
- [ ] Action 2 – Refactor the authentication service delivery layer to execute automated fallback routing to secondary Sender IDs upon detecting three consecutive provider rejection errors. – Owner: Backend Core Engineering – Due: 2026-04-30

## Open Items
- [ ] KB article to be written detailing the step-by-step manual routing override procedure for SMS Sender IDs. – Owner: Documentation Team – Due: 2026-04-12
