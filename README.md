# 🧠 Brain Tumor Detection via LLMs vs. CNN: A Comparative Study

[![Dataset](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?logo=kaggle)](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)](https://python.org)

> **Can large language models read brain MRI scans as well as a trained CNN?**  
> This study benchmarks 8 state-of-the-art multimodal LLMs against a convolutional neural network on binary brain tumor classification from MRI images, contributing to the growing field of AI-assisted medical imaging.

---

## 📌 Abstract

The rapid advancement of multimodal large language models (LLMs) has opened new possibilities in medical image interpretation. This paper presents a systematic evaluation of eight frontier LLMs on a binary brain tumor detection task using the publicly available Brain MRI Images for Brain Tumor Detection dataset. We compare their performance against a custom-trained convolutional neural network (CNN) using standard classification metrics: accuracy, sensitivity (recall), specificity, precision, and F1 score. Our findings provide empirical insights into the current capabilities and limitations of LLMs in clinical imaging contexts.

---

## 📂 Dataset

**Source:** [Brain MRI Images for Brain Tumor Detection – Kaggle](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection)

| Split | Count |
|---|---|
| Tumor-positive (Yes) | 155 images |
| Tumor-negative (No) | 98 images |
| **Total** | **253 images** |

Each image is a brain MRI scan. The task is **binary classification**: tumor present (`Yes`) or absent (`No`). Each LLM was prompted with the raw MRI image and asked to determine whether a tumor is visible.

---

## 🤖 Models Evaluated

| # | Model | Provider | Type |
|---|---|---|---|
| 1 | **ChatGPT 5.4 Thinking** | OpenAI | Reasoning-augmented |
| 2 | **ChatGPT 5.5 Thinking** | OpenAI | Reasoning-augmented |
| 3 | **ChatGPT o3** | OpenAI | Standard multimodal |
| 4 | **Gemini 3.1 Thinking** | Google DeepMind | Reasoning-augmented |
| 5 | **Gemini 3.1 Pro** | Google DeepMind | Standard multimodal |
| 6 | **Claude Sonnet 4.6** | Anthropic | Standard multimodal |
| 7 | **Claude Sonnet 4.5** | Anthropic | Standard multimodal |
| 8 | **Claude Haiku 4.5** | Anthropic | Lightweight multimodal |
| 9 | **CNN (Baseline)** | Custom | Trained classifier |

---

## 📊 Results

### Classification Metrics per Model

| Model | Accuracy | Sensitivity | Specificity | Precision | F1 Score | TP | TN | FP | FN |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ChatGPT 5.4 Thinking** | **0.949** | **0.994** | 0.878 | 0.928 | **0.960** | 154 | 86 | 12 | 1 |
| ChatGPT 5.5 Thinking | 0.933 | 0.961 | 0.888 | 0.931 | 0.946 | 149 | 87 | 11 | 6 |
| Gemini 3.1 Thinking | 0.921 | 0.974 | 0.837 | 0.904 | 0.938 | 151 | 82 | 16 | 4 |
| Gemini 3.1 Pro | 0.905 | 0.968 | 0.806 | 0.888 | 0.926 | 150 | 79 | 19 | 5 |
| Claude Sonnet 4.5 | 0.862 | 0.858 | **0.867** | **0.911** | 0.884 | 133 | 85 | 13 | 22 |
| Claude Sonnet 4.6 | 0.846 | 0.955 | 0.673 | 0.822 | 0.884 | 148 | 66 | 32 | 7 |
| ChatGPT o3 | 0.877 | 0.981 | 0.714 | 0.844 | 0.907 | 152 | 70 | 28 | 3 |
| Claude Haiku 4.5 | 0.791 | 0.871 | 0.663 | 0.804 | 0.836 | 135 | 65 | 33 | 20 |

> **TP** = True Positive, **TN** = True Negative, **FP** = False Positive, **FN** = False Negative  
> **Sensitivity** = recall for tumor-positive class (clinically critical — missing a tumor is dangerous)  
> **Specificity** = recall for tumor-negative class

### Key Observations

- 🥇 **ChatGPT 5.4 Thinking** achieves the best overall performance with **94.9% accuracy** and an exceptional **sensitivity of 99.4%** (only 1 missed tumor out of 155).
- 🧠 **Reasoning-augmented models** (ChatGPT 5.4/5.5 Thinking, Gemini 3.1 Thinking) consistently outperform their standard counterparts, suggesting that chain-of-thought reasoning aids visual medical analysis.
- ⚠️ **Claude Haiku 4.5** is the weakest performer across all metrics, consistent with its lightweight architecture.
- 📉 **False Positive rates** are notably high in Claude Sonnet 4.6, Claude Haiku 4.5, and ChatGPT o3, which may cause unnecessary follow-up diagnostics in clinical settings.
- 🎯 **Claude Sonnet 4.5** achieves the highest **specificity (86.7%)** and **precision (91.1%)** among Claude models, indicating a more conservative, high-confidence detection strategy.
- 🏥 In a medical context, **sensitivity is paramount** — failing to detect a tumor (false negative) carries far greater clinical risk than a false positive. On this criterion, ChatGPT 5.4 Thinking is the standout model.

---

## 🏗️ Repository Structure

```
├── data/
│   ├── noListFinal.xlsx        # LLM outputs for tumor-negative images
│   └── yesListFinal.xlsx       # LLM outputs for tumor-positive images
├── cnn/
│   ├── model.py                # CNN architecture definition
│   ├── train.py                # Training script
│   └── evaluate.py             # Evaluation script
├── llm_evaluation/
│   ├── prompt_template.txt     # Prompt used for all LLMs
│   └── results_analysis.py     # Metric computation from Excel files
├── figures/
│   └── ...                     # Confusion matrices, comparison charts
├── README.md
└── requirements.txt
```

---

## ⚙️ Methodology

### LLM Evaluation Protocol

Each MRI image was submitted to all 8 multimodal LLMs using an identical, standardized prompt:

> *"This is a brain MRI scan. Based on the image, is a brain tumor present? Answer with only 'Yes' or 'No'."*

Responses were recorded in binary format (`Yes` / `No`) and compared against ground-truth labels from the dataset. No fine-tuning or few-shot examples were used — all evaluations are **zero-shot**.

### CNN Baseline

A convolutional neural network was trained on the same dataset using an 80/20 train-test split. The architecture follows standard practices for medical image binary classification (details in `cnn/model.py`).

### Evaluation Metrics

All metrics were computed using the standard definitions:

$$\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$$

$$\text{Sensitivity (Recall)} = \frac{TP}{TP + FN}$$

$$\text{Specificity} = \frac{TN}{TN + FP}$$

$$\text{Precision} = \frac{TP}{TP + FP}$$

$$\text{F1} = 2 \cdot \frac{\text{Precision} \cdot \text{Sensitivity}}{\text{Precision} + \text{Sensitivity}}$$

---

## 🚀 Reproducing the Results

### Prerequisites

```bash
pip install -r requirements.txt
```

### Reproduce LLM Metric Analysis

```bash
python llm_evaluation/results_analysis.py \
  --no_list data/noListFinal.xlsx \
  --yes_list data/yesListFinal.xlsx
```

### Train and Evaluate CNN

```bash
# Download dataset from Kaggle first
python cnn/train.py --data_dir ./brain_tumor_dataset/
python cnn/evaluate.py --model_path ./cnn/saved_model.pth
```

---

## 📖 Citation

If you use this work, please cite:

```bibtex
@article{yourname2025braintumor,
  title     = {Brain Tumor Detection via Multimodal LLMs vs. CNN: A Comparative Benchmark Study},
  author    = {Your Name and Co-Authors},
  journal   = {Journal Name},
  year      = {2025},
  url       = {https://github.com/yourusername/your-repo}
}
```

---

## 📜 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

The dataset is provided by [Navoneel Chakrabarty on Kaggle](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection) under its respective license.

---

## 🤝 Acknowledgments

- Dataset: Navoneel Chakrabarty (Kaggle)
- Models evaluated: OpenAI (ChatGPT), Google DeepMind (Gemini), Anthropic (Claude)
