## 2. Feature Scaling (Normalisasi Data)

**Feature Scaling** (Penyekalaan Fitur) adalah salah satu tahapan krusial dalam prapemrosesan data yang bertujuan untuk mendesak / menempatkan seluruh variabel bebas (*independent features*) dari suatu himpunan data ke dalam rentang rasio angka matematika yang setara dan seragam.

**Alasan Mengapa Skala Fitur Harus Seragam:**
Mayoritas model algoritma *Machine Learning*, di antaranya seperti _K-Nearest Neighbors (KNN)_ atau _Support Vector Machines (SVM)_, beroperasi berdasarkan perhitungan **jarak** (*Euclidean distance*). Apabila sebuah himpunan data mencakup atribut dengan rentang nilai yang tajam perbedaannya — contohnya fitur **"Umur"** berkisar belasan hingga puluhan sentimeter, sedangkan **"Harga Rumah"** berada di kisaran ratusan juta hingga miliaran — maka kalkulasi jarak algoritma akan secara teknis didominasi secara masif oleh rentang fitur yang nilainya raksasa (dalam hal ini *Harga Rumah*). Hal ini bisa membutakan model analitik. Itulah sebab mengapa meredam seluruh rentang menjadi porsi seimbang sangat mutlak dibutuhkan demi memastikan keadilan sumbangsih bobot evaluasi data (*equal weight contribution*).

Berikut adalah jenis-jenis metode standarisasi matematis yang paling sering diterapkan. Untuk mempermudah pemahaman, bayangkan kita memiliki sebuah *dataset mini* (*dummy data*) tentang **Pendapatan Bersih** (dalam nominal jutaan) dari 4 orang pemuda:

| Nama | Pendapatan (X) |
|---|---|
| A | 2 |
| B | 4 |
| C | 8 |
| D | 6 |

Dari data di atas, kita dapat menyarikan fakta-fakta statistik awalnya:
* **Min** = 2
* **Max** = 8
* **Mean (Rata-rata / $\mu$)** = $\frac{(2 + 4 + 8 + 6)}{4} = 5$
* **Standar Deviasi ($\sigma$) populasi** = $2.236$
* **Nilai Absolut Max** = 8

---

### 2.1 Min-Max Normalization

*Min-Max Scaling* adalah teknik yang menekan pergeseran nilai minimum dan nilai maksimum dari sampel data ke dalam proporsi rasional baru yang relatif lebih mungil. Formasi yang paling disukai adalah menyematkan rentang skala konversi menjadi area eksklusif antara batas `0` hingga batas mentok `1`.

* **Rumus:**
  $$X_{new} = \frac{X - X_{min}}{X_{max} - X_{min}}$$

* **Keterangan:**
  * $X$: Nilai data orisinil observasi saat ini.
  * $X_{min}, X_{max}$: Nilai terkecil dan terbesar dalam fitur tersebut.

**Contoh Perhitungan Min-Max:**
Penyebut (Jarak Min ke Max) = $8 - 2 = 6$
* Pendapatan A ($X=2$): $X_{new} = \frac{2 - 2}{6} = \frac{0}{6} = \mathbf{0}$
* Pendapatan B ($X=4$): $X_{new} = \frac{4 - 2}{6} = \frac{2}{6} \approx \mathbf{0.333}$
* Pendapatan C ($X=8$): $X_{new} = \frac{8 - 2}{6} = \frac{6}{6} = \mathbf{1}$
* Pendapatan D ($X=6$): $X_{new} = \frac{6 - 2}{6} = \frac{4}{6} \approx \mathbf{0.667}$

*Hasil:* Seluruh nilai pendapatan kini bertransformasi rata berada secara padat di petak jangkauan $0$ hingga $1$.

---

### 2.2 Standarisasi (Z-Score)

Metode ini memfokuskan pusat distribusi data sedemikian rupa agar mendapati letak rata-rata (*Mean*) di titik **0** dan memancarkan standar deviasi berjumlah **1**. Standarisasi *Z-Score* berkhasiat karena ia tidak terlalu sesak memberangus nilai ekstrem pinggiran (*outlier*) menjadi sekadar desimal mungil.

* **Rumus:**
  $$Z = \frac{X - \mu}{\sigma}$$

* **Keterangan:**
  * $\mu$ (Mu): Nilai titik pusat rata-rata.
  * $\sigma$ (Sigma): Deviasi standar (rentang jangkauan pergerakan simpangan).

**Contoh Perhitungan Z-Score:**
Diketahui $\mu = 5$ dan $\sigma = 2.236$
* Pendapatan A ($X=2$): $Z = \frac{2 - 5}{2.236} = \frac{-3}{2.236} \approx \mathbf{-1.341}$
* Pendapatan B ($X=4$): $Z = \frac{4 - 5}{2.236} = \frac{-1}{2.236} \approx \mathbf{-0.447}$
* Pendapatan C ($X=8$): $Z = \frac{8 - 5}{2.236} = \frac{3}{2.236} \approx \mathbf{1.341}$
* Pendapatan D ($X=6$): $Z = \frac{6 - 5}{2.236} = \frac{1}{2.236} \approx \mathbf{0.447}$

*Hasil:* Nilai pendapatan kini berpusat persis seimbang di angka $0$, di mana subjek yang berpendapatan di bawah rata-rata mendapatkan identitas deviasi bernilai negatif.

---

### 2.3 Decimal Scaling

*Decimal Scaling* memperpendek jangkauan dengan mekanisme memundurkan titik posisi desimal dari setiap angka asli menuju kiri, didasarkan murni pada tingkat digit absolut (ukuran panjang nol) yang menempel lekat dari nilai paling maksimal yang ada dalam observasi.

* **Rumus:**
  $$X_{new} = \frac{X}{10^j}$$

* **Keterangan:**
  * $j$ adalah unit kekuatan absolut (*power*) eksponen letak sepuluh yang merepresentasikan jumlah digit maksimal.

**Contoh Perhitungan Decimal Scaling:**
Di antara angka $2, 4, 8, 6$, angka mutlak tertinggi adalah **$8$**. Angka 8 hanya memiliki **1 digit**, sehingga nilai pangkat $j = 1$. Pembaginya akan jatuh pada $10^1 = 10$.
* Pendapatan A ($X=2$): $X_{new} = \frac{2}{10^1} = \mathbf{0.2}$
* Pendapatan B ($X=4$): $X_{new} = \frac{4}{10^1} = \mathbf{0.4}$
* Pendapatan C ($X=8$): $X_{new} = \frac{8}{10^1} = \mathbf{0.8}$
* Pendapatan D ($X=6$): $X_{new} = \frac{6}{10^1} = \mathbf{0.6}$

*(Catatan bonus: Andaikata ada pemuda E berpendapatan $150$ (berjumlah 3 digit), maka $j=3$ alias $1000$ dan seluruh angka barisan dari A hingga E wajib diseragamkan dibagi dengan angka seribu `1000`).*
