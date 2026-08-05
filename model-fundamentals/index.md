# Model Fundamentals

How AI models learn and represent data, explained from first principles: information theory and the compression⇄prediction equivalence behind cross-entropy pre-training, and model families beyond LLMs such as Large Database Models (embeddings over structured relational data). The conceptual bedrock under harness engineering and model-selection decisions.

## Articles

- [[compression-is-intelligence|Compression is Intelligence — Entropy, Cross-Entropy, and LLM Pre-Training]] — Shannon's information/entropy rediscovered via compression limits; cross-entropy as the forced choice of LLM loss; distillation and KL divergence (3Blue1Brown, parts 1–2)
- [[large-database-models|Large Database Models — AI for SQL Data]] — LDMs bring embeddings and semantic queries directly into relational databases: binning numerics, rows as bag-of-words sentences, similarity/anomaly/analogy in standard SQL (IBM Technology)

## Related Topics

- [[../model-evaluation/index|Model Evaluation]] — measuring the behaviour these training objectives produce
- [[../claude-code-practice/index|Claude Code Practice]] — model/effort selection decisions grounded in how weights and inference work
- [[../knowledge-engineering/index|Knowledge Engineering]] — embeddings and semantic retrieval applied to knowledge bases rather than SQL tables
