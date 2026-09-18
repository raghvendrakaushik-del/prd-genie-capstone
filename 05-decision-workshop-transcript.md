# RelayOps - Decision Workshop, 2026-09-11

SYNTHETIC SOURCE MATERIAL. Not user-approved. Product scope: relayops-pilot-v1.

Participants: Maya Sen (PM), Aditi Rao (Engineering), Leo Chen (Core Events), Isha Shah (Security), Noor Ali (Analytics), Elena Park (Design), Ravi Menon (QA). All are fictional.

[09:00] Maya: We are resolving the discovery gaps today. This release is a pilot, not general availability. Three accounts only. Do not carry forward unresolved statements from the older discovery notes as if they were current decisions.
[09:04] Leo: I previously said worker availability was unknown. For this synthetic scenario, staging is now committed for September 25, 2026. Record the commitment and its critical-path risk separately.
[09:07] Aditi: We should not replay all failures. Only timeouts and 5xx within 30 days. A 4xx can indicate invalid business data, and automatic replay would be unsafe. Successful or running deliveries are also ineligible.
[09:10] Maya: Agreed. Treat that as DEC-01. Bulk replay remains excluded.
[09:12] Leo: Deduplication is scoped to account, delivery and the request key, for 24 hours. The same body returns the same job ID; a changed body is a 409. Simultaneous requests must obey the same rule. A new queue entry must never precede durable acceptance and its audit/outbox record.
[09:16] Isha: Auditors only read. Every replay needs actor, account, original delivery, replay job, request time and outcome. Keep audit events 90 days, while the delivery search window stays 30 days. These are different retention purposes, not conflicting requirements.
[09:20] Elena: Five-second polling would create needless requests. We agree on 15 seconds while an active job is visible, stopping when hidden or terminal. A failed status request must show stale state rather than success. Manual refresh is available.
[09:24] Noor: I own the pilot dashboard. The 45-minute baseline is synthetic. Our target is at most 15 minutes median. Report unresolved ticket counts too, so we do not make the metric look better by dropping slow unresolved cases.
[09:28] Maya: Customer export is excluded. The auditor can use the read-only audit view. We are not promising CSV formula preservation or spreadsheet behavior for this product.
[09:31] Ravi: Performance acceptance is p95 less than 800 ms at 500 concurrent operators, with 100,000 records per account. Thirty minutes of steady-state load after warmup. Do not round any of these numbers.
[09:34] Isha: TLS 1.2 or higher. No payload bodies, signing secrets or authorization headers in the UI or app logs. Account scope comes from identity, never a request selector. Negative authorization tests are a launch gate.
[09:38] Maya: Requirements review September 18, design September 21, development September 22, integration September 28, QA October 5 and pilot launch October 12. Pilot review November 9. The current source catalog contains the complete field-level decisions.
[09:42] Aditi: These are source proposals for the capstone, not an actual user approval. The PRD Genie reviewer must inspect extracted evidence and explicitly approve before the generator can run.
