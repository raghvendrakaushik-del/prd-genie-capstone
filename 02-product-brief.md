# RelayOps - Detailed Product Brief

SYNTHETIC SOURCE MATERIAL. Not user-approved. Product scope: relayops-pilot-v1.

Fictional product scenario. Product-team characters, accounts, budgets and measurements are synthetic; the document author and actual reviewer are user-supplied identities.

A tenant-isolated desktop console that lets integration operators investigate failed webhook deliveries and replay eligible events without an engineering-owned script. A three-account pilot covers investigation, permission management, replay orchestration and audit history; the existing identity service and Delivery API v2 remain the systems of record.

Support currently collects an event identifier, asks an engineer to inspect logs, waits for an engineering script to replay the event, and asks again for the result. This creates repeated handoffs, weak operator visibility and inconsistent replay audit records.

## Business goal
Reduce the median elapsed time from a failed-delivery support report to a confirmed replay result from a synthetic baseline of 45 minutes to at most 15 minutes during the four-week pilot.

## User goal
Enable authorized Integration Operators to find failures, inspect evidence, start one eligible replay and see its final outcome without requesting script access.

## Pilot delivery
- budget: Synthetic pilot engineering budget: USD 48,000, including a USD 3,000 infrastructure ceiling for the four-week pilot.
- team: One PM, one designer, two backend engineers, one frontend engineer and one QA engineer; security and identity owners provide part-time review.
- rollout: Enable the feature for exactly three fictional accounts, account-pilot-a, account-pilot-b and account-pilot-c, on 2026-10-12 after go/no-go approval.
- rollback: Disable replay controls using the per-account feature flag; continue read-only inspection and let already accepted jobs finish. Do not delete queued jobs or audit records.
- support: Core Events owns worker incidents; Identity owns authorization incidents; Security owns audit gaps. Maya coordinates the pilot incident review.

## Personas
### Integration Operator
- need: Investigate and replay eligible failed deliveries in the operator's own account.
- workaround: Open a support ticket and ask engineering to run an internal replay script.
- permissions: Read delivery records; replay only when an Account Admin has explicitly granted replay permission.

### Account Admin
- need: Control which operators may initiate replay within the admin's account.
- workaround: Ask the identity team to change role assignments manually.
- permissions: Grant or revoke operator replay permission; no cross-account administration.

### Auditor
- need: Inspect delivery history and replay audit evidence within the auditor's account.
- workaround: Request CSV extracts from engineering and correlate ticket IDs manually.
- permissions: Read delivery and audit history only; cannot replay or change permissions.

## Success metrics
### Median resolution time
- baseline: 45 minutes across 20 fictional historical tickets
- target: At most 15 minutes
- window: Four-week pilot, 2026-10-12 through 2026-11-08
- measurement: Median of support_reported_at to replay_terminal_at for eligible resolved pilot tickets; report unresolved-ticket count separately to avoid survivor bias.
- owner: Noor Ali, Product Analytics

### Self-service completion rate
- baseline: 0% in the fictional manual process
- target: At least 80% of eligible replay incidents completed without an engineer running a script
- window: Same four-week pilot
- measurement: Eligible completed incidents with operator-started replay divided by all eligible completed incidents; exclude ineligible 4xx failures and publish the exclusion count.
- owner: Noor Ali, Product Analytics

### Audit completeness
- baseline: 60% of 20 fictional historical tickets have a traceable actor and outcome
- target: 100% of accepted replay jobs have all required audit fields and a terminal or current state
- window: Daily checks throughout pilot
- measurement: Reconcile accepted replay_job_id values against append-only audit events; any missing identifier or actor fails release readiness.
- owner: Isha Shah, Security Engineering
