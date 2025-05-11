# Sistem Deteksi Klaim Asuransi Perjalanan

## Ikhtisar Proyek
Proyek ini mengembangkan model pembelajaran mesin untuk memprediksi keabsahan klaim asuransi perjalanan, memungkinkan agensi asuransi untuk membedakan klaim valid dan tidak valid secara efisien. Sistem ini bertujuan untuk mengurangi upaya verifikasi manual, meminimalkan pembayaran klaim yang salah, dan meningkatkan efisiensi operasional. Dataset yang digunakan berisi data historis pemegang polis, termasuk fitur seperti destinasi, jenis produk, durasi perjalanan, dan status klaim.

## Pernyataan Masalah
Tantangan utama adalah kesulitan membedakan klaim asuransi perjalanan yang valid dan tidak valid, yang menyebabkan inefisiensi dan potensi kerugian finansial. Proyek ini menjawab pertanyaan analitis berikut:
- Fitur apa yang paling memengaruhi status klaim?
- Apakah durasi perjalanan atau destinasi tertentu meningkatkan kemungkinan klaim?
- Bagaimana faktor demografis (misalnya, usia, jenis kelamin) memengaruhi keabsahan klaim?
- Apa dampak jenis produk asuransi atau kanal distribusi terhadap klaim?
- Apakah ada pola klaim berdasarkan nilai penjualan polis atau komisi agensi?

## Dataset
- **Sumber**: [Dataset Asuransi Perjalanan](https://drive.google.com/drive/folders/1iVx5k6tWglqfHb05o0DElg8JHg7VVG_J)
- **Deskripsi**: Dataset berisi data historis pemegang polis asuransi perjalanan dengan fitur seperti:
  - `Agency`: Nama agensi yang menjual asuransi.
  - `Agency Type`: Jenis agensi (misalnya, Maskapai Penerbangan, Agensi Perjalanan).
  - `Distribution Channel`: Saluran penjualan (misalnya, Online, Offline).
  - `Product Name`: Nama produk asuransi.
  - `Gender`: Jenis kelamin pemegang polis.
  - `Duration`: Durasi perjalanan.
  - `Destination`: Tujuan perjalanan.
  - `Net Sales`: Jumlah penjualan polis.
  - `Commision (in value)`: Komisi agensi.
  - `Age`: Usia pemegang polis.
  - `Claim`: Variabel target (Ya/Tidak) yang menunjukkan status klaim.
- **Wawasan Kunci**: Dataset tidak seimbang, dengan ~98,47% klaim "Tidak" dan ~1,53% klaim "Ya".

## Struktur Proyek
- **Notebook**: `Capstone3_InsuranceTravel_Dinmar v1.2.ipynb` berisi analisis lengkap, pra-pemrosesan, pengembangan model, dan evaluasi.
- **Model**: Model terbaik (XGBoost) disimpan sebagai `xgbclassifier_best_model.pkl`.
- **Visualisasi**: Grafik batang yang membandingkan performa model sebelum dan sesudah tuning disimpan sebagai `comparison_metrics_bar_chart.png`.
- **Data**: Dataset dimuat dari `data_travel_insurance.csv`.

## Metodologi
1. **Pra-pemrosesan Data**:
   - Menangani nilai hilang (misalnya, ~71% data `Gender` hilang) menggunakan teknik imputasi.
   - Mengkodekan variabel kategorikal (misalnya, `Agency`, `Destination`) menggunakan OneHotEncoder dan BinaryEncoder.
   - Menskalakan fitur numerik (`Duration`, `Net Sales`, `Commision`, `Age`) menggunakan StandardScaler.
   - Mengatasi ketidakseimbangan kelas menggunakan teknik seperti SMOTE dalam pipeline.

2. **Pemodelan**:
   - Model yang diuji: Regresi Logistik, Pohon Keputusan, Random Forest, Gradient Boosting, XGBoost, dan metode ensemble (Voting, Stacking).
   - Penyetelan hiperparameter dilakukan menggunakan RandomizedSearchCV dan GridSearchCV.
   - Model terbaik: XGBoost yang telah disetel dengan recall meningkat (38,31%) dan ROC AUC (76,98%).

3. **Metrik Evaluasi**:
   - Metrik utama: Akurasi, Recall, Presisi, Skor F1, Akurasi Seimbang, G-Mean, ROC AUC, PR AUC.
   - Peningkatan setelah tuning:
     - Recall: 28,03% → 38,31% (deteksi klaim valid lebih baik).
     - ROC AUC: 75,70% → 76,98%.
     - PR AUC: 9,07% → 11,26%.
     - Presisi tetap rendah (9,23%), menunjukkan adanya false positives.

## Instalasi
1. Klon repositori:
   ```bash
   git clone <url_repositori>
   cd sistem-deteksi-klaim-asuransi-perjalanan
   ```
2. Instal dependensi:
   ```bash
   pip install -r requirements.txt
   ```
3. Pastikan dataset (`data_travel_insurance.csv`) ditempatkan di direktori proyek.

## Penggunaan
1. Buka Jupyter Notebook:
   ```bash
   jupyter notebook Capstone3_InsuranceTravel_Dinmar\ v1.2.ipynb
   ```
2. Jalankan sel secara berurutan untuk:
   - Memuat dan memproses data.
   - Melatih dan mengevaluasi model.
   - Memvisualisasikan metrik performa.
3. Gunakan model yang disimpan (`xgbclassifier_best_model.pkl`) untuk prediksi:
   ```python
   import pickle
   import pandas as pd

   # Muat model
   with open('xgbclassifier_best_model.pkl', 'rb') as file:
       model = pickle.load(file)

   # Siapkan data (pastikan sesuai format yang telah diproses)
   data = pd.read_csv('data_anda.csv')
   prediksi = model.predict(data)
   ```

## Kesimpulan Bisnis
1. **Efisiensi Meningkat**: Model mengurangi verifikasi manual dengan otomatisasi validasi awal klaim, dengan recall 38,31% untuk klaim valid.
2. **Fitur Kunci**: Durasi, Net Sales, Komisi, Destinasi, dan Nama Produk sangat memengaruhi klaim, memungkinkan penetapan harga berbasis risiko.
3. **Ketidakseimbangan Kelas**: Deteksi kelas minoritas ("Ya") meningkat, meskipun presisi perlu disetel untuk mengurangi false positives.
4. **Segmentasi Pelanggan**: Wawasan demografis dan produk memungkinkan penawaran asuransi yang disesuaikan.
5. **Dampak Finansial**: Deteksi klaim yang akurat meminimalkan pembayaran klaim tidak valid, menghemat biaya dan menjaga kepercayaan pelanggan.
6. **Keunggulan Kompetitif**: Proses klaim yang cepat dan transparan memperkuat posisi pasar.

## Rekomendasi Bisnis
### Jangka Pendek
1. **Integrasikan Model**: Terapkan model XGBoost untuk prediksi klaim real-time dalam lingkungan uji coba.
2. **Sesuaikan Ambang Batas**: Tingkatkan ambang prediksi (misalnya, ke 0,7) untuk meningkatkan presisi dan mengurangi false positives.
3. **Tingkatkan Pengumpulan Data**: Atasi data `Gender` yang hilang melalui formulir yang lebih lengkap atau insentif pelanggan.
4. **Fokus pada Segmen Berisiko Tinggi**: Prioritaskan destinasi (misalnya, AS, Singapura) dan produk (misalnya, 2-way Comprehensive Plan) dengan tingkat klaim tinggi.

### Jangka Panjang
1. **Sempurnakan Penetapan Harga**: Gunakan kepentingan fitur (misalnya, Durasi, Usia) untuk menyesuaikan premi polis berisiko tinggi.
2. **Pemeliharaan Model**: Latih ulang model setiap 3–6 bulan dengan data baru untuk menjaga akurasi.
3. **Kembangkan Produk Khusus**: Ciptakan asuransi khusus untuk segmen berisiko tinggi (misalnya, pelancong lansia, perjalanan panjang).
4. **Deteksi Penipuan**: Bangun model deteksi penipuan untuk mengidentifikasi klaim mencurigakan.
5. **Optimalkan Pemasaran**: Manfaatkan dominasi kanal online untuk kampanye digital yang ditargetkan.

## Peningkatan di Masa Depan
- Tingkatkan presisi dengan teknik resampling lanjutan atau pembelajaran sensitif biaya.
- Kumpulkan lebih banyak data untuk mengurangi nilai hilang dan meningkatkan ketahanan model.
- Jelajahi model pembelajaran mendalam untuk potensi peningkatan performa dengan dataset besar.
- Integrasikan fitur deteksi penipuan untuk mengurangi risiko finansial lebih lanjut.

## Kontributor
- **Dinmar Pratama**: Analisis data, pengembangan model, dan analisis bisnis.
LinkedIn: [Dinmar Pratama](https://www.linkedin.com/in/dinmar-pratama-2516b8224/)
Medium: [Dinmar Pratama](https://medium.com/@dinmarpratama)

## Lisensi
Proyek ini dilisensikan di bawah Lisensi MIT. Lihat file `LICENSE` untuk detailnya.
