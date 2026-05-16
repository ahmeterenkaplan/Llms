# 🧠 Brain Tumor Detection via Large Language Models: A Comparative Benchmark Study

> **A SCI-level benchmarking study evaluating the zero-shot diagnostic capability of state-of-the-art large language models (LLMs) on brain MRI classification.**

---

## 📋 Overview

This repository accompanies the paper **"Can Large Language Models Detect Brain Tumors? A Zero-Shot Benchmark on MRI Images"** (under review). We systematically evaluate eight frontier LLMs against a standard brain MRI dataset, comparing their diagnostic performance using classical classification metrics (Accuracy, Precision, Recall, Specificity, F1 Score).

The study provides the first large-scale, model-to-model benchmark of LLM vision capabilities in a real-world medical imaging context — without any fine-tuning or few-shot prompting.

---

## 🗂️ Dataset

**Brain MRI Images for Brain Tumor Detection**  
Source: [Kaggle — Navoneel Chakrabarty](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection)

| Split | Tumor (Yes) | No Tumor (No) | Total |
|-------|-------------|---------------|-------|
| Train | 124 | 78 | 202 |
| Test  | 31  | 20 | 51  |
| **Total** | **155** | **98** | **253** |

Each image is a T1-weighted contrast-enhanced MRI scan. The task is binary classification: **Tumor Present (Yes)** vs. **No Tumor (No)**.

---

## 🤖 Models Evaluated

| Model | Provider | Category |
|-------|----------|----------|
| **ChatGPT 5.4 Thinking** | OpenAI | Reasoning |
| **ChatGPT 5.5 Thinking** | OpenAI | Reasoning |
| **ChatGPT o3** | OpenAI | Reasoning |
| **Gemini 3.1 Pro** | Google DeepMind | Standard |
| **Gemini 3.1 Thinking** | Google DeepMind | Reasoning |
| **Claude Sonnet 4.6** | Anthropic | Standard |
| **Claude Sonnet 4.5** | Anthropic | Standard |
| **Claude Haiku 4.5** | Anthropic | Lightweight |

All models were prompted in a **zero-shot** setting with identical instructions. No model was fine-tuned or given in-context examples.

---

## 📊 Results

### Overall Performance (N = 253)

| Model | Accuracy | Precision | Recall | Specificity | F1 Score |
|-------|----------|-----------|--------|-------------|----------|
| **ChatGPT 5.4 Thinking** | **0.9486** | 0.9277 | **0.9935** | 0.8776 | **0.9595** |
| ChatGPT 5.5 Thinking | 0.9328 | **0.9312** | 0.9613 | **0.8878** | 0.9460 |
| Gemini 3.1 Thinking | 0.9209 | 0.9042 | 0.9742 | 0.8367 | 0.9379 |
| Gemini 3.1 Pro | 0.9051 | 0.8876 | 0.9677 | 0.8061 | 0.9259 |
| ChatGPT o3 | 0.8775 | 0.8444 | 0.9806 | 0.7143 | 0.9075 |
| Claude Sonnet 4.5 | 0.8617 | 0.9110 | 0.8581 | 0.8673 | 0.8837 |
| Claude Sonnet 4.6 | 0.8458 | 0.8222 | 0.9548 | 0.6735 | 0.8836 |
| Claude Haiku 4.5 | 0.7905 | 0.8036 | 0.8710 | 0.6633 | 0.8359 |

### Test Set Performance (N = 51)

| Model | Accuracy | Precision | Recall | Specificity | F1 Score |
|-------|----------|-----------|--------|-------------|----------|
| **ChatGPT 5.4 Thinking** | **1.0000** | **1.0000** | **1.0000** | **1.0000** | **1.0000** |
| ChatGPT 5.5 Thinking | 0.9804 | 1.0000 | 0.9677 | 1.0000 | 0.9836 |
| Gemini 3.1 Thinking | 0.9412 | 0.9375 | 0.9677 | 0.9000 | 0.9524 |
| ChatGPT o3 | 0.9020 | 0.8824 | 0.9677 | 0.8000 | 0.9231 |
| Gemini 3.1 Pro | 0.9020 | 0.9062 | 0.9355 | 0.8500 | 0.9206 |
| Claude Sonnet 4.6 | 0.8824 | 0.8788 | 0.9355 | 0.8000 | 0.9062 |
| Claude Sonnet 4.5 | 0.8627 | 0.9615 | 0.8065 | 0.9500 | 0.8772 |
| Claude Haiku 4.5 | 0.7647 | 0.7879 | 0.8387 | 0.6500 | 0.8125 |

> ⚠️ **Note:** ChatGPT 5.4 Thinking achieved perfect classification on the test set (Accuracy = 1.00, F1 = 1.00). This is an empirical finding and may reflect characteristics of this specific test partition.

### Key Findings

- **ChatGPT 5.4 Thinking** achieves the highest overall accuracy (94.86%) and F1 score (0.9595), with a near-perfect recall of 99.35% — critically important in a medical screening context.
- **OpenAI reasoning models** (5.4 Thinking, 5.5 Thinking, o3) consistently outperform both Google and Anthropic models across all metrics.
- **Gemini 3.1 Thinking** ranks third overall (Accuracy: 92.09%, F1: 0.9379), demonstrating that reasoning-mode variants consistently outperform their standard counterparts.
- **Claude Sonnet 4.6** exhibits high recall (0.9548) but lower specificity (0.6735), indicating a tendency toward false positives.
- **Claude Haiku 4.5** shows the weakest performance overall, expected given its lightweight architecture.
- **Reasoning-mode models** (Thinking variants) uniformly outperform standard models from the same family.

---

## 🧪 Methodology

### Prompt Design

Each model was presented with a brain MRI image and asked to classify it as tumor-positive or tumor-negative. The prompt was identical across all models:

```
You are a medical image analysis assistant. Examine the provided brain MRI scan and determine whether a brain tumor is present.

Respond with ONLY one word: "Yes" if a tumor is present, or "No" if no tumor is detected.
```

### Evaluation Protocol

- Images were presented one at a time with no additional clinical context.
- Responses were parsed as binary (Yes/No) and compared against ground truth labels.
- Metrics computed: TP, TN, FP, FN → Accuracy, Precision, Recall (Sensitivity), Specificity, F1 Score.
- Evaluation was performed separately for Train, Test, and All splits.

---

## 📁 Repository Structure

```
.
├── data/
│   ├── noList_addedType.xlsx       # Per-image model responses — No Tumor class
│   ├── yesList_addedType.xlsx      # Per-image model responses — Tumor class
│   └── metrics_report.xlsx        # Aggregated performance metrics (All/Train/Test)
├── results/
│   └── figures/                   # Performance charts and confusion matrices
├── notebooks/
│   └── analysis.ipynb             # Metric computation and visualization
└── README.md
```

---

## 📈 Confusion Matrix Summary (Overall)

|  | **Predicted: Yes** | **Predicted: No** |
|--|----|----|
| **Actual: Yes** | TP | FN |
| **Actual: No** | FP | TN |

| Model | TP | TN | FP | FN |
|-------|----|----|----|----|
| ChatGPT 5.4 Thinking | 154 | 86 | 12 | 1 |
| ChatGPT 5.5 Thinking | 149 | 87 | 11 | 6 |
| Gemini 3.1 Thinking | 151 | 82 | 16 | 4 |
| Gemini 3.1 Pro | 150 | 79 | 19 | 5 |
| ChatGPT o3 | 152 | 70 | 28 | 3 |
| Claude Sonnet 4.5 | 133 | 85 | 13 | 22 |
| Claude Sonnet 4.6 | 148 | 66 | 32 | 7 |
| Claude Haiku 4.5 | 135 | 65 | 33 | 20 |

---

## ⚙️ Reproducibility

All model queries were performed via official APIs or chat interfaces during [insert date range]. Results may vary slightly across sessions due to model non-determinism. To maximize reproducibility:

- Temperature was set to 0 (or lowest available) where configurable.
- Each image was evaluated independently in a fresh session.
- Raw responses are archived in the `data/` directory.

---

## 📖 Citation

If you use this work, please cite:

```bibtex
@article{yourname2025llmbrain,
  title     = {Can Large Language Models Detect Brain Tumors? A Zero-Shot Benchmark on MRI Images},
  author    = {[Author Names]},
  journal   = {[Journal Name]},
  year      = {2025},
  note      = {Under Review}
}
```

---

## 📜 License

This project is licensed under the MIT License. The dataset is subject to its original [Kaggle license](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection).

---

## 🙏 Acknowledgements

- Dataset provided by [Navoneel Chakrabarty](https://www.kaggle.com/navoneel) via Kaggle.
- Model APIs: OpenAI, Google DeepMind, Anthropic.

---

*For questions or collaborations, please open an issue or contact the corresponding author.*
