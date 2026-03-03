# 📊 Collecting Data & Pengukuran Jarak

Tahap Data Understanding tidak hanya tentang memahami tipe atribut, tetapi juga memastikan proses awal pengumpulan data (Data Collecting) berjalan dengan baik, serta menguasai pendekatan perhitungan jarak (keterkaitan) di antara baris-baris data dari *dataset* yang terkumpul.

## 1. Pengumpulan Data (Data Collecting)

Dataset Karyawan ini merupakan hasil **simulasi pengumpulan data** (Data Collecting) yang bersumber dari sistem internal HR Perusahaan X. Tujuannya adalah membangun tabel metrik untuk pemetaan profil kompensasi dan struktur demografi karyawan.

[🔗 **Unduh Dataset Karyawan (CSV)**](files/dataset_karyawan.csv)

Berikut adalah keseluruhan data dari *dataset* hasil *collecting* yang tersimpan di file `dataset_karyawan.csv`:

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
| K011        | Operasional | S1                 | Menikah           | 1991        | 6,500,000      |
| K012        | IT          | SMA                | Belum Menikah     | 2000        | 4,000,000      |
| K013        | HR          | D3                 | Menikah           | 1993        | 5,500,000      |
| K014        | Marketing   | S1                 | Menikah           | 1990        | 8,000,000      |
| K015        | Keuangan    | SMA                | Belum Menikah     | 1999        | 4,000,000      |
| K016        | Operasional | D3                 | Menikah           | 1994        | 5,000,000      |
| K017        | IT          | S1                 | Belum Menikah     | 1997        | 7,500,000      |
| K018        | HR          | S1                 | Menikah           | 1992        | 6,500,000      |
| K019        | Keuangan    | S2                 | Menikah           | 1986        | 13,000,000     |
| K020        | Marketing   | S2                 | Menikah           | 1988        | 14,000,000     |
| K021        | Operasional | SMA                | Belum Menikah     | 2001        | 3,800,000      |
| K022        | IT          | D3                 | Menikah           | 1995        | 6,000,000      |

*(Total tabel: 22 baris data observasi).*

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

## 5. Konsep Pengukuran Jarak & Skenario Simulasi Perhitungan

Dalam ranah *Data Mining*, kalkulasi **Distance Measurements** berfungsi krusial untuk **menentukan metrik kesamaan/perbedaan** antara baris data satu dengan lainnya. Tujuannya agar algoritma (misal *Clustering*) bisa dengan cerdas merangkai relasi kelompok karyawan bertipikal homogen dari *pool* populasi secara otomatis.

Mari kita membedah *behind-the-scenes* bagaimana sebuah algoritma mengukur kedekatan profil antara dua *sample* riil dari database kita, misalnya antara **Karyawan K001** dan **Karyawan K002**:

* **K001** = `IT` | `S1` | `Menikah (1)` | `1990` | `Rp 8.500.000`
* **K002** = `HR` | `S1` | `Belum Menikah (0)` | `1995` | `Rp 6.000.000`

Karena dataset tersusun atas tipe yang radikal berbeda, metode hukum jaraknya dipisah sesuai kasta tipe datanya:

### a. Numerik (Jarak Skala Penuh)
Mengukur margin mutlak dari deret *Interval* dan *Ratio*. Jika profil **K001** dilambangkan rute $A$ dan **K002** rute $B$:

* **Euclidean Distance**: Tarikan garis miring antara dua titik *scatterplot* (mirip hipotenusa pitagoras). 
  $$ d = \sqrt{\sum_{i=1}^{n} (A_i - B_i)^2} $$
* **Manhattan Distance**: Kalkulasi "City Block". Melakukan tabulasi sumbu mutlak vertikal ditambah horisontal (karena relasi gaji tak miring menembus gedung).
  $$ d = \sum_{i=1}^{n} |A_i - B_i| $$

> **✏️ Tujuan & Perhitungan (Misal: Rentang Generasi Usia `Tahun_Lahir`):**<br>
> *Tujuan:* Algoritma ingin tahu apakah mereka dari angkatan yang sama atau terpaut jauh era kerjanya.<br>
> Profil **K001** = 1990 | Profil **K002** = 1995.<br>
> *Manhattan Distance Umur:* $\|1990 - 1995\| = \text{Selisih } \mathbf{5} \text{ Tahun Jarak Generasi}.$

### b. Biner (Simetris & Asimetris)
Rumus untuk fitur bersimbol {1} dan {0}. 

* **Simple Matching (SMC)**: Menghitung persentase "cocok" dari semua atribut biner. Digunakan jika kondisi ketidakhadiran {0} juga vital untuk diobservasi algoritmanya layaknya probabilitas {1}.
  * *Hukum (Satu Atribut Tabrakan):* Sama persis (1-1 / 0-0) dikenakan poin keterkaitan **`0`** *(sangat identik)*. Jika nilainya timpang silang (1-0) nilainya dikenakan poin kesenjangan **`1`** *(Terjauh)*.

> **✏️ Tujuan & Perhitungan (`Status_Pernikahan` - K001 vs K002):**<br>
> *Tujuan:* Membandingkan kesamaan tanggungan sosial profil.<br>
> **K001** = Menikah (1) | **K002** = Belum Menikah (0).<br>
> Karena label elemennya berseberangan (1 tidak sama dengan 0), maka status kekerabatannya dihukum dengan kesenjangan sebesar **1 poin mutlak**.

### c. Nominal
Tipe di mana kategorinya tidak bisa dijumlah/dikurangi, melainkan hanya dinilai "SAMA" atau "BEDA JENIS".

* **Perbandingan Sederhana**: Kalau *string* / namanya persis, gap jaraknya adalah `0`. Berubah satu huruf saja dan beda makna, maka ditarik garis renggang maksimal `1`.

> **✏️ Tujuan & Perhitungan (`Departemen` - K001 vs K002):**<br>
> *Tujuan:* Mengukur relasi ranah pekerjaan harian yang sering mereka hadapi.<br>
> **K001** = Departemen IT | **K002** = Departemen HR.<br>
> Keduanya berasal dari teritori kerja departemen yang radikal terpisah fungsinya. Tentu jarak kedekatannya secara nominal dihantam poin **1**.

### d. Ordinal
Hierarki kelas (pangkat) tak bisa dikurangi biasa. Jauhi merumus langsung (S1 dikurangi SMA), tapi rubah ke indeks rasio proporsi jenjangnya di dataset.

* **Transformasi ke Numerik Rasio**: Setiap jabatan *rank* dijepit pada desimal bentangan `0.0` sampai mentok `1.0`.
  * Konversi skala kita: SMA(1) $\rightarrow$ D3(2) $\rightarrow$ S1(3) $\rightarrow$ S2(4). Maka *Max_Rank* batas atasnya = 4 tingkat. 
  $$ \text{Normalisasi Level Data } = \frac{Rank\_Yang\_Diukur - 1}{Max\_Rank - 1} $$

> **✏️ Tujuan & Perhitungan (`Tingkat_Pendidikan` - K001 vs K002):**<br>
> *Tujuan:* Mempelajari gap latar belakang riwayat akademis struktural mereka.<br>
> **K001**(S1) = Berada di tampuk Rank 3 menengah.<br>
> **K002**(S1) = Juga menduduki Rank 3.<br>
> Karena persis satu level jenjang pendidikan (keduanya Rank 3; dengan rasio transformasi bernilai $\mathbf{0.67}$ dan $\mathbf{0.67}$), maka tabrakan matematis kurikulum mereka menyisakan margin bersih **0.0 poin (Identik sejawat)**.

### e. Mixed-Type Variables (Gower Distance)
Di sinilah krusialnya peninggalan algoritma jenius "Gower". Bagaimana caranya mesin bisa menambahkan dan menyatukan poin selisih Jutaan Rupiah dari tabel *Gaji* dengan poin kecil (0 atau 1) dari tabel kelas *Departemen* tanpa bias memonopoli nilai akhirnya?
Gower secara luwes menengahi: Ia akan memeras normalisasi rentangan poin jarak dari **seluruh dimensi apapun** agar batas bawah *gap*-nya patuh tertahan di `0.0` (Super Identik) hingga batas selisih mentok *absolute* `1.0` (Paling Mustahil Sangat Berlawanan).

> **✏️ Finalisasi Tujuan Simulasi Kumulatif Karyawan K001 & K002:**<br>
> *Tujuan Integrasi:* Sistem mengeksekusi konklusi paripurna terhadap 5 pilar dimensi tabel (Gap Gaji, Selisih Umur, Pangkat Pendidikan, Departemen, & Pernikahan) di waktu bersamaan.<br><br>
> 1. _Jarak Nominal Departemen (IT sejajar HR):_ Beda faksi $\rightarrow$ Skor gap **1.0**<br>
> 2. _Jarak Ordinal Pangkat Pendidikan (S1 sejajar S1):_ Lulusan sama $\rightarrow$ Skor gap **0.0**<br>
> 3. _Jarak Biner Status (Menikah sejajar Belum):_ Tanggungan beda $\rightarrow$ Skor gap **1.0**<br>
> 4. _Normalisasi Jarak Interval Umur [1990-1995]:_ Selisih tahun mereka (5 th) dikomparasi dengan rentang bentang paling tua & termuda sedatabase (Pegawai Lahir 1985 s/d 2001 $\rightarrow$ total 16 th beda bentangan). Nilainya $(5/16) \rightarrow$ Skor gap usia terejawantahkan di angka **0.3125**<br>
> 5. _Normalisasi Jarak Gaji [Rp8.5jt vs Rp6jt]:_ Selisih gaji mutlak K001&2 sebesar Rp2.5jt diukur terhadap kelonggaran dompet gap kasta gaji Perusahaan (Bos Tetinggi IT K007 Rp15jt vs Staff Dasar SMA Rp3.8jt $\rightarrow$ margin range data terbesarnya $11.2jt$). Proyeksi gapnya $(2.5 / 11.2) \rightarrow$ Meruncing pada rasio desimal gap gaji **0.2232**

> **Konklusi Tabulasi Matrix Gower Akhir (Rata-rata Simpang):**
> 
> $$ \text{Total Distance} = \frac{\text{Jarak C1} + \text{Jarak C2} + \text{Jarak C3} + \text{Jarak C4} + \text{Jarak C5}}{\text{Total Atribut}} $$
> $$ \text{Total Index} = \frac{1.0 + 0 + 1.0 + 0.3125 + 0.2232}{5} $$
> $$ \text{Total Index} = \mathbf{0.5071} $$
>
> *(💡 ***Hasil Akhir Keceradasan Buatan:*** Mesin Data Mining mengambil keputusan akhir bahwa jika keseluruhan preferensi riwayat pegawai disatukan, Karyawan K001 memiliki probabilitas rekam kelembagaan yang bersinggungan membuar/menjauh arah profil kompensasinya di angka persentase deviasi rata-rata rasio* **50.71%** *apabila dibandingkan dengan sosok Karyawan K002 sedepartemen).*
---

## 6. Skenario Eksekusi Orange Data Mining

Dengan berbekal software alat rekayasa visual model seperti **Orange Data Mining**, eksekusi pembuktian kalkulasi distance matrik atas 22 baris data campuran dilakukan semudah menyusun puzzle alur di kanvas:

1. **Memasukkan/Mengoleksi Data Modul (Data Collecting)**: 
   Drag *Widget* berlogo **"CSV File Import"** (Atau standar modul *"File"*). Di barisan panel sebelah kanannya, browse letak rute data simulasi buatan kita `dataset_karyawan.csv`. Orange cerdas akan secara auto-mendikte tipe datanya (seperti Gaji Per Bulan jadi Numeric).
2. **Review Sinkronasi Dataset**: 
   - Lempar simpul "File" tersebut menyambung ke modul perantara pengecekan tabel bernama **"Data Table"** agar representasi hamparan koleksi simulasi database CSV bisa Anda pantau terekam lurus sempurna masuk ke Orange.
3. **Pengaturan Mesin Distance & Formula**: 
   - Sambungkan jalur rute widget "File"-nya menuju modul komponen algoritma pusat **"Distances"**.
   - Klik 2 kali ikon "Distances". Di jendela Propertiesnya, arahkan tuas perbandingan pengawasan antar sesama target mengunci opsi mode **"Rows (Baris)"**. 
   - Pada dropdown Distance Metric, pilih formula ajaib penyesuaian mixed variables: **"Gower"**. Biarkan Orange mengekstrasi normalisasi kompleks jutaan gaji dengan nominal secara senyap di belakang layar memproses 22 baris data (Mirip hitungan `K001 vs K002 = 0.5071`).
4. **Analisis Hasil Keluaran Visualisasi Pengelompokan**: 
   Hubungkan tali *output* dari blok Node 'Distances' ini untuk dilempar ke instrumen konveksi hasil penelusuran:
   - Hubungkan ke widget visual kanvas tabular besar yakni **"Distance Matrix"**. Anda akan menyaksikan silang menyingkap matriks persentase kedekatan antar-anggota angka probabilitas layaknya kotak *spreadsheet heatmap*.
   - Terhubung lebih jauh lagi dengan menautkan Matrix Gower tsb kepada instrumen pilar akhir algoritma yang mengelompokkan data otomatis berjudul **"Hierarchical Clustering"**. Amati kanvas gambarnya akan bercabang berhimpitan seperti akar rumput *Dendrogram*. Menilik karyawan K001 barisnya dikonstruksikan berumpun dkk dengan siapa saja di grup yang sedekat mungkin preferensinya.
