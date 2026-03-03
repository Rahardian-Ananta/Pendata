# 📊 Data Understanding – Collecting Data & Pengukuran Jarak

Tahap Data Understanding tidak hanya tentang memahami tipe atribut, tetapi juga memastikan proses awal pengumpulan data (Data Collecting) berjalan dengan baik, serta menguasai pendekatan perhitungan jarak (keterkaitan) di antara baris-baris data dari *dataset* yang terkumpul.

## 1. Pengumpulan Data (Data Collecting)

Dataset Karyawan ini merupakan hasil **simulasi pengumpulan data** (Data Collecting) yang bersumber dari sistem internal HR Perusahaan X. Tujuannya adalah membangun tabel metrik untuk pemetaan profil kompensasi dan struktur demografi karyawan.

Berikut adalah representasi (cuplikan) langsung *dataset* hasil *collecting* yang tersimpan di file `dataset_karyawan.csv`:

| ID_Karyawan | Departemen  | Tingkat_Pendidikan | Status_Pernikahan | Tahun_Lahir | Gaji_Per_Bulan |
| ----------- | ----------- | ------------------ | ----------------- | ----------- | -------------- |
| K001        | IT          | S1                 | Menikah           | 1990        | 8,500,000      |
| K002        | HR          | S1                 | Belum Menikah     | 1995        | 6,000,000      |
| K003        | Keuangan    | S2                 | Menikah           | 1988        | 12,000,000     |
| K004        | IT          | D3                 | Belum Menikah     | 1998        | 5,500,000      |
| K005        | Marketing   | S1                 | Menikah           | 1992        | 7,500,000      |
| K006        | Operasional | SMA                | Menikah           | 1985        | 4,500,000      |
| K007        | IT          | S2                 | Menikah           | 1989        | 15,000,000     |
| K008        | Marketing   | D3                 | Belum Menikah     | 1997        | 5,000,000      |
| K009        | HR          | S2                 | Menikah           | 1987        | 11,000,000     |
| K010        | Keuangan    | S1                 | Belum Menikah     | 1996        | 7,000,000      |
| ...         | ...         | ...                | ...               | ...         | ...            |
| K022        | IT          | D3                 | Menikah           | 1995        | 6,000,000      |

*(Total tabel asli berisi 22 baris observasi)*

---

## 2. Identifikasi Fitur dan Label

* **Fitur (Features / Atribut Bebas)**: `ID_Karyawan`, `Departemen`, `Tingkat_Pendidikan`, `Status_Pernikahan`, `Tahun_Lahir`. Kolom-kolom ini membentuk lanskap profil individu masing-masing pegawai.
* **Label (Target / Atribut Terikat)**: `Gaji_Per_Bulan`.
* **Alasan Pemilihan Label**: Atribut gaji (bertipe klasifikasi/regresi) dipilih sebagai kelas penyelesaian masalah (*Target*). Kita berasumsi perusahaan ingin menggali estimasi penganggaran insentif mana yang cocok diberikan kepada kandidat baru, jika pola profil (Pendidikan, Usia, Departemen) milik kandidat dicocokkan dengan data tabel lama.

---

## 3. Tipe Data Observasi

Pengelompokan tipe sangat krusial karena setiap tipe data **wajib** dihitung jarak kedekatannya menggunakan rumus spesifik yang berbeda-beda.

| Kolom | Tipe Data | Kategori |
| --- | --- | --- |
| ID_Karyawan | Kategorikal | Nominal |
| Departemen | Kategorikal | Nominal |
| Tingkat_Pendidikan | Kategorikal | Ordinal |
| Status_Pernikahan | Kategorikal | Biner |
| Tahun_Lahir | Numerik | Interval |
| Gaji_Per_Bulan | Numerik | Ratio |

---

## 4. Penjelasan Setiap Tipe Data

* **Nominal**: Data kategori murni sebagai nama (label) tanpa hierarki sama sekali. (Contoh: `Departemen` IT tidak berarti lebih superior daripada cabang operasional; mereka murni unit yang sederajat).
* **Ordinal**: Data kategori yang memiliki tingkatan/pangkat yang bermakna secara logis. Jarak urutannya jelas secara kelas. (Contoh: `Tingkat_Pendidikan` dengan ranking D3 ada di atas SMA, S1 di atas D3).
* **Biner**: Kasus turunan khusus dari tipe nominal yang opsi populasinya cuma mentok berjumlah DUA kondisi saja. (Contoh: `Status_Pernikahan` berisikan murni opsi tunggal Menikah / Belum Menikah).
* **Interval**: Angka matematika berurutan seragam, *tetapi ia tidak menganut konsep nilai "Nol Mutlak" (Absolute Zero)*. (Contoh: `Tahun_Lahir` 1995 dan 2000 berselisih 5 tahun pasti, tetapi angka kalender tahun 0 bukan mengartikan "ketiadaan masa", itu hanya titik arbitrary sejarah semata).
* **Ratio**: Angka absolut tertinggi dari piramida matematika yang sungguh mempunyai batas "Nol Mutlak", layak nilainya dikali/dibagi/dijumlah. (Contoh: `Gaji_Per_Bulan`, besaran Gaji Rp 0 memiliki makna riil: ya, karyawan itu tidak dapat uang sepeserpun di akhir bulan. Rp 10 Juta tentu mutlak hitungannya 2 kali lipat dari Rp 5 Juta).

---

## 5. Konsep Pengukuran Jarak & Perhitungan Algoritmanya

Dalam algoritma Machine Learning (seperti KNN atau Clustering), kedekatan antar 2 baris karyawan diputuskan melalui kalkulasi yang dinamakan **Distance Measurements**. 

Rumus diatur berbeda menyesuaikan kasta *Tipe Data* pada sel dimensinya.

### a. Numerik (Jarak Skala Penuh)
Data yang direpresentasikan berupa angka mutlak diolah seolah kita menata peta koordinat mistar.
Misalkan ada Titik A (Pekerja 1) dan Titik B (Pekerja 2).

* **Euclidean Distance**: Formula asali jarak lurus (sisi miring) antar dua titik persis bak teori pitagoras. 
  * *Rumus:* $d = \sqrt{\sum (A_i - B_i)^2}$
* **Manhattan Distance**: Karena riilnya kita berjalan tak bisa menembus blok gedung layaknya garis miring, cara kedua ini murni menaksir total jarak tempuh mendatar horizontal ditambahkan bentangan tegak layaknya berjalan di blok perempatan kota.
  * *Rumus:* $d = \sum |A_i - B_i|$

> **✏️ Perhitungan Manual (Numerik Interval `Tahun_Lahir` - K010 vs K012):**
> K010 = Lahir 1996, K012 = Lahir 2000.
> *Manhattan Distance:* $\|1996 - 2000\| = \text{Selisih } 4 \text{ Poin Jarak Usia}.$

### b. Biner (Simetris & Asimetris)
Metrik khusus mengevaluasi 2 dimensi (misal disimbolkan kehadiran sebagai {1} & ketiadaan sebagai {0}). 

* **Simple Matching (SMC)**: Menghitung nilai yang cocok di semua front / total keseluruhan atribut. Bagus jika ketidakhadiran (nilai 0) juga dirasa sama pentingnya buat dicocokkan dengan kehadiran (nilai 1).
  * *Rumus satu kolom tunggal:* Jika Sama Persis (1-1 / 0-0) = Poin jarak kemiripan diset **`0`** *(sangat mirip)*. Bila beda ganjil (1-0) = Poin dihempas ke jarak **`1`** *(Terjauh)*.
* **Jaccard Distance**: SMC level advance. Diterapkan spesifik **MENGABAIKAN** atribut bila kondisinya sama-sama (0-0). Pencocokan difokuskan pada nilai aktif (1-1) saja. (Contohnya, tak ada gunanya mencocokkan kemiripan kesamaan pasien berdua yang statusnya sama-sama `Flu = Tidak`, utamakan kemiripan data bila ada pasien `Sakit Jantung = Ya`).

> **✏️ Perhitungan Manual (Biner `Status_Pernikahan` - K001 vs K004):**
> K001 = Menikah (1), K004 = Belum Menikah (0).
> Karena label elemennya tidak sinkron, selisih skor ketidaksamaannya dihukum **1 mutlak**.

### c. Nominal
Tipe di mana dua label cuma berpotensi "SAMA JENIS" atau "BEDA JENIS".

* **Perbandingan Sederhana (0/1)**: Kalau isinya kembar identik persis, nilainya 0. Bedakan status sedikit saja (Misal "Timur", satu lagi "Barat"), ia digaris dengan nilai kesenjangan mutlak maksimal 1.

> **✏️ Perhitungan Manual (Nominal `Departemen` - K001 vs K007):**
> K001 = IT, K007 = IT. Karena kategori departemen mereka persis tak ada gap, jarak pergeserannya dianugerahi **0**.

### d. Ordinal
Tipe hierarki pangkat tak dapat dihitung pakai pitagoras, harus direduksi menjadi rasio bertingkat.

* **Transformasi ke Numerik Rasio**: Setiap jenjang dirubah ke fraksi desimal (Skala rentang mutlak rasio 0 hingga 1).
  * Rank kita: SMA(1) -> D3(2) -> S1(3) -> S2(4). Maka *MaxRank* = 4. 
  * Rumus Normalisasi: $\text{Normalisasi Data } = \frac{Rank\_Data - 1}{Max\_Rank - 1}$

> **✏️ Perhitungan Manual (Ordinal `Tingkat_Pendidikan` - K006 vs K007):**
> K006(SMA) = Berada di dasar Rank 1.
> K007(S2) = Berada di tampuk Rank 4.
> *Konversi K006:* $(1-1)/(4-1) = 0 / 3 = \mathbf{0.0}$
> *Konversi K007:* $(4-1)/(4-1) = 3 / 3 = \mathbf{1.0}$
> *Tarik Manhattan Distance Keduanya:* $\|0.0 - 1.0\| = \mathbf{1.0}$ (Artinya, dari kacamata algoritma kemiripan pendidikannya terpecah menjalar sangat jauh sejauh mungkin).

### e. Data Campuran (Mixed-Type Variables / Gower Distance)
Di sinilah krusialnya algoritma cerdas, taksir saja ketika baris satu karyawan berisikan gado-gado klasifikasi (IT), urutan rank (SMA), biner (Belum menikah), dan Rasio Jutaan (Rp 9 Juta). 

* **Metode Gower Distance**: Kalau pakai tipe Manhattan numerik murni, skor jarak Gaji yang selisihnya "Jutaan" akan menenggelamkan bobot perhitungan 0/1 skala Biner. Gower menetralkannya! Algoritma Gower memeras standarisasi keseluruhan selisih di dimensi tak karuan ini agar kesenjangannya patuh terpangkas **hanya di area 0.0 (Mirip Identik) hingga batas 1.0 (Paling Berbeda)**. Tiap atribut akan dinormalisasi dulu rumusannya mandiri, lalu angka gap ditabulasi rata-ratanya pada garis finis akhir perhitungan kombinasi.

> **✏️ Simulasi Gower Distance Total - Karyawan K001 vs K002 (Kalkulasi Matriks Akhir):**
> 1. _Jarak antar Departemen (IT vs HR):_ Nominal Bedanya jauh -> $\text{Skor gap } \mathbf{1.0}$
> 2. _Jarak antar Pendidikan (S1 vs S1):_ Pangkatnya Sama -> $\text{Skor gap } \mathbf{0.0}$
> 3. _Jarak antar Pernikahan (Menikah vs Belum):_ Biner Berlawanan -> $\text{Skor gap } \mathbf{1.0}$
> 4. _Jarak gap Tahun_Lahir (1990 vs 1995)_ -> Selisih (5 th) dibagi Max Range data umur terkurung di dataset (16 th) = $(5/16) \rightarrow \text{Skor gap } \mathbf{0.3125}$
> 5. _Jarak gap Gaji (8.5jt vs 6jt)_ -> Selisih uang (2.5jt) difraksikan pada range mutlak batas gaji tabel (Max gaji 15jt - Min 3.8jt) = $(2.5 / 11.2) \rightarrow \text{Skor gap } \mathbf{0.2232}$

> **Total Jarak Penjumlahan Gower = Rata-Rata Seluruh Skor Atribut**
> $= \frac{1.0 + 0 + 1.0 + 0.3125 + 0.2232}{5 \text{ atribut total}}$
> $= \mathbf{0.5071}$ 
> *(Artinya mesin Data Mining berkesimpulan, Karyawan K001 dan K002 punya rentang gap ketidakmiripan jarak kedekatan sebesar* **50.7%***)*

---

## 6. Skenario Eksekusi Orange Data Mining

Dengan berbekal software alat rekayasa visual model seperti **Orange Data Mining**, eksekusi pembuktian kalkulasi distance matrik atas 22 row data campuran dilakukan semudah menyusun puzzle alur di kanvas:

1. **Memasukkan/Mengoleksi Data Modul (Data Collecting)**: 
   Drag *Widget* berlogo **"CSV File Import"** (Atau standar modul *"File"*). Di barisan panel sebelah kanannya, browse letak rute data simulasi buatan kita `dataset_karyawan.csv`. Orange cerdas akan secara auto-mendikte tipe datanya (seperti Gaji Per Bulan jadi Numeric).
2. **Review Sinkronasi Dataset**: 
   - Lempar simpul "File" tersebut menyambung ke modul perantara pengecekan tabel bernama **"Data Table"** agar representasi hamparan koleksi simulasi database CSV bisa Anda pantau terekam lurus sempurna masuk ke Orange.
3. **Pengaturan Mesin Distance & Formula**: 
   - Sambungkan jalur rute widget "File"-nya menuju modul komponen algoritma pusat **"Distances"**.
   - Klik 2 kali ikon "Distances". Di jendela Propertiesnya, arahkan tuas perbandingan pengawasan antar sesama target mengunci opsi mode **"Rows (Baris)"**. 
   - Pada dropdown Distance Metric, pilih formula ajaib penyesuaian mixed variables: **"Gower"**. Biarkan Orange mengekstrasi normalisasi kompleks jutaan gaji dengan nominal secara senyap di belakang layar memproses 22 row data (Mirip hitungan `K001 vs K002 = 0.5071`).
4. **Analisis Hasil Keluaran Visualisasi Pengelompokan**: 
   Hubungkan tali *output* dari blok Node 'Distances' ini untuk dilempar ke instrumen konveksi hasil penelusuran:
   - Hubungkan ke widget visual kanvas tabular besar yakni **"Distance Matrix"**. Anda akan menyaksikan silang menyingkap matriks persentase kedekatan antar-anggota angka probabilitas layaknya kotak *spreadsheet heatmap*.
   - Terhubung lebih jauh lagi dengan menautkan Matrix Gower tsb kepada instrumen pilar akhir algoritma yang mengelompokkan data otomatis berjudul **"Hierarchical Clustering"**. Amati kanvas gambarnya akan bercabang berhimpitan seperti akar rumput *Dendrogram*. Menilik karyawan K001 barisnya dikonstruksikan berumpun dkk dengan siapa saja di grup yang sedekat mungkin preferensinya.
