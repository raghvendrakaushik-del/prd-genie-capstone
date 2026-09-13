# RelayOps - API and Acceptance Catalog

SYNTHETIC SOURCE MATERIAL. Not user-approved. Product scope: relayops-pilot-v1.

## FR-001 - Filter delivery history

Provide an account-scoped delivery list. Combine supplied filters with AND. Use UTC ISO 8601 date bounds, inclusive start and exclusive end. Limit the selected interval and record age to the previous 30 days. Paginate 50 records per page, ordered by timestamp descending and delivery ID descending as the tie-breaker. An empty result displays a clear zero-results message.

GET /v2/deliveries?status=&endpoint_id=&event_type=&from=&to=&cursor=. Returns HTTP 200 with items and next_cursor; invalid filters return 400. Tenant context comes from the authenticated identity, never a caller-supplied account selector.

Priority: Must Have

AC-001
GIVEN an authenticated operator in account-pilot-a and delivery records across two accounts
WHEN the operator filters status=failed, endpoint=ep-12, event_type=invoice.paid and a valid UTC date range
THEN only account-pilot-a records matching every filter are returned in the specified order, with at most 50 records per page

AC-002
GIVEN an authenticated operator and a date range longer than 30 days or with start greater than or equal to end
WHEN the operator requests delivery history
THEN the API returns HTTP 400 with code INVALID_DATE_RANGE and the UI explains the allowed range without returning delivery records

## FR-002 - Inspect delivery evidence

Display delivery ID, endpoint ID, event type, UTC timestamp, HTTP status code or the literal network_timeout for a timeout, attempt count and last failure reason. Do not expose raw payload bodies, signing secrets or authorization headers. Show replay eligibility and the supported reason when ineligible.

GET /v2/deliveries/{delivery_id}. Returns HTTP 200 for an accessible record, otherwise 404. Existing identity checks precede data lookup.

Priority: Must Have

AC-003
GIVEN an operator with access to a failed delivery in the same account
WHEN the operator opens its detail view
THEN all seven specified evidence fields and replay eligibility are displayed, without raw payloads or secret headers

AC-004
GIVEN an operator requesting a delivery ID that does not exist in the authenticated account
WHEN the operator opens the detail view
THEN the API returns HTTP 404 with code DELIVERY_NOT_FOUND and reveals no other account metadata

## FR-003 - Request a single eligible replay

Require current replay permission and an explicit confirmation displaying delivery ID and endpoint. Accept only network timeouts or HTTP 5xx failures from the previous 30 days. Reject successful, 4xx, too-old or already-running deliveries. Reuse the immutable original payload and signing configuration. A durable acceptance transaction creates an outbox entry and replay_job_id before dispatch.

POST /v2/deliveries/{delivery_id}/replays with required Idempotency-Key and body {"reason":"operator_recovery"}. Responses: 202 accepted; 400 missing key; 401 unauthenticated; 403 missing replay permission; 404 inaccessible delivery; 409 key/body conflict; 422 ineligible; 503 failed durable acceptance.

Priority: Must Have

AC-005
GIVEN a permitted operator, an eligible same-account HTTP 5xx failure, and a fresh Idempotency-Key
WHEN the operator confirms and sends a replay request
THEN the API returns HTTP 202 with replay_job_id and queued status, and one durable replay job is accepted

AC-006
GIVEN a permitted operator and an HTTP 4xx, successful, older-than-30-days or already-running delivery
WHEN the operator requests replay
THEN the API returns HTTP 422 with code DELIVERY_NOT_ELIGIBLE and enqueues no replay job

## FR-004 - Deduplicate replay requests

Deduplicate atomically by authenticated account ID, delivery ID and Idempotency-Key for 24 hours from first acceptance. A duplicate identical request returns the original job ID and current status without enqueueing another job. A duplicate key with a different body returns a conflict. Simultaneous duplicate requests are subject to the same atomic guarantee.

The replay POST endpoint owns deduplication. The deduplication key includes tenant and delivery identifiers. Keys are not shared across accounts. Expired keys require a fresh eligibility check before any new replay.

Priority: Must Have

AC-007
GIVEN an accepted replay and its Idempotency-Key within the 24-hour window
WHEN the same account retries the identical request concurrently or sequentially
THEN every accepted response references the original replay_job_id and exactly one job has been enqueued

AC-008
GIVEN an accepted replay key within its 24-hour window
WHEN the same account and delivery reuse the key with a different request body
THEN the API returns HTTP 409 with code IDEMPOTENCY_CONFLICT and no second job is created

## FR-005 - Observe replay status

Show queued, running, succeeded or failed plus the last status-update timestamp. Poll every 15 seconds while queued/running and the browser tab is visible. Stop on terminal state or hidden tab; refresh immediately when visible again. A manual refresh control is always available. On a transient polling error, keep the last known state, label it stale and retry on the next normal poll; never claim success from missing data.

GET /v2/replay-jobs/{replay_job_id}. HTTP 200 includes replay_job_id, status, updated_at and sanitized failure_code for failed jobs. Cross-account or absent jobs return 404. State transitions are queued -> running -> succeeded|failed.

Priority: Must Have

AC-009
GIVEN a visible browser tab showing a queued or running replay
WHEN 15 seconds elapse
THEN the UI requests current status and updates only from a successful response; polling stops when succeeded or failed is returned

AC-010
GIVEN a queued replay view that becomes hidden or experiences a failed status request
WHEN the tab is hidden and later visible, or the status request fails
THEN background polling stops while hidden; visibility restoration triggers immediate refresh; a failed request preserves the last known state and displays a stale-state warning

## FR-006 - Manage replay permissions

The permission screen lists operators in the authenticated admin account and the current replay permission. Require confirmation for grant and revoke. Re-evaluate permission on every replay POST; a revocation must take effect within 60 seconds. A permission change produces an audit event containing admin actor, target operator, account, action and UTC timestamp.

PUT /v2/operators/{operator_id}/replay-permission with {"enabled":true|false}. HTTP 200 after durable update and audit; 403 for non-admins; 404 for a target outside the account.

Priority: Must Have

AC-011
GIVEN an Account Admin and an operator in the same account
WHEN the admin confirms a grant or revoke
THEN the permission record changes, an audit event is written, and the new permission is enforced on replay requests within 60 seconds

AC-012
GIVEN an Integration Operator or Auditor without the Account Admin role
WHEN the user calls the permission-management endpoint
THEN the API returns HTTP 403 with code ADMIN_ROLE_REQUIRED and makes no permission change

## FR-007 - Inspect immutable audit history

Provide a read-only audit view filterable by UTC date range, actor ID and delivery ID. Every accepted replay records actor ID, account ID, original delivery ID, replay_job_id, UTC request timestamp and current/final outcome. Terminal updates append events rather than overwriting history. Audit retention is 90 days even though replay eligibility and delivery search are 30 days. Permission-change events are also visible. No export feature is included.

GET /v2/audit-events?from=&to=&actor_id=&delivery_id=&cursor=. Returns append-only events for up to 90 days. The public pilot API exposes no audit update or delete operation.

Priority: Must Have

AC-013
GIVEN an Auditor in an account with accepted replay jobs and terminal outcomes
WHEN the auditor filters the audit view
THEN only that account's audit events appear and every replay is traceable through all six required audit fields

AC-014
GIVEN an authenticated Auditor
WHEN the auditor attempts replay, permission modification or audit modification
THEN each prohibited operation is rejected with HTTP 403 and no write or replay is performed

## FR-008 - Enforce account isolation

Derive account scope exclusively from the authenticated identity. Apply scope checks in both service authorization and data queries. Never trust an account ID from the request body or query string. Use non-disclosing 404 responses for inaccessible record IDs and 403 for prohibited actions on accessible records. Audit internal authorization failures without logging payload secrets.

All /v2 console routes use authenticated account scope. API authorization is mandatory even when the corresponding UI action is hidden.

Priority: Must Have

AC-015
GIVEN a valid user session from account-pilot-a and a delivery or replay job belonging only to account-pilot-b
WHEN the user requests that identifier or supplies a forged account selector
THEN the API returns HTTP 404 or rejects the unsupported selector and returns no account-pilot-b data

AC-016
GIVEN a cross-account test matrix covering delivery list, detail, replay, permission management and audit history
WHEN the isolation suite executes with all three pilot account identities
THEN zero cross-account reads or writes succeed and no replay job is enqueued by a cross-account request
