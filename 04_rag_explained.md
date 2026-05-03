# RAG Explained Simply

## What is RAG?

RAG stands for **Retrieval-Augmented Generation**. The name is intimidating, but the idea is simple:

**Before asking the LLM a question, search for relevant documents first. Then include those documents in the prompt so the model answers based on real sources instead of memory.**

That's it. It's a search step before a generation step.

## A concrete example

Without RAG:
```
You: "What is the thermal conductivity of graphene?"
LLM: (answers from memory, might be wrong)
```

With RAG:
```
Step 1: Search your document database for "thermal conductivity graphene"
Step 2: Find a paragraph that says "...approximately 5000 W/mK..."
Step 3: Send to the LLM: "Based on this source: [paragraph], answer: What is the thermal conductivity of graphene?"
LLM: (answers based on the actual document)
```

The model can still hallucinate, but now it's much less likely because the answer is right there in the context. And if it does get it wrong, you can check — because you know what the source said.

## How the search works: Vector search

Traditional search matches keywords. You search for "graphene thermal" and get documents containing those words.

Vector search is smarter. It converts text into numbers (called "embeddings") that capture *meaning*, not just words. So searching for "heat conduction in graphene" would also find documents about "thermal conductivity of graphene" even though the words are different.

In Python, this looks like:

```python
from sentence_transformers import SentenceTransformer
import faiss

# Convert your documents into vectors
model = SentenceTransformer('all-MiniLM-L6-v2')
doc_vectors = model.encode(["Document 1 text...", "Document 2 text..."])

# Build a search index
index = faiss.IndexFlatIP(384)  # 384 = vector dimension
index.add(doc_vectors)

# Search for relevant documents
query_vector = model.encode(["thermal conductivity of graphene"])
scores, indices = index.search(query_vector, k=3)  # top 3 results
```

That's a working RAG search in about 10 lines of Python.

## Why RAG helps with hallucination

1. **The model has real sources** — It's less likely to invent information when the correct answer is in its context
2. **You can verify** — You know exactly what sources were provided, so you can check if the model's answer matches
3. **It's grounded** — The model can only reference documents you gave it, not fabricated ones
4. **It's updatable** — Add new documents to your index and the model has access to them immediately, no retraining needed

## What RAG doesn't solve

RAG reduces hallucination but doesn't eliminate it. The model can still:
- Misinterpret the source document
- Mix information from multiple sources incorrectly
- Add claims not present in any source
- Get numbers wrong even when the source is right there

This is why RAG is *layer 1* of our pipeline, not the entire solution. You still need structured output, verification, and scoring on top of it.

---

## References

1. Lewis, P., et al. (2020). "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks." *NeurIPS 2020*. https://arxiv.org/abs/2005.11401
2. Gao, Y., et al. (2024). "Retrieval-Augmented Generation for Large Language Models: A Survey." *arXiv:2312.10997*. https://arxiv.org/abs/2312.10997
3. Johnson, J., Douze, M., & Jégou, H. (2019). "Billion-scale similarity search with GPUs." *IEEE Transactions on Big Data*. https://github.com/facebookresearch/faiss
4. Reimers, N., & Gurevych, I. (2019). "Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks." *EMNLP 2019*. https://arxiv.org/abs/1908.10084
5. IBM Research. "What is Retrieval-Augmented Generation?" https://www.ibm.com/think/topics/retrieval-augmented-generation

---

*Next: [Measuring Truth: How to Score LLM Outputs →](05_measuring_truth.md)*
