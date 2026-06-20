# Customer-segmentation-in-the-digital-marketing-ML-Final-Project-

=======
# Data Engineering & Preprocessing Pipeline
**Proyek Analisis Perilaku Pelanggan E-Commerce - Kelompok 6**

## Deskripsi
Modul ini bertanggung jawab atas persiapan data mentah menjadi data yang siap dimodelkan. Proses mencakup pembersihan data, ekstraksi fitur perilaku pelanggan (Feature Engineering), dan penyaringan anomali (Noise Filtering) menggunakan pendekatan *density-based* untuk menjaga integritas klaster utama.

## Alur Kerja (Pipeline)
1. **Data Cleaning**: 
   - Mengeliminasi *missing values* pada entitas `CustomerID`.
   - Menghapus anomali kuantitas negatif/nol dan riwayat transaksi batal.
2. **Feature Engineering**: 
   - Agregasi data dari level transaksi menjadi level pelanggan.
   - Ekstraksi fitur utama: Recency, Frequency, Monetary (RFM).
   - Ekstraksi fitur tambahan: `AvgSpending` (Rata-rata pengeluaran) dan `UniqueProducts` (Keragaman jenis produk).
3. **Standarisasi**: 
   - Normalisasi skala fitur menggunakan `StandardScaler` untuk mengoptimalkan perhitungan jarak euclidian.
4. **Noise Filtering (DBSCAN)**: 
   - Menggunakan algoritma DBSCAN untuk mendeteksi *outliers* / *noise* ekstrem (pelanggan klaster `-1`).
   - Memisahkan data *noise* agar tidak mendistorsi pembentukan *centroid* pada tahap pemodelan K-Means selanjutnya.

## Struktur Data Output
Modul ini menghasilkan dua dataset terpisah:
- `clean_customer_features.csv`: Dataset *inliers* (4.303 pelanggan) yang bersih dan terstandarisasi. Siap dieksekusi oleh *Unsupervised Learning Specialist*.
- `anomalous_customers.csv`: Dataset *outliers* (36 pelanggan anomali) untuk keperluan analisis terpisah.

## Reproduksibilitas
Untuk menjalankan pipeline ini secara lokal, pastikan *environment* telah terinstal dependensi berikut:
```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```
>>>>>>> origin/main
