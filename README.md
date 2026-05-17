# Comparative Evaluation of CNN Architectures and Multimodal Large Language Models for Brain MRI Tumor Detection

> **SCI-style research repository** for benchmarking classical deep learning CNN architectures and modern multimodal large language models (MLLMs) on binary brain tumor detection from MRI images.

[![Dataset](https://img.shields.io/badge/Dataset-Kaggle-blue)](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection)
[![Task](https://img.shields.io/badge/Task-Brain%20Tumor%20Detection-red)](#)
[![Modality](https://img.shields.io/badge/Modality-Brain%20MRI-lightgrey)](#)
[![Status](https://img.shields.io/badge/Status-Research%20Study-green)](#)

---

## Abstract

Brain tumor detection from magnetic resonance imaging (MRI) is a clinically important classification task in medical image analysis. This repository presents a comparative experimental study between convolutional neural network (CNN) architectures and multimodal large language models (MLLMs) for binary brain MRI classification. The study uses the publicly available **Brain MRI Images for Brain Tumor Detection** dataset from Kaggle and evaluates model predictions under a unified metric protocol.

The dataset images were processed with multiple MLLMs, including **Gemini 3.1 Pro**, **Gemini 3.1 Thinking**, **ChatGPT 5.5 Thinking**, **ChatGPT 5.4 Thinking**, **ChatGPT o3**, **Claude Sonnet 4.6**, **Claude Sonnet 4.5**, and **Claude Haiku 4.5**. In parallel, multiple CNN architectures were trained and evaluated on the same binary tumor/no-tumor classification task. Experimental results indicate that leading MLLMs can match or exceed the best CNN architectures in aggregate test-set metrics, while CNN performance remains strongly dependent on architecture selection and training behavior.

---

## Keywords

Brain MRI, Brain Tumor Detection, Medical Image Classification, Convolutional Neural Networks, Multimodal Large Language Models, Explainable AI, Computer-Aided Diagnosis, Deep Learning, Comparative Benchmarking

---

## Dataset

The study uses the Kaggle dataset:

**Brain MRI Images for Brain Tumor Detection**  
Dataset URL: https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection

The task is formulated as a binary classification problem:

| Class | Description |
|---|---|
| `Yes` | MRI image contains a brain tumor |
| `No` | MRI image does not contain a brain tumor |

In the reported experimental protocol, the MLLM evaluation file contains:

| Split | Total Images | Tumor / Yes | Non-tumor / No |
|---|---:|---:|---:|
| Train | 202 | 124 | 78 |
| Test | 51 | 31 | 20 |
| All | 253 | 155 | 98 |

---

## Research Objective

The main objective of this study is to evaluate whether state-of-the-art multimodal large language models can perform reliable brain MRI tumor detection when compared with conventional CNN-based medical image classifiers.

The study specifically investigates:

1. The classification performance of multiple CNN architectures on brain MRI images.
2. The zero-shot or prompt-based diagnostic performance of MLLMs on the same task.
3. The comparative behavior of both paradigms using identical evaluation metrics.
4. The error profiles of models in terms of false positives and false negatives.
5. The potential of MLLMs as assistive tools for medical image understanding.

---

## Evaluated Models

### Multimodal Large Language Models

The following MLLMs were evaluated:

| Model |
|---|
| Gemini 3.1 Pro |
| Gemini 3.1 Thinking |
| ChatGPT 5.5 Thinking |
| ChatGPT 5.4 Thinking |
| ChatGPT o3 |
| Claude Sonnet 4.6 |
| Claude Sonnet 4.5 |
| Claude Haiku 4.5 |

### CNN Architectures

The CNN benchmark includes 17 architectures, including:

| CNN Model |
|---|
| DenseNet169 |
| DenseNet201 |
| InceptionV3 |
| ResNet101V2 |
| VGG16 |
| Xception |
| DenseNet121 |
| ResNet152V2 |
| ResNet50V2 |
| MobileNet |
| VGG19 |
| AlexNet |
| MobileNetV2 |
| ResNet101 |
| ResNet152 |
| NASNetMobile |
| ResNet50 |

---

## Methodology

The experimental pipeline follows a controlled comparative design.

### 1. Data Preparation

MRI images were organized into binary classes: `Yes` and `No`. The same image-level labels were used for all evaluated methods. The experimental split used for MLLM evaluation contains 202 training images and 51 test images.

### 2. CNN-Based Classification

CNN models were trained on the training partition and evaluated on the test partition. The final-epoch training accuracy and test-set performance metrics were recorded. Models were ranked according to test accuracy, and the top-performing CNN architectures were highlighted.

### 3. MLLM-Based Classification

Each MRI image was independently processed using the selected MLLMs. The model outputs were converted into binary predictions:

```text
Yes = tumor detected
No  = no tumor detected
```

For each MLLM, confusion matrix components were calculated:

- True Positive (TP)
- True Negative (TN)
- False Positive (FP)
- False Negative (FN)

### 4. Evaluation Metrics

All models were evaluated using the same metric suite:

| Metric | Formula |
|---|---|
| Accuracy | `(TP + TN) / (TP + TN + FP + FN)` |
| Precision | `TP / (TP + FP)` |
| Recall / Sensitivity | `TP / (TP + FN)` |
| Specificity | `TN / (TN + FP)` |
| F1 Score | `2 × Precision × Recall / (Precision + Recall)` |

For CNN models, additional metrics such as AUC and Cohen’s Kappa were also reported.

---

## Main Results

### MLLM Test-Set Performance

| Rank | Model | TP | TN | FP | FN | Accuracy | Precision | Recall | Specificity | F1 Score |
|---:|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | ChatGPT 5.4 Thinking | 31 | 20 | 0 | 0 | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| 2 | ChatGPT 5.5 Thinking | 30 | 20 | 0 | 1 | 0.9804 | 1.0000 | 0.9677 | 1.0000 | 0.9836 |
| 3 | Gemini 3.1 Thinking | 30 | 18 | 2 | 1 | 0.9412 | 0.9375 | 0.9677 | 0.9000 | 0.9524 |
| 4 | Gemini 3.1 Pro | 29 | 17 | 3 | 2 | 0.9020 | 0.9062 | 0.9355 | 0.8500 | 0.9206 |
| 5 | ChatGPT o3 | 30 | 16 | 4 | 1 | 0.9020 | 0.8824 | 0.9677 | 0.8000 | 0.9231 |
| 6 | Claude Sonnet 4.6 | 29 | 16 | 4 | 2 | 0.8824 | 0.8788 | 0.9355 | 0.8000 | 0.9062 |
| 7 | Claude Sonnet 4.5 | 25 | 19 | 1 | 6 | 0.8627 | 0.9615 | 0.8065 | 0.9500 | 0.8772 |
| 8 | Claude Haiku 4.5 | 26 | 13 | 7 | 5 | 0.7647 | 0.7879 | 0.8387 | 0.6500 | 0.8125 |

### CNN Test-Set Summary

The top CNN architectures achieved a test accuracy of **0.9412**. The best CNN group included:

| CNN Model | Test Accuracy | F1 Score | AUC | Kappa |
|---|---:|---:|---:|---:|
| DenseNet169 | 0.9412 | 0.9401 | 0.9742 | 0.8732 |
| DenseNet201 | 0.9412 | 0.9409 | 0.9677 | 0.8755 |
| InceptionV3 | 0.9412 | 0.9414 | 0.9677 | 0.8777 |
| ResNet101V2 | 0.9412 | 0.9414 | 0.9903 | 0.8777 |
| VGG16 | 0.9412 | 0.9401 | 0.9774 | 0.8732 |
| Xception | 0.9412 | 0.9409 | 0.9560 | 0.8755 |

---

## Comparative Interpretation

The results show a clear performance hierarchy between the evaluated model families.

The strongest MLLM, **ChatGPT 5.4 Thinking**, achieved perfect classification on the test set, with no false positives and no false negatives. **ChatGPT 5.5 Thinking** ranked second, missing only one tumor case while producing no false alarms. **Gemini 3.1 Thinking** also demonstrated strong performance, matching the best CNN accuracy level of 0.9412 while maintaining high recall.

The best CNN models reached a shared test accuracy of **0.9412**, indicating that several conventional deep learning architectures can perform strongly on this dataset. However, CNN results varied substantially across architectures, with lower-performing models showing reduced generalization. In contrast, the MLLM results were generally concentrated in a narrower and higher performance range, although model-specific differences remained visible.

From a clinical perspective, false negatives are especially important because they represent missed tumor cases. The lowest false-negative count was obtained by **ChatGPT 5.4 Thinking** with `FN = 0`, followed by **ChatGPT 5.5 Thinking**, **Gemini 3.1 Thinking**, and **ChatGPT o3** with `FN = 1`.

---

## Key Findings

- The best MLLM achieved **1.0000 test accuracy and 1.0000 F1 score**.
- The best CNN architectures achieved **0.9412 test accuracy**.
- ChatGPT 5.5 Thinking produced **zero false positives** and only **one false negative**.
- Gemini 3.1 Thinking matched the best CNN-level accuracy while maintaining high tumor recall.
- CNN performance was highly architecture-dependent.
- The results suggest that MLLMs may be promising assistive tools for medical image interpretation, especially when evaluated under controlled binary classification settings.

---

## Repository Structure

A suggested repository structure is shown below:

```text
brain-mri-llm-cnn-comparison/
│
├── data/
│   ├── raw/                  # Original Kaggle dataset images
│   ├── train/                # Training images
│   └── test/                 # Test images
│
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_cnn_training.ipynb
│   ├── 03_llm_prediction_analysis.ipynb
│   └── 04_results_visualization.ipynb
│
├── results/
│   ├── cnn_metrics.csv
│   ├── llm_metrics.csv
│   ├── confusion_matrices/
│   └── figures/
│
├── src/
│   ├── preprocessing.py
│   ├── train_cnn.py
│   ├── evaluate.py
│   └── metrics.py
│
├── README.md
└── requirements.txt
```

---

## How to Reproduce

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/brain-mri-llm-cnn-comparison.git
cd brain-mri-llm-cnn-comparison
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

Example Python dependencies:

```text
numpy
pandas
scikit-learn
matplotlib
seaborn
tensorflow
keras
opencv-python
jupyter
```

### 3. Download the Dataset

Download the dataset from Kaggle:

```text
https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection
```

Then place the images under the `data/raw/` directory.

### 4. Run the Experiments

```bash
python src/train_cnn.py
python src/evaluate.py
```

For notebook-based analysis, run the notebooks in the following order:

```text
01_data_preprocessing.ipynb
02_cnn_training.ipynb
03_llm_prediction_analysis.ipynb
04_results_visualization.ipynb
```

---

## Scientific Contribution

This repository contributes a comparative benchmark between CNN-based medical image classifiers and MLLMs for brain MRI tumor detection. Unlike classical CNN-only studies, this work evaluates how modern vision-capable language models behave on a medical image classification task and compares them against established deep learning architectures using the same evaluation protocol.

The study may support future research on:

- MLLM-assisted medical image screening
- Hybrid CNN–LLM decision systems
- Prompt-based medical image classification
- Error analysis of AI systems in radiological tasks
- Explainable AI for tumor/no-tumor prediction

---

## Limitations

This study should be interpreted with caution.

- The dataset size is limited.
- The task is binary and does not include tumor type segmentation or grading.
- MLLM outputs may vary depending on prompt design and model version.
- Test-set performance may not directly generalize to real clinical environments.
- The results do not replace expert radiological diagnosis.

---

## Medical Disclaimer

This repository is intended for academic and research purposes only. The models and results presented here are not intended for direct clinical diagnosis, treatment planning, or medical decision-making. Any medical interpretation of MRI images must be performed by qualified healthcare professionals.

---

## Citation

If you use this repository or its experimental results in your work, please cite it as:

```bibtex
@misc{brain_mri_llm_cnn_comparison,
  title        = {Comparative Evaluation of CNN Architectures and Multimodal Large Language Models for Brain MRI Tumor Detection},
  author       = {Kaplan, Eren},
  year         = {2026},
  howpublished = {GitHub repository},
  note         = {Brain MRI tumor detection using CNNs and multimodal large language models}
}
```

---

## License

This project is released for academic and research purposes. Please check the original Kaggle dataset page for dataset-specific license and usage conditions.

---

## Contact

For questions, suggestions, or collaboration requests, please open an issue on the GitHub repository.
