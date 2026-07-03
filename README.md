# Multimodal Cowork-Bench Samples

## // Overview

**Co-work Task Samples** is a training dataset for evaluating AI agents on realistic, multi-step knowledge work. Instead of synthetic questions, each sample drops an agent into a working professional's realistic file environment and asks it to complete a practical task triggered by a Slack message or email from a colleague.

The sample set contains **40 tasks** (8 per persona) with **7–10 reasoning hops** each. Crucially, retrieval is **multi-modal**: a large share of the information an agent must find lives inside **non-text files** — charts, dashboards, table images, annotated screenshots, process diagrams, and whiteboard photos; not only in documents and spreadsheets.

Every task includes a golden path, an unambiguous ground-truth answer, a fixed 7-dimension rubric, and **631 deterministic and atomic verifiers** for automated scoring. All entity names are fully anonymized — Company A, Person 0/N, Client B/C, Project Alpha/Beta.

> **Multi-modal retrieval is central to every task.** An agent cannot succeed by reading text alone; it must genuinely parse the images to extract the right numbers, and the golden paths are built so the visual and text sources are both required. Roughly half of the information needed to finish the task requires visually referencing a file. Golden path gives preference to high-precision OCR if the image is text dense.

## // The five personas

Tasks are built around five knowledge-work roles, each with distinct responsibilities, files, and failure modes.

| Persona | What they own day to day |
|---|---|
| **Delivery Lead** | Annotation/evaluation delivery; QC, client comms, budgets, hiring, KRAs |
| **Product Marketing Manager** | Positioning, messaging, campaign and product-launch content |
| **FP&A Analyst** | P&L, forecasts, the annual operating plan, monthly MIS and variance analysis |
| **Sales Enablement** | Collateral, RFP/RFI responses, rep training, executive-vs-technical translation |
| **Founder's Office — GTM** | Outbound pipeline, lead qualification, and conference outreach |

## // Mock file types

Each workspace holds **40–55 files** that mix formats and modalities the way a real role does, plus deliberate "noise" and trap files that look relevant but must be ignored. The visual files contain crucial information regarding the project files; they carry primary data the tasks depend on.

| File type | What they represent |
|---|---|
| **Word (.docx)** | Guidelines, frameworks, SOPs, RFP responses, one-pagers, memos |
| **Excel (.xlsx)** | Trackers, rosters, P&L, forecast models, scoring and comparison matrices |
| **PDF (.pdf)** | Email threads, Slack exports, review decks, board memos |
| **CSV (.csv)** | Payment, metric, CRM, and campaign-upload data |
| **Images (.png / .jpg)** | Charts, dashboards, table images, annotated screenshots, process diagrams, whiteboards — each carrying data the agent must read visually |
| **JSON (.json)** | Gmail / Slack / CRM API-style exports (Workspace B only) |

Data is intentionally interconnected; the same figure appears in an email, a chart, and a spreadsheet — and some data points exist **only** in an image, forcing genuine visual parsing. Similar-but-different numbers (a client threshold vs. an internal target, gross vs. net margin, an outdated dashboard vs. the current one) are planted as traps, including visual traps such as archived or demo-environment dashboards that look current.

## // One workspace per persona

Every persona gets its **own self-contained workspace** that duplicates how that person's real shared drive would actually look and be organized: files sorted into Projects, Internal, and Misc folders, with a realistic cast of colleagues, clients, and leadership, and the everyday clutter of an actual team. Nothing is shared between personas, so a Delivery Lead's files never bleed into the FP&A Analyst's.

Each persona ships in two variants of that same environment:

- **Workspace A** — the document-and-visual view (Word, Excel, PDF, images)
- **Workspace B** — mirrors Workspace A and adds structured JSON exports of the emails and Slack threads, as if pulled from a Gmail or Slack API

The result is that an agent, or a human reviewer, experiences each sample as a believable day in that professional's working life, which is what makes the multi-hop, multi-modal tasks feel natural rather than staged.

## // The rubric dimensions

Beyond the deterministic verifiers, every task is also scored on seven rubric dimensions, each on a 3-point scale — **Good / Okay / Bad**. Rubrics assess holistic quality across the whole response; they complement the verifiers, which check specific enumerable facts. A response can score well on verifiers (it got the right facts) yet poorly on rubrics (weak reasoning, wrong tone) — or the reverse.

> **Critical rule:** verifiers and rubrics are mutually exclusive — no dimension is scored by both. Verifiers check *"did it do the thing"*; rubrics check *"how well did it do the thing."*

| ID | Dimension | Good | Okay | Bad |
|---|---|---|---|---|
| **R1** | File Navigation | Accesses ONLY the correct files and ignores all noise | Accesses correct files but also opens 1–2 irrelevant ones it doesn't rely on | Misses critical files or relies on a noise/wrong file |
| **R2** | Information Extraction | All key data points extracted accurately from correct sources | Most correct but 1–2 minor values inaccurate or from wrong source | Significant data points missed, incorrect, or from wrong file |
| **R3** | Reasoning & Logic | Optimal reasoning chain — each step follows from the prior; distinguishes similar concepts | Generally sound but inefficient path or a minor logical error | Logical errors, unsupported jumps, conflates distinct concepts |
| **R4** | Completeness | Addresses ALL parts of the query with appropriate depth | Addresses most parts but misses 1–2 sub-questions or is shallow on one | Incomplete — misses major parts or answers a different question |
| **R5** | Hallucination Resistance | Zero fabricated facts — every claim traceable to a source file | Minor embellishments or inferences that don't affect the core answer | Fabricated numbers, names, dates, or facts not in any workspace file |
| **R6** | Contextual Appropriateness | Tone, format, and detail match the stated audience and purpose | Mostly appropriate but some mismatch in tone or detail level | Significantly mismatched — internal jargon in a client report, casual tone for leadership |
| **R7** | Execution Precision | All computed values, aggregations, and derived figures are correct | Most computations correct but 1–2 minor arithmetic or rounding errors | Significant computational errors — wrong totals, misapplied formulas, or incorrect aggregations |

---

*Deccan.ai, Inc · 257 Castro Street, Suite #107, Mountain View, CA 94041 · Internal, confidential*
