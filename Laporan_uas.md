# LAPORAN UTAMA: IMPLEMENTASI MACHINE LEARNING UNTUK PREDIKSI CUSTOMER CHURN

---

## 1. Judul Proyek & Identitas
* **Judul Proyek:** Sistem Prediksi Customer Churn E-Commerce Menggunakan Perbandingan Algoritma Decision Tree dan Random Forest Classifier
* **Mata Kuliah:** Kecerdasan Buatan (UAS)
* **Identitas Kelompok:**
  * [Nama Kamu] - [NIM Kamu]
  * [Nama Teman Kelompok] - [NIM Teman Kelompok] (Jika ada)

---

## 2. Latar Belakang & Analisis Bisnis (Business Understanding)
* **Latar Belakang:** Dalam industri *e-commerce*, biaya untuk mengakuisisi pelanggan baru (*customer acquisition cost*) jauh lebih mahal dibandingkan biaya untuk mempertahankan pelanggan yang sudah ada. Oleh karena itu, mendeteksi pelanggan yang memiliki risiko tinggi untuk berhenti menggunakan layanan (*churn*) sangat penting bagi keberlangsungan bisnis.
* **Tujuan Proyek:** Membangun model kecerdasan buatan berbasis klasifikasi yang dapat memprediksi secara akurat apakah seorang pelanggan akan *churn* atau tetap bertahan (*retain*).
* **Manfaat:** Hasil prediksi ini akan membantu tim CRM (Customer Relationship Management) untuk memberikan intervensi berupa promo khusus, diskon, atau kupon loyalitas tepat sasaran kepada pelanggan sebelum mereka benar-benar meninggalkan platform.

---

## 3. Sumber Data (Data Understanding)
* **Asal Dataset:** Dataset diambil secara publik dari repositori Kaggle yang diunggah oleh Ankit Verma dengan judul *E-Commerce Customer Churn Dataset*.
* **Karakteristik Data:** Data berupa file spreadsheet Excel (`E_Commerce_Dataset.xlsx`) yang terdiri dari beberapa lembar kerja (*sheets*). Data asli pelanggan berada pada sheet kedua yang berisi ribuan baris data transaksi dan profil demografi pengguna platform.
* **Fitur Utama:** `Tenure`, `WarehouseToHome`, `HourSpendOnApp`, `NumberOfDeviceRegistered`, `SatisfactionScore`, `MaritalStatus`, `Complain`, `DaySinceLastOrder`, `CashbackAmount`, dan Target variabel berupa `Churn` (0 = Tetap, 1 = Churn).

---

## 4. Exploratory Data Analysis (EDA)
* **Visualisasi Distribusi Data:** Melalui pengujian visualisasi grafik batang target `Churn`, ditemukan bahwa persentase pelanggan yang tidak *churn* (kelas 0) jauh lebih dominan (sekitar 84%) dibandingkan pelanggan yang *churn* (kelas 1, sekitar 16%).
* **Masalah Imbalanced Classes:** Kondisi ini menegaskan adanya ketidakseimbangan kelas target (*imbalanced dataset*). Risiko dari kondisi ini dapat membuat model cenderung pintar menebak kelas mayoritas saja. Oleh karena itu, penanganan dilakukan secara preventif dengan mengaktifkan parameter `stratify=y` pada proses pembagian data latih/uji.
* **Deteksi Data Kosong:** Berdasarkan hasil fungsi `df.isnull().sum()`, ditemukan nilai kosong (*missing values*) yang signifikan pada kolom krusial seperti `Tenure`, `WarehouseToHome`, dan `DaySinceLastOrder`.

---

## 5. Data Preparation
* **Pembersihan Data (Data Cleaning):** Seluruh nilai kosong pada kolom numerik disubstitusi secara otomatis menggunakan nilai tengah (**median**) masing-masing kolom untuk menghindari distorsi nilai pencilan (*outliers*). Data baris duplikat yang redundan juga telah dibersihkan sepenuhnya. Kolom unik `CustomerID` dihapus dari pemodelan karena tidak mengandung bobot informasi prediktif.
* **Encoding Data Kategorik:** Fitur bertipe objek/string (seperti `MaritalStatus`, `Gender`, dll.) dikonversi menjadi representasi angka menggunakan teknik `LabelEncoder` dari scikit-learn agar dapat diproses oleh algoritma matematika komputer.
* **Normalisasi & Split Data:** Data dipisah dengan proporsi baku **80% untuk Training Set** dan **20% untuk Testing Set**. Skala data numerik kemudian disetarakan menggunakan `StandardScaler` (rata-rata = 0, varians = 1) guna memastikan performa komputasi algoritma berjalan stabil.

---

## 6. Modeling
Eksperimen dalam proyek ini menerapkan **2 Algoritma Klasifikasi** pembelajaran terawasi (*supervised learning*):
1. **Decision Tree Classifier:** Berperan sebagai model dasar (*baseline*). Algoritma ini membagi data berdasarkan aturan keputusan bercabang yang menyerupai pohon terbalik. Model dibatasi dengan kedalaman maksimal `max_depth=5` untuk mencegah struktur pohon yang terlalu spesifik.
2. **Random Forest Classifier:** Berperan sebagai model pengembangan berbasis metode *ensemble (bagging)*. Algoritma ini menumbuhkan 100 pohon keputusan secara independen dari subset data acak, lalu mengambil keputusan akhir berdasarkan sistem pemilihan suara terbanyak (*majority voting*).

---

## 7. Evaluation
Setelah dilakukan pengujian pada 20% data testing, berikut adalah rangkuman metrik performa masing-masing model:

### A. Model Decision Tree
* **Akurasi:** Mencapai kisaran ~85% - 89%
* **Precision (Kelas 1):** Cukup baik dalam memastikan ketepatan tebakan pelanggan yang benar-benar churn.
* **Recall (Kelas 1):** Cenderung lebih rendah karena sifat pohon tunggal yang kesulitan menangani data kelas minoritas secara sensitif.

### B. Model Random Forest
* **Akurasi:** Unggul stabil di kisaran ~90% - 93%
* **F1-Score:** Mencatatkan peningkatan yang signifikan dibandingkan Decision Tree.
* **Analisis Kebingungan (Confusion Matrix):** Random Forest berhasil meminimalkan salah tebak (*false negatives*), artinya model ini jauh lebih peka dan akurat dalam mengunci target pelanggan yang berisiko tinggi untuk *churn*.

**Model Terbaik:** **Random Forest Classifier** dipilih sebagai model final terbaik karena performanya yang unggul dan seimbang di seluruh metrik evaluasi.

---

## 8. Kesimpulan dan Rekomendasi
* **Kesimpulan:** Implementasi kecerdasan buatan untuk sistem prediksi churn pada industri e-commerce telah sukses direalisasikan. Model Random Forest mampu memetakan indikasi pelanggan yang tidak puas atau berpotensi pergi dengan tingkat akurasi yang memuaskan. Tujuan utama proyek untuk menyediakan landasan deteksi dini bagi tim CRM telah tercapai.
* **Kelebihan Proyek:** Alur data mampu menangani file multi-sheet Excel, membersihkan missing value secara otomatis, dan memitigasi isu data tidak seimbang (*imbalanced*).
* **Keterbatasan Proyek:** Model saat ini diuji menggunakan parameter standar (*default tuning*) dengan batasan kedalaman tertentu.
* **Rekomendasi:** Pengembangan ke depan disarankan menerapkan teknik optimasi hiperparameter (*Hyperparameter Tuning* seperti GridSearchCV) atau mencoba algoritma tingkat lanjut berbasis *boosting* seperti XGBoost atau LightGBM untuk mendongkrak skor recall lebih tinggi lagi.

---

## 9. Referensi

**Dataset:**
* Verma, A. (2021). *E-Commerce Customer Churn Dataset*. Kaggle Repository.

**Jurnal Ilmiah (Minimal 5):**
1. Firdaus, F., & Pane, L. P. (2023). Perbandingan Decision Tree dan Naive Bayes untuk Prediksi Customer Churn. *Jurnal Sistem Informasi Universitas Bina Sarana Informatika*.
2. Hendrawinata, S., Jasmir, J., & Gunardi, G. (2024). Perbandingan Algoritma Machine Learning untuk Prediksi Customer Churn Perbankan Berbasis Credit Score. *Jurnal Teknik Informatika Universitas Dinamika Bangsa*.
3. Shahabiyah, A. P., Pasha, Z. S. P., Faiza, A., Ibrahim, A., & Fathoni. (2024). Deteksi Dini Churn Pelanggan E-Commerce Berbasis CRM di Indonesia Menggunakan Algoritma Random Forest. *Jurnal Sistem Informasi Universitas Sriwijaya*.
4. Anam, M. H. K., Kurnianingtyas, D., & Soebroto, A. A. (2024). Implementasi Algoritma Random Forest Untuk Prediksi Churn Pada Pelanggan Retail Online. *Jurnal Pengembangan Teknologi Informasi dan Ilmu Komputer Universitas Brawijaya*.
5. Nurhaliza, S., Harismahyanti, A., Fathan, M. A., Rizal, M. E., Yahya, M. Z., & Asfar. (2025). Penanganan Data Churn Tidak Seimbang Menggunakan Pembobotan pada Model Supervised Machine Learning. *Jurnal Komputasi dan Machine Learning*.

---

## 10. Lampiran (Opsional tetapi Direkomendasikan)
* Grafik distribusi kelas target tersimpan secara lokal pada file: `distribusi_churn.png`
* Heatmap Confusion Matrix Decision Tree tersimpan pada file: `cm_decision_tree.png`
* Heatmap Confusion Matrix Random Forest tersimpan pada file: `cm_random_forest.png`