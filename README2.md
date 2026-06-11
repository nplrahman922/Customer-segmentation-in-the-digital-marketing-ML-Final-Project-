# Panduan Eksekusi Analisis Perbandingan Algoritma K-means

Dokumen ini menjelaskan urutan langkah-langkah untuk menjalankan dan membandingkan performa model **K-means-QLDE** dengan berbagai algoritma metaheuristik pembanding lainnya.

## Struktur Notebook Baru

Berikut adalah notebook yang telah disiapkan untuk proses perbandingan:

- `02_unsupervised_learning_method.ipynb`: Implementasi algoritma utama (K-means QLDE).
- `02.1_kmeans_standard.ipynb`: K-means Standar (Baseline).
- `02.2_kmeans_de.ipynb`: K-means dengan Differential Evolution (F Konstan, tanpa Q-learning).
- `02.3_kmeans_pso.ipynb`: K-means dengan Particle Swarm Optimization (PSO).
- `02.4_kmeans_eoa.ipynb`: K-means dengan Equilibrium Optimizer Algorithm (EOA).
- `03_comparison_analysis.ipynb`: Notebook untuk memuat, menganalisis, dan memvisualisasikan hasil perbandingan secara rapi.

---

## Urutan Eksekusi (How to Run)

Ikuti langkah-langkah di bawah ini secara berurutan untuk mendapatkan hasil perbandingan yang komprehensif.

### Langkah 1: Jalankan Notebook Utama (QLDE)
1. Buka dan jalankan semua cell di `notebooks/02_unsupervised_learning_method.ipynb`.
2. **PENTING**: Anda **harus** menjalankan script penyimpan hasil di bagian paling akhir notebook tersebut. Script tersebut akan menyimpan metrik (seperti `qlde_inertia.npy`, `qlde_silhouette.npy`, dll) ke folder `models/` agar bisa diakses oleh notebook perbandingan.
   *(Lihat `notebooks/save_qlde_results.py` jika Anda butuh kode cell untuk menyimpannya).*

### Langkah 2: Jalankan Algoritma Pembanding (02.1 - 02.4)
Anda dapat menjalankan notebook-notebook ini dalam urutan apa saja. Semua notebook ini akan secara otomatis:
1. Menjalankan proses clustering.
2. Menyimpan data hasil prediksi label ke folder `data/Labeled/` dengan nama file `hasildata-[nama-algoritma].csv`.
3. Menyimpan skor evaluasi (SSE, Silhouette, dll) ke dalam folder `models/`.

Jalankan semua cell pada notebook berikut:
- Buka dan *Run All* pada `notebooks/02.1_kmeans_standard.ipynb`
- Buka dan *Run All* pada `notebooks/02.2_kmeans_de.ipynb`
- Buka dan *Run All* pada `notebooks/02.3_kmeans_pso.ipynb`
- Buka dan *Run All* pada `notebooks/02.4_kmeans_eoa.ipynb`

### Langkah 3: Analisis Perbandingan
Setelah **SEMUA** notebook (02, 02.1, 02.2, 02.3, 02.4) berhasil dieksekusi:
1. Buka `notebooks/03_comparison_analysis.ipynb`.
2. *Run All* cell pada notebook ini.
3. Notebook ini akan memuat seluruh hasil metrik dari folder `models/` dan akan menghasilkan:
   - Tabel ringkasan komparasi performa semua algoritma (Highlight hijau untuk nilai terbaik).
   - Grafik Bar perbandingan metrik (SSE, Silhouette Score, Davies-Bouldin, dan Calinski-Harabasz).
   - Grafik Kurva Konvergensi (Convergence Curve) yang menggabungkan seluruh algoritma metaheuristik.

---

## Output yang Dihasilkan
Setelah semua langkah selesai, Anda akan mendapati struktur file hasil sebagai berikut:

**Hasil Labeled Data (`data/Labeled/`):**
- `hasildata-kmeans-qlde.csv` *(Harus disave/direname manual/lewat cell save)*
- `hasildata-kmeans-standard.csv`
- `hasildata-kmeans-de.csv`
- `hasildata-kmeans-pso.csv`
- `hasildata-kmeans-eoa.csv`

**Hasil Plot Visualisasi dan Metrics (`models/`):**
- `comparison_metrics.png` (Grafik bar perbandingan 4 metrik)
- `comparison_convergence.png` (Grafik kurva optimasi/konvergensi)
- Berbagai file `.npy` yang berisi skor metrik masing-masing algoritma.
