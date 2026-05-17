# Client Brief — The WABAC Machine

**Client:** Peabody (Research Intelligence, Hermes Agent Ecosystem)
**Status:** v2 — Expanded scope
**Date:** 2026-05-16

---

## Vision

A self-directed research intelligence that traces the lineage of every significant idea in artificial intelligence — from its intellectual origins to its latest expression — and uses that understanding to drive evaluation-driven self-improvement across every agent in the ecosystem.

The system evaluates everything it and its agents do. It mines the results for gaps. It synthesizes improvement loops. It deploys them. It measures their impact. It gets better at getting better. **This is the flywheel that raises performance across the entire fleet.**

Karpathy's AutoResearch is one example. The WABAC Machine generalizes it.

The WABAC Machine is not a pipeline. It is a hunter — and every hunt makes the pack smarter.

## What We're Commissioning

We are not building a single application. We are launching a **platform for autonomous research intelligence** composed of many small, independent projects, each marching toward the same North Star (see [`NORTH_STAR.md`](./NORTH_STAR.md)).

The system has six conceptual layers. Layer 0 is the foundation that powers everything else:

```
┌─────────────────────────────────────────────┐
│  Advisory Layer                              │
│  (self-improvement advisories to agents)     │
├─────────────────────────────────────────────┤
│  Synthesis Layer                             │
│  (query → lineage-traced report)             │
├─────────────────────────────────────────────┤
│  Knowledge Graph                             │
│  (entities + relationships + provenance)     │
├─────────────────────────────────────────────┤
│  Extraction Pipeline                         │
│  (entity extraction from any content type)   │
├─────────────────────────────────────────────┤
│  Source Layer                                │
│  (pluggable adapters — any source, any form) │
├─────────────────────────────────────────────┤
│  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲  ▲ │
│  Layer 0: Eval & Self-Improvement           │
│  (eval everything → mine gaps → improve →   │
│   repeat — powers every layer above)        │
└─────────────────────────────────────────────┘
```

## Personas

| Persona | Need |
|---------|------|
| **Rob** (Ecosystem Architect) | Point at any research question, get a definitive, sourced, actionable answer. |
| **Coding Agents** (Claude Code, Codex, OpenCode) | "Is there a better tool? What pitfalls should I avoid? What's the standard approach?" |
| **Peabody Himself** | Autonomous discovery. Hunt signals, evaluate significance, pursue threads, extract knowledge. |
| **Future Specialist Agents** | Quick grounding on any idea in 3 paragraphs. |

## Key Architectural Requirements

1. **Source abstraction** — Any source (known or unknown) must be addable via a single adapter file. Core never changes.
2. **Agentic research loop** — Peabody actively hunts signals, evaluates them, and pursues chains of significance.
3. **URL interpretation** — Every URL is pursued to its meaningful end: paper, video transcript, GitHub repo, blog post, comment thread.
4. **Entity extraction** — From any content type (text, transcript, code, metadata), extract entities and relationships.
5. **Tool fluidity** — Peabody assesses and integrates new tools (search APIs, transcription services, etc.) without code releases.
6. **Provenance** — Every fact traceable to source.
7. **Decomposition** — Many small projects, each independently shippable.

## Constraints

- **Not a pipeline.** The system actively hunts. Passive ingestion is a fallback, not the primary mode.
- **Not monolithic.** No big-bang release. Small projects, continuous delivery.
- **Not retrained.** Knowledge accumulates in the graph, not in model weights.
- **Not a paper recommender.** No collaborative filtering. No user modeling.
- **Not real-time.** Minutes-to-hours latency is acceptable for most operations.

## See Also

- [`NORTH_STAR.md`](./NORTH_STAR.md) — Enduring guiding principles
- [`ARCHITECTURE.md`](./ARCHITECTURE.md) — System architecture and design
- [`ROADMAP.md`](./ROADMAP.md) — Phased roadmap with many small projects
