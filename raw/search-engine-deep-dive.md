# Search Engines & Full-Text Search — Deep Dive Notes
> Study notes compiled from a mentorship session. Stack: .NET / C#. Career target: Mid → Senior Backend Engineer.

---

## Table of Contents
1. [What Is a Search Engine (In Simple Terms)?](#what-is-a-search-engine)
2. [The Inverted Index — The Core Data Structure](#the-inverted-index)
3. [BM25 — Relevance Scoring](#bm25--relevance-scoring)
4. [The Search Mechanism Spectrum](#the-search-mechanism-spectrum)
5. [LeanLucene — A Real-World .NET Example](#leanlucene--a-real-world-net-example)
6. [The Write/Read Split — A Key Architecture Insight](#the-writeread-split)
7. [Where The Industry Is Heading — Hybrid Search](#where-the-industry-is-heading)
8. [Interview Cheat Sheet](#interview-cheat-sheet)
9. [Career Relevance & Next Steps](#career-relevance--next-steps)

---

## What Is a Search Engine?

In simple terms:

> A search engine is a mechanism for indexing a large collection of documents so they can be retrieved **quickly** and in order of **relevance** to the query.

The two key words are **fast** and **relevant**. Any search solution that doesn't address both is incomplete for real-world use.

---

## The Inverted Index

### The Book Analogy (Never Forget This)

If you want to find a term in a book, you don't flip through every chapter. You go to the **index at the back**, look up the term, and get the page numbers instantly.

An **inverted index** is exactly that — but for documents.

```
Normal Index (document → words):
  Doc A → ["lean", "startup", "method"]
  Doc B → ["lean", "coffee", "morning"]

Inverted Index (word → documents):
  "lean"    → [Doc A, Doc B]
  "startup" → [Doc A]
  "coffee"  → [Doc B]
```

When you search for `"lean"`, you don't scan every document. You **look up the word** and instantly get the list. That's O(1) lookup instead of O(n) scan.

### Why This Matters at Scale

| Documents | `string.Contains()` | Inverted Index |
|-----------|---------------------|----------------|
| 1,000     | Fast enough         | Fast           |
| 100,000   | Slow                | Fast           |
| 10,000,000| Unusable            | Fast           |

The index pays a one-time build cost so every query is cheap.

---

## BM25 — Relevance Scoring

**BM25 = "Best Match 25"** — the 25th iteration of a research family from City University London, 1990s. It's the default scoring algorithm in Elasticsearch, Solr, and LeanLucene.

### The Problem It Solves

`string.Contains()` gives you a binary yes/no. BM25 gives you a **score** — so you can rank results by relevance.

### Three Core Ideas

#### 1. Term Frequency (TF) — with diminishing returns

The more times a search term appears in a document, the more relevant it is — **but not infinitely so**.

```
"lean" appears 1x  →  big score bump
"lean" appears 2x  →  medium bump
"lean" appears 10x →  tiny bump (saturates)
```

Think of it like caffeine. The first coffee wakes you up. The fifth one barely helps.

#### 2. Inverse Document Frequency (IDF) — rare words matter more

If a word appears in *every* document, it carries no signal.

```
"the"    → in 999,000/1,000,000 docs → nearly worthless
"lucene" → in 42/1,000,000 docs      → very strong signal
```

BM25 automatically rewards rare, specific terms and discounts common ones — no manual stop-word list required.

#### 3. Document Length Normalization

A document that mentions a term once across 10 words is likely more relevant than one that mentions it once across 10,000 words. BM25 normalizes for this.

### The Formula (Conceptual)

```
Score(doc, query) = IDF × (TF × (k1 + 1))
                         ─────────────────────────────────
                         TF + k1 × (1 - b + b × (docLen / avgDocLen))
```

- `k1` (~1.2) — controls TF saturation rate
- `b` (~0.75) — controls length normalization strength
- Both are **tunable**

### Full Query Flow

```
Query: "lean startup"
            │
            ▼
     ┌─────────────┐
     │  Tokenizer  │  → ["lean", "startup"]
     └─────────────┘
            │
     ┌──────┴───────┐
     │              │
  "lean"        "startup"
  IDF: 0.8      IDF: 2.1   ← "startup" rarer → worth more
     │              │
  Score per doc  Score per doc
     │              │
     └──────┬───────┘
            ▼
     Final Score = sum → rank → return top N
```

---

## The Search Mechanism Spectrum

```
Simple ────────────────────────────────────────── Powerful
  │                                                   │
  ▼                                                   ▼
Contains    LIKE    Regex    B-Tree    Inverted    Vector
                             Index      Index       KNN
```

### 1. Linear Scan (`string.Contains`, SQL `LIKE '%term%'`)
- **How:** Read every document, check for match
- **Good for:** Tiny datasets, zero setup
- **Fails at:** Any real scale — O(n) per query

### 2. B-Tree Index (SQL Server default)
- **How:** Sorted tree structure — O(log n) traversal
- **Good for:** Exact matches, range queries (`WHERE price > 100`)
- **Fails at:** Full-text search inside paragraphs — only understands whole field values, not words *within* them

### 3. Regex
- **How:** Pattern matching engine, scans character by character
- **Good for:** Structured patterns (emails, phone numbers, validation)
- **Fails at:** Scale — still requires scanning all documents

### 4. Inverted Index (Lucene, Elasticsearch, LeanLucene)
- **How:** Pre-built word → documents mapping + BM25 scoring
- **Good for:** Full-text search across large document sets with relevance ranking
- **Trade-off:** Build cost upfront, more memory/storage

### 5. Vector / KNN Search (AI-era search)
- **How:** Convert text to numeric vectors (embeddings) using an AI model. Find nearest neighbors by cosine similarity.
- **Good for:** Semantic/meaning-based search — finds `"automobile"` when you search `"car"`
- **Why it's important now:** Powers RAG (Retrieval Augmented Generation) in AI systems

```
"car"  →  [0.2, 0.8, 0.1, 0.9, ...]  ← embedding vector

Doc A ("automobile"):  cosine similarity → 0.97  ✅
Doc B ("recipe"):      cosine similarity → 0.12  ❌
```

### Decision Framework

| Use Case | Best Mechanism |
|----------|---------------|
| Find user by email | B-Tree (SQL index) |
| Pattern matching / validation | Regex |
| Full-text across paragraphs | Inverted Index |
| Find by meaning, not just keywords | Vector / KNN |
| Both keyword + semantic (modern AI apps) | **Hybrid Search** |

---

## LeanLucene — A Real-World .NET Example

**Repo:** [github.com/jordansrowles/leanlucene](https://github.com/jordansrowles/leanlucene)

A .NET-native full-text search engine inspired by Apache Lucene. Targets `net10.0+`. Pure C# with minimal dependencies.

### Why It's Worth Studying

- No Elasticsearch cluster needed — embedded in-process search
- Implements inverted index + BM25 + vector search in one library
- Production-quality: benchmarked against Lucene.NET, OpenTelemetry support, atomic commits
- Great source code for understanding how a real search engine is architected

### Key Components

```
Your App
   │
   ▼
IndexWriter          ← Builds and maintains the inverted index
   │  Buffers in RAM → flushes immutable Segments to disk
   ▼
[seg_0] [seg_1] [seg_2]    ← memory-mapped files (fast reads)
   │
   ▼
IndexSearcher        ← Queries the index, scores with BM25
   │
   ▼
Search Results (topN hits, scores, highlights, facets)
```

### Quick Start (C#)

```csharp
var dir = new MMapDirectory("path/to/index");
var config = new IndexWriterConfig();

using var writer = new IndexWriter(dir, config);
var doc = new LeanDocument();
doc.Add(new TextField("title", "hello world", stored: true));
doc.Add(new StringField("id", "1", stored: true));
writer.AddDocument(doc);
writer.Commit();

using var searcher = new IndexSearcher(dir);
var results = searcher.Search("hello", "title", topN: 10);
```

### Supported Query Types (Study These)

| Query | What It Does |
|-------|-------------|
| `TermQuery` | Exact single term match |
| `BooleanQuery` | Combine Must / Should / MustNot clauses |
| `PhraseQuery` | Ordered sequence of terms, with slop |
| `FuzzyQuery` | Edit-distance matching ("wrold" → "world") |
| `VectorQuery` | KNN by cosine similarity |
| `RrfQuery` | Reciprocal Rank Fusion — combines multiple result sets |
| `GeoBoundingBoxQuery` | Geographic filtering |

---

## The Write/Read Split

One of the most important architectural patterns in LeanLucene — and in system design generally:

```
IndexWriter  (write path)  =  Expensive. Happens once or incrementally.
                               Builds the index. Flushes segments to disk.

IndexSearcher (read path)  =  Cheap. Happens on every query.
                               Reads memory-mapped segments. Returns results fast.
```

**The principle:** Pay the cost once on write so every read is cheap.

This same pattern appears everywhere in software:
- **Database indexes** — slow inserts, fast reads
- **Caching** — compute once, serve many times
- **Compiled code** — slow compilation, fast execution
- **CDNs** — distribute assets once, serve globally

Recognizing this pattern across different contexts is a senior-level skill.

---

## Where The Industry Is Heading

### Hybrid Search = Inverted Index + Vector Search

Modern AI-powered search combines both mechanisms:

```
Query: "fast document retrieval system"
          │
    ┌─────┴──────┐
    │            │
Keyword        Vector
Search         Search
(BM25)         (KNN)
    │            │
    └─────┬──────┘
          │
     RRF Fusion     ← Reciprocal Rank Fusion
          │
     Final ranked results
```

**Why this matters for your career:**
- Elasticsearch 8.x added vector search
- LeanLucene supports both out of the box
- Every AI application doing RAG (Retrieval Augmented Generation) is doing hybrid search under the hood
- This is a highly sought skill in 2025-2026

---

## Interview Cheat Sheet

### "How would you add search to this system?"

| Engineer Level | Typical Answer |
|---------------|----------------|
| Junior | `WHERE column LIKE '%query%'` |
| Mid-level | "Use Elasticsearch" |
| **Senior** | "Depends on scale and operational budget. For embedded, low-ops search, something like LeanLucene with BM25 gives relevance ranking without cluster overhead. For enterprise analytics at scale, I'd reach for OpenSearch or Elasticsearch. If semantic search matters, I'd add a vector store or use a hybrid approach." |

### Key Terms to Own

| Term | One-Line Definition |
|------|---------------------|
| Inverted index | Word → document mapping; enables O(1) term lookup |
| BM25 | Relevance scoring: balances term frequency, rarity, and doc length |
| TF-IDF | Predecessor to BM25; still used in some contexts |
| Segment | Immutable unit of an index; merged in the background |
| Memory-mapped file | OS maps disk file into memory — zero-copy fast reads |
| KNN / Vector search | Find nearest neighbors by semantic similarity |
| Hybrid search | Combines BM25 + vector for keyword + semantic results |
| RRF | Reciprocal Rank Fusion — merges ranked lists from multiple sources |
| Atomic commit | Write succeeds completely or not at all — no partial states |

---

## Career Relevance & Next Steps

### Why Search Engine Knowledge Gets You Hired

1. **It's infrastructure-level thinking** — not just CRUD, but understanding how data retrieval actually works at scale
2. **It crosses stacks** — the same concepts apply to Elasticsearch, SQL full-text search, Azure Cognitive Search, and AI vector DBs
3. **AI relevance** — RAG pipelines, semantic search, and LLM context retrieval all sit on these foundations
4. **It's rare** — most backend devs can hook up Elasticsearch; few understand *why* it works

### Portfolio Project Ideas Using These Concepts

| Project | Why It Stands Out |
|---------|------------------|
| Local log file search CLI (`logwatcher`) | Full-text + fuzzy search over structured text — real dev tool |
| Code search tool for `.cs` files | Like `grep` but ranked by relevance — indexing your own codebase |
| LeanLucene MCP server | Expose an index as an AI tool — bridges search + AI workflow |
| Benchmark: LeanLucene vs SQL LIKE vs Regex | Data-driven comparison — shows you understand tradeoffs |

---

*Notes compiled May 2026. Stack: .NET 8/10, C#. Target: Mid → Senior Backend Engineering roles.*
