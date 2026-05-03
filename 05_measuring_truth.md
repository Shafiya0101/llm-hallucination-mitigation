# Measuring Truth: How to Score LLM Outputs

## Why measuring matters

"The model seems better now" isn't good enough. You need numbers. Without measurement, you can't tell if your improvements are real or if you just got lucky with your test examples.

## Four things to measure

### 1. Faithfulness — Does the answer match the source?

The simplest check: compare the words in the generated answer against the words in the source document. If the answer introduces lots of new words not in the source, it might be adding unsupported information.

```python
def faithfulness(generated, source):
    """What fraction of content overlaps between generated and source text?"""
    gen_words = set(generated.lower().split())
    src_words = set(source.lower().split())
    overlap = len(gen_words & src_words)
    total = len(gen_words | src_words)
    return overlap / total  # 1.0 = perfect match, 0.0 = completely different
```

This is simple but effective. In our experiment, this single metric separated hallucinated from clean text with high statistical significance.

### 2. Citation accuracy — Are the references real?

If the model claims to reference a source, does that source actually exist in your corpus? This is a yes/no check per citation.

```python
def citation_accuracy(claimed_sources, real_corpus):
    """What fraction of cited sources actually exist?"""
    verified = sum(1 for s in claimed_sources if s in real_corpus)
    return verified / len(claimed_sources) if claimed_sources else 0
```

### 3. Numerical accuracy — Are the numbers right?

Extract all numbers from the generated text and compare them against the numbers in the source text. If "5000 W/mK" in the source becomes "-5000 W/mK" in the output, that's caught.

```python
import re

def numerical_accuracy(generated, source):
    """What fraction of numbers in the output match the source?"""
    gen_nums = [float(n) for n in re.findall(r'-?\d+\.?\d*', generated)]
    src_nums = [float(n) for n in re.findall(r'-?\d+\.?\d*', source)]
    
    if not gen_nums:
        return None  # no numbers to check
    
    matched = 0
    for gn in gen_nums:
        for sn in src_nums:
            if sn != 0 and abs(gn - sn) / abs(sn) < 0.05:  # within 5%
                matched += 1
                break
    return matched / len(gen_nums)
```

### 4. Completeness — Did it cover everything important?

Does the answer address the key points from the source, or did it cherry-pick easy parts and skip the rest?

## Combining the scores

Each metric catches different problems. You combine them with weights:

```python
overall = (0.35 * faithfulness 
         + 0.25 * citation_accuracy 
         + 0.25 * numerical_accuracy 
         + 0.15 * completeness)
```

The weights reflect priorities: faithfulness matters most, completeness least (an incomplete-but-correct answer is better than a complete-but-wrong one).

## How to know if the difference is real

If pipeline A scores 0.73 and pipeline B scores 0.98, is that a real improvement or random noise? This is where basic statistics help:

- **Run multiple samples** — Don't test on one example. Use 20, 50, or more.
- **Compute confidence intervals** — A score of "0.73 ± 0.17" tells you more than just "0.73"
- **Check for overlap** — If the confidence ranges of two pipelines don't overlap, the difference is real

In our experiment with 20 samples, the hallucinated group scored 0.73 ± 0.17 and the clean group scored 0.98 ± 0.05. The ranges don't overlap — the difference is real, not luck.

## The takeaway

Measuring hallucination isn't rocket science. Word overlap, number matching, and citation lookup are enough to build a useful scoring system. The hard part isn't the math — it's the discipline to actually measure instead of eyeballing.

---

## References

1. Min, S., et al. (2023). "FActScore: Fine-grained Atomic Evaluation of Factual Precision in Long Form Text Generation." *EMNLP 2023*. https://arxiv.org/abs/2305.14251
2. Es, S., et al. (2024). "RAGAS: Automated Evaluation of Retrieval Augmented Generation." *arXiv:2309.15217*. https://arxiv.org/abs/2309.15217
3. Manakul, P., Liusie, A., & Gales, M.J.F. (2023). "SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models." *EMNLP 2023*. https://arxiv.org/abs/2303.08896

---

*Back to [main page →](../README.md)*
