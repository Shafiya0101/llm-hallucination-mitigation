# The Five Types of Scientific Hallucination

Not all hallucinations are the same. Understanding the different types helps you build targeted detection for each one.

## 1. Fabricated Citations

**What it looks like:** The model invents a paper that doesn't exist.

> "According to Zhang et al. (2022) in *Nature Materials*, graphene nanoribbons exhibit..."

The author names are plausible. The journal is real. The year makes sense. But the paper doesn't exist. This is the most dangerous type because it's the hardest to spot without actually searching for the reference.

**How to catch it:** Cross-reference every citation against a real database of papers.

## 2. Numerical Errors

**What it looks like:** The model gives a wrong number — wrong magnitude, wrong units, or wrong value entirely.

> "The thermal conductivity of graphene is approximately 500 W/mK."

The real value is ~5000 W/mK. Off by 10x. The sentence structure is perfect, the units are correct, and the value is in a "plausible" range — just wrong.

**How to catch it:** Compare numerical values against known reference databases and check for order-of-magnitude errors.

## 3. Unsupported Claims

**What it looks like:** The model makes a claim that goes beyond what any source actually says.

> "This result definitively resolves the long-standing debate in the field."

The source might say "these results suggest..." but the model escalates tentative findings into definitive conclusions. This is subtle — the claim isn't factually wrong, it's just not supported by the evidence.

**How to catch it:** Compare the strength of claims against the source material. Look for hedging language in sources vs. absolute language in outputs.

## 4. Entity Confusion

**What it looks like:** The model swaps a material, person, or concept for a similar one.

> "WS2 has a direct band gap of 1.8 eV in its monolayer form."

This is actually true of MoS2, not WS2 (which has a band gap of ~2.0 eV). The model confuses related materials that appear in similar contexts in its training data.

**How to catch it:** Verify that the specific entity mentioned is the one the source actually discusses.

## 5. Temporal Errors

**What it looks like:** The model gets dates wrong.

> "Graphene was first isolated in 1994 by Geim and Novoselov."

The real year is 2004. The model remembers the researchers and the discovery but assigns the wrong year — possibly mixing it up with another event from its training data.

**How to catch it:** Cross-validate dates against source documents.

---

## The key pattern

Notice that each type requires a different detection strategy. There's no single prompt or technique that catches all five. This is why we build a **multi-layer verification pipeline** — each layer catches a different type of error.

| Type | Detection Strategy | Python Tool |
|------|-------------------|-------------|
| Fabricated Citations | Cross-reference against corpus | Text similarity + lookup |
| Numerical Errors | Compare against reference values | NumPy + reference database |
| Unsupported Claims | Compare claim strength vs. source | Text comparison |
| Entity Confusion | Verify entity-fact alignment | Named entity matching |
| Temporal Errors | Validate dates against sources | Regex + cross-validation |

---

## References

1. Ji, Z., et al. (2023). "Survey of Hallucination in Natural Language Generation." *ACM Computing Surveys*, 55(12). https://doi.org/10.1145/3571730
2. Huang, L., et al. (2023). "A Survey on Hallucination in Large Language Models." *arXiv:2311.05232*. https://arxiv.org/abs/2311.05232
3. Zhang, Y., et al. (2023). "Siren's Song in the AI Ocean: A Survey on Hallucination in Large Language Models." *arXiv:2309.01219*. https://arxiv.org/abs/2309.01219
4. Rawte, V., Sheth, A., & Das, A. (2023). "A Survey of Hallucination in Large Foundation Models." *arXiv:2309.05922*. https://arxiv.org/abs/2309.05922

---

*Next: [Why Prompt Engineering Alone Doesn't Fix It →](03_beyond_prompting.md)*
