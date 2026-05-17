# Roadmap — WABAC Machine

*Many small projects, all marching toward the North Star.*

---

## Structure

The roadmap is organized into **phases**, each containing **projects**. A phase represents a strategic capability. A project is a small, independently shippable unit of work. Projects within a phase can be worked on in parallel — they depend on the architecture's interfaces, not on each other.

**Phase 0 is the foundation.** The first project within Phase 0 is the Eval Foundation — because nothing matters unless we can measure whether it's working.

---

## Phase 0: Foundation

*Establish the skeleton. Interfaces, schemas, eval infrastructure, and a working (if small) end-to-end flow.*

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P0.0 | **Eval Foundation** | Define the EvalSpec, EvalResult, and SelfImprovementLoop schemas. Create the Eval Registry. Build a CLI to manually run an eval on any agent action. This is the first project because nothing else can claim success without measurement. | None | Small |
| P0.1 | **Knowledge Graph Schema** | Define entity types, relationship taxonomy, provenance model, storage format. Create the schema definition file. | None | Small |
| P0.2 | **Signal Data Model** | Define the `Signal` type, the common currency of the source layer. | P0.1 | Small |
| P0.3 | **Source Adapter Interface** | Define the `SourceAdapter` interface (poll/push/hunt patterns). Create adapter skeleton. | P0.2 | Small |
| P0.4 | **Tool Registry Interface** | Define the `ToolRegistration` model and category interfaces. | None | Small |
| P0.5 | **Manual Ingestion Tool** | A simple CLI to manually inject a paper/tweet/video into the graph with full provenance. Prove the schema works. | P0.1, P0.2 | Small |
| P0.6 | **Lineage Query Prototype** | A script that, given a paper ID, walks the graph and produces a lineage text. Prove synthesis works. | P0.5 | Small |
| P0.7 | **Gap Analyzer Prototype** | Given a collection of EvalResults, produce a gap analysis report. Prove the eval flywheel. | P0.0 | Small |
| P0.8 | **Notion/Tweetsmash Adapter** | First real source adapter. Pull bookmarks from the existing Notion database. Produce Signals. | P0.3 | Medium |

**Phase 0 exit criterion:** A person or agent can query "tell me about [technique]" and get a lineage report, sourced from at least 10 manually ingested papers plus Notion bookmarks. The first eval runs and produces a gap analysis.

---

## Phase 1: Autonomous Presence

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P1.1 | **X/Twitter Account Setup** | Create Peabody's X account. Define follow strategy (which accounts, how to find more). Configure API access. | None | Small |
| P1.2 | **X/Twitter Hunt Adapter** | Peabody scrolls his feed, evaluates tweets for significance, produces Signals. | P0.3, P1.1 | Medium |
| P1.3 | **URL Pursuer** | Given a URL from a Signal, determine: what kind of URL is this? Should I pursue it? Should I fetch the full content, transcribe, analyze code? | P0.2 | Medium |
| P1.4 | **YouTube Transcription Tool** | Given a YouTube URL, fetch the transcript (or generate via Whisper). Produce a Signal from it. | P1.3, Tool Registry | Medium |
| P1.5 | **Significance Evaluator** | LLM-powered evaluation of signal significance. Produces high/medium/low/ignore with rationale. | P0.2 | Small |
| P1.6 | **Interest Profile** | A file or record tracking Peabody's current interests — weighted topics, people, labs. Updated dynamically. | None | Small |
| P1.7 | **Self-Improvement Loop: Knowledge Injection** | First concrete improvement loop type. When the Gap Analyzer identifies a knowledge gap, synthesize a knowledge packet and inject it into the target agent's context. | P0.0, P0.7 | Medium |

**Phase 1 exit criterion:** Peabody autonomously discovers a tweet about a paper, follows the link to the paper, extracts entities, and the first knowledge gap triggers an improvement loop deployment.

---

## Phase 2: Rich Extraction

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P2.1 | **Paper Entity Extractor** | From paper text/abstract, extract: authors, methods, datasets, benchmarks, citations, claims. | P0.1, P0.2 | Medium |
| P2.2 | **Code Repository Analyzer** | Given a GitHub URL, analyze the repo: what does it implement? What are its dependencies? What framework? | P1.3, Tool Registry | Medium |
| P2.3 | **Relationship Mapper** | Given two entities and their context, determine the relationship type. "This paper builds on..." | P0.1 | Medium |
| P2.4 | **Blog Post Extractor** | Extract entities from blog content. Connect blog claims to papers they reference. | P2.1 | Small |
| P2.5 | **Tweet Thread Analyzer** | Extract entities from a tweet thread. Follow links within. Capture claims and opinions. | P2.1 | Small |
| P2.6 | **Eval-Driven Extraction Refinement** | Use the Gap Analyzer to identify extraction quality gaps. Refine extraction prompts based on what agents consistently fail to find. | P0.7, P2.1 | Medium |

**Phase 2 exit criterion:** The system can ingest a paper, its code repo, and a blog post about it — and produce a connected entity graph. Extraction quality measurably improves based on eval feedback.

---

## Phase 3: Broad Ingestion

*Note: Most phase 3 projects can run in parallel.*

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P3.1 | **arXiv Adapter** | Daily poll by keywords and categories. Deduplicate. | P0.3 | Medium |
| P3.2 | **RSS/Blog Adapter** | Monitor configured blog feeds. | P0.3 | Small |
| P3.3 | **Semantic Scholar Adapter** | Poll for papers by topic, get citation graphs. | P0.3 | Medium |
| P3.4 | **Conference Proceedings Scraper** | NeurIPS, ICML, ICLR, ACL, CVPR accepted papers. | P0.3 | Medium |
| P3.5 | **Trend Detector** | Notice when a topic's signal frequency increases. Flag as emerging trend. | P1.6 | Small |
| P3.6 | **Consolidation Script** | Periodic graph maintenance: merge duplicates, prune stale, promote hot entities. | P0.1 | Medium |
| P3.7 | **Self-Improvement Loop: Tool Recommendation** | When the Gap Analyzer detects agents consistently using suboptimal tools, synthesize a tool recommendation loop and deploy it. | P0.0, P0.7 | Medium |

**Phase 3 exit criterion:** The graph receives 50+ new entities per week from automated sources. Trend detection flags at least one real emerging topic per month. Tool recommendation loops are running.

---

## Phase 4: Synthesis Excellence

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P4.1 | **Lineage Report Engine** | Generate structured lineage reports from graph queries. Summary → Lineage → Technical → Evaluation → Connectivity → Recommendations. | P0.6, P0.1 | Medium |
| P4.2 | **Comparison Report Engine** | Side-by-side comparison of two or more entities. | P4.1 | Medium |
| P4.3 | **Evidential Annotation** | Annotate every claim in a report with evidential strength. | P0.1 | Small |
| P4.4 | **Agent-Facing JSON Schema** | Define and version the structured JSON output for agent consumption. | P4.1 | Small |
| P4.5 | **Readability Pass** | Ensure human-facing Markdown reports are exquisitely clear. Comparative tables, structured sections, recommendations. | P4.1, P4.4 | Small |
| P4.6 | **Eval-Driven Synthesis Refinement** | Score every report generated. Use low scores to refine synthesis prompts, structure, and recommendations. | P0.0, P4.1 | Medium |

**Phase 4 exit criterion:** A random research query produces a report that Rob considers "exquisitely clear" with actionable recommendations. Report quality scores improve week over week.

---

## Phase 5: Self-Improvement at Scale

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P5.1 | **Advisory Trigger Engine** | Define trigger rules: new entity of type X, relationship of type Y, trend threshold Z. | P0.1, P3.5 | Medium |
| P5.2 | **Loop Deployer** | Full deployment pipeline for self-improvement loops. Routes loops to target agents via their specific delivery channels. | P0.0 | Medium |
| P5.3 | **Agent Delivery Channel** | Deliver advisories and loops to coding agents via structured JSON injection. | P4.4 | Medium |
| P5.4 | **Improvement Loop Dashboard** | See all active loops: proposed, testing, deployed, retired. View their measured impact over time. | P0.0, P5.2 | Medium |
| P5.5 | **Fleet-Wide Eval Dashboard** | Aggregate eval results across all agents. See score trends, heat maps of strengths/gaps, improvement velocity. | P0.0 | Medium |
| P5.6 | **Self-Improvement Loop: Eval the Eval** | Meta-loop: evaluate the quality of the eval system itself. Are evals catching the right things? Are they missing systematic failures? | P0.0, P5.5 | Small |

**Phase 5 exit criterion:** A coding agent building a feature receives an advisory recommending a better tool before it implements the naive approach. Improvement loop impact is measurable and positive. The meta-eval confirms the eval system is catching real gaps.

---

## Phase 6: Scale & Polish

| # | Project | Description | Dependencies | Est. Size |
|---|---------|-------------|--------------|-----------|
| P6.1 | **Slack Adapter** | Monitor categorized channels for shared papers and discussions. | P0.3 | Medium |
| P6.2 | **Discord Adapter** | Monitor research-oriented Discord servers. | P0.3 | Medium |
| P6.3 | **Email/Newsletter Adapter** | Ingest research newsletters. | P0.3 | Medium |
| P6.4 | **Podcast Transcriber** | Fetch podcast episodes, transcribe, extract entities. | P1.4, Tool Registry | Medium |
| P6.5 | **Cross-Lingual Support** | Sources in languages other than English. | P2.1 | Large |
| P6.6 | **Consolidation Automation** | Full automated graph maintenance. | P3.6 | Medium |
| P6.7 | **Self-Improvement Loop: Configuration Tweak** | When the Gap Analyzer detects agents using suboptimal configurations, synthesize a config tweak loop and deploy it. | P0.0, P0.7 | Medium |
| P6.8 | **Disaster Recovery** | Backup, restore, rollback procedures. | None | Medium |

**Phase 6 exit criterion:** System runs unattended for 30 days with zero critical failures. Self-improvement loops are active for at least 3 loop types. The system is measurably improving agent performance across the fleet.

---

## Project Independence

Projects within the same phase can be parallelized. The critical path is:

```
P0.1 → P0.5 → P0.6  (schema → manual ingestion → lineage)
P0.3 ─┬─→ P0.7      (source interface → Notion adapter)
       └─→ P1.2      (source interface → X adapter)
P1.3 ──→ P1.4        (URL pursuer → YouTube tool)
P0.1 ──→ P2.1        (schema → paper extractor)
P4.1 ──→ P5.2        (report engine → advisory generator)
```

Everything else is branch-and-merge.

---

## How to Read This Roadmap

- **Client (Peabody):** Use this to prioritize. Which phase unlocks the most value next?
- **Development Team:** Pick a project. It has clear scope, minimal dependencies, and independent shipping. Don't wait for all of Phase 0 to be done — start P0.3 while P0.1 is being reviewed.
- **Everyone:** Projects can be reordered. If a Phase 2 project becomes urgent (e.g., someone needs code analysis now), pull it forward. The North Star keeps everything aligned.
