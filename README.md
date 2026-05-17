# Brain Tumor Detection: CNN vs Multimodal LLM Benchmark

## Overview

This repository contains the experimental materials and results for a comparative research study on **brain tumor detection from MRI images** using two different artificial intelligence paradigms:

1. **Convolutional Neural Networks (CNNs)** trained with transfer learning
2. **Multimodal Large Language Models (LLMs)** evaluated in a zero-shot image classification setting

The study compares classical deep learning architectures with modern vision-capable LLMs on the same public brain MRI dataset. The goal is to evaluate whether multimodal LLMs can match or outperform CNN-based models in a low-data medical imaging scenario.

> **Important:** This repository is intended for academic research only. The models and results must not be used as a clinical diagnostic tool without external validation and approval by medical professionals.

---

## Research Context

Brain tumor detection from magnetic resonance imaging (MRI) is a critical task in computer-aided diagnosis. CNNs have traditionally been the dominant approach in medical image classification, but recent multimodal LLMs can process both text and images without task-specific training.

This project investigates the following research question:

> Can zero-shot multimodal LLMs achieve performance comparable to or better than CNN models fine-tuned on a brain MRI tumor detection dataset?

The work is structured as a controlled comparison between **17 ImageNet-pretrained CNN architectures** and **8 multimodal LLMs**.

---

## Dataset

Dataset used in this study:

**Brain MRI Images for Brain Tumor Detection**  
Kaggle Dataset: <https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection>

The dataset contains **253 brain MRI images** divided into two classes:

| Class | Number of Images |
|---|---:|
| Tumor / Yes | 155 |
| No Tumor / No | 98 |
| **Total** | **253** |

The dataset was split into:

| Split | Total | Tumor | No Tumor |
|---|---:|---:|---:|
| Training | 202 | 124 | 78 |
| Test | 51 | 31 | 20 |

A fixed **80/20 train-test split** was used for all CNN and LLM experiments to ensure direct comparability.

---

## Models Evaluated

### CNN Models

The following 17 CNN architectures were evaluated using transfer learning:

- AlexNet
- VGG16
- VGG19
- ResNet50
- ResNet101
- ResNet152
- ResNet50V2
- ResNet101V2
- ResNet152V2
- DenseNet121
- DenseNet169
- DenseNet201
- InceptionV3
- Xception
- MobileNet
- MobileNetV2
- NASNetMobile

All CNN models used ImageNet-pretrained weights. The convolutional base was used with a classification head for binary tumor detection.

### Multimodal LLM Models

The following 8 multimodal LLMs were evaluated in a zero-shot setting:

- Gemini 3.1 Pro
- Gemini 3.1 Thinking
- ChatGPT 5.5 Thinking
- ChatGPT 5.4 Thinking
- ChatGPT o3
- Claude Sonnet 4.6
- Claude Sonnet 4.5
- Claude Haiku 4.5

Each LLM received a single MRI image with a standardized prompt and was required to answer only with `yes` or `no`.

---

## Methodology

### CNN Pipeline

The CNN workflow consisted of:

1. Loading MRI images from the dataset
2. Resizing images to `224 x 224`
3. Normalizing pixel values to the `[0, 1]` range
4. Applying transfer learning using ImageNet-pretrained CNN backbones
5. Training each model for 50 epochs
6. Evaluating each model on the same held-out test set

General CNN configuration:

| Parameter | Value |
|---|---|
| Input size | 224 x 224 |
| Optimizer | Adam |
| Learning rate | 1e-4 |
| Batch size | 32 |
| Epochs | 50 |
| Loss function | Binary cross-entropy |
| Split | 80% train / 20% test |

### LLM Pipeline

The LLM workflow consisted of:

1. Sending each MRI image to a multimodal LLM
2. Using the same prompt for every model and image
3. Parsing the model response as either `yes` or `no`
4. Comparing the response with the ground-truth label
5. Computing the same evaluation metrics used for CNNs

Prompt used:

```text
Is there a brain tumor in this MR image? Reply only with 'yes' or 'no'.
```

No fine-tuning, few-shot examples, or additional medical context was provided to the LLMs.

---

## Evaluation Metrics

The following metrics were used:

- Accuracy
- Precision
- Recall / Sensitivity
- Specificity
- F1-score
- Cohen's Kappa
- AUC
- Confusion matrix analysis

These metrics were selected because accuracy alone is not sufficient in medical imaging tasks. In particular, false negatives are clinically important because a missed tumor case may delay diagnosis and treatment.

---

## Main Results

### Best CNN Results

Six CNN architectures achieved the highest CNN test accuracy of **94.12%**:

| CNN Model | Test Accuracy | F1-Score | AUC |
|---|---:|---:|---:|
| DenseNet169 | 0.9412 | 0.9401 | 0.9742 |
| DenseNet201 | 0.9412 | 0.9409 | 0.9677 |
| InceptionV3 | 0.9412 | 0.9414 | 0.9677 |
| ResNet101V2 | 0.9412 | 0.9414 | 0.9903 |
| VGG16 | 0.9412 | 0.9401 | 0.9774 |
| Xception | 0.9412 | 0.9409 | 0.9560 |

The CNN experiments showed that model architecture strongly affects performance in low-data medical imaging tasks. Some models generalized well, while others showed overfitting despite high training accuracy.

### LLM Test Results

| LLM Model | TP | TN | FP | FN | Test Accuracy | Precision | F1-Score |
|---|---:|---:|---:|---:|---:|---:|---:|
| ChatGPT 5.4 Thinking | 31 | 20 | 0 | 0 | 1.0000 | 1.0000 | 1.0000 |
| ChatGPT 5.5 Thinking | 30 | 20 | 0 | 1 | 0.9804 | 1.0000 | 0.9836 |
| Gemini 3.1 Thinking | 30 | 18 | 2 | 1 | 0.9412 | 0.9375 | 0.9524 |
| Gemini 3.1 Pro | 29 | 17 | 3 | 2 | 0.9020 | 0.9062 | 0.9206 |
| ChatGPT o3 | 30 | 16 | 4 | 1 | 0.9020 | 0.8824 | 0.9231 |
| Claude Sonnet 4.6 | 29 | 16 | 4 | 2 | 0.8824 | 0.8788 | 0.9062 |
| Claude Sonnet 4.5 | 25 | 19 | 1 | 6 | 0.8627 | 0.9615 | 0.8772 |
| Claude Haiku 4.5 | 26 | 13 | 7 | 5 | 0.7647 | 0.7879 | 0.8125 |

The best-performing LLM, **ChatGPT 5.4 Thinking**, achieved perfect classification on the 51-image test set. ChatGPT 5.5 Thinking and Gemini 3.1 Thinking also performed competitively and reached or exceeded the best CNN results.

---

## Key Findings

- Multimodal LLMs demonstrated strong zero-shot performance on brain MRI tumor detection.
- The best LLM model achieved **100% test accuracy** on the held-out test set.
- The best CNN models achieved **94.12% test accuracy**.
- CNN performance varied significantly depending on architecture.
- Some CNNs, such as ResNet50 and NASNetMobile, showed signs of overfitting.
- Reasoning-oriented LLM variants generally performed better than their standard counterparts.
- False-negative cases remain the most important error type for clinical screening scenarios.

---

## Comparative Interpretation

The results suggest that modern multimodal LLMs may be highly effective in small-scale medical image classification tasks, even without task-specific fine-tuning. However, the findings should be interpreted carefully because the dataset is small and the test set contains only 51 images.

CNNs remain valuable because they are transparent in terms of training procedure, reproducible in local environments, and easier to deploy in controlled medical imaging pipelines. LLMs, on the other hand, offer strong zero-shot performance but may be affected by prompt design, API versioning, model updates, and limited interpretability.

A promising future direction is the development of hybrid systems that combine CNN-based feature extraction with LLM-based reasoning and explanation.

---

## Suggested Repository Structure

```text
brain-tumor-cnn-vs-llm/
│
├── data/
│   ├── train/
│   │   ├── yes/
│   │   └── no/
│   └── test/
│       ├── yes/
│       └── no/
│
├── notebooks/
│   ├── cnn_training.ipynb
│   ├── llm_evaluation.ipynb
│   └── metrics_analysis.ipynb
│
├── results/
│   ├── cnn_results.csv
│   ├── llm_results.csv
│   ├── confusion_matrices/
│   └── figures/
│
├── src/
│   ├── data_loader.py
│   ├── cnn_models.py
│   ├── train.py
│   ├── evaluate.py
│   └── metrics.py
│
├── README.md
├── requirements.txt
└── LICENSE
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/your-username/brain-tumor-cnn-vs-llm.git
cd brain-tumor-cnn-vs-llm
```

Create and activate a virtual environment:

```bash
python -m venv venv
source venv/bin/activate
```

For Windows:

```bash
venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Example dependencies:

```text
tensorflow
keras
numpy
pandas
scikit-learn
matplotlib
seaborn
opencv-python
jupyter
```

---

## Usage

### Train CNN Models

```bash
python src/train.py
```

### Evaluate CNN Models

```bash
python src/evaluate.py
```

### Analyze Metrics

```bash
jupyter notebook notebooks/metrics_analysis.ipynb
```

LLM evaluations should be performed according to the API access rules of each model provider. The exact prompt and output parsing strategy should be kept fixed for reproducibility.

---

## Reproducibility Notes

To reproduce the experiments:

1. Download the dataset from Kaggle.
2. Keep the same train-test split used in the study.
3. Use the same image preprocessing steps for all CNN models.
4. Use the same prompt for every LLM.
5. Report confusion matrices and class-wise metrics, not only accuracy.
6. Record model versions and API dates for all LLM experiments.

Because LLM APIs may change over time, exact replication of LLM results may require documenting the model version, date of inference, temperature settings, and prompt configuration.

---

## Limitations

- The dataset contains only 253 images.
- The test set contains only 51 images, so confidence intervals may be wide.
- The dataset is not a large multicenter clinical dataset.
- The task is binary classification only: tumor vs. no tumor.
- LLM outputs may be affected by prompt wording and model version updates.
- External validation is required before drawing clinical conclusions.

---

## Future Work

Potential extensions of this work include:

- Testing on larger and more diverse MRI datasets
- Evaluating multiclass tumor classification
- Adding tumor localization or segmentation
- Applying Grad-CAM or other explainable AI methods to CNN predictions
- Testing prompt engineering strategies for LLMs
- Performing repeated LLM runs to measure output stability
- Building hybrid CNN + LLM clinical decision-support pipelines

---

## Authors

- Mustafa Yurdakul  
- Ahmet Eren Kaplan  

Department of Computer Engineering, Kırıkkale University, Kırıkkale, Türkiye

---

## Citation

If you use this repository or results in your work, please cite the related study:

```bibtex
@article{yurdakul2026brainTumorCnnLlm,
  title={A Comparative Evaluation of Convolutional Neural Networks and Multimodal Large Language Models for Brain Tumor Detection on Magnetic Resonance Images},
  author={Yurdakul, Mustafa and Kaplan, Ahmet Eren},
  year={2026},
  note={Manuscript / preprint}
}
```

---

## License

This project is intended for academic and research purposes. Please check the original dataset license and usage conditions on Kaggle before redistribution.

---

## Disclaimer

This project does not provide medical advice. The results are experimental and should not be used for diagnosis, treatment planning, or clinical decision-making without expert medical review and independent validation.
