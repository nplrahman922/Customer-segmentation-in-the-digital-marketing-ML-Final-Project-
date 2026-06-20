# Panduan Eksekusi Analisis dan Klasifikasi Pelanggan (Supervised Learning)

Dokumen ini menjelaskan urutan langkah-langkah untuk mengeksekusi model klasifikasi *Supervised Learning*. Tahap ini bertujuan untuk mengevaluasi hasil pelabelan dari tahap *Unsupervised Learning*, mengekstrak Aturan Bisnis (*Business Rules*), dan membangun "mesin penebak" (model prediktif) terbaik yang siap diintegrasikan ke sistem Web UI.

Dalam fase ini, kita menggunakan **4 Algoritma Klasifikasi** untuk membedah pola pelanggan:
1. **Decision Tree (DT):** Model *White-Box* untuk mengekstrak Aturan Bisnis berbasis logika *If-Else*.
2. **Support Vector Machine (KSVM):** Model *Black-Box* untuk mencari batas *hyperplane* optimal dengan keseimbangan performa.
3. **AdaBoost:** Model kolaboratif (*Ensemble Learning*) yang tangguh dan praktis tanpa memerlukan *Scaling*.
4. **Artificial Neural Network (ANN):** Model "otak tiruan" (*Multi-Layer Perceptron*) untuk meraih tingkat akurasi maksimal.

---

## Ringkasan Hasil Evaluasi Model
Berdasarkan hasil pengujian secara menyeluruh menggunakan metode *Cross-Validation* (CV=5) pada 100% data, berikut adalah tabel komparasi performa dari keempat algoritma terhadap masing-masing dataset *clustering*:

| Dataset (Sumber Label) | Algoritma Klasifikasi | Validasi Mean Akurasi (%) | Test Set Akurasi (%) | Waktu Komputasi (detik) |
| :--- | :--- | :---: | :---: | :---: |
| **STANDARD (Baseline)**| Decision Tree (DT) | 84.75 | 83.39 | 0.0475 |
| | AdaBoost | 85.67 | 87.31 | 0.7427 |
| | Kernel SVM (KSVM) | 89.70 | 88.93 | 0.1904 |
| | Artificial Neural Network | 96.86 | **97.92** | 13.7391 |
| **QLDE (Paper)** | Decision Tree (DT) | 82.76 | 82.58 | 0.0562 |
| | Kernel SVM (KSVM) | 86.90 | 86.04 | 0.2094 |
| | AdaBoost | 87.74 | 87.77 | 0.7650 |
| | Artificial Neural Network | 96.48 | **96.77** | 14.0172 |
| **DE** | Decision Tree (DT) | 85.06 | 84.78 | 0.0439 |
| | AdaBoost | 86.25 | 85.35 | 0.7395 |
| | Kernel SVM (KSVM) | 90.30 | 89.97 | 0.1922 |
| | Artificial Neural Network | 96.71 | **97.12** | 14.0380 |

**Kesimpulan Analisis:**
* **Jawara Akurasi:** Algoritma **ANN** mendominasi secara mutlak dengan akurasi pengujian tertinggi (hingga **~98%**) pada seluruh skenario dataset. Model ini sangat direkomendasikan jika tujuan utamanya adalah ketepatan prediksi pelanggan.
* **Keseimbangan Performa (Akurasi & Waktu):** **Kernel SVM** membuktikan dirinya sebagai model yang sangat stabil dengan akurasi tinggi (~90%) dan kecepatan eksekusi yang luar biasa cepat (hanya ~0.2 detik).
* **Interpretasi Aturan:** Meskipun akurasinya paling dasar (~84%), **Decision Tree** tetap sangat esensial karena hanya model ini yang mampu memberikan teks Aturan Bisnis (Business Rules) yang logis dan dapat dibaca manusia.

---

## Struktur Notebook

Proyek ini telah dikelompokkan ke dalam 13 file Jupyter Notebook yang terstruktur berdasarkan dataset hasil *clustering*:

**Dataset Standard K-Means:**
* `04.1.1_classification_decision_tree_standard.ipynb`
* `04.1.2_classification_svm_standard.ipynb`
* `04.1.3_classification_adaboost_standard.ipynb`
* `04.1.4_classification_ann_standard.ipynb`

**Dataset K-Means QLDE:**
* `04.2.1_classification_decision_tree_qlde.ipynb`
* `04.2.2_classification_svm_qlde.ipynb`
* `04.2.3_classification_adaboost_qlde.ipynb`
* `04.2.4_classification_ann_qlde.ipynb`

**Dataset K-Means DE:**
* `04.3.1_classification_decision_tree_de.ipynb`
* `04.3.2_classification_svm_de.ipynb`
* `04.3.3_classification_adaboost_de.ipynb`
* `04.3.4_classification_ann_de.ipynb`

**Evaluasi Keseluruhan:**
* `04.4_classification_comparison.ipynb`

---

## Urutan Eksekusi (How to Run)

Ikuti langkah-langkah di bawah ini secara berurutan untuk melatih model dan menghasilkan file `.pkl` (Model & Scaler) yang akan digunakan pada tahap *Deployment*.

### Langkah 1: Persiapan Dataset
Pastikan proses dari tahap *Unsupervised Learning* telah selesai dan dataset telah diberi label. File-file berikut WAJIB berada di dalam direktori `data/Labeled/`:
* `hasildata_kmeans-standard.csv`
* `hasildata_kmeans-qlde.csv`
* `hasildata_kmeans-de.csv`

### Langkah 2: Ekstraksi Aturan Bisnis (Decision Tree)
1. Eksekusi notebook `04.x.1` untuk setiap dataset.
2. Notebook ini akan mencetak *Classification Report* dan teks **Business Rules** yang siap diserahkan kepada tim *Marketing*.
3. Model akan diekspor otomatis ke direktori `models/`.

### Langkah 3: Optimasi Akurasi (SVM, AdaBoost, ANN)
1. Eksekusi notebook `04.x.2` (SVM), `04.x.3` (AdaBoost), dan `04.x.4` (ANN) untuk setiap dataset.
2. **PENTING (Khusus SVM & ANN):** Sebelum proses *training*, model ini membutuhkan penyetaraan metrik angka. Skrip akan secara otomatis menggunakan `StandardScaler`.

### Langkah 4: Analisis Perbandingan (*Comparison*)
1. Buka dan jalankan *Run All* pada file `04.4_classification_comparison.ipynb`.
2. Sistem akan secara otomatis melatih ulang (*retrain*) semua kombinasi algoritma dan dataset, melakukan Validasi Silang (*Cross-Validation*), dan menghitung waktu komputasi.
3. Hasil eksekusi akan menampilkan tabel metrik lengkap dan **Grafik Bar** perbandingan akurasi.

---

## Output yang Dihasilkan

Setelah semua langkah dieksekusi, berbagai keluaran siap digunakan untuk proses integrasi (*Deployment*).

**Laporan & Aturan (Tercetak di Notebook):**
* *Classification Report* (Akurasi, Precision, Recall, F1-Score).
* Teks Aturan Bisnis (*Business Rules*) untuk setiap bentuk sebaran data.
* Visualisasi grafik komparasi performa model.

**File Model untuk Deployment (dalam folder `models/`):**
Sistem akan menggenerasi berbagai versi file `.pkl` sesuai dengan algoritma dan datasetnya, misalnya:
* **Penebak:** `model_dt_classification_[metode].pkl`, `model_svm_classification_[metode].pkl`, `model_adaboost_classification_[metode].pkl`, dan `model_ann_classification_[metode].pkl`.
* **Scaler:** `scaler_svm_[metode].pkl` dan `scaler_ann_[metode].pkl` (wajib dipanggil untuk menyetarakan angka sebelum data pelanggan baru ditebak oleh mesin SVM atau ANN).