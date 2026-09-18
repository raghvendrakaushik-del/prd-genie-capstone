# RelayOps - NFR and Operations Decisions

SYNTHETIC SOURCE MATERIAL. Not user-approved. Product scope: relayops-pilot-v1.

## NFR-001 - Performance
- requirement: The delivery-list API must respond in less than 800 ms at p95 under the declared pilot workload.
- target: p95 < 800 ms with 500 concurrent operators and 100,000 delivery records per account.
- verification: Run a 30-minute steady-state load test after five-minute warmup, using realistic filter combinations; report p50, p95, p99, errors and sample count.
- owner: Ravi Menon
- related_requirements: ['FR-001']

## NFR-002 - Audit retention
- requirement: Accepted replay and permission audit events must remain queryable for 90 days and append-only throughout that interval.
- target: 90-day retention; 100% required audit-field completeness for accepted replay jobs.
- verification: Retention-boundary test, append-only permission check and daily job-to-audit reconciliation.
- owner: Isha Shah
- related_requirements: ['FR-003', 'FR-006', 'FR-007']

## NFR-003 - Browser compatibility
- requirement: The pilot console must support desktop Chrome 140 and Edge 140 at viewport widths of 1280 pixels and above.
- target: All pilot acceptance scenarios pass in both specified desktop browser versions. Mobile support is explicitly excluded.
- verification: Cross-browser regression suite on the pinned synthetic acceptance environment. These versions are fictional project test targets, not a current-browser recommendation.
- owner: Ravi Menon
- related_requirements: ['FR-001', 'FR-002', 'FR-005', 'FR-006', 'FR-007']

## NFR-004 - Security and privacy
- requirement: Use TLS 1.2 or higher for console-to-API communication; redact raw payloads, signing secrets and authorization headers from UI and application logs.
- target: Zero forbidden secret values in the security test corpus; all five route families enforce account isolation.
- verification: TLS configuration check, secret-canary scanning and negative authorization tests before launch.
- owner: Isha Shah
- related_requirements: ['FR-002', 'FR-003', 'FR-008']

## Dependencies
[
  {
    "name": "Delivery API v2",
    "owner": "Leo Chen, Core Events",
    "status": "Existing API; pilot contract additions due 2026-09-25",
    "risk": "Delayed status-query support prevents end-to-end replay observation.",
    "mitigation": "Contract tests at integration start; feature flag remains off if readiness fails."
  },
  {
    "name": "Replay-worker API",
    "owner": "Leo Chen, Core Events",
    "status": "Synthetic commitment: staging available 2026-09-25",
    "risk": "Worker readiness is a critical-path dependency.",
    "mitigation": "Daily contract smoke test from 2026-09-25; launch requires successful enqueue-to-terminal test."
  },
  {
    "name": "Customer-account identity service",
    "owner": "Omar Khan, Identity Team",
    "status": "Existing service; replay-permission endpoint contract signed 2026-09-11",
    "risk": "Stale role propagation could retain revoked access.",
    "mitigation": "Every replay authorization checks current permission; revocations take effect within 60 seconds."
  },
  {
    "name": "Append-only audit store",
    "owner": "Isha Shah, Security Engineering",
    "status": "Existing storage; retention policy configured by 2026-09-28",
    "risk": "Audit write failure could create an untraceable replay.",
    "mitigation": "Atomic outbox transaction records acceptance before dispatch; do not accept replay if transaction fails."
  }
]

## Rollback
Disable replay controls using the per-account feature flag; continue read-only inspection and let already accepted jobs finish. Do not delete queued jobs or audit records.
