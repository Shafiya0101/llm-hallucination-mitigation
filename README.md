# Mitigating Hallucination in Scientific LLM Systems

### A Python Engineering Framework

---

## What is this about?

Large Language Models (ChatGPT, Claude, etc.) sometimes make things up — they invent fake research papers, give wrong numbers, or make claims that no source supports. In science, that's dangerous. A researcher who trusts a fabricated citation might put it in a real paper.

This project presents a Python approach to **mitigating these errors systematically**, using tools you already know: Pydantic, NumPy, FAISS, and SciPy.

## Who is this for?

- Python developers who use LLMs in their workflows
- Data scientists and analysts who need to trust their outputs
- Anyone curious about how to verify what an AI tells you

No ML expertise required. If you can write a Python function, you can follow along.

---

## Reading Materials

### Articles — What is hallucination and why does it matter?

📖 [What is LLM Hallucination?](articles/01_what_is_hallucination.md) — A plain-language explanation with real examples of things that went wrong

📖 [The Five Types of Scientific Hallucination](articles/02_hallucination_types.md) — A taxonomy: fabricated citations, wrong numbers, unsupported claims, entity confusion, temporal errors

📖 [Why Prompt Engineering Alone Doesn't Fix It](articles/03_beyond_prompting.md) — Why "be accurate" in your system prompt isn't enough, and what engineering approaches work better

📖 [RAG Explained Simply](articles/04_rag_explained.md) — What Retrieval-Augmented Generation is, how vector search works, and why it helps ground LLM answers in real sources

📖 [Measuring Truth: How to Score LLM Outputs](articles/05_measuring_truth.md) — How to quantify whether your mitigations are working, without needing a PhD in statistics

### External Resources

- 📺 [Retrieval Augmented Generation (IBM)](https://www.ibm.com/think/topics/retrieval-augmented-generation) — Clear overview of RAG
- 📺 [Pydantic Documentation](https://docs.pydantic.dev/) — The library we use for structured output validation
- 📺 [FAISS Wiki](https://github.com/facebookresearch/faiss/wiki) — Facebook's vector search library
- 📄 [Survey: Hallucination in LLMs](https://arxiv.org/abs/2311.05232) — Academic survey for those who want deeper reading

---

## The Approach

The framework treats hallucination as an engineering problem, not a prompting problem. It has four layers:

**1. Retrieval Grounding** — Search real scientific documents with FAISS and sentence-transformers so the LLM answers from actual sources, not from memory.

**2. Structured Generation** — Force the LLM to return validated JSON with Pydantic: every claim needs evidence, a citation, and a confidence score.

**3. Programmatic Verification** — Check numbers against reference values, compare generated text to source documents, run a rule engine to catch missing evidence.

**4. Quantitative Evaluation** — Measure faithfulness, citation accuracy, and numerical accuracy for every output. Use statistical testing to confirm improvements are real.

---

## Contact

**Author:** Shafiya Kausar 
**Email:** shafiya.kausar@aivancity.education

---

> This repository contains reading materials and background articles. Code and implementation details are available separately.
