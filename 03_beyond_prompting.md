# Why Prompt Engineering Alone Doesn't Fix Hallucination

## The tempting solution

When people first encounter hallucination, the natural reaction is: "Just tell the model to be accurate."

```
System prompt: "Only state facts you are certain about. 
If you don't know something, say so. Never make up citations."
```

This helps a little. But it doesn't solve the problem. Here's why.

## The fundamental issue

An LLM doesn't have a "certainty meter." When it generates text, it doesn't distinguish between "I know this from reliable training data" and "I'm pattern-matching and this sounds right." The instruction to "only state facts you are certain about" assumes the model can tell the difference. It can't.

Adding "if you don't know, say so" helps the model decline more often, but it also makes it decline when it *does* know. And it still hallucinates confidently in many cases — the prompt didn't change the underlying generation mechanism.

## What prompting CAN do

Prompting is useful for:

- **Reducing the frequency** of hallucination (models do hallucinate less with careful instructions)
- **Formatting output** (asking for structured JSON, requesting citations)
- **Setting expectations** (telling the model to hedge uncertain claims)

But prompting alone gives you no way to **verify** whether the output is correct. You're still trusting the model's word for it.

## The engineering approach

Instead of asking the model to be trustworthy, we **build systems that verify** its output. This is the same principle used everywhere in software engineering: don't trust input, validate it.

### Layer 1: Give it real sources (RAG)
Instead of letting the model answer from memory, you search a database of real documents and include the relevant passages in the prompt. Now the model has actual sources to reference — and you know what those sources say.

### Layer 2: Force structure (Pydantic)
Instead of free-form text, require the model to output a structured format: claim, evidence, citation, confidence score. If it can't fill in the citation field, you know something is unsupported.

### Layer 3: Check automatically (Verification)
After the model responds, run Python code that validates the answer: Do the cited sources exist? Do the numbers match known values? Is the claim actually supported by the evidence?

### Layer 4: Measure (Scoring)
Track how often hallucination occurs across your pipeline. Compare different configurations. Know whether your mitigations are actually working.

## The mindset shift

| Prompting Approach | Engineering Approach |
|---|---|
| "Please be accurate" | Check if the output is accurate |
| "Don't make up citations" | Verify every citation exists |
| "Only use reliable sources" | Provide the sources and cross-reference |
| "Tell me your confidence" | Measure confidence from external signals |
| Hope it works | Prove it works with data |

The talk demonstrates how to build each of these layers in Python, using libraries you probably already know.

---

## References

1. Tonmoy, S.M., et al. (2024). "A Comprehensive Survey of Hallucination Mitigation Techniques in Large Language Models." *arXiv:2401.01313*. https://arxiv.org/abs/2401.01313
2. Shuster, K., et al. (2021). "Retrieval Augmentation Reduces Hallucination in Conversation." *Findings of EMNLP 2021*. https://arxiv.org/abs/2104.07567
3. Lewis, P., et al. (2020). "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks." *NeurIPS 2020*. https://arxiv.org/abs/2005.11401
4. Pydantic Documentation — Data validation using Python type annotations. https://docs.pydantic.dev/

---

*Next: [RAG Explained Simply →](04_rag_explained.md)*
