# Brain MRI Tumor Detection: LLM-Based Visual Reasoning vs CNN Baseline

Bu repository, **Brain MRI Images for Brain Tumor Detection** veri seti üzerinde farklı büyük dil modellerinin/görsel akıl yürütme modellerinin ve CNN tabanlı bir yaklaşımın beyin MR görüntülerinde tümör tespiti performansını karşılaştırmak amacıyla hazırlanmıştır.

Çalışma, SCI makale formatına uygun şekilde; veri seti, yöntem, model çıktıları, değerlendirme metrikleri ve karşılaştırmalı sonuçları raporlayacak biçimde organize edilmiştir.

> ⚠️ **Not:** Bu çalışma akademik/deneysel amaçlıdır. Üretilen sonuçlar klinik tanı yerine kullanılamaz. Tıbbi kararlar uzman radyolog ve klinik hekim değerlendirmesi gerektirir.

---

## Dataset

Çalışmada kullanılan veri seti:

**Brain MRI Images for Brain Tumor Detection**  
Kaggle: https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection

Veri seti iki sınıftan oluşmaktadır:

| Class | Description | Number of Images |
|---|---:|---:|
| `yes` | Tumor detected / Tümör var | 155 |
| `no` | No tumor / Tümör yok | 98 |
| **Total** |  | **253** |

Bu çalışmada görüntüler ikili sınıflandırma problemi olarak ele alınmıştır:

```text
Yes = Brain tumor exists
No  = Brain tumor does not exist
```

---

## Study Objective

Bu projenin temel amacı, klasik CNN tabanlı görüntü sınıflandırma yaklaşımı ile güncel multimodal LLM/görsel model yaklaşımlarını aynı veri seti üzerinde karşılaştırmaktır.

Araştırma soruları:

1. Multimodal LLM modelleri beyin MR görüntülerinde tümör var/yok kararını ne kadar başarılı verebilir?
2. Modellerin tümörlü görüntülerdeki duyarlılığı ve tümörsüz görüntülerdeki özgüllüğü nasıl değişmektedir?
3. LLM tabanlı görsel akıl yürütme çıktıları CNN tabanlı yaklaşıma göre nasıl konumlanmaktadır?
4. Modeller yanlış pozitif ve yanlış negatif üretme eğiliminde midir?

---

## Evaluated Models

Bu çalışmada aşağıdaki modeller değerlendirilmiştir:

### Large Language / Vision-Language Models

- Gemini 3.1 Pro
- ChatGPT 5.5 Thinking
- Claude Sonnet 4.6
- Claude Haiku 4.5
- Claude Sonnet 4.5
- Gemini 3.1 Thinking
- ChatGPT 5.4 Thinking
- ChatGPT o3

### Deep Learning Baseline

- Convolutional Neural Network (CNN)

CNN modeli bu repository’de klasik görüntü sınıflandırma referansı olarak konumlandırılmıştır. CNN sonuçları ayrı bir deney dosyası veya notebook üzerinden eklendiğinde aşağıdaki karşılaştırma tablosuna dahil edilebilir.

---

## Methodology

Genel deney akışı aşağıdaki gibidir:

1. Kaggle veri seti indirildi.
2. Görüntüler `yes` ve `no` sınıflarına göre ayrıldı.
3. Her görüntü LLM modellerine tek tek verildi.
4. Modellerden yalnızca iki olası çıktı üretmeleri istendi:

```text
Yes
No
```

5. Model tahminleri Excel dosyalarında toplandı:

```text
yesListFinal.xlsx
noListFinal.xlsx
```

6. Tahminler gerçek etiketlerle karşılaştırıldı.
7. Her model için aşağıdaki metrikler hesaplandı:

- Accuracy
- Sensitivity / Recall
- Specificity
- Precision
- F1-Score

---

## Evaluation Metrics

Bu çalışmada `Yes` sınıfı pozitif sınıf olarak kabul edilmiştir.

| Metric | Formula | Meaning |
|---|---|---|
| Accuracy | `(TP + TN) / (TP + TN + FP + FN)` | Genel doğru sınıflandırma oranı |
| Sensitivity / Recall | `TP / (TP + FN)` | Tümörlü görüntüleri doğru yakalama oranı |
| Specificity | `TN / (TN + FP)` | Tümörsüz görüntüleri doğru tanıma oranı |
| Precision | `TP / (TP + FP)` | Tümör var denilen görüntülerde doğruluk oranı |
| F1-Score | `2 * Precision * Recall / (Precision + Recall)` | Precision ve Recall dengesini gösteren skor |

---

## LLM Model Results

Aşağıdaki sonuçlar, yüklenen `yesListFinal.xlsx` ve `noListFinal.xlsx` dosyalarındaki model çıktılarından hesaplanmıştır.

| Model | TP | FN | TN | FP | Accuracy (%) | Sensitivity (%) | Specificity (%) | Precision (%) | F1-Score (%) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Gemini 3.1 Pro | 150 | 5 | 79 | 19 | 90.51 | 96.77 | 80.61 | 88.76 | 92.59 |
| ChatGPT 5.5 Thinking | 149 | 6 | 87 | 11 | 93.28 | 96.13 | 88.78 | 93.12 | 94.60 |
| Claude Sonnet 4.6 | 148 | 7 | 66 | 32 | 84.58 | 95.48 | 67.35 | 82.22 | 88.36 |
| Claude Haiku 4.5 | 135 | 20 | 65 | 33 | 79.05 | 87.10 | 66.33 | 80.36 | 83.59 |
| Claude Sonnet 4.5 | 133 | 22 | 85 | 13 | 86.17 | 85.81 | 86.73 | 91.10 | 88.37 |
| Gemini 3.1 Thinking | 151 | 4 | 82 | 16 | 92.09 | 97.42 | 83.67 | 90.42 | 93.79 |
| ChatGPT 5.4 Thinking | 154 | 1 | 86 | 12 | **94.86** | **99.35** | 87.76 | 92.77 | **95.95** |
| ChatGPT o3 | 152 | 3 | 70 | 28 | 87.75 | 98.06 | 71.43 | 84.44 | 90.75 |

---

## Majority Voting Result

LLM modellerinin toplu kararını incelemek için basit çoğunluk oylaması uygulanmıştır. Sekiz modelden en az beşi `Yes` dediyse sonuç `Yes`, aksi durumda `No` kabul edilmiştir.

| Method | TP | FN | TN | FP | Accuracy (%) | Sensitivity (%) | Specificity (%) | Precision (%) | F1-Score (%) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| LLM Majority Voting | 150 | 5 | 86 | 12 | 93.28 | 96.77 | 87.76 | 92.59 | 94.64 |

---

## Class-Based Observations

### Tumor Class: `Yes`

Tümörlü görüntülerde modeller genel olarak yüksek başarı göstermiştir. En yüksek tümör yakalama oranı **ChatGPT 5.4 Thinking** modelinde görülmüştür.

| Model | Correct Yes | Wrong No | Yes-Class Accuracy (%) |
|---|---:|---:|---:|
| Gemini 3.1 Pro | 150 | 5 | 96.77 |
| ChatGPT 5.5 Thinking | 149 | 6 | 96.13 |
| Claude Sonnet 4.6 | 148 | 7 | 95.48 |
| Claude Haiku 4.5 | 135 | 20 | 87.10 |
| Claude Sonnet 4.5 | 133 | 22 | 85.81 |
| Gemini 3.1 Thinking | 151 | 4 | 97.42 |
| ChatGPT 5.4 Thinking | 154 | 1 | **99.35** |
| ChatGPT o3 | 152 | 3 | 98.06 |

### Non-Tumor Class: `No`

Tümörsüz görüntülerde model performansları daha değişkendir. Bazı modeller tümörsüz görüntülerde daha fazla yanlış pozitif üretmiştir.

| Model | Correct No | Wrong Yes | No-Class Accuracy / Specificity (%) |
|---|---:|---:|---:|
| Gemini 3.1 Pro | 79 | 19 | 80.61 |
| ChatGPT 5.5 Thinking | 87 | 11 | **88.78** |
| Claude Sonnet 4.6 | 66 | 32 | 67.35 |
| Claude Haiku 4.5 | 65 | 33 | 66.33 |
| Claude Sonnet 4.5 | 85 | 13 | 86.73 |
| Gemini 3.1 Thinking | 82 | 16 | 83.67 |
| ChatGPT 5.4 Thinking | 86 | 12 | 87.76 |
| ChatGPT o3 | 70 | 28 | 71.43 |

---

## Key Findings

- En yüksek genel doğruluk **ChatGPT 5.4 Thinking** modelinde elde edilmiştir.
- Tümörlü görüntülerde genel başarı oldukça yüksektir; birçok model `%95+` sensitivity değerine ulaşmıştır.
- Tümörsüz görüntülerde performans daha değişkendir; bazı modeller yanlış pozitif üretmeye daha yatkındır.
- **ChatGPT 5.5 Thinking**, tümörsüz sınıfta en yüksek specificity değerini vermiştir.
- Basit çoğunluk oylaması, tekil modellerin çoğuna göre dengeli bir sonuç üretmiştir.
- LLM modelleri özellikle tümörlü sınıfta güçlü görsel karar performansı göstermiştir; ancak klinik güvenilirlik için CNN/transfer learning, çapraz doğrulama, harici test seti ve uzman doğrulaması ile desteklenmesi gerekir.

---

## CNN Baseline

CNN modeli, çalışmada klasik derin öğrenme tabanlı referans model olarak kullanılmıştır. CNN sonuçları aşağıdaki formatta raporlanabilir:

| Model | Accuracy (%) | Sensitivity (%) | Specificity (%) | Precision (%) | F1-Score (%) |
|---|---:|---:|---:|---:|---:|
| CNN | `TO_BE_ADDED` | `TO_BE_ADDED` | `TO_BE_ADDED` | `TO_BE_ADDED` | `TO_BE_ADDED` |

Önerilen CNN deney adımları:

1. Görüntüleri yeniden boyutlandırma
2. Piksel normalizasyonu
3. Train/validation/test ayrımı
4. CNN mimarisi oluşturma
5. Binary classification eğitimi
6. Confusion matrix ve metrik hesaplama
7. LLM sonuçları ile karşılaştırma

---

## Suggested Repository Structure

```text
.
├── data/
│   ├── yes/
│   └── no/
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_cnn_training.ipynb
│   ├── 03_llm_result_analysis.ipynb
│   └── 04_comparative_evaluation.ipynb
├── results/
│   ├── yesListFinal.xlsx
│   ├── noListFinal.xlsx
│   ├── llm_metrics.csv
│   ├── cnn_metrics.csv
│   └── confusion_matrices/
├── src/
│   ├── preprocessing.py
│   ├── cnn_model.py
│   ├── metrics.py
│   └── visualization.py
├── README.md
└── requirements.txt
```

---

## Installation

```bash
git clone https://github.com/USERNAME/REPOSITORY_NAME.git
cd REPOSITORY_NAME
pip install -r requirements.txt
```

Example `requirements.txt`:

```text
numpy
pandas
matplotlib
seaborn
scikit-learn
tensorflow
opencv-python
openpyxl
jupyter
```

---

## Usage

### 1. Download Dataset

Download the dataset from Kaggle:

```bash
kaggle datasets download -d navoneel/brain-mri-images-for-brain-tumor-detection
```

Then unzip it into the `data/` directory.

### 2. Analyze LLM Outputs

Place the model output files into the `results/` directory:

```text
results/yesListFinal.xlsx
results/noListFinal.xlsx
```

Then run:

```bash
jupyter notebook notebooks/03_llm_result_analysis.ipynb
```

### 3. Train CNN Baseline

```bash
jupyter notebook notebooks/02_cnn_training.ipynb
```

### 4. Compare Models

```bash
jupyter notebook notebooks/04_comparative_evaluation.ipynb
```

---

## Possible Visualizations

Bu çalışma için aşağıdaki grafikler önerilir:

- Model bazlı Accuracy karşılaştırması
- Sensitivity vs Specificity grafiği
- Confusion matrix görselleştirmeleri
- LLM modelleri ile CNN karşılaştırma grafiği
- Yanlış pozitif ve yanlış negatif örnek analizi

---

## Limitations

- Veri seti küçüktür ve yalnızca 253 görüntü içermektedir.
- LLM modelleri deterministic olmayan çıktılar üretebilir; aynı görüntü farklı sorgularda farklı yorumlanabilir.
- Prompt tasarımı model performansını etkileyebilir.
- Görüntülerin kalite, kesit, kontrast ve artefakt farklılıkları sonuçları etkileyebilir.
- CNN için daha güvenilir sonuçlar elde etmek adına çapraz doğrulama ve veri artırma yöntemleri kullanılmalıdır.
- Bu çalışma klinik tanı aracı olarak değil, akademik karşılaştırma çalışması olarak değerlendirilmelidir.

---

## Citation

Dataset citation:

```text
Navoneel Chakrabarty, Brain MRI Images for Brain Tumor Detection, Kaggle.
https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection
```

Bu repository veya çalışma kullanılırsa lütfen ilgili Kaggle veri setini ve bu projeyi referans gösteriniz.

---

## License

Bu proje akademik ve araştırma amaçlı hazırlanmıştır. Veri setinin kullanım koşulları için Kaggle veri seti sayfasındaki lisans bilgileri dikkate alınmalıdır.

---

## Author

Prepared for SCI-style comparative analysis of brain MRI tumor detection using multimodal LLMs and CNN-based deep learning.
