# RelayOps - Stakeholder Notes and Resolutions

SYNTHETIC SOURCE MATERIAL. Not user-approved. Product scope: relayops-pilot-v1.

## DEC-01
Question: Which delivery failures are eligible?
Resolution: Only network timeouts and HTTP 5xx failures from the previous 30 days are eligible. HTTP 4xx, successful and already-running deliveries are not eligible.
Owner: Maya Sen
Date: 2026-09-11

## DEC-02
Question: What happens when a key is reused?
Resolution: Within 24 hours, the same account, delivery and Idempotency-Key with an identical body returns the original replay_job_id without another enqueue. Reusing the key with a different body returns HTTP 409.
Owner: Leo Chen
Date: 2026-09-11

## DEC-03
Question: How often does job status refresh?
Resolution: The UI polls job status every 15 seconds only while the job is queued or running and the tab is visible. Polling stops on terminal status or hidden tab and refreshes immediately when the tab becomes visible. Manual refresh is available.
Owner: Aditi Rao
Date: 2026-09-11

## DEC-04
Question: Is customer-facing export included?
Resolution: No customer-facing export in the pilot. Auditors use the account-scoped on-screen audit view.
Owner: Maya Sen
Date: 2026-09-11

## DEC-05
Question: Who owns KPI reporting?
Resolution: Noor Ali owns daily dashboard reconciliation and the final four-week pilot report.
Owner: Maya Sen
Date: 2026-09-11

## DEC-06
Question: Is the worker availability still unknown?
Resolution: The earlier discovery uncertainty is superseded by the synthetic Core Events commitment of staging availability on 2026-09-25. This is a sample scenario decision, not a real team commitment.
Owner: Leo Chen
Date: 2026-09-11

## Scope exclusions
- Bulk replay and scheduled replay
- Creating or editing webhook endpoints
- New webhook signing algorithms
- Mobile and native applications
- AI-generated failure explanations
- Customer-facing CSV or spreadsheet export in the pilot

## Explicit assumptions
[
  {
    "statement": "The three fictional pilot accounts use the existing customer-account identity service and have no external SSO migration during this pilot.",
    "owner": "Omar Khan, Identity Team",
    "validation": "Verified in the synthetic pilot-readiness inventory on 2026-09-11."
  },
  {
    "statement": "A replay reuses the original immutable event payload and existing signing configuration; it does not create or edit an event.",
    "owner": "Leo Chen, Core Events",
    "validation": "Recorded as decision DEC-02 and included in replay contract review."
  },
  {
    "statement": "The synthetic workload of 100,000 delivery records per account and 500 concurrent operators is sufficient for pilot capacity testing.",
    "owner": "Aditi Rao, Engineering Lead",
    "validation": "Explicit pilot sizing decision; expansion beyond this envelope requires a new capacity review."
  }
]

## Milestones
[
  {
    "milestone": "Requirements and architecture review",
    "date": "2026-09-18",
    "owner": "Maya Sen and Aditi Rao",
    "exit": "Reviewer approves source-backed requirements, API contracts and scope decisions."
  },
  {
    "milestone": "Design Complete",
    "date": "2026-09-21",
    "owner": "Elena Park, Product Design",
    "exit": "Desktop interaction and permission-error designs signed off."
  },
  {
    "milestone": "Development Start",
    "date": "2026-09-22",
    "owner": "Aditi Rao",
    "exit": "Sprint backlog accepted and mocked API contracts available."
  },
  {
    "milestone": "Integration ready",
    "date": "2026-09-28",
    "owner": "Leo Chen",
    "exit": "Identity, worker and audit integration contract tests pass."
  },
  {
    "milestone": "QA / Testing",
    "date": "2026-10-05",
    "owner": "Ravi Menon, QA Lead",
    "exit": "Functional, tenant-isolation, replay-deduplication, browser and capacity tests pass."
  },
  {
    "milestone": "Launch",
    "date": "2026-10-12",
    "owner": "Maya Sen",
    "exit": "Go/no-go approval; three account feature flags enabled; monitoring and rollback staffed."
  },
  {
    "milestone": "Pilot review",
    "date": "2026-11-09",
    "owner": "Noor Ali",
    "exit": "Four-week KPI report, incident review and rollout recommendation reviewed."
  }
]
