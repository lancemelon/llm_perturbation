# llm_perturbation

## Introduction
Recent work on robustness testing and adversarial attacks for large language models (LLMs) has relied heavily on heuristic perturbations such as random noise injection or token swapping. While effective for stress-testing models, these approaches lack a systematic framework for controlling the nature of perturbations. In this work, I propose a wavelet-based method for perturbing LLM embedding spaces. By treating embeddings as discrete signals, I apply the discrete wavelet transform (DWT) to decompose them into low- and high-frequency components. Perturbations can then be systematically introduced by modifying specific frequency bands—for example, zeroing out low- or high-frequency coefficients.

## Advantages / Preliminary Results
This framework provides two advantages: (1) precise control over the scale and structure of perturbations across different frequency bands, and (2) a pathway toward interpretable robustness testing, as frequency-based manipulations may correspond to semantic versus stylistic information in embeddings. Preliminary experiments show that perturbing low-frequency components reduces model confidence, while perturbing high-frequency components increases it, with relatively low KL-divergence between perturbed and original logit distributions. These findings suggest that wavelet perturbations preserve global semantic structure while systematically altering local embedding characteristics.

## Future Work / Final Remarks
Wavelet perturbations offer a principled tool for probing the robustness and interpretability of LLMs. Future work will explore their role in adversarial attack generation, sensitivity benchmarking, and developing more resilient NLP systems.

## Workflow
### Embedding + Wavelet Perturbation

  - Embed each sentence.
  
  - Apply DWT → decompose into low- and high-frequency components.
  
  - Perturb embeddings: zero out low-frequency or high-frequency coefficients (and optionally other methods).
  
  - Reconstruct embeddings (inverse wavelet transform).

### Next-token Prediction (Autoregressive)

  - Feed perturbed embeddings into the model.
  
  - For each sentence, generate top-k logits for the next n tokens (e.g., next 5 tokens).

### Metrics per sentence
  
  - For each perturbation and each sentence, calculate:
  
  - Mean confidence of top-k tokens.
  
  - KL divergence between perturbed vs. original top-k distributions.
  
  - Top-k flips (how many predicted tokens changed rank).

### Aggregate metrics across dataset

  - Average each metric over all sentences.
  
  - Optional: compute variance to show consistency of effect.

### Visualization & Analysis

  - Bar chart or heatmap of mean confidence shifts for low vs high-frequency perturbations.
  
  - Top-k flips as a separate plot.

KL divergence plotted per perturbation type.

Highlight patterns: e.g., low-frequency removal decreases confidence, high-frequency increases it slightly, etc.
