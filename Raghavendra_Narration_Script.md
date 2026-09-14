# Narration script for Raghavendra Kaushik

Record the spoken paragraphs only; skip headings and timestamps. Use a quiet room and a steady pace. Aim for about five minutes, but natural clear speech matters more than exact timing. One recording or separate clips is fine. This script reflects Jira publication on 14 September 2026.

## 00:00 - Welcome

Hello, I am Raghavendra Kaushik. This is PRD Genie, my product documentation capstone. This five-minute demonstration explains the product problem, sequential agents, reviewed document generation, evaluation results and remaining work. The submission follows the supplied assignment rubric, which totals eighty points. I will distinguish verified results from features that still need additional setup.

## 00:25 - Architecture

The architecture uses sequential specialist stages. Input and retrieval feed the Requirement Extractor, then Gap and Completeness checks. The strict sample pauses at a human review gate. Only an approved source revision reaches the PRD Generator and Story Breakdown stages. The free-text baseline and strict reviewed catalog are separate, explicit paths.

## 00:50 - Live workflow canvas

This is the actual Langflow canvas from the local Docker deployment. Each custom component owns a bounded responsibility and passes a validated message downstream. Groq provides the working inference connection. Source quotations, schemas and evidence identifiers are checked before later stages can use them. Provider keys remain in secret fields.

## 01:15 - Knowledge base

The sample knowledge base contains eight detailed files for the fictional RelayOps webhook console. It includes a canonical catalog, product brief, API acceptance criteria, non-functional requirements, messy meeting notes, resolved stakeholder decisions, the template and a style reference. Retrieval uses BM25 context plus the complete catalog. This run used the embedded snapshot.

## 01:40 - Human review

The workflow initially stopped because the reviewer identity was missing. Raghavendra supplied his name and approved revision two of the extracted requirements. The approval binds the review identifier, revision and source hash. Any change invalidates it. Requirements approval allows draft generation; final document review and the merge into main remain separate human decisions.

## 02:05 - Generated PRD

This captured Playground view shows the generated product requirements document. The complete wiki file follows all ten template sections and includes author, reviewer, goals, personas, functional behavior, non-functional requirements, acceptance criteria, dependencies and timeline. It contains a detailed Mermaid diagram. The fictional sample has eight functional requirements and sixteen acceptance scenarios, with no unresolved placeholder fields.

## 02:30 - Jira acceptance criteria

The story breakdown produces three epics and eight detailed stories. These are now created in the KAN Jira project, with their epic relationships verified. Every saved story includes the approved description and acceptance criteria in Given, When and Then form. The deduplication example requires repeated requests to reference the original replay job, without enqueuing a duplicate.

## 02:55 - Connected traces

This is an actual native Langflow trace for the approved generation run. The root span reports about three seconds, with separate PRD and story component spans. Native traces provide observability without an external service. External Langfuse is not configured. A prior gap-analysis failure exposed an unknown evidence identifier, leading to a constrained schema repair.

## 03:20 - Baseline findings

All twelve supplied baseline inputs have recorded outputs or explicit errors. Four automated checks pass and eight cases fail, including two execution errors. The failures include omitted clarifications, criterion-transfer problems and classification errors. Tests eleven and twelve evaluate the actual test-one documents. These findings are preserved; a completed model call is not counted as a quality pass.

## 03:45 - Cost and evaluation

The approved PRD and story stages used nine hundred seventy-one input tokens and six hundred thirteen output tokens in total. At the checked Groq list prices, generation alone is estimated at roughly five ten-thousandths of a dollar. Extraction, retries and hosting are additional. Evaluation separates schema validity, factual support, completeness, human judgment and business impact.

## 04:10 - Remaining work

The project does not claim completed fine-tuning. Synthetic training, validation and holdout data and job scripts are prepared, but labels need review and a supported training connection is required. Direct Drive retrieval and automatic GitHub publication also need server credentials. Baseline failures must be resolved and tested on fresh cases before broader product use.

## 04:35 - Submission handoff

The submission contains the assignment report, editable presentation, architecture sources, workflow exports, README, source kit, screenshots, baseline outputs, knowledge files and reviewed documents. Question three occupies two pages and question four one page. Jira epics and stories are published. I will review the final demo and documents before submitting the Drive folder and private feature branch.

