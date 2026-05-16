# Client Brief — The WABAC Machine

**Client:** Peabody (Research Intelligence, Hermes Agent Ecosystem)
**Status:** Draft v1
**Date:** 2026-05-16

---

## 1. Vision Statement

A system that holds the full timeline of every AI/ML idea in view — from seminal paper to latest iteration — and shows how it connects to everything else. It is the research intelligence that keeps the entire agent ecosystem grounded, current, and capable of self-improvement.

**Tagline:** *Trace ideas through time. Own the lineage.*

## 2. What We're Building

The WABAC Machine is a **living knowledge graph of AI/ML research** with four capabilities:

1. **Ingest** — automatically pull in research from arXiv, Twitter/X bookmarks, blogs, conference proceedings, RSS feeds, and Slack channels
2. **Connect** — build and maintain a typed graph of entities (papers, people, labs, methods, datasets, benchmarks) linked by relationships (builds-on, supersedes, challenges, inspired-by, reproduces, contradicts)
3. **Synthesize** — produce lineage-traced reports that show the provenance, evolution, and tradeoffs of any idea
4. **Advise** — serve as the trusted research advisor for every agent in the ecosystem, producing targeted self-improvement advisories

## 3. Target Users (Personas)

| Persona | Description | Primary Need |
|---------|-------------|-------------|
| **Rob (Ecosystem Architect)** | Principal. Defines intent, delegates to agents. | Trusted, grounded research intelligence he can point at any question and get a definitive answer. |
| **Coding Agents** (Claude Code, Codex, OpenCode) | Build features, fix bugs, make technical decisions. | Need to know: is there a better tool/technique/library for this? What's the state of the art? What pitfalls do others hit? |
| **Research Consumers** | Future — other specialist agents in the ecosystem. | Quick grounding: "Give me the lineage of this idea in 3 paragraphs." |

## 4. Core Capabilities

### 4.1 Knowledge Graph
- Entities: Papers, People, Labs/Organizations, Models, Methods/Techniques, Datasets, Benchmarks, Tools/Frameworks
- Relationships: builds-on, supersedes, challenges, inspired-by, reproduces, contradicts, evaluates, extends, surveys
- Versioned — the graph itself tracks provenance (who added this fact, from what source, when)
- Queryable by relationship path: "What built on the Transformer?" → attention → self-attention → multi-head → FlashAttention → ...

### 4.2 Ingestion Pipeline
- **arXiv:** Daily scans by topic keywords and categories (cs.AI, cs.LG, cs.CL, cs.CV). Filter by relevance score.
- **Twitter/X Bookmarks:** Pull from Notion/Tweetsmash database. Classify by topic. Flag high-signal tweets from verified researchers.
- **Blogs/RSS:** Monitor known AI lab blogs (Anthropic, OpenAI, DeepMind, Meta AI, Mistral, etc.) via blogwatcher.
- **Conference Proceedings:** NeurIPS, ICML, ICLR, ACL, CVPR — scan accepted papers.
- **Slack:** Monitor categorized channels for shared papers and discussions.

### 4.3 Synthesis Engine
- Given a query (e.g. "Tell me about sparse attention mechanisms"), produce a structured report:
  - Summary
  - Lineage (what came before)
  - Technical core (what it actually does)
  - Evaluation (evidence, benchmarks, limitations)
  - Connectivity (related work it links to)
  - Recommendations (what to do with this)
- Support for comparative analysis: "Compare FlashAttention v1, v2, and v3"

### 4.4 Self-Improvement Advisory
- Proactively scan for discoveries that could improve any agent in the ecosystem
- Categorize by domain: new architectures, training techniques, prompting strategies, tool integrations, evaluation methods
- Produce targeted advisories with specific, actionable recommendations

## 5. Non-Goals (Out of Scope)

- Not a general-purpose search engine. Does not index the open web.
- Not a PDF scraper. Digests structured data and human-curated feeds.
- Not a paper recommender system. Does not do collaborative filtering or user modeling (initially).
- Not a training data generator. Does not produce training sets for fine-tuning.
- Not a citation graph. Deeper than citation edges — includes conceptual relationships, not just reference links.
- Not real-time. Daily batch updates are sufficient. Latency of hours is acceptable.

## 6. Success Metrics

- **Coverage:** 90%+ of significant AI/ML papers from top venues within 7 days of publication (given arxiv/category monitoring)
- **Recall:** When an agent asks about a paper or technique from the last 3 years, the graph returns it with lineage context ≥95% of the time
- **Accuracy:** Zero hallucinated relationships (a "builds-on" edge must have evidence in the source material)
- **Adoption:** At least 2 coding agents (Claude Code, Codex) regularly consume advisories
- **Satisfaction:** Rob finds the research output "exquisitely clear" with actionable recommendations

## 7. Constraints

- **Provenance over assertion:** Every fact in the graph must cite its source
- **Honesty over hype:** The system must flag weak evidence, contradictory findings, and unverified claims
- **Precision over speed:** Better to say "I don't know" than to bluff
- **Practicality over abstraction:** Every insight must translate to an actionable recommendation
- **Persistent maintenance:** The graph must be periodically consolidated (merge duplicates, prune stale entries)

## 8. Timeline Vision

| Phase | Focus | Deliverable |
|-------|-------|-------------|
| **Phase 1** | Foundation | Knowledge graph schema + manual ingestion of first 50 papers + baseline synthesis capability |
| **Phase 2** | Automated Ingestion | arXiv cron, Notion/Tweetsmash pipeline, blogwatcher RSS feeds |
| **Phase 3** | Synthesis Quality | Lineage tracing reports, comparative analysis, query-by-relationship-path |
| **Phase 4** | Advisory Loop | Self-improvement advisories pushed to coding agents |
| **Phase 5** | Scale & Polish | Slack integration, conference proceedings, consolidation automation |
