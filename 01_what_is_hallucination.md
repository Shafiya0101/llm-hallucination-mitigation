# What is LLM Hallucination?

## The short version

When you ask ChatGPT, Claude, or any Large Language Model a question, it sometimes makes up an answer that *sounds* perfectly correct but is completely fabricated. This is called **hallucination**.

It's not a bug in the traditional sense. The model isn't crashing or returning an error. It's confidently generating text that looks right, reads right, and feels right — but is wrong.

## Real examples that actually happened

**Fabricated legal citations.** In 2023, a New York lawyer used ChatGPT to prepare a court brief. The model generated six case citations that looked completely legitimate — real-sounding case names, plausible court references, reasonable legal arguments. None of the cases existed. The lawyer was sanctioned by the judge.

**Invented research papers.** Researchers have found that when asked for references on a topic, LLMs frequently generate fake papers with plausible titles, realistic author names (sometimes mixing real authors from the field), and made-up DOIs. These "phantom references" have started appearing in actual published research.

**Wrong numbers with high confidence.** Ask an LLM about the thermal conductivity of graphene and it might say "approximately 500 W/mK" instead of the correct ~5000 W/mK. Off by a factor of 10, but stated with the same confidence as a correct answer.

## Why does this happen?

LLMs don't "know" facts the way a database does. They've learned statistical patterns from text. When you ask a question, the model generates the most *probable* next words based on what it's seen during training. Most of the time, the most probable words happen to be correct. But sometimes the most probable-sounding answer is wrong.

The model has no internal mechanism to check: "Wait, does this paper actually exist?" or "Is this number right?" It just generates text that *fits the pattern* of a correct answer.

## Why does this matter for science?

In casual conversation, a hallucinated fact is annoying. In science, it's dangerous:

- A fabricated citation in a paper can mislead other researchers
- A wrong numerical value can propagate through calculations
- An unsupported claim can influence experimental design
- Once published, errors are hard to retract

The core problem: **LLMs are convincing writers, not reliable sources.** If you use them in your research workflow, you need a way to verify their output automatically.

## What can we do about it?

This is what the talk is about. Instead of hoping the model gets it right, we build Python tools that **check** its output:

1. **Ground the answers** — Make the LLM answer from real documents, not from memory
2. **Force structure** — Require citations, confidence scores, and evidence for every claim
3. **Verify automatically** — Check numbers, validate citations, flag unsupported claims
4. **Measure everything** — Quantify how often hallucination happens and whether your fixes work

The key insight: **hallucination is a verification problem, not a prompting problem.** You can't prompt your way out of it, but you can engineer your way around it.

---

## References

1. Azamfirei, R., Kudchadkar, S.R., & Fackler, J. (2023). "Large language models and the perils of their hallucinations." *Critical Care*, 27(1), 120. https://doi.org/10.1186/s13054-023-04393-x
2. Alkaissi, H., & McFarlane, S.I. (2023). "Artificial Hallucinations in ChatGPT: Implications in Scientific Writing." *Cureus*, 15(2). https://doi.org/10.7759/cureus.35179
3. Huang, L., et al. (2023). "A Survey on Hallucination in Large Language Models." *arXiv:2311.05232*. https://arxiv.org/abs/2311.05232

---

*Next: [The Five Types of Scientific Hallucination →](02_hallucination_types.md)*
