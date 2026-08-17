# The Soft-Label Bottleneck

**Evidence from Constrained LLM Distillation for Clinical De-identification in Brazilian Portuguese**

Code, prompts, soft-label caches, and reproducibility artifacts accompanying the paper.

> Distilling a large language model into a compact token-classification encoder requires a probability distribution over the label set for every input token. However, LLMs are typically evaluated for clinical de-identification through generative rewriting with entity tags, an interface that does not naturally expose such token-level distributions. This repository quantifies the performance cost of imposing that constraint while holding model scale fixed.

## Notebooks

### `anonymed_kd_pipeline.ipynb` — Main experimental pipeline

Runs the complete experimental pipeline end to end and writes all reported results to a single `results.json` file.

The notebook performs, in order:

- preprocessing of the AnonyMED-BR synthetic partition into BIO-labeled token windows;
- corpus statistics;
- evaluation of the rule-based baseline;
- constrained decoding of Qwen2.5-7B-Instruct to obtain per-token soft-label distributions;
- knowledge distillation into BERTimbau-base;
- paired hyperparameter search;
- multi-seed training and evaluation;
- INT8 quantization and CPU latency benchmarking;
- entity-level evaluation;
- paired bootstrap and permutation significance tests; and
- generation of the figures reported in the paper.

### `anonymed_phase4.ipynb` — Interface vs. scale and ten-seed evaluation

Contains two complementary experiments.

The first evaluates the **same Qwen2.5-7B-Instruct model** using a generative de-identification interface — rewriting text with entity tags, following the setup of Schiezaro et al. — on the same test split and under the same evaluation metric.

Because model scale, data, and evaluation are held fixed, this experiment isolates the performance cost associated with the **decoding interface** rather than differences in model capacity.

The second experiment extends the baseline-versus-distilled comparison from five to ten random seeds and recomputes the paired *t*-test, confidence interval, and observed statistical power.

## Data

This work uses the **synthetic partition of AnonyMED-BR**:

<https://huggingface.co/datasets/Venturus/AnonyMED-BR>

The real-data partition of AnonyMED-BR is not publicly available. We did not have access to it and did not use it in any experiment.

**All results reported in this repository and in the associated experiments therefore refer exclusively to synthetic clinical text.**
