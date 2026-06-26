# Customer Segmentation in Digital Marketing — ML Final Project

> **Referensi Paper:**
> Wang, G. (2025). *Customer segmentation in the digital marketing using a Q-learning based differential evolution algorithm integrated with K-means clustering.* PLoS ONE 20(2): e0318519. [https://doi.org/10.1371/journal.pone.0318519](https://doi.org/10.1371/journal.pone.0318519)

Proyek ini mengimplementasikan framework segmentasi pelanggan berbasis machine learning yang mengintegrasikan K-means clustering dengan algoritma metaheuristik **QLDE (Q-Learning based Differential Evolution)**, dilanjutkan evaluasi menggunakan 4 model supervised learning sebagai klasifikasi label pelanggan.

---

## 📁 Struktur Proyek

```
├── data/
│   ├── raw/
│   │   └── data.csv                          # Dataset transaksi e-commerce mentah
│   ├── processed/
│   │   ├── customer_features_raw.csv         # 11 fitur RFM level pelanggan (belum discale)
│   │   ├── customer_features_scaled.csv      # Fitur yang sudah di-scale (StandardScaler)
│   │   └── customer_features_pca.csv         # Fitur setelah reduksi PCA (6 komponen)
│   └── Labeled/
│       ├── hasildata_kmeans-qlde.csv         # Label cluster dari K-Means QLDE
│       ├── hasildata_kmeans-standard.csv     # Label cluster dari Standard K-Means
│       └── hasildata_kmeans-de.csv           # Label cluster dari K-Means DE
│
├── notebooks/
│   ├── 01_eda_and_preprocessing.ipynb        # EDA & preprocessing pipeline
│   ├── 02.0_unsupervised_learning_method.ipynb  # K-Means QLDE (algoritma utama paper)
│   ├── 02.1_kmeans_standard.ipynb            # Standard K-Means (baseline)
│   ├── 02.2_kmeans_de.ipynb                  # K-Means + Differential Evolution (F konstan)
│   ├── 02.3_kmeans_pso.ipynb                 # K-Means + Particle Swarm Optimization
│   ├── 02.4_kmeans_eoa.ipynb                 # K-Means + Equilibrium Optimizer Algorithm
│   ├── 03_comparison_analysis.ipynb          # Analisis perbandingan metrik unsupervised
│   ├── 04.1.1_classification_decision_tree_standard.ipynb
│   ├── 04.1.2_classification_svm_standard.ipynb
│   ├── 04.1.3_classification_adaboost_standard.ipynb
│   ├── 04.1.4_classification_ann_standard.ipynb
│   ├── 04.2.1_classification_decision_tree_qlde.ipynb
│   ├── 04.2.2_classification_svm_qlde.ipynb
│   ├── 04.2.3_classification_adaboost_qlde.ipynb
│   ├── 04.2.4_classification_ann_qlde.ipynb
│   ├── 04.3.1_classification_decision_tree_de.ipynb
│   ├── 04.3.2_classification_svm_de.ipynb
│   ├── 04.3.3_classification_adaboost_de.ipynb
│   ├── 04.3.4_classification_ann_de.ipynb
│   └── 04.4_classification_comparison.ipynb  # Komparasi semua model klasifikasi
│
├── models/                                   # Output model, scaler, metrik, dan visualisasi
├── references/                               # Paper dan referensi akademis
├── requirements.txt
└── README.md
```

---

## 🔬 Alur Pipeline Lengkap

```
[Data Mentah]
     ↓
[01] EDA & Preprocessing
  → Cleaning, Feature Engineering (11 fitur RFM), PowerTransformer, StandardScaler, PCA (6 PC)
     ↓
[02] Unsupervised Learning — Clustering (K=6)
  → 02.0 K-Means QLDE  |  02.1 Standard  |  02.2 DE  |  02.3 PSO  |  02.4 EOA
     ↓
[03] Analisis Perbandingan Metrik Clustering
     ↓
[04] Supervised Learning — Klasifikasi Label Cluster
  → Decision Tree | SVM | AdaBoost | ANN  (per dataset: Standard, QLDE, DE)
     ↓
[04.4] Komparasi Semua Model Klasifikasi
```

---

## 📊 Hasil Perbandingan Clustering (Notebook 03)

Metrik diambil langsung dari file `.npy` yang tersimpan di `models/`:

| Algoritma | SSE ↓ | Silhouette ↑ | DBI ↓ | CHI ↑ | Runtime ↓ |
|-----------|------:|-------------:|------:|------:|----------:|
| **K-Means QLDE** | 19,914.25 | **0.2535 ✓** | **1.3404 ✓** | 1,116.64 | 1.65s |
| **Standard K-Means** | **19,459.15 ✓** | 0.2142 | 1.4749 | **1,162.98 ✓** | **0.14s ✓** |
| **K-Means DE** | 19,908.04 | 0.2520 | 1.3456 | 1,117.24 | 4.31s |

> **Interpretasi:** QLDE menghasilkan kualitas pemisahan cluster yang lebih baik (Silhouette & DBI), sementara Standard K-Means unggul di SSE, CHI, dan kecepatan. Tidak ada satu algoritma yang dominan di semua metrik.

---

## 🤖 Hasil Perbandingan Klasifikasi (Notebook 04.4)

Evaluasi dengan Cross-Validation (CV=5) menggunakan 100% data:

| Dataset (Sumber Label) | Algoritma | CV Mean Acc (%) | Test Acc (%) | Waktu (s) |
|:-----------------------|:----------|:---------------:|:------------:|----------:|
| **Standard K-Means** | Decision Tree | 84.75 | 83.39 | 0.05 |
| | SVM (RBF) | 89.70 | 88.93 | 0.19 |
| | AdaBoost | 85.67 | 87.31 | 0.74 |
| | **ANN (MLP)** | 96.86 | **97.92** | 13.74 |
| **K-Means QLDE** | Decision Tree | 82.76 | 82.58 | 0.06 |
| | SVM (RBF) | 86.90 | 86.04 | 0.21 |
| | AdaBoost | 87.74 | 87.77 | 0.77 |
| | **ANN (MLP)** | 96.48 | **96.77** | 14.02 |
| **K-Means DE** | Decision Tree | 85.06 | 84.78 | 0.04 |
| | SVM (RBF) | 90.30 | 89.97 | 0.19 |
| | AdaBoost | 86.25 | 85.35 | 0.74 |
| | **ANN (MLP)** | 96.71 | **97.12** | 14.04 |

**Kesimpulan:**
- **ANN** mendominasi akurasi (~97–98%) di semua dataset — direkomendasikan jika prioritas adalah ketepatan prediksi.
- **SVM** menawarkan keseimbangan terbaik antara akurasi tinggi (~89–90%) dan kecepatan komputasi (~0.2s).
- **Decision Tree** tetap esensial untuk mengekstrak *Business Rules* (If-Else logic) yang dapat dibaca manusia.

---

## ⚙️ Parameter Algoritma

| Algoritma | Parameter Utama |
|-----------|----------------|
| **Standard K-Means** | `K=6`, `init=k-means++`, `n_init=30`, `max_iter=25` |
| **K-Means QLDE** | `K=6`, `pop_size=30`, `max_iter=60`, `F_init=0.7`, `Cr=0.9`, `mu=3.9` |
| **K-Means DE** | `K=6`, `pop_size=30`, `max_iter=60`, `F=0.5` (konstan), `Cr=0.7`, `mu=3.7` |
| **K-Means PSO** | `K=6`, `pop_size=30`, `max_iter=100` |
| **K-Means EOA** | `K=6`, `pop_size=30`, `max_iter=100` |

---

## 🚀 Cara Menjalankan (How to Run)

### 1. Instalasi Dependensi

```bash
pip install -r requirements.txt
```

### 2. Tahap 1 — EDA & Preprocessing

```
Jalankan: notebooks/01_eda_and_preprocessing.ipynb
Output  : data/processed/customer_features_{raw,scaled,pca}.csv
          models/pca_model.pkl, models/power_transformer.pkl, models/standard_scaler.pkl
```

### 3. Tahap 2 — Unsupervised Learning (Clustering)

Jalankan notebook berikut secara **berurutan atau terpisah** (masing-masing berdiri sendiri):

| Notebook | Algoritma | Output Label |
|----------|-----------|-------------|
| `02.0_unsupervised_learning_method.ipynb` | K-Means QLDE | `hasildata_kmeans-qlde.csv` |
| `02.1_kmeans_standard.ipynb` | Standard K-Means | `hasildata_kmeans-standard.csv` |
| `02.2_kmeans_de.ipynb` | K-Means DE | `hasildata_kmeans-de.csv` |
| `02.3_kmeans_pso.ipynb` | K-Means PSO | `hasildata_kmeans-pso.csv` |
| `02.4_kmeans_eoa.ipynb` | K-Means EOA | `hasildata_kmeans-eoa.csv` |

Setiap notebook menyimpan metrik (`.npy`) dan visualisasi (`.png`) ke folder `models/` secara otomatis.

### 4. Tahap 3 — Analisis Perbandingan Clustering

```
Jalankan: notebooks/03_comparison_analysis.ipynb
Prasyarat: Notebook 02.0–02.2 sudah dijalankan (file .npy tersedia di models/)
Output  : Tabel & grafik komparasi metrik, models/comparison_metrics.png
```

### 5. Tahap 4 — Supervised Learning (Klasifikasi)

Pastikan file berlabel sudah ada di `data/Labeled/`, lalu jalankan notebook berikut:

**Dataset Standard K-Means:**
```
04.1.1 → Decision Tree   (Business Rules)
04.1.2 → SVM (RBF)       (perlu StandardScaler)
04.1.3 → AdaBoost
04.1.4 → ANN / MLP       (perlu StandardScaler)
```

**Dataset K-Means QLDE:**
```
04.2.1 → Decision Tree
04.2.2 → SVM (RBF)
04.2.3 → AdaBoost
04.2.4 → ANN / MLP
```

**Dataset K-Means DE:**
```
04.3.1 → Decision Tree
04.3.2 → SVM (RBF)
04.3.3 → AdaBoost
04.3.4 → ANN / MLP
```

### 6. Tahap 5 — Komparasi Semua Model Klasifikasi

```
Jalankan: notebooks/04.4_classification_comparison.ipynb
Output  : Tabel perbandingan akurasi semua kombinasi algoritma × dataset
```

---

## 📦 Output Model (untuk Deployment)

Semua model dan scaler tersimpan di folder `models/`:

| File | Keterangan |
|------|-----------|
| `model_dt_classification_[metode].pkl` | Decision Tree classifier |
| `model_svm_classification_[metode].pkl` | SVM classifier |
| `model_adaboost_classification_[metode].pkl` | AdaBoost classifier |
| `model_ann_classification_[metode].pkl` | ANN/MLP classifier |
| `scaler_svm_[metode].pkl` | StandardScaler untuk SVM |
| `scaler_ann_[metode].pkl` | StandardScaler untuk ANN |
| `pca_model.pkl` | Model PCA (6 komponen) |
| `power_transformer.pkl` | PowerTransformer (preprocessing) |

> `[metode]` = `standard`, `qlde`, atau `de`

---

## 📚 Referensi

- Wang, G. (2025). *Customer segmentation in the digital marketing using a Q-learning based differential evolution algorithm integrated with K-means clustering.* PLoS ONE 20(2). [`references/Paper.pdf`](references/Paper.pdf)
- Al-kababchee et al. (2023). *Enhancement of K-means clustering in big data based on equilibrium optimizer algorithm.* [`references/Enhancement_of_K-means_clustering_in_big_data_base.pdf`](references/Enhancement_of_K-means_clustering_in_big_data_base.pdf)
- Mukhamediev et al. (2022). *Review of AI and ML Technologies.* Mathematics, 10(15). [`references/mathematics-10-02552-v2.pdf`](references/mathematics-10-02552-v2.pdf)
- Sarvari et al. (2016). *Performance evaluation of customer segmentation based on RFM.* [`references/Peiman_Emerald.pdf`](references/Peiman_Emerald.pdf)
