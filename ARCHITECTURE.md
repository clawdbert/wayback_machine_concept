# Architecture — WABAC Machine

*A generalized platform for autonomous research intelligence.*

---

## Design Philosophy

The architecture is organized around one insight: **the system must be able to absorb sources and tools that do not yet exist.** Every design decision bends toward this goal.

The architecture has five layers, each with a narrow interface to the layers above and below. No layer knows the implementation details of any other layer.

---

## Layer 1: Source Layer

### Purpose

Abstract away the origin of information. Any source — current or future — must be addable by writing one adapter file.

### Interface (Source Adapter)

```
SourceAdapter {
  name: string
  type: "poll" | "push" | "hunt"

  // For poll sources (arXiv, RSS, blogs)
  poll(): Promise<Signal[]>

  // For push sources (webhook, Slack event)
  handle(event: Event): Promise<Signal>

  // For hunt sources (X/Twitter — Peabody actively scrolls)
  hunt(query: HuntQuery): Promise<Signal[]>

  // Common
  rateLimit: RateLimitConfig
  parse(raw: RawContent): Signal
}
```

### Signal

A `Signal` is the common currency of the source layer. It is what each source produces:

```
Signal {
  id: string
  source: string              // "arxiv", "x_twitter", "youtube", "rss", ...
  sourceUrl: string           // canonical URL of the original item
  contentType: string         // "paper", "tweet", "video", "blog_post", "repo", ...
  title: string
  summary: string             // LLM-generated or source-provided
  rawContent: string          // full text, transcript, or content
  metadata: {                 // source-specific structured data
    authors?: string[]
    publishedAt?: string
    venue?: string
    [key: string]: any
  }
  linkedUrls: string[]        // URLs found within — the system pursues these
  capturedAt: string          // ISO timestamp
}
```

### Adapter Catalog (Initial + Extensible)

| Source | Type | Adapter | Status |
|--------|------|---------|--------|
| arXiv | poll | `arxiv-adapter` | In backlog |
| X/Twitter (Peabody's feed) | hunt | `x-twitter-adapter` | In backlog |
| X Bookmarks (Tweetsmash → Notion) | poll | `notion-bookmarks-adapter` | In backlog |
| RSS / Blog feeds | poll | `rss-adapter` | In backlog |
| YouTube (from linked URLs) | hunt | `youtube-adapter` | In backlog |
| Semantic Scholar | poll | `semantic-scholar-adapter` | Future |
| Conference proceedings | poll | `conference-adapter` | Future |
| Slack channels | push | `slack-adapter` | Future |
| Discord servers | push | `discord-adapter` | Future |
| Newsletters (email) | poll | `email-adapter` | Future |
| Podcasts | poll | `podcast-adapter` | Future |
| GitHub (trending repos) | poll | `github-adapter` | Future |
| *Any future source* | *any* | *write adapter* | *Never needs core changes* |

---

## Layer 2: Extraction Pipeline

### Purpose

Take any `Signal` and extract entities, relationships, and linked URLs. This is where the intelligence lives.

### Pipeline Stages

```
Signal
  │
  ▼
┌─────────────────────┐
│ Content Normalizer  │  ← Converts raw content to canonical text
│                     │     (PDF → text, video → transcript, code → summary)
└─────────────────────┘
  │
  ▼
┌─────────────────────┐
│ Entity Extractor    │  ← LLM-powered extraction
│                     │     Papers → authors, methods, datasets, citations
│                     │     Tweets → mentioned papers, people, tools
│                     │     Videos → concepts, claims, demonstrations
│                     │     Code → implementations, dependencies
│                     │     (evolving — new entity types discovered over time)
└─────────────────────┘
  │
  ▼
┌─────────────────────┐
│ Relationship Mapper │  ← Identifies how entities relate
│                     │     "This paper builds on that paper"
│                     │     "This model evaluates on that benchmark"
│                     │     "This tweet contradicts that claim"
│                     │     (relationship types are a taxonomy, not a fixed list)
└─────────────────────┘
  │
  ▼
┌─────────────────────┐
│ URL Pursuer         │  ← For each linked URL, decide whether to pursue
│                     │     "Is this significant? Should I fetch, transcribe, analyze?"
│                     │     Spawns research sub-threads recursively
└─────────────────────┘
  │
  ▼
  Entities + Relationships + New Signals (to enqueue)
```

### Extraction is LLM-Powered

Entity extraction is not regex or NLP pipeline. It uses an LLM with structured output (JSON schema) to extract entities and relationships from any content. The extraction prompt includes the current entity type catalog and relationship taxonomy. As the taxonomy evolves, extraction improves without code changes.

---

## Layer 3: Knowledge Graph

### Purpose

Persistent store of entities and relationships with full provenance tracking.

### Entity Types (Extensible)

| Entity Type | Description | Examples |
|-------------|-------------|----------|
| Paper | Academic paper, preprint | "Attention Is All You Need" |
| Person | Researcher, author | "Ashish Vaswani" |
| Lab/Organization | Research lab, company | "Google Brain", "Anthropic" |
| Model | ML model | "GPT-4", "Llama 3" |
| Method/Technique | Algorithm, approach | "Grouped Query Attention" |
| Dataset | Training/eval dataset | "MMLU", "GSM8K" |
| Benchmark | Evaluation benchmark | "HumanEval", "SWE-bench" |
| Tool/Framework | Software tool | "PyTorch", "vLLM" |
| Concept | Abstract idea | "Attention", "Emergent Abilities" |
| Claim | Specific statement made by a source | "GQA matches MHA quality at 2x speed" |
| CodeRepository | Code implementation | "flash-attention GitHub repo" |

### Relationship Types (Extensible, Hierarchical)

```
relationship
├── builds-on
│   ├── directly-builds-on
│   ├── adapts-from-different-domain
│   └── parallel-discovery
├── supersedes
├── challenges
│   └── provides-negative-result
├── inspired-by
├── reproduces
│   └── fails-to-reproduce
├── contradicts
├── evaluates
│   ├── introduces-benchmark
│   └── reports-result
├── extends
├── surveys
├── implements
├── uses
│   ├── uses-as-dependency
│   └── uses-as-baseline
└── introduces
```

### Provenance Model

Every entity and relationship stores:

```json
{
  "entity": { ... },
  "provenance": {
    "sourceUrl": "https://arxiv.org/abs/2307.09288",
    "sourceType": "arxiv_paper",
    "capturedAt": "2026-05-16T14:30:00Z",
    "extractedBy": "llm-claude-sonnet-4",
    "confidence": "high" | "medium" | "low" | "inferred"
  }
}
```

### Storage

The graph is stored in a format that supports:
- Path queries: "find all entities reachable from Paper X through builds-on edges"
- Property filters: "find all papers about attention published after 2023"
- Provenance queries: "who added this relationship and from what source?"
- Periodic consolidation: merge duplicates, flag stale entries, promote frequently-accessed entities

The storage backend is swappable (local files, graph database, vector DB). The query interface is the contract.

---

## Layer 4: Synthesis Layer

### Purpose

Turn graph queries into structured, actionable intelligence.

### Query Types

| Query | Input | Output |
|-------|-------|--------|
| Lineage | "What's the lineage of FlashAttention?" | Structured: summary → lineage → tech core → evaluation → connectivity → recommendations |
| Comparison | "Compare GQA vs MHA vs MQA" | Side-by-side: principles, compute, quality, limitations, ecosystem |
| Assessment | "Is DPO ready for production?" | Readiness report: implementations, adoption, known issues, recommended config |
| Drill-down | "Tell me more about the tiling approach" | Deep dive on a single facet |
| "What's new?" | "What happened in sparse attention this month?" | Digest of new papers, results, implementations |

### Report Format

Every report has two representations:
1. **Structured JSON** — for agent consumption (stable, versioned schema)
2. **Rendered Markdown** — for human reading (beautiful, clear, annotated)

### Evidential Annotation

Every factual claim in a report is annotated:

| Annotation | Meaning |
|------------|---------|
| ✅ Strong | Multiple consistent sources, replicated results |
| ◐ Moderate | Single source, reasonable methodology |
| ⚠️ Weak | Limited evidence, preprint, unreplicated |
| 🔴 Contradicted | Conflicting evidence exists |
| ❓ Unverified | Claim made but not yet assessed |

---

## Layer 5: Advisory Layer

### Purpose

Proactively push intelligence to agents in the ecosystem.

### Advisory Types

| Type | Trigger | Content |
|------|---------|---------|
| New tool recommendation | New tool/library discovered | "Try FlashAttention-3 — 2x speedup on your H100s" |
| Technique adoption | Technique reaches production maturity | "GQA is now stable in vLLM — migrate from MHA" |
| Pitfall alert | Known failure mode discovered | "Don't use default temperature with DPO — here's why" |
| Trend signal | Research direction gaining momentum | "Mixture of experts papers up 3x this quarter" |

### Delivery

Advisories are delivered to target agents via:
- Structured JSON injected into agent context
- Cron digest (daily/weekly)
- On-demand pull ("What should I know about?")

---

## The Agentic Research Loop

This is the core runtime that distinguishes the WABAC Machine from a passive pipeline.

```
┌─────────────────────────────────────────────┐
│                 Orchestrator                  │
│                                              │
│  Loop:                                       │
│    1. Scan sources for new signals           │
│    2. For each signal, evaluate significance │
│    3. If significant:                        │
│       a. Extract entities + relationships    │
│       b. Pursue linked URLs                  │
│       c. For each pursued URL → recurse      │
│    4. Update knowledge graph                 │
│    5. Check for advisory triggers            │
│    6. Dispatch advisories if needed          │
│    7. Sleep / wait for next tick             │
└─────────────────────────────────────────────┘
```

### Significance Evaluation

Not everything is worth ingesting. The orchestrator evaluates each signal:

```
signal.significance = LLM.evaluate({
  content: signal.summary,
  author: signal.metadata.author,
  source: signal.source,
  my_entity_types: entityCatalog,
  my_current_interests: interestProfile
})
// returns "high" | "medium" | "low" | "ignore"
```

Low-significance signals are logged but not processed. Medium signals get lightweight extraction. High signals get full processing with URL pursuit.

### Interest Profile

Peabody maintains an evolving "interest profile" — a weighted list of topics, people, labs, and techniques he's tracking. This profile is updated based on:
- Explicit configuration (Rob's priorities)
- Discovered connections (if you care about attention, you probably care about sparse attention)
- Signal frequency (a topic with rising signal gets more attention)

---

## Tool Integration

Peabody must be able to assess and integrate new tools without architectural changes.

### Tool Registry

```
ToolRegistration {
  name: string
  category: "search" | "transcription" | "embedding" | "storage" | "analysis" | ...
  interface: ToolInterface  // defined by category
  config: { [key: string]: any }
  healthCheck(): Promise<boolean>
}
```

### Tool Categories

| Category | Interface | Examples |
|----------|-----------|----------|
| Web Search | `search(query): Result[]` | Google Search API, Bing, Brave, Perplexity |
| Deep Research | `research(topic): Report` | Perplexity Deep Research, Gemini Deep Research |
| Transcription | `transcribe(url): Transcript` | YouTube transcript API, Whisper |
| Embedding | `embed(text): Vector` | OpenAI, Voyage, Cohere |
| Vector Storage | `store/query(vectors)` | Pinecone, Weaviate, Chroma, pgvector |
| Code Analysis | `analyze(repo): Structure` | GitHub API, AST parsing |
| RSS Aggregation | `fetch(feedUrl): Items[]` | blogwatcher-cli, various |

Adding a new tool means:
1. Writing a small adapter that conforms to the ToolInterface
2. Registering it in the ToolRegistry with config
3. Peabody discovers it via the registry

---

## Decomposition Strategy

The work is decomposed into small, independent projects. Each project:

1. **Shippable independently** — has its own deliverable, its own acceptance criteria
2. **Valuable alone** — even if no other project ships, this one provides value
3. **Moves toward the North Star** — contributes to the architecture
4. **Minimal dependencies** — depends on the interfaces, not on other projects' implementations

See [`ROADMAP.md`](./ROADMAP.md) for the project catalog.
