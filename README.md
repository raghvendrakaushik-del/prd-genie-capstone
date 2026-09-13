# PRD Genie — IK Capstone September 2026

Author and capstone reviewer: **Raghavendra Kaushik**.

Working branch: `Feature/Raghav_IK_Capstone_SEP2026`. Raghavendra reviews the generated document and merges the branch into `main` after final document approval.

## Review these deliverables

- [wiki.md — complete PRD](wiki.md): all ten template sections, eight functional requirements, four NFRs, sixteen Given/When/Then scenarios, detailed Mermaid architecture, metrics, dependencies, decisions and timeline.
- [Detailed Jira stories](stories.md) and [Jira import CSV](jira-import.csv): three epics and eight stories. Use the administrative Jira CSV importer and map Issue ID/Parent for hierarchy. No Jira issues have been published.
- [Approved source](approved-source.json) and [review receipt](review-manifest.json): requirements approved by Raghavendra Kaushik, revision 2, September 13, 2026. Approval of source requirements precedes generation; final document sign-off is separate.
- [Langflow import](prd-genie.v2-reviewed.langflow.json): ten nodes with retrieval, four agents, durable human review and GitHub publication.
- [Complete source build kit](PRD-Genie-Build-Kit.zip): unpack to obtain the organized `prd-genie/` source tree, tests, original resources, prompt/schema files, workflow builders, training datasets and detailed setup instructions. Source code is packaged in this ZIP; the root files are the review/deployment artifacts.

## Knowledge and operation

The root `01` through `08` files form the detailed sample corpus. All product scenario facts and fictional product-team names are synthetic; the author and reviewer names are user-provided. The original corpus intentionally leaves reviewer identity empty so the workflow asks the person who actually reviews. The approved source includes the explicit answer and the PRD has no missing fields.

[Google Drive knowledge folder](https://drive.google.com/drive/u/0/folders/121xKPik0_bfY8N6y43BNlNKRpWqD09jV) contains the eight files plus manifest. Nine uploads were verified. The live run used an explicit, identical embedded snapshot. Configure a Google service-account credential in Langflow before switching to direct Drive API retrieval.

Import the workflow into the existing Langflow instance, bind the Groq secret variable, and send `{"action":"extract","product_id":"relayops-pilot-v1"}` in Playground. Review the returned extraction, answer missing fields, and explicitly approve the exact current revision/hash. The flow blocks generation before approval and invalidates old approvals after edits. Current-product facts come from the catalog and reviewer answers; the previous PRD is style context only.

The workflow uses constrained extraction from a structured catalog. Arbitrary free-form ingestion into that catalog remains future integration work; the earlier free-text baseline extractor is included in the source kit.

## Verified and pending

The live flow accepted the source approval and generated the PRD and stories with Groq. Twenty-two review/export software tests pass. [Generation checks](v2-generated-checks.json) verify the ten sections, sixteen criteria and absence of blank fields/placeholders. [Live status](v2-live-status.json) records actual execution; local documents use the same deterministic renderer and exact live approval receipt rather than claiming a raw database export.

This branch's initial project delivery is committed through the signed-in GitHub browser. Future automatic Langflow commits require a repository-scoped Contents-write token in the secret field. Configure repository `raghvendrakaushik-del/prd-genie-capstone` and this feature branch. The latest import names the generated document `wiki.md`. Do not enter tokens in chat or commit them.

Fine-tuning data and submission/comparison scripts are prepared (48 training, 16 validation, 16 holdout examples). **No training job has run and no tuned model is deployed.** Human label review and a supported training connection are required. Direct Drive API authentication, live automatic GitHub publication, remaining baseline model evaluations, external Langfuse, slides and demonstration video remain pending. Full capstone completion is not claimed.

The original `manifest.json` describes the pre-upload source package; the later live-status receipt records the completed Drive upload. Mount the review SQLite directory to a persistent Docker volume before relying on survival across container replacement.
