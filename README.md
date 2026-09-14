# PRD Genie capstone

Author and requirements reviewer: **Raghavendra Kaushik**. Updated 14 September 2026.

PRD Genie turns source material into reviewed product requirements and traceable Jira stories. The demonstrated runtime is local Langflow with the existing Groq GPT-OSS 120B connection. The complete fictional RelayOps example uses a structured knowledge catalog, a durable human-review gate and deterministic document rendering.

## Current delivery

- [Complete approved-source PRD](wiki.md): ten template sections, eight functional requirements, four NFRs, sixteen GIVEN/WHEN/THEN scenarios and detailed Mermaid architecture.
- [Jira KAN project](https://raghvendrakaushik.atlassian.net/jira/software/projects/KAN/list): three epics KAN-4 through KAN-6 and eight stories KAN-7 through KAN-14 are created. Every saved story description and epic parent was verified. [Publication receipt](jira-publication-receipt.json).
- [Assignment report](PRD_Genie_Assignment_Report.pdf): Q1 ideation, Q2 charter, two-page Q3 writeup and one-page Q4 reflection, mapped to the supplied 80-point rubric.
- [Editable 23-slide presentation](PRD_Genie_Raghavendra_Kaushik.pptx), [baseline report](Baseline_Evaluation_Report.pdf) and [submission guide](Submission_Guide.pdf).
- [Complete documents archive](Capstone_Raghav_SEP2026_Documents.zip): source kit, workflow exports, twelve baseline inputs and outputs/errors, knowledge files, screenshots, review receipts and Jira artifacts.
- [Google Drive submission folder](https://drive.google.com/drive/u/0/folders/1NOfn4t_hZHsP4QaP58lgV3a-CT4J3I_K).

The September 13 report/deck describes Jira as import-ready; publication was completed September 14. [Current status](00_READ_FIRST_CURRENT_STATUS.md) records this change. The final demo video is awaiting Raghavendra's own narration recording; a transcript is not claimed as a completed video.

## Reproduce the reviewed demonstration

1. Start the existing local Langflow Docker instance at `http://localhost:7860`; keep its database on a persistent volume.
2. Import `prd-genie.v2-reviewed.langflow.json`. Bind model-enabled components to the existing `GROQ_API_KEY` secret variable. Never put credentials in chat or Git.
3. Send `{"action":"extract","product_id":"relayops-pilot-v1"}` in the Playground.
4. Read the extracted Markdown review packet. Resolve missing fields using the answer action displayed by that run, including the actual reviewer identity.
5. Approve the exact review ID, revision and source hash displayed for the completed packet. Amendments invalidate earlier approval. Never reuse an approval to authorize changed evidence.
6. Review the generated wiki and stories. Requirements approval permits draft generation; final PRD acceptance and merge into main remain Raghavendra's decision.

The separate `prd-genie.baseline-tested.langflow.json` accepts the supplied free-text cases and includes the dynamic Gap Analyzer evidence-ID constraint used in the recorded tests. Its exported output mode is JSON. T11 and T12 evaluate actual T1 artifacts; they are not independent model calls. Full setup, source code, prompt/schema definitions and test instructions are in the source kit within the documents archive.

## Evidence and limits

The strict v2 review/export software suite passed 22 tests and the complete RelayOps artifacts passed deterministic checks. The official free-text baseline recorded **4 automated passes and 8 failures**, including 2 execution errors. These outcomes are separate; semantic human review is still pending. Native Langflow tracing is connected. External Langfuse is not configured.

Eight detailed knowledge files are stored in Drive. The demonstrated flow used their explicit embedded snapshot with BM25 context and a pinned complete catalog. Direct server-side Drive access still needs credentials. Automatic arbitrary narrative-to-catalog normalization is not implemented.

GitHub version publication logic and mock tests are included, but automatic server-side commits still require a scoped repository credential and live verification. This repository was updated through the signed-in browser. Fine-tuning preparation includes 48 training, 16 validation and 16 held-out synthetic examples and job/comparison scripts. **No training job has run and no tuned model is deployed.**

All sample product metrics, budgets, accounts and product-team characters are fictional. The author and actual capstone reviewer are user-supplied identities. The private branch `Feature/Raghav_IK_Capstone_SEP2026` remains unmerged.
