# UTS — Klasifikasi Kesuburan Tanah Menggunakan KNN di Orange Data Mining

Laporan ini merangkum proses analisis data untuk memprediksi tingkat kesuburan tanah menggunakan algoritma **K-Nearest Neighbors (KNN)** di perangkat lunak **Orange Data Mining**. Variabel target yang diprediksi adalah label **"Subur"** atau **"Tidak Subur"** berdasarkan parameter kimia dan fisik tanah: pH, N Total, P Tersedia, K Tersedia, C Organik, dan Tekstur Tanah.

---

## 1. Pendahuluan

Klasifikasi kesuburan tanah merupakan salah satu penerapan *machine learning* di bidang pertanian. Dengan mengenali pola dari parameter-parameter tanah, sebuah model dapat secara otomatis mengkategorikan apakah suatu sampel tanah tergolong subur atau tidak. Pada praktikum ini digunakan algoritma **K-Nearest Neighbors (KNN)** karena algoritma ini mudah dipahami secara intuitif dan efektif untuk dataset berukuran kecil-menengah.

Perangkat lunak yang digunakan adalah **Orange Data Mining** — sebuah platform *visual programming* berbasis Python yang memungkinkan analisis data dilakukan melalui antarmuka grafis tanpa perlu menulis kode secara eksplisit.

---

## Unduh Berkas 

Untuk mengunduh file dataset dan file *workflow* Orange Data Mining ini melalui link berikut:

- {download}`Dataset Kesuburan Tanah (CSV) <files/dataset_kesuburan_tanah_missing.csv>`
- {download}`File Workflow Orange UTS (OWS) <files/UTS_PENDATA.ows>`

---

## 2. Dataset

Dataset yang digunakan adalah `dataset_kesuburan_tanah_missing.xlsx` yang memuat sampel-sampel tanah dengan kolom sebagai berikut:

| Kolom         | Tipe Data   | Keterangan                                    |
|---------------|-------------|-----------------------------------------------|
| ID            | Meta        | Identifikasi unik sampel (tidak dipakai model) |
| pH            | Numerik     | Tingkat keasaman tanah                        |
| N Total       | Numerik     | Kandungan Nitrogen total                      |
| P Tersedia    | Numerik     | Kandungan Fosfor tersedia                     |
| K Tersedia    | Numerik     | Kandungan Kalium tersedia                     |
| C Organik     | Numerik     | Kandungan Karbon organik                      |
| Tekstur Tanah | Kategorikal | Jenis tekstur tanah (lempung, pasir, dll.)    |
| Label         | Target      | Kelas: **Subur** / **Tidak Subur**            |

Dataset ini mengandung **beberapa nilai yang hilang (missing values)** pada kolom N Total dan C Organik, sehingga diperlukan tahap *preprocessing* sebelum pelatihan model.

---

## 3. Alur Kerja (Workflow) Orange Data Mining

Seluruh analisis dilakukan dalam satu alur kerja visual di Orange yang terdiri dari **11 node**. Berikut adalah gambaran keseluruhan *workflow*:

```{figure} images/orange_actual_workflow.png
:alt: Diagram alur kerja lengkap Orange Data Mining
:align: center

**Gambar 1.** Alur kerja lengkap Orange Data Mining — dari pemuatan data CSV hingga evaluasi model KNN dan visualisasi.
```

Secara garis besar, alur kerja terbagi menjadi **tiga jalur**:

- **Jalur Utama (Preprocessing → Model):** CSV File Import → Data Table → Select Columns → Distributions → Box Plot → Impute → Continuize → Preprocess → kNN → Test and Score → Confusion Matrix
- **Jalur Visualisasi Eksplorasi:** Select Columns → Box Plot
- **Jalur Visualisasi Dimensi:** Preprocess → PCA → Scatter Plot

---

## 4. Penjelasan Node dan Setup

### 4.1 Node CSV File Import

Node **CSV File Import** adalah titik masuk data ke dalam alur kerja. Node ini bertugas membaca file dataset dari penyimpanan lokal.

```{figure} images/image_uts/csv_file_import.png
:alt: Tampilan node CSV File Import di Orange
:align: center

**Gambar 2.** Node CSV File Import — dialog pemilihan file dataset.
```

**Cara Setup:**

1. Dari panel *Widgets* di sebelah kiri, cari **File** atau **CSV File Import** di kategori *Data*.
2. Seret node ke kanvas (*canvas*) Orange.
3. Klik dua kali node tersebut untuk membuka dialog.
4. Klik tombol **Browse (...)** dan pilih file `dataset_kesuburan_tanah_missing.xlsx`.
5. Orange akan otomatis mendeteksi delimiter dan tipe data setiap kolom.
6. Periksa pratinjau data — pastikan semua kolom terbaca dengan benar.
7. Klik **OK**.

Output node ini berupa **Data** yang akan dialirkan ke node berikutnya.

---

### 4.2 Node Data Table

Node **Data Table** berfungsi menampilkan data dalam format tabel sehingga dapat diperiksa secara visual sebelum diproses lebih lanjut.

```{figure} images/image_uts/data_table.png
:alt: Tampilan node Data Table di Orange
:align: center

**Gambar 3.** Node Data Table — pratinjau seluruh isi dataset beserta nilai yang hilang.
```

**Cara Setup:**

1. Seret node **Data Table** dari panel *Widgets* kategori *Data* ke kanvas.
2. Hubungkan output **Data** dari node *CSV File Import* ke input node *Data Table* dengan menarik garis penghubung.
3. Klik dua kali untuk membuka tampilan tabel.
4. Di sini Anda dapat memeriksa isi data, termasuk melihat sel yang kosong (*missing values*) yang ditandai dengan warna berbeda atau tanda tanya.

:::{note}
Node Data Table juga memungkinkan pemilihan baris tertentu. Output **Selected Data** yang terhubung ke node berikutnya akan meneruskan data yang dipilih (atau seluruh data jika tidak ada seleksi).
:::

---

### 4.3 Node Select Columns

Node **Select Columns** berfungsi menentukan peran setiap kolom: mana yang menjadi **fitur**, **target**, atau **meta**.

```{figure} images/image_uts/select_column.png
:alt: Tampilan node Select Columns di Orange
:align: center

**Gambar 4.** Node Select Columns — pengaturan peran kolom (Feature, Target, Meta).
```

**Cara Setup:**

1. Seret node **Select Columns** dari panel *Widgets* kategori *Data*.
2. Hubungkan output **Data** dari *Data Table* ke input node ini.
3. Klik dua kali untuk membuka dialog pengaturan kolom.
4. Atur peran kolom sebagai berikut menggunakan drag-and-drop antar panel:

| Kolom         | Peran yang Diatur |
|---------------|-------------------|
| ID            | **Meta** — tidak digunakan dalam perhitungan model |
| pH            | **Features** — fitur input model |
| N Total       | **Features** — fitur input model |
| P Tersedia    | **Features** — fitur input model |
| K Tersedia    | **Features** — fitur input model |
| C Organik     | **Features** — fitur input model |
| Tekstur Tanah | **Features** — fitur input model |
| Label         | **Target** — variabel yang ingin diprediksi |

5. Klik **OK**.

:::{important}
Mengatur kolom **ID** sebagai *Meta* sangat penting agar nilai numerik ID tidak dianggap sebagai fitur oleh model, yang dapat menyebabkan hasil evaluasi yang tidak valid (*data leakage*).
:::

Dari node ini, data mengalir ke node *Distributions* untuk dianalisis lebih lanjut.

---

### 4.4 Node Box Plot *(Jalur Eksplorasi)*

Node **Box Plot** digunakan untuk memvisualisasikan distribusi dan sebaran nilai setiap fitur numerik, sekaligus mengidentifikasi keberadaan *outlier*.

```{figure} images/image_uts/box_plot.png
:alt: Tampilan node Box Plot di Orange
:align: center

**Gambar 5.** Node Box Plot — visualisasi sebaran dan outlier pada tiap fitur.
```

**Cara Setup:**

1. Seret node **Box Plot** dari panel *Widgets* kategori *Visualize*.
2. Hubungkan output **Data** dari *Distributions* ke input node *Box Plot*.
3. Klik dua kali untuk membuka visualisasi.
4. Pilih variabel yang ingin divisualisasikan dari dropdown *Variable*.
5. Aktifkan opsi **Order by Median** untuk memudahkan perbandingan antar kelas.

:::{note}
Dari node **Distributions**, data diteruskan ke **Box Plot** sebelum masuk ke tahap **Impute** untuk memastikan visualisasi outlier pada data asli terlihat jelas.
:::

---

### 4.5 Node Distributions *(Jalur Utama)*

Node **Distributions** menampilkan histogram distribusi frekuensi setiap fitur, membantu memahami bentuk sebaran data (normal, miring kiri/kanan, bimodal, dll.).

```{figure} images/image_uts/distributions.png
:alt: Tampilan node Distributions di Orange
:align: center

**Gambar 6.** Node Distributions — histogram distribusi frekuensi fitur-fitur tanah.
```

**Cara Setup:**

1. Seret node **Distributions** dari panel *Widgets* kategori *Visualize*.
2. Hubungkan output **Data** dari *Select Columns* ke input node *Distributions*.
3. Klik dua kali untuk membuka histogram.
4. Pilih fitur yang ingin diamati dari dropdown *Variable*.
5. Aktifkan **Show Continuous Distribution** untuk menampilkan kurva distribusi.

Output **Data** dari node ini diteruskan ke tahap *Box Plot*.

---

### 4.6 Node Impute

Node **Impute** menangani nilai-nilai yang hilang (*missing values*) dalam dataset agar tidak mengganggu proses pelatihan model.

```{figure} images/image_uts/impute.png
:alt: Tampilan node Impute di Orange
:align: center

**Gambar 7.** Node Impute — pengaturan metode pengisian nilai kosong (Average/Most Frequent).
```

**Cara Setup:**

1. Seret node **Impute** dari panel *Widgets* kategori *Preprocess*.
2. Hubungkan output **Data** dari *Box Plot* ke input node *Impute*.
3. Klik dua kali untuk membuka dialog pengaturan.
4. Pada bagian **Default Method**, pilih **Average / Most Frequent**:
   - Untuk kolom numerik (N Total, C Organik): nilai kosong diisi dengan **rata-rata** kolom.
   - Untuk kolom kategorikal: nilai kosong diisi dengan **nilai yang paling sering muncul**.
5. Klik **OK**.

:::{note}
Metode *Average* dipilih karena merupakan pilihan yang aman dan tidak memperkenalkan bias besar pada distribusi data. Nilai kosong pada N Total dan C Organik akan terisi secara otomatis.
:::

---

### 4.7 Node Continuize

Node **Continuize** mengubah variabel kategorikal (bertipe teks) menjadi representasi numerik agar dapat diproses oleh algoritma KNN yang berbasis jarak.

```{figure} images/image_uts/continuize.png
:alt: Tampilan node Continuize di Orange
:align: center

**Gambar 8.** Node Continuize — pengaturan One-Hot Encoding untuk variabel Tekstur Tanah.
```

**Cara Setup:**

1. Seret node **Continuize** dari panel *Widgets* kategori *Preprocess*.
2. Hubungkan output **Data** dari *Impute* ke input node *Continuize*.
3. Klik dua kali untuk membuka dialog pengaturan.
4. Pada kolom **Tekstur Tanah**, pilih metode **One-Hot Encoding** (atau *"First value as base"*).
5. Klik **OK**.

**Mengapa One-Hot Encoding?**
KNN menghitung jarak antar titik data menggunakan rumus matematis. Data teks seperti "Lempung" atau "Pasir" tidak bisa dihitung jaraknya secara langsung. One-Hot Encoding mengubah satu kolom teks menjadi beberapa kolom biner (0 atau 1):

| Tekstur Tanah | Lempung | Pasir | Debu |
|---------------|---------|-------|------|
| Lempung       | 1       | 0     | 0    |
| Pasir         | 0       | 1     | 0    |
| Debu          | 0       | 0     | 1    |

---

### 4.8 Node Preprocess

Node **Preprocess** merupakan *hub* preprocessing yang mengintegrasikan beberapa langkah transformasi data sekaligus, terutama **normalisasi fitur**.

```{figure} images/image_uts/preproses.png
:alt: Tampilan node Preprocess di Orange
:align: center

**Gambar 9.** Node Preprocess — pengaturan normalisasi Z-score pada seluruh fitur.
```

**Cara Setup:**

1. Seret node **Preprocess** dari panel *Widgets* kategori *Preprocess*.
2. Hubungkan output **Data** dari *Continuize* ke input node *Preprocess*.
3. Klik dua kali untuk membuka dialog.
4. Tambahkan preprocessor **Normalize Features** dengan klik tombol **+** atau seret dari panel kiri.
5. Pilih metode: **Standardize (Z-score normalization)**
   - Rumus: $z = \dfrac{x - \mu}{\sigma}$
   - Setiap fitur diubah agar memiliki **rata-rata = 0** dan **standar deviasi = 1**.
6. Klik **OK**.

:::{important}
**Normalisasi adalah langkah wajib untuk KNN.** Tanpa normalisasi, fitur dengan satuan atau skala besar (misalnya K Tersedia dalam ppm yang bernilai ratusan) akan mendominasi perhitungan jarak Euclidean, membuat fitur lain seperti pH (skala 0–14) tidak berpengaruh signifikan terhadap hasil klasifikasi.
:::

Dari node **Preprocess**, data mengalir ke **dua jalur**:
- **Jalur kNN** (jalur evaluasi model)
- **Jalur PCA → Scatter Plot** (jalur visualisasi dimensi)

---

### 4.9 Node PCA → Scatter Plot *(Jalur Visualisasi)*

**PCA (Principal Component Analysis)** mereduksi dimensi data berdimensi tinggi menjadi 2–3 dimensi agar dapat divisualisasikan.

```{figure} images/image_uts/pca.png
:alt: Tampilan node PCA di Orange
:align: center

**Gambar 10.** Node PCA — reduksi dimensi data ke komponen utama (PC1 dan PC2).
```

```{figure} images/image_uts/scatter_plot.png
:alt: Tampilan Scatter Plot hasil PCA di Orange
:align: center

**Gambar 11.** Scatter Plot — visualisasi pemisahan kelas Subur dan Tidak Subur di ruang PCA.
```

**Cara Setup PCA:**

1. Seret node **PCA** dari panel *Widgets* kategori *Unsupervised*.
2. Hubungkan output **Preprocessed Data** dari *Preprocess* ke input node *PCA*.
3. Klik dua kali dan atur jumlah komponen: **2** (untuk visualisasi 2D).
4. Klik **Apply**.

**Cara Setup Scatter Plot:**

1. Seret node **Scatter Plot** dari panel *Widgets* kategori *Visualize*.
2. Hubungkan output **Data** dari *PCA* ke input node *Scatter Plot*.
3. Atur sumbu X: **PC1** dan sumbu Y: **PC2**.
4. Atur warna titik berdasarkan kolom **Label** untuk melihat pemisahan kelas.

:::{note}
Scatter Plot PCA bersifat **visualisasi eksplorasi** — digunakan untuk melihat apakah dua kelas (Subur dan Tidak Subur) sudah terpisah secara visual di ruang PCA, sebelum model KNN dilatih.
:::

---

### 4.10 Node kNN (Learner)

Node **kNN** mendefinisikan arsitektur dan parameter algoritma K-Nearest Neighbors.

```{figure} images/image_uts/knn.png
:alt: Tampilan pengaturan node kNN di Orange
:align: center

**Gambar 12.** Node kNN — pengaturan k=7 dengan metrik Euclidean.
```

**Cara Setup:**

1. Seret node **kNN** dari panel *Widgets* kategori *Model*.
2. **Node kNN tidak perlu dihubungkan ke data secara langsung** — ia berfungsi sebagai *Learner* yang dioper ke node *Test and Score*.
3. Klik dua kali untuk membuka dialog dan atur parameter:

| Parameter | Nilai | Keterangan |
|-----------|-------|------------|
| Number of Neighbors (k) | **7** | Jumlah tetangga terdekat yang dipertimbangkan |
| Metric | **Euclidean** | Rumus perhitungan jarak antar titik |
| Weight | **Uniform** | Semua tetangga memiliki bobot yang sama |

4. Klik **OK**.

**Cara Kerja KNN:**

$$d(x, y) = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}$$

Untuk setiap data uji, dihitung jarak Euclidean ke seluruh data latih. Sebanyak **k = 7** tetangga terdekat dipilih, dan **kelas mayoritas** di antara 7 tetangga tersebut menjadi hasil prediksi.

---

### 4.11 Node Test and Score

Node **Test and Score** mengukur performa model KNN secara objektif menggunakan metode evaluasi yang valid.

```{figure} images/image_uts/test_and_score.png
:alt: Hasil evaluasi pada node Test and Score di Orange
:align: center

**Gambar 13.** Node Test and Score — hasil Cross Validation 5-fold dengan metrik evaluasi.
```

**Cara Setup:**

1. Seret node **Test and Score** dari panel *Widgets* kategori *Evaluate*.
2. Hubungkan dua input:
   - Output **Preprocessed Data** dari node *Preprocess* → input **Data**
   - Output **Learner** dari node *kNN* → input **Learner**
3. Klik dua kali untuk membuka dialog.
4. Pilih metode evaluasi: **Cross Validation**
5. Atur **Number of Folds:** `5`
6. Klik **Apply**.

**Penjelasan Cross Validation 5-Fold:**

Data dibagi menjadi 5 bagian (*fold*) yang sama besar. Model dilatih sebanyak 5 kali — setiap kali menggunakan 4 bagian sebagai data latih dan 1 bagian sebagai data uji secara bergantian. Hasil akhir adalah rata-rata dari 5 percobaan, sehingga evaluasi lebih stabil dan tidak bias.

---

### 4.12 Node Confusion Matrix

Node **Confusion Matrix** memberikan visualisasi detail mengenai hasil prediksi dibandingkan dengan kelas aktual.

```{figure} images/image_uts/confusion_matri.png
:alt: Confusion Matrix hasil prediksi KNN di Orange
:align: center

**Gambar 14.** Node Confusion Matrix — seluruh prediksi tepat berada di diagonal utama.
```

**Cara Setup:**

1. Seret node **Confusion Matrix** dari panel *Widgets* kategori *Evaluate*.
2. Hubungkan output **Evaluation Results** dari node *Test and Score* ke input node *Confusion Matrix*.
3. Klik dua kali untuk membuka visualisasi.
4. Pilih model **kNN** dari dropdown jika tersedia lebih dari satu model.

**Cara Membaca Confusion Matrix:**

|                          | **Prediksi: Subur** | **Prediksi: Tidak Subur** |
|--------------------------|---------------------|---------------------------|
| **Aktual: Subur**        | TP (True Positive)  | FN (False Negative)       |
| **Aktual: Tidak Subur**  | FP (False Positive) | TN (True Negative)        |

Pada hasil percobaan ini, seluruh nilai berada pada **diagonal utama** (TP dan TN = jumlah sampel), sedangkan nilai FP dan FN adalah **0**. Ini menunjukkan model tidak membuat kesalahan prediksi sama sekali.

---

## 5. Hasil Evaluasi

Berdasarkan pengujian menggunakan Cross Validation 5-fold pada node **Test and Score**, diperoleh hasil sebagai berikut:

| Metrik Evaluasi   | Nilai     | Keterangan |
|-------------------|-----------|------------|
| **Accuracy (CA)** | **1.000** | Model mengklasifikasikan **100%** data dengan benar |
| **Precision**     | **1.000** | Tidak ada prediksi "Subur" yang sebenarnya "Tidak Subur" |
| **Recall**        | **1.000** | Seluruh sampel tanah subur berhasil terdeteksi |
| **F1-Score**      | **1.000** | Nilai harmonik sempurna antara Precision dan Recall |

---

## 6. Analisis dan Kesimpulan

### Mengapa Preprocessing Bertahap Sangat Penting untuk KNN?

KNN adalah algoritma berbasis jarak — ia tidak "belajar" parameter seperti model lain, melainkan hanya membandingkan jarak antar titik data. Ini berarti kualitas data sangat menentukan kualitas prediksi:

| Langkah | Node | Tujuan |
|---------|------|--------|
| Imputasi | Impute | Mengisi nilai kosong agar tidak ada baris yang terbuang |
| Konversi Kategorikal | Continuize | Mengubah teks menjadi angka agar jarak dapat dihitung |
| Normalisasi | Preprocess | Menyamakan skala semua fitur agar kontribusinya proporsional |

### Ringkasan Alur Node

```
[CSV File Import]
       │ Data
       ▼
 [Data Table]
       │ Selected Data
       ▼
[Select Columns]
       │ Data
       ▼
[Distributions]
       │ Data
       ▼
  [Box Plot]
       │ Data
       ▼
   [Impute]
       │ Data
       ▼
 [Continuize]
       │ Data
       ▼
 [Preprocess] ──── Data ────► [PCA] ──► [Scatter Plot]
       │ Preprocessed Data
       ├──────────────────────────────► [kNN (Learner)]
       │                                      │ Learner
       └──────────────────────────────► [Test and Score]
                                              │ Evaluation Results
                                              ▼
                                      [Confusion Matrix]
```

Penggunaan algoritma KNN dengan parameter $k=7$ dan metrik Euclidean, dikombinasikan dengan pipeline preprocessing yang terstruktur (Impute → Continuize → Normalize), terbukti sangat efektif untuk dataset kesuburan tanah ini. Hasil evaluasi menunjukkan **akurasi sempurna 100%**, yang mengindikasikan pola parameter kimia tanah sangat konsisten dan terstruktur dengan baik antara kelas Subur dan Tidak Subur.
