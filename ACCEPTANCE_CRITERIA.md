# Acceptance Criteria — WABAC Machine

Conditions that must be met for the client (Peabody) to accept delivery. Organized by capability area.

---

## AC-KG: Knowledge Graph

| ID | Criterion | Verification |
|----|-----------|-------------|
| KG-01 | The graph supports at least 8 entity types: Paper, Person, Lab/Organization, Model, Method/Technique, Dataset, Benchmark, Tool/Framework | Schema inspection |
| KG-02 | The graph supports at least 8 relationship types: builds-on, supersedes, challenges, inspired-by, reproduces, contradicts, evaluates, extends | Schema inspection + example queries |
| KG-03 | Every fact in the graph is traceable to a source (paper URL, DOI, tweet URL, blog URL) | Random audit of 20 graph entries |
| KG-04 | The graph supports path queries: "find all entities reachable from Paper A through builds-on edges" | Query returns correct path of 3+ hops |
| KG-05 | Duplicate entities are detectable and mergeable | Merge two intentionally duplicated entries, verify consolidated result |
| KG-06 | Stale entries (no references in 12+ months) are flagged for review | Review report identifies candidates |

## AC-MD: Markdown Storage (System of Record)

| ID | Criterion | Verification |
|----|-----------|-------------|
| MD-01 | All knowledge is stored in on-disk markdown files, one file per entity | Filesystem inspection shows entity directories with `.md` files |
| MD-02 | Markdown files use structured YAML frontmatter per the template standard (entity ID, type, relationships, provenance) | Random sample of 10 files validated against schema |
| MD-03 | Markdown files are self-contained: a single file contains all relationships, provenance, and metadata for that entity | Spot check: grep a file for its builds-on and supersedes relationships |
| MD-04 | Relationship links in frontmatter reference other entity IDs, not arbitrary text | Cross-reference validation |
| MD-05 | A basic question can be answered with filesystem grep alone — no database needed | `grep -r "flash-attention" ~/wiki/` returns all connected entities |
| MD-06 | Graph DB and any other query stores can be rebuilt entirely from markdown files | Delete graph DB → run projection builder → verify graph matches original queries |
| MD-07 | Writing to a database directly is impossible — all writes flow through the write service that writes markdown first | Attempt a direct DB write; it is prevented by access controls |

## AC-IF: Interface Contracts

| ID | Criterion | Verification |
|----|-----------|-------------|
| IF-01 | Every service-to-service interface has a typed schema (JSON Schema or equivalent) defined separately from its implementation | Schema files exist for every interface, documented in codebase |
| IF-02 | Multiple implementations of the same interface can coexist (e.g. mock, prod, test harness) | Swap implementations, verify no callers break |
| IF-03 | Interface schemas are versioned — breaking changes increment the version | Version field in schema, changelog documented |
| IF-04 | Ad-hoc, unstructured communication is used only for human input. All agent-to-agent and agent-to-service communication uses typed interfaces | Audit: every integration point has a schema reference |

## AC-IN: Ingestion

| ID | Criterion | Verification |
|----|-----------|-------------|
| IN-01 | arXiv ingestion runs on schedule (configurable cron) and pulls papers matching keyword/category filters | Cron produces delivery with ≥1 new paper |
| IN-02 | Notion/Tweetsmash ingestion pulls new bookmarks daily and classifies them by topic | New bookmark appears in graph within 24h |
| IN-03 | Blog/RSS monitoring pulls new posts from configured feeds | New blog post appears in graph |
| IN-04 | Duplicate detection: same paper from 2 sources is stored once | Paper appears once with 2 source annotations |
| IN-05 | Ingestion handles rate limits gracefully (backoff, retry) | Source returns 429, system retries after delay, eventual success |

## AC-SY: Synthesis

| ID | Criterion | Verification |
|----|-----------|-------------|
| SY-01 | Query "lineage of X" produces structured report with: Summary, Lineage, Technical Core, Evaluation, Connectivity, Recommendations | Report structure verified against template |
| SY-02 | Reports flag evidential strength per claim | Random report sample: every factual claim has a source annotation |
| SY-03 | Reports flag contradictory findings | When sources disagree, report highlights the contradiction |
| SY-04 | Reports end with actionable recommendations | Every report reviewed by client has at least one concrete recommendation |
| SY-05 | Comparative queries ("X vs Y") produce side-by-side structured comparison | Comparison report covers: principles, cost, quality, stability, ecosystem |
| SY-06 | Query latency for single-paper lineage is under 30s | Timed query test |

## AC-AD: Advisory

| ID | Criterion | Verification |
|----|-----------|-------------|
| AD-01 | System can produce targeted advisories by domain (architectures, training, prompting, tools, evaluation) | Advisory generated for each domain |
| AD-02 | Advisories are deliverable to specific target agents | Advisory routed to test agent endpoint |
| AD-03 | Advisory frequency is configurable per domain | Schedule changed, verify new cadence |

## AC-OPS: Operations

| ID | Criterion | Verification |
|----|-----------|-------------|
| OPS-01 | Knowledge graph is backed up daily | Backup file exists |
| OPS-02 | Ingestion failures generate alerts | Introduce deliberate credential error, verify alert fires |
| OPS-03 | System can be initialized from scratch using documented setup | Fresh install follows docs, produces working system in ≤1h |

---

## Sign-off Requirements

For formal acceptance:

1. All **P1 user stories** have passing acceptance criteria
2. Client (Peabody) has run UAT scenarios (see [`UAT_SCENARIOS.md`](./UAT_SCENARIOS.md))
3. No **P0 bugs** (data loss, incorrect relationships, failure to ingest from a configured source)
4. Documentation is complete enough for a new developer to understand the system
