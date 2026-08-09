---
type: synthesis
title: Large Database Models — AI for SQL Data
description: LDMs train embeddings over selected tables in a relational database so semantic queries (similarity, anomaly, clustering, analogy) run as standard SQL where the data lives — reaching the ~99% of enterprise data that never gets in front of an LLM.
bundle: ai-engineering
topic: model-fundamentals
tags:
- model-fundamentals
- knowledge-management
- ai-native-business
resource: https://www.youtube.com/watch?v=uU1EP9_4qBU
sources:
- id: transcript-what-are-large-database-models-ai-for-sql-data
  resource: Resources/transcriptions/transcript-what-are-large-database-models-ai-for-sql-data.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-05T09:00:00Z'
status: stable
related:
- ai-engineering/model-fundamentals/compression-is-intelligence.md
- ai-engineering/ai-product-development/when-not-to-use-ai.md
---

# Large Database Models — AI for SQL Data

**Source:** [What Are Large Database Models? AI for SQL Data](https://www.youtube.com/watch?v=uU1EP9_4qBU) (IBM Technology, Martin Keen)

---

## Summary

A Large Database Model (LDM) is trained not on text corpora but on selected tables or views in a relational database — where, by estimate, 99% of enterprise data lives and never ends up in front of an LLM, often behind encryption and access control (source: transcript-what-are-large-database-models-ai-for-sql-data.md). By learning vector embeddings of column values from how they co-occur across rows, an LDM lets anyone who can write SQL ask semantic questions ("customers most similar to this one, per the model trained on this table") without a data scientist hand-picking WHERE-clause fields, and with the data used in place rather than moved to an external AI system (source: transcript-what-are-large-database-models-ai-for-sql-data.md).

## The problem with the rigid-SQL path

The traditional similar-customers flow — data scientist builds a profile, writes `WHERE age BETWEEN 20 AND 40 AND city = 'New York' AND beauty_spend > 1000` — has three costs: someone must *guess* which of dozens of columns correlate with similarity; the extract-load-analyse-report loop is slow; and it is expensive — IBM reports customers spend roughly 32–40% of IT budget just moving data around, and data that leaves its regulated environment gets harder to secure and track (source: transcript-what-are-large-database-models-ai-for-sql-data.md). The LDM query instead asks for IDs most similar to a given customer per the trained model — and can still combine with ordinary WHERE clauses, mixing semantic and standard SQL (source: transcript-what-are-large-database-models-ai-for-sql-data.md).

## How an LDM works — five steps

1. **Pick a table/view** and classify each column: categorical (discrete values), numeric (continuous), or key (source: transcript-what-are-large-database-models-ai-for-sql-data.md).
2. **Tokenize every value.** Embeddings map similar words (cat/kitten) to nearby vectors, but numbers are just tokens to a model — 37 and 38 are no closer than 37 and "kitten", and rare continuous values (37.5, 37.6) appear too seldom to learn good vectors. So numeric columns are **binned** by a clustering algorithm (ages 37 and 38 land in the same bucket and become literally the same token), and every value is tagged with its column name so "New York" in `city` differs from "New York" elsewhere (source: transcript-what-are-large-database-models-ai-for-sql-data.md).
3. **Each row becomes an unordered "sentence"** (bag of words): every token — `city:New York`, `age:B7`, `category:beauty` — relates equally to every other token in the row regardless of column position (source: transcript-what-are-large-database-models-ai-for-sql-data.md).
4. **Self-supervised training** over the row-sentences learns a vector for each unique token; values appearing in similar rows end up near each other (cities where customers behave alike cluster) (source: transcript-what-are-large-database-models-ai-for-sql-data.md).
5. **Vectors exposed through SQL.** The trained model loads back into the database, enabling similarity, dissimilarity (transactions least like the normal pattern), clustering, analogy (does one relationship look like another), and commonality queries — all as standard SQL in place (source: transcript-what-are-large-database-models-ai-for-sql-data.md).

## In production

Live uses include insurance (retrieving the most similar past contracts from millions of records to predict quote success), fraud teams flagging transactions unlike typical patterns, contract portfolios spotting outlier agreements, and product-similarity exploration in food retail (nutritionally similar to toffee-covered almonds: oatmeal) (source: transcript-what-are-large-database-models-ai-for-sql-data.md). Commercially, IBM shipped the first LDM-based product in 2022 — SQL Data Insights in Db2 for z/OS — and SQL Data Insights Pro (March 2026) extends it to unstructured text alongside structured columns with incremental model refresh (source: transcript-what-are-large-database-models-ai-for-sql-data.md).

## Key Takeaways

- LDMs attack the 99% of enterprise data locked in relational databases, not the 1% LLMs see.
- The clever bits are in tokenization: bin numeric values into clusters so nearby numbers share a token, and namespace every value by its column.
- Rows as bag-of-words sentences + self-supervised embedding training turns co-occurrence structure into a queryable vector space.
- Two properties drive adoption: the model runs where the data already lives (no movement, no new security surface), and anyone who can write SQL can ask semantic questions without a pipeline.

## Related

- [[compression-is-intelligence]] · [Compression is Intelligence](../model-fundamentals/compression-is-intelligence.md) — the representation-learning fundamentals (embeddings, self-supervised objectives) LDMs apply to tabular data
- [[when-not-to-use-ai]] · [Knowing When Not to Use AI](../ai-product-development/when-not-to-use-ai.md) — companion IBM framing: LDMs sit in the ML quadrant (patterns in structured data), not the generative one
