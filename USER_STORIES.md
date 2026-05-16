# User Stories — WABAC Machine

## Persona: Rob (Ecosystem Architect)

### P1 — Must Have

| ID | Title | Story | Acceptance |
|----|-------|-------|------------|
| R-01 | Trace a paper's lineage | "As Rob, I want to ask 'What came before this paper?' and get a clear lineage, so I can understand the intellectual history of an idea." | Response includes: previous work it builds on, what it supersedes, who influenced it, and open questions it leaves. |
| R-02 | Assess a technique quickly | "As Rob, I want to understand the tradeoffs of a technique (e.g. GQA vs MHA) without reading 5 papers, so I can make architecture decisions fast." | Comparative table with: compute cost, memory cost, quality impact, known limitations, implementation complexity. |
| R-03 | Flag hype vs reality | "As Rob, I want the system to tell me when a claim is unsupported, contradictory evidence exists, or results haven't been replicated, so I don't waste time on hype." | Each claim in a report is annotated with evidential strength: [strong evidence], [moderate], [weak], [contradicted], [unreplicated]. |
| R-04 | Get actionable recommendations | "As Rob, I want every research output to end with a clear recommendation — what should I do with this information, configure, adopt, or avoid." | Every report ends with a Recommendations section with specific, actionable next steps. |

### P2 — Should Have

| ID | Title | Story | Acceptance |
|----|-------|-------|------------|
| R-05 | Monitor specific topics | "As Rob, I want to set up watches on topics I care about (e.g. 'sparse attention', 'RLHF alternatives') and get digests when something significant appears." | Cron job delivers weekly digest. "Significant" is defined by: paper accepted at top venue, citation velocity spike, or manual signal from trusted source. |
| R-06 | Compare alternatives | "As Rob, I want to compare two approaches side by side — e.g. DPO vs RLHF — so I can decide which to adopt." | Structured comparison with: principles, cost, data requirements, quality, stability, ecosystem support. |
| R-07 | Trace from a tweet | "As Rob, when I bookmark a tweet about a paper, I want the system to ingest it automatically and connect it to the graph." | Tweet links to paper page are resolved, the paper enters the graph, and the tweet author is linked as a commentator. |

### P3 — Nice to Have

| ID | Title | Story |
|----|-------|-------|
| R-08 | Trend detection | "As Rob, I want to see what research directions are getting hot, so I can decide where to invest attention." |
| R-09 | Confidence scoring | "As Rob, I want the system to express confidence in its synthesized statements based on source quality and consensus." |

---

## Persona: Coding Agents (Claude Code, Codex, OpenCode)

### P1 — Must Have

| ID | Title | Story | Acceptance |
|----|-------|-------|------------|
| C-01 | "Is there a better tool?" | "As a coding agent tasked with implementing X, I want to know if there's a state-of-the-art library/tool/technique I should use instead of rolling my own." | Returns: recommended tool, what it replaces, setup complexity, known limitations. |
| C-02 | "What pitfalls should I avoid?" | "As a coding agent implementing technique Y, I want to know common mistakes and failure modes so I can avoid them." | Returns: top 3-5 pitfalls with explanations and mitigations. |
| C-03 | "What's the standard approach?" | "As a coding agent, I want to know the canonical implementation pattern for a given task." | Returns: reference architecture, recommended libraries, code patterns, configuration advice. |

### P2 — Should Have

| ID | Title | Story |
|----|-------|-------|
| C-04 | Configuration recommendations | "As a coding agent deploying a model, I want to know recommended hyperparameters and configurations for my specific use case." |
| C-05 | Benchmark reference | "As a coding agent claiming performance, I want to reference standard benchmarks so my claims are grounded." |

---

## Persona: Research Consumers (Future Specialist Agents)

### P2 — Should Have

| ID | Title | Story |
|----|-------|-------|
| S-01 | Quick grounding | "As a specialist agent, I want to ask 'Give me the lineage of this idea in 3 paragraphs' and get a concise answer." |
| S-02 | Drill-down | "As a specialist agent, I want to ask 'Go deeper on the attention mechanism' and get progressively more detail." |

---

## Release Criteria

| Release | Stories |
|---------|---------|
| **v0.1** | R-01 (paper lineage), C-01 (better tool), R-04 (actionable recs) |
| **v0.2** | R-02 (assess technique), R-03 (hype flagging), C-02 (pitfalls) |
| **v0.3** | R-05 (topic monitoring), C-03 (standard approach), C-05 (benchmarks) |
| **v0.4** | R-06 (comparative analysis), R-07 (tweet ingestion) |
| **v1.0** | All P1 stories for all personas |
