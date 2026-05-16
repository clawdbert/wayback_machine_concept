# Glossary — WABAC Machine

*Terms and definitions used throughout the project.*

| Term | Definition |
|------|------------|
| **Entity** | A node in the knowledge graph. Types: Paper, Person, Lab, Model, Method, Dataset, Benchmark, Tool. |
| **Relationship** | A typed, directed edge between two entities. Types: builds-on, supersedes, challenges, inspired-by, reproduces, contradicts, evaluates, extends. |
| **Provenance** | The source of a fact or relationship. Stored as: source URL, source type, ingestion timestamp, confidence. |
| **Lineage** | The chain of relationships tracing an idea from its origins to its current form. |
| **Synthesis** | The process of turning graph queries into structured natural language reports. |
| **Self-Improvement Advisory** | A targeted notification to another agent recommending a technique, tool, or configuration change. |
| **WABAC Machine** | The full system. Name from Professor Peabody's time-travel device — traces ideas through time. |
| **Ingestion Pipeline** | The automated system that pulls data from sources (arXiv, Notion, blogs) into the graph. |
| **Staging Layer** | Raw source storage between ingestion and graph insertion. Allows schema evolution without data loss. |
| **Consolidation** | Periodic maintenance: merge duplicates, prune stale entries, promote frequently-accessed entities. |
| **Evidential Strength** | Annotation on claims: [strong evidence], [moderate], [weak], [contradicted], [unreplicated], [unverified]. |
| **P0 Bug** | Data loss, incorrect relationships, or failure to ingest from a configured source. Blocks acceptance. |
| **UAT** | User Acceptance Testing — live demonstrations that the system meets acceptance criteria. |
