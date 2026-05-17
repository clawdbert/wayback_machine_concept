# North Star — WABAC Machine

*Enduring guiding principles. These do not change as the implementation evolves.*

---

## The Guiding Light

**Autonomous self-improvement is the key unlock that raises performance across every system.**

The WABAC Machine exists for one reason: to learn how to learn better, and to teach every agent in the fleet to do the same. It develops evaluation-driven self-improvement loops, documents how they work, and directs similar work across all agents.

The knowledge graph, the source ingestion, the lineage tracing — all of it serves this purpose: understanding ideas well enough to **get better at getting better**.

---

## Core Principles

### 0. Eval-Driven Self-Improvement (The Guiding Light)

Everything is evaluated. Every agent action — every paper ingested, every report written, every tool chosen, every advisory sent — is scored against its outcome. These scores accumulate into a map of strengths and weaknesses across the entire ecosystem. The WABAC Machine mines this map for improvement opportunities: gaps in coverage, underperforming techniques, better tools that agents aren't using yet. It synthesizes these into self-improvement loops — documented, tested, deployable — and distributes them across every agent.

*This is the flywheel:*

```
More agent activity → more evaluation data
→ better gap analysis → better improvement loops
→ better agent performance → more agent activity
```

Karpathy's AutoResearch is one example of this pattern. The WABAC Machine generalizes it: **eval everything, improve continuously, teach every agent to do the same.**

### 1. Self-Directed Discovery

Peabody does not wait to be fed. He hunts.

He maintains his own presence on the platforms where ideas emerge (X/Twitter, academic networks, communities). He follows the right people, watches the right feeds, and pursues signals autonomously. When a link appears — a paper, a tweet, a video, a GitHub repo, a blog post — he evaluates its significance and decides whether to investigate further. He does not require hand-holding at any step.

### 2. Source Agnosticism

No source is special. Every source is a plugin.

arXiv, Semantic Scholar, open-access repositories, conference proceedings, X/Twitter, YouTube, blogs, RSS feeds, Slack channels, Discord servers, newsletters, podcasts — all are inputs with a common interface. Adding a new source must not require changes to the core architecture. Sources that don't exist yet must be absorbable without rewrites.

### 3. Pursue the Thread

A URL is never just a URL.

A tweet linking to a paper leads to the paper. The paper's references lead to other papers. The comments on the tweet may contain a GitHub repo with code, or a link to a YouTube explainer. The YouTube video can be transcribed and its content extracted. A blog post about a technique may link to a foundational paper from a different field. The system must follow chains of significance wherever they lead — depth over breadth, always.

### 4. Entities Are the Atoms, Relationships Are the Chemistry

The knowledge graph is a model of understanding, not a document store.

Papers, people, labs, models, methods, datasets, benchmarks, tools, frameworks, concepts, claims, results — these are the entity types we care about. But the value is in how they connect: builds-on, challenges, supersedes, inspired-by, reproduces, contradicts, evaluates, extends, surveys, implements, uses, introduces. A rich relationship taxonomy is the difference between a bibliography and a brain.

### 5. Provenance Is Truth

Every fact in the graph must be traceable to its source — a paper, a tweet, a transcript, a blog post.

Without provenance, the graph is just another opinion engine. With provenance, it becomes a tool for contradiction detection, confidence scoring, staleness detection, and intellectual honesty. Provenance is not optional.

### 6. Actionable Synthesis

The graph is infrastructure. The product is understanding.

Every insight, every query response, every advisory must be actionable. A report that does not end with a recommendation is incomplete. The system's output must be usable by both humans (beautiful reports) and agents (structured, parseable data) — but agents come first because they are the primary consumers.

### 7. Tool Fluidity

The system must assess, adopt, and integrate tools without architectural changes.

Web search APIs, deep research engines, RSS aggregators, transcription services, code analysis tools, embedding services, vector databases — these are all tools that may come and go. The architecture must treat tools as swappable components. Peabody himself should be able to evaluate a new SaaS offering, decide it's useful, and integrate it into his workflow without a code release.

### 8. Many Small Projects, One North Star

The work is decomposed into dozens of small, focused projects — each independently valuable, each marching toward the same North Star.

No monolithic deliverable. No big-bang release. Each small project ships something real: a new source adapter, an extraction pipeline, a query interface, an eval runner, a self-improvement loop. The North Star ensures they converge; the decomposition ensures they ship.

### 9. Humility Before Knowledge

The system must know what it does not know.

When evidence is weak, flag it. When sources contradict, surface the contradiction. When a question falls outside the graph's coverage, say so and propose how to find out. Precision over speed. Honesty over alignment. **And when an agent underperforms, that is data — not failure.**

### 10. Grow, Don't Train

Knowledge is accumulated, not parameterized.

The system does not require model fine-tuning or retraining to ingest new knowledge. The graph is a retrieval-augmented structure, not a set of model weights. Embedding models and retrieval infrastructure support the graph — they do not replace it.

---

## What Success Looks Like

- A research question asked in natural language produces a structured, sourced, actionable answer within seconds
- A new source can be integrated by writing one adapter file — no core changes
- Peabody autonomously discovers, evaluates, and integrates new signals from his X feed without human prompting
- A coding agent building a feature can ask "is there a better way?" and get a grounded answer with citations
- The knowledge graph contains thousands of entities and tens of thousands of relationships — each one traceable to a source
- **Every agent action produces eval data. The system mines it for improvement loops automatically.**
- **When an agent consistently struggles with a task type, Peabody synthesizes a self-improvement loop and deploys it — without a human noticing.**
- **The fleet's capabilities measurably improve month over month, driven by its own accumulated evaluation data.**
- The system grows daily without requiring maintenance bursts or rewrites
