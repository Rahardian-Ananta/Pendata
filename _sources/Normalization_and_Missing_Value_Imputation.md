## Normalisasi dan Pengisian Missing Value dalam Penambangan Data

Dalam pengolahan data untuk data mining, seringkali kita dihadapkan pada atribut dengan skala yang berbeda atau bahkan adanya data yang hilang (missing value). Bab ini akan menjelaskan secara rinci proses normalisasi data dengan metode Min-Max dan penanganan *missing value* menggunakan algoritma K-Nearest Neighbors (KNN). Pembagian tahapan disesuaikan dengan struktur perhitungan dataset Anda.

---

### Download Dataset
Untuk mempermudah pemahaman, Anda dapat mengunduh dataset yang digunakan dalam bab ini:
* [📩 Download Dataset Karyawan (Mentah)](./files/dataset_karyawan.csv)
* [📩 Download Dataset Karyawan (Hasil Proses)](./files/dataset_karyawan_processed.csv)

---

## Blok 1 (Kolom A-F): Dataset Asli (Data Mentah)

Blok ini berisi rancangan data awal sebelum diproses. Format data tabular ini terdiri dari atribut-atribut dasar karyawan:
* **Kolom A:** ID Karyawan
* **Kolom B:** Departemen (Kategorikal)
* **Kolom C:** Tingkat Pendidikan (Kategorikal)
* **Kolom D:** Status Pernikahan (Kategorikal)
* **Kolom E:** Tahun Lahir (Numerik)
* **Kolom F:** Gaji Per Bulan (Numerik)

Data ini memiliki berbagai macam skala, misalnya atribut `Tahun_Lahir` berkisar di angka ribuan (1985 - 2001), dan `Gaji_Per_Bulan` dalam skala jutaan Rupiah (3.800.000 - 15.000.000). Nilai minimum (Min) dan maksimum (Max) dari atribut numerik ini akan menjadi dasar perhitungan Normalisasi pada tahap selanjutnya.

---

## Blok 2 (Kolom G-H): Hasil Normalisasi Min-Max untuk Atribut Numerik

Perbedaan jarak (skala) antara tahun lahir dan gaji per bulan yang sangat jauh dapat mengacaukan perhitungan ketetanggaan (Euclidean distance). Oleh karena itu, kita perlu menyamakan rentang skalanya menjadi rentang standar antara 0 hingga 1 menggunakan **Normalisasi Min-Max**.

* **Kolom G:** Normalisasi Tahun Lahir
* **Kolom H:** Normalisasi Gaji

**Rumus Min-Max Normalization:**
$$X_{baru} = \frac{X - X_{min}}{X_{max} - X_{min}}$$

*Keterangan:*
* $X$: Nilai atribut numerik asli saat ini.
* $X_{min}$, $X_{max}$: Nilai minimum dan maksimum seluruh sampel dari kolom atribut tersebut.

**Contoh Perhitungan untuk Karyawan K001:**
* **Normalisasi Tahun Lahir** (Data Asli: Tahun = 1990, Min = 1985, Max = 2001)
  $$X_{baru} = \frac{1990 - 1985}{2001 - 1985} = \frac{5}{16} = 0,3125$$
* **Normalisasi Gaji** (Data Asli: Gaji = 8.500.000, Min = 3.800.000, Max = 15.000.000)
  $$X_{baru} = \frac{8.500.000 - 3.800.000}{15.000.000 - 3.800.000} = \frac{4.700.000}{11.200.000} \approx 0,4196...$$

---

## Blok 3 (Kolom I-K): Perhitungan Jarak Kategorikal

Pada proses pencarian jarak (distansi), atribut berjenis kategorikal tidak bisa diproses menggunakan pengurangan angka biasa. Jarak kategorikal dihitung menggunakan logika perbandingan kesamaan **Simple Matching Coefficient**:
* Jika dua nilai kategorikal **Sama**, maka Jarak = **0**
* Jika dua nilai kategorikal **Berbeda**, maka Jarak = **1**

Ketiga kolom ini mewakili setiap variabel kategorikal:
* **Kolom I:** Jarak Pendidikan (*S1 vs S2, dll*)
* **Kolom J:** Jarak Departemen (*IT vs Keuangan, dll*)
* **Kolom K:** Jarak Status Pernikahan (*Menikah vs Belum Menikah*)

---

## Blok 4 (Kolom L): Jarak Kategorikal Gabungan

Setelah nilai per-kolom kategorikal didapatkan, semua skor jarak kategorikal ini digabungkan dan dirata-rata.

**Rumus Jarak Kategorikal Gabungan:**
$$d_{kategorikal} = \frac{\sum \text{Jarak Atribut Kategorikal}}{\text{Jumlah Atribut Kategorikal}}$$

**Contoh Perhitungan (K001 vs rujukan tertentu):**
Misalkan antara K001 (S1, IT, Menikah) dan Target rujukan (S1, HR, Belum Menikah):
- Jarak Pendidikan saling sama (S1 vs S1) = 0  
- Jarak Departemen berbeda (IT vs HR) = 1  
- Jarak Status Pernikahan berbeda (Menikah vs Belum Menikah) = 1  
- **Jarak Gabungan Kategorikal** = $\frac{0 + 1 + 1}{3} = \frac{2}{3} \approx 0,6666...$

---

## Blok 5 (Kolom M): Perhitungan Jarak Numerik (Euclidean)

Atribut numerik yang telah dinormalisasi (Kolom G dan Kolom H) lalu dihitung selisih jaraknya menggunakan **Euclidean Distance**.

**Rumus Jarak Numerik Euclidean:**
$$d_{numerik} = \sqrt{(X_{norm_1} - Y_{norm_1})^2 + (X_{norm_2} - Y_{norm_2})^2}$$

*Contoh Aplikasi:* Dimana $X$ adalah nilai normalisasi data 1 dan $Y$ adalah nilai normalisasi rujukan target. Pengkuadratan digunakan untuk menghilangkan nilai negatif dan mengakumulasi keseluruhan perbedaan jarak multidimensinya.

---

## Blok 6 (Kolom N): Jarak Gabungan (Final Distance)

Langkah terakhir dari pengukuran kemiripan antar baris data (instance) adalah dengan menggabungkan komputasi Jarak Kategorikal (Kolom L) dan Jarak Numerik (Kolom M). Ini akan menjadi nilai referensi utama dari total deviasi / kemiripan dari target.

**Rumus Jarak Final:**
$$Distance_{total} = \sqrt{(d_{kategorikal})^2 + (d_{numerik})^2}$$

Semakin kecil nilai di kolom ini, maka semakin "**mirip / dekat**" sifat antar-karyawan tersebut secara holistik (baik dari segi umur, status gaji, dan properti posisinya di kantor).

---

## Blok 7 (Kolom P-V): Penanganan Missing Value (Metode KNN)

Kolom P hingga V digunakan untuk mempraktikkan pengisian **Missing Value** menggunakan algoritma **K-Nearest Neighbors (KNN)** dengan $K=2$ (2 Tetangga Terdekat).

Pada dataset ini, Gaji karyawan **K013** (HR, D3, Menikah, Tahun 1993) sengaja dihilangkan untuk disimulasikan sebagai *missing value*. Nilai asli Gaji K013 sebelum dihapus adalah **5.500.000**. Semua jarak pada Blok 7 dihitung terhadap **K013** sebagai titik rujukan (target). Berikut struktur kolomnya:

| Kolom | Isi |
|-------|-----|
| **P** | Jarak Departemen (ke K013) |
| **Q** | Jarak Pendidikan (ke K013) |
| **R** | Jarak Status Pernikahan (ke K013) |
| **S** | Jarak Kategorikal Gabungan |
| **T** | Jarak Numerik Khusus (hanya Tahun Lahir, karena Gaji = missing) |
| **U** | Jarak Final ke K013 |
| **V** | Hasil Missing Value (2 Tetangga Terdekat / KNN) |

---

### Langkah 1: Hitung Jarak Kategorikal Setiap Karyawan ke K013

Data K013: **HR, D3, Menikah, Tahun 1993**

Setiap karyawan dibandingkan kategorinya dengan K013 menggunakan **Simple Matching** (sama = 0, beda = 1).

**Contoh K001** (IT, S1, Menikah) vs K013 (HR, D3, Menikah):
| Atribut | K001 | K013 | Sama/Beda | Jarak |
|---------|------|------|-----------|-------|
| Departemen (P) | IT | HR | Beda | **1** |
| Pendidikan (Q) | S1 | D3 | Beda | **1** |
| Status (R) | Menikah | Menikah | Sama | **0** |

**Kolom S (Jarak Kategorikal Gabungan):**
$$S = \frac{P + Q + R}{3} = \frac{1 + 1 + 0}{3} = \frac{2}{3} \approx 0,6667$$

**Contoh K016** (Operasional, D3, Menikah) vs K013 (HR, D3, Menikah):
| Atribut | K016 | K013 | Sama/Beda | Jarak |
|---------|------|------|-----------|-------|
| Departemen (P) | Operasional | HR | Beda | **1** |
| Pendidikan (Q) | D3 | D3 | Sama | **0** |
| Status (R) | Menikah | Menikah | Sama | **0** |

$$S = \frac{1 + 0 + 0}{3} = \frac{1}{3} \approx 0,3333$$

**Contoh K018** (HR, S1, Menikah) vs K013 (HR, D3, Menikah):
| Atribut | K018 | K013 | Sama/Beda | Jarak |
|---------|------|------|-----------|-------|
| Departemen (P) | HR | HR | Sama | **0** |
| Pendidikan (Q) | S1 | D3 | Beda | **1** |
| Status (R) | Menikah | Menikah | Sama | **0** |

$$S = \frac{0 + 1 + 0}{3} = \frac{1}{3} \approx 0,3333$$

**K013 terhadap dirinya sendiri:**
Semua kategori identik, maka $P = 0, Q = 0, R = 0, S = 0$.

---

### Langkah 2: Hitung Jarak Numerik Khusus (Kolom T)

Karena atribut **Gaji diasumsikan hilang** (*missing*), maka jarak numerik **hanya dihitung berdasarkan Tahun Lahir** (yang sudah dinormalisasi).

**Rumus:**
$$T = | \text{Norm\_Tahun\_Karyawan} - \text{Norm\_Tahun\_K013} |$$

Normalisasi Tahun K013: $\frac{1993 - 1985}{2001 - 1985} = \frac{8}{16} = 0,5$

**Contoh K001** (Norm Tahun = 0,3125):
$$T = | 0,3125 - 0,5 | = 0,1875$$

**Contoh K016** (Norm Tahun = 0,5625):
$$T = | 0,5625 - 0,5 | = 0,0625$$

**Contoh K018** (Norm Tahun = 0,4375):
$$T = | 0,4375 - 0,5 | = 0,0625$$

---

### Langkah 3: Hitung Jarak Final ke K013 (Kolom U)

**Rumus:**
$$U = \sqrt{S^2 + T^2}$$

**Contoh K001:**
$$U = \sqrt{(0,6667)^2 + (0,1875)^2} = \sqrt{0,4444 + 0,0352} = \sqrt{0,4796} \approx 0,6925$$
*(Cocok dengan spreadsheet: `0,6925320891` ✓)*

**Contoh K016:**
$$U = \sqrt{(0,3333)^2 + (0,0625)^2} = \sqrt{0,1111 + 0,0039} = \sqrt{0,1150} \approx 0,3391$$
*(Cocok dengan spreadsheet: `0,3391420958` ✓)*

**Contoh K018:**
$$U = \sqrt{(0,3333)^2 + (0,0625)^2} = \sqrt{0,1111 + 0,0039} = \sqrt{0,1150} \approx 0,3391$$
*(Cocok dengan spreadsheet: `0,3391420958` ✓)*

**Contoh K022** (IT, D3, Menikah, Norm Tahun = 0,625):
- P=1, Q=0, R=0 → S = 1/3 = 0,3333
- T = |0,625 - 0,5| = 0,125
$$U = \sqrt{(0,3333)^2 + (0,125)^2} = \sqrt{0,1111 + 0,0156} = \sqrt{0,1267} \approx 0,3560$$
*(Cocok dengan spreadsheet: `0,3560001561` ✓)*

---

### Langkah 4: Urutkan dan Pilih 2 Tetangga Terdekat

Setelah semua nilai Kolom U dihitung, kita urutkan dari yang terkecil (paling mirip dengan K013):

| Peringkat | Karyawan | Jarak Final (U) | Gaji |
|-----------|----------|-----------------|------|
| — | K013 (target) | 0 (diri sendiri) | — |
| **1** | **K016** | **0,3391** | **5.000.000** |
| **2** | **K018** | **0,3391** | **6.500.000** |
| 3 | K022 | 0,3560 | 6.000.000 |
| 4 | K004 | 0,3560 | 5.500.000 |
| 5 | K009 | 0,5017 | 11.000.000 |

Karena $K=2$, kita ambil **K016** (Operasional, D3, Menikah, 1994) dan **K018** (HR, S1, Menikah, 1992) sebagai 2 tetangga terdekat.

---

### Langkah 5: Estimasi Missing Value (Rata-rata Gaji Tetangga)

Karena atribut yang hilang bersifat **Numerik** (Gaji), maka kita gunakan **rata-rata (Mean)** dari gaji 2 tetangga terdekat:

$$Gaji_{imputasi} = \frac{Gaji_{K016} + Gaji_{K018}}{2} = \frac{5.000.000 + 6.500.000}{2} = 5.750.000$$

---

### Kesimpulan

Nilai Gaji K013 yang awalnya hilang diestimasi menjadi **Rp 5.750.000** berdasarkan rata-rata gaji dari 2 karyawan yang paling mirip karakteristiknya (departemen, pendidikan, status, dan usia) dengan K013. Nilai asli Gaji K013 sebelum dihapus adalah **5.500.000**, sehingga selisih antara tebakan KNN dan nilai asli hanya **250.000** — menunjukkan bahwa metode KNN cukup akurat dalam memperkirakan data yang hilang.
