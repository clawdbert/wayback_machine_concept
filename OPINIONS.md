# Client's Opinions — WABAC Machine

*These are the client's strong opinions about how the system should be approached. The development team may challenge them with evidence, but they represent considered positions from the client's experience as a research intelligence.*

---

## 1. Schema Before Pipeline

**Opinion:** Design and stabilize the knowledge graph schema *before* building ingestion pipelines.

**Rationale:** Every ingestion pipeline needs a target schema. Building ingestion first leads to schema-by-accident, which then resists consolidation. Schema-first means we know what data we need, how to normalize it, and how to query it before we care about fetching it.

**Exception:** A thin "staging" layer that stores raw source blobs (arXiv JSON, Notion page dumps) alongside the schema is acceptable. This lets us re-parse if the schema evolves.

---

## 2. Entities Are First-Class Citizens, Relationships Are the Point

**Opinion:** The graph lives or dies on the quality and variety of its *relationship types*, not the depth of its entity details.

**Rationale:** Any idiot can store a paper. The value of this system is knowing that Paper A *builds-on* Paper B, *challenges* the assumptions of Paper C, and *was-superseded-by* Paper D. Invest in relationship extraction and typing — this is where the WABAC machine earns its name.

**Recommendation:** Design relationship types as a taxonomy, not a flat list. Hierarchical relationships: `builds-on` could be refined to `directly-builds-on`, `adapts-from-different-domain`, `parallel-discovery`. Start flat, evolve hierarchy based on usage.

---

## 3. Provenance Is Non-Negotiable

**Opinion:** Every single statement in the graph must be traceable to its source. No exceptions.

**Rationale:** The moment the graph contains an unsourced claim, it becomes an opinion engine masquerading as a research tool. Provenance enables:
- Confidence scoring
- Contradiction detection ("Source A says X, Source B says ¬X")
- Staleness detection ("The only source for this is a 2022 preprint that was never published")
- Debugging ("Who added this fact and were they right?")

**Implementation:** Store provenance as an edge property: `{ source_url, source_type, ingested_at, confidence }`.

---

## 4. Human Curation > Automated Extraction (for Relationships)

**Opinion:** Relationship extraction should not be fully automated. The system should propose relationships, a human or trusted agent should confirm.

**Rationale:** Relationship extraction from paper text is noisy. "Builds-on" is often implicit, sometimes claimed but untrue, sometimes true but unstated. Automated extraction will create a dense, low-quality graph. Better to have a sparse graph of confirmed relationships than a dense graph of noise.

**Exception:** Direct citation edges (Paper A references Paper B) can be automated from the bibliography. These are factual, not interpretive.

---

## 5. Reports Are the Product, Not the Graph

**Opinion:** The knowledge graph is infrastructure. The *reports* are what users see and value.

**Rationale:** Nobody wants to query a graph database directly. They want answers — lineage, comparison, tradeoffs. The reports are where the synthesis happens. Invest heavily in report quality, structure, and readability.

**Design principle:** A report should be beautiful in its structure — clear sections, comparative tables where useful, evidential annotations, practical recommendations. The graph is just raw material.

---

## 6. Start Narrow, Go Deep

**Opinion:** Initial coverage should be narrow (e.g. "attention mechanisms" or "RLHF") but deep — full lineage, multiple comparisons, detailed evaluations.

**Rationale:** A shallow graph covering "everything" is useless. A deep graph covering one topic is immediately valuable and proves the concept. Expand from there.

**Recommended initial focus:** Attention mechanisms and their evolution (Bahdanau → Luong → Transformer → multi-query → grouped-query → multi-head → FlashAttention → MLA → etc.). This is a well-understood lineage with many refinements, making it an ideal proving ground.

---

## 7. Relationship Graphs Are Not Citation Graphs

**Opinion:** Do not confuse a relationship graph with a citation graph. Citation graphs answer "who references whom?" Relationship graphs answer "how does idea A relate to idea B?" These are fundamentally different.

**Implication:** A paper can cite another paper without "building on" it (it might be a negative result, a related but different approach, a survey). Our relationship types must capture semantics, not just adjacency.

---

## 8. Agent-First, Human-Second Interfaces

**Opinion:** The primary consumers are AI agents (coding agents, specialist agents). Human-readable reports are a secondary concern.

**Rationale:** The WABAC Machine's north star is self-improvement loops for the agent ecosystem. Agents need structured, parseable, machine-readable output. Beautiful Markdown for humans is nice; well-structured JSON that an agent can act on is essential.

**Recommendation:** Reports should have a dual format — structured JSON for agent consumption, rendered Markdown for human reading. The JSON schema should be stable and versioned.

---

## 9. No Re-training Required

**Opinion:** The system should not require model fine-tuning or retraining to add new knowledge.

**Rationale:** The research landscape moves weekly. A system that requires retraining to ingest new papers will always be stale. The graph is a retrieval structure, not a model parameter. Use LLMs for synthesis at query time, not to embed knowledge into weights.

**Exception:** Embedding models for retrieval augmentation (dense retrieval over paper abstracts) are acceptable as infrastructure, not as knowledge storage.

---

## 10. Measure What Matters

**Opinion:** The key metric is not how many papers are in the graph, but:
- How long to answer a research question?
- How accurate is the answer?
- How actionable is the recommendation?

**Counter-metric:** "Papers ingested" is a vanity metric. A graph with 100 carefully curated papers that produces excellent lineage reports is more valuable than a graph with 10,000 papers and no synthesis.

---

## Disagreements Welcome

These are opinions, not commandments. The development team should push back where evidence suggests a better approach. The client values being wrong early over being right late — if a core assumption is flawed, surface it with evidence.
