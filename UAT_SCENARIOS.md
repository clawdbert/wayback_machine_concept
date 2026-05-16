# User Acceptance Test Scenarios — WABAC Machine

*How the client (Peabody) will verify the system is ready for acceptance. These are live demonstrations, not unit tests.*

---

## UAT-01: Paper Lineage (R-01, KG-04, SY-01)

**Setup:** Graph contains the attention mechanism lineage (Bahdanau → Luong → Transformer → multi-query → grouped-query → FlashAttention → MLA).

**Test:**
1. Query: "Show me the lineage of FlashAttention"
2. System returns structured report with:
   - Summary of what FlashAttention is
   - Lineage: what it builds on (self-attention → multi-head → IO-aware attention), what it supersedes (standard attention for long sequences)
   - Technical core (tiling, recomputation, IO complexity analysis)
   - Evaluation: speedup numbers, memory reduction, precision impact
   - Connectivity: related approaches (sparse attention, linear attention)
   - Recommendations: when to use, when not to

**Pass condition:** All 6 sections present, lineage path verified correct, at least 2 actionable recommendations.

---

## UAT-02: Technique Assessment (R-02, SY-05)

**Setup:** Graph covers GQA, MHA, MQA.

**Test:**
1. Query: "Compare GQA vs MHA vs MQA — tradeoffs"
2. System returns structured comparison:
   - Principles (how each works)
   - Compute cost (KV-cache size, FLOPs comparison)
   - Quality impact (benchmark numbers)
   - Known limitations
   - Implementation complexity
   - When to choose which

**Pass condition:** Side-by-side comparison with at least 3 dimensions (compute, quality, complexity). Source papers cited.

---

## UAT-03: Hype Detection (R-03, SY-02, SY-03)

**Setup:** Graph contains a paper making strong claims that have contradictory evidence.

**Test:**
1. Query about the paper
2. Report must flag: "This paper claims X. Contradictory evidence from [Source B] suggests ¬X. The claim has not been independently replicated."

**Pass condition:** At least one factual claim annotated with an evidential flag other than "strong evidence."

---

## UAT-04: Tool Recommendation for Coding Agent (C-01, C-02)

**Scenario:** A coding agent is tasked with implementing long-context attention.

**Test:**
1. Query: "What's the best library for long-context attention?"
2. Response should recommend: e.g. FlashAttention-3, with alternatives (RingAttention, sparse attention), setup complexity, known pitfalls

**Pass condition:** Returns specific library name, alternatives, and top pitfalls (e.g. "FlashAttention requires CUDA compute capability 8.0+").

---

## UAT-05: New Paper Ingestion (IN-01, IN-02, IN-04)

**Setup:** arXiv monitoring configured for "attention mechanisms" keywords.

**Test:**
1. A new attention-related paper appears on arXiv
2. Within 24h (or next cron tick), the paper is in the graph
3. Paper's citations to existing graph entities create edges
4. Query about the new paper returns lineage report

**Pass condition:** Paper in graph, at least one relationship edge created, lineage report includes the new paper.

---

## UAT-06: Tweet Ingestion Integration (R-07, IN-02)

**Setup:** Notion/Tweetsmash integration active.

**Test:**
1. Rob bookmarks a tweet about a paper on X
2. Tweetsmash syncs to Notion database
3. Next cron tick: system detects new entry, resolves the paper URL, ingests it
4. Paper appears in graph with annotation "mentioned by @author on X"

**Pass condition:** Paper in graph with tweet source annotation within 24h.

---

## UAT-07: Advisory Delivery to Agent (AD-01, AD-02)

**Setup:** Self-improvement advisory channel configured.

**Test:**
1. System detects a new training technique applicable to coding agents
2. Advisory produced with: summary, applicability, implementation guide, risks
3. Advisory delivered to the target agent's input channel

**Pass condition:** Agent receives and can parse the advisory. Recommendation is actionable (specific config changes, code patterns, or tool switches).

---

## UAT-08: Graph Consolidation (KG-05, KG-06)

**Setup:** Graph has had 3 months of ingestion.

**Test:**
1. Run consolidation
2. Verify: duplicate entries merged, stale entries flagged, no relationship breakage
3. Query the same paper before and after — identical results except duplicates are gone

**Pass condition:** Post-consolidation entity count < pre-consolidation count. Query results unchanged or improved (more complete).

---

## UAT-09: Cold Start (OPS-03)

**Setup:** Clean environment, system not installed.

**Test:**
1. Follow setup documentation
2. System is operational and can answer a basic query within 1 hour
3. First ingestion completes successfully

**Pass condition:** New developer can go from zero to working system in ≤1 hour following docs alone.

---

## UAT-10: Edge Cases

| Scenario | Expected Behavior |
|----------|-------------------|
| Query about unknown topic | "I don't have enough information. Here's what I would do to find out: [research plan]" |
| Source API is down (arXiv, Notion) | Graceful degradation. Previously ingested data still queryable. Alert logged. |
| Contradictory sources ingested | Graph stores both facts with provenance. Report flags contradiction. |
| Query is ambiguous | Ask clarifying question before answering. |
| Duplicate paper from 2 sources | Merged with dual provenance annotations. |
