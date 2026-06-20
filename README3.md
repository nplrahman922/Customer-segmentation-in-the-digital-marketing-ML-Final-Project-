# Panduan Eksekusi Analisis dan Klasifikasi Pelanggan (Supervised Learning)

Dokumen ini menjelaskan urutan langkah-langkah untuk mengeksekusi model klasifikasi *Supervised Learning* menggunakan algoritma **Decision Tree** dan **Support Vector Machine (SVM)**. Tahap ini bertujuan untuk mengekstrak Aturan Bisnis (*Business Rules*) dan memprediksi segmentasi pelanggan berdasarkan label terbaik dari tahap *Unsupervised Learning* sebelumnya.

## Struktur Notebook Baru

Berikut adalah notebook yang telah disiapkan untuk proses pelatihan dan perbandingan model klasifikasi:

- `04.1_classification_decision_tree.ipynb`: Implementasi algoritma Decision Tree untuk klasifikasi dan ekstraksi Aturan Bisnis.
- `04.2_classification_svm.ipynb`: Implementasi algoritma Support Vector Machine (SVM) dengan *Standard Scaler*.
- `04.3_classification_comparison.ipynb`: Notebook komparasi performa akurasi antara Decision Tree dan SVM menggunakan label dari seluruh algoritma *clustering*.

---

## Urutan Eksekusi (How to Run)

Ikuti langkah-langkah di bawah ini secara berurutan untuk melatih model dan menghasilkan file `.pkl` yang akan digunakan pada tahap *Deployment* (Web UI).

### Langkah 1: Persiapan Dataset
Pastikan seluruh proses dari tahap *Unsupervised Learning* (Anggota 2) telah selesai. Proses klasifikasi ini sangat bergantung pada ketersediaan data berikut di dalam folder `data/Labeled/` (atau direktori tempat penyimpanan data hasil *clustering*):
- `hasildata-kmeans-standard.csv` (Dataset utama)
- `hasildata-kmeans-de.csv`
- `hasildata-kmeans-pso.csv`
- `hasildata-kmeans-eoa.csv`

### Langkah 2: Ekstraksi Aturan Bisnis dengan Decision Tree
1. Buka dan jalankan semua *cell* di `notebooks/04.1_classification_decision_tree.ipynb`.
2. Notebook ini akan menggunakan `hasildata-kmeans-standard.csv` (metode terbaik) untuk melatih model.
3. Notebook akan mencetak teks evaluasi performa dan logika **Business Rules (If-Else)**.
4. Di bagian akhir, notebook akan secara otomatis mengekspor model ke `models/model_dt_classification.pkl`.

### Langkah 3: Optimasi Akurasi dengan SVM
1. Buka dan jalankan semua *cell* di `notebooks/04.2_classification_svm.ipynb`.
2. Notebook ini juga menggunakan `hasildata-kmeans-standard.csv`.
3. **PENTING:** Untuk mencegah kendala komputasi dan *processing delays*, data latih (*train size*) untuk SVM dibatasi hingga maksimal **2.000 sampel**.
4. Di bagian akhir, notebook akan menyimpan *scaler* dan model ke `models/scaler_svm.pkl` dan `models/model_svm_classification.pkl`.

### Langkah 4: Analisis Perbandingan Klasifikasi
1. Buka dan *Run All* *cell* pada `notebooks/04.3_classification_comparison.ipynb`.
2. Notebook ini akan memuat seluruh dataset hasil *clustering* dan secara otomatis mengujinya pada model Decision Tree dan SVM.
3. Hasil eksekusi akan menampilkan **Grafik Bar** perbandingan akurasi klasifikasi untuk mengevaluasi efektivitas setiap metode pelabelan.

---

## Output yang Dihasilkan
Setelah seluruh langkah di atas selesai dieksekusi, Anda akan memperoleh berbagai keluaran (*output*) berikut yang siap diserahkan kepada Anggota 4 untuk proses integrasi ke dalam sistem Web UI.

**Laporan & Aturan (Tercetak di Notebook):**
- *Classification Report* (Akurasi, Precision, Recall, F1-Score).
- Teks Aturan Bisnis (*Business Rules*) dari model Decision Tree.

**File Model untuk Deployment (`models/`):**
- `model_dt_classification.pkl` (Model prediksi Decision Tree)
- `scaler_svm.pkl` (Alat standarisasi data yang wajib digunakan sebelum memanggil prediksi SVM)
- `model_svm_classification.pkl` (Model prediksi SVM)