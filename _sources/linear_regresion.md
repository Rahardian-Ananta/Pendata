# Regresi Linear


## 1. Perhitungan Secara Analitik (Metode Matriks)

### 1.1 Setup Data Koordinat

Berdasarkan grafik koordinat plot, berikut adalah data pasang surut titik $(x, y)$ yang digunakan sebagai basis data:

* Titik A = $(2, 2)$
* Titik B = $(4, 3)$
* Titik C = $(5, 5)$
* Titik D = $(3, 4)$
* Titik E = $(3, 3)$
* Titik F = $(4, 5)$
* Titik G = $(5, 6)$

### Tabel Data Sederhana
| Titik | Nilai $X$ (Fitur) | Nilai $Y$ (Target) |
| :---: | :---: | :---: |
| **A** | 2 | 2 |
| **B** | 4 | 3 |
| **C** | 5 | 5 |
| **D** | 3 | 4 |
| **E** | 3 | 3 |
| **F** | 4 | 5 |
| **G** | 5 | 6 |

Untuk mencari koefisien regresi linier $y = \beta_0 + \beta_1 x$ dengan rumus matriks (*Normal Equation*), kita gunakan rumus:


$$\hat{\beta} = (X^T X)^{-1} X^T Y$$

Di mana:

* $\beta_0$ adalah *Intercept* (titik potong sumbu Y)
* $\beta_1$ adalah *Slope* (kemiringan garis)

### Langkah A: Menyusun Matriks $X$ dan Vektor $Y$

Matriks $X$ berukuran $7 \times 2$. Kolom pertama diisi angka 1 (sebagai basis untuk mencari *intercept* $\beta_0$), dan kolom kedua diisi oleh nilai koordinat $x$ dari titik A sampai G. Vektor $Y$ berisi koordinat $y$.

$$X = \begin{bmatrix} 1 & 2 \\ 1 & 4 \\ 1 & 5 \\ 1 & 3 \\ 1 & 3 \\ 1 & 4 \\ 1 & 5 \end{bmatrix}, \quad Y = \begin{bmatrix} 2 \\ 3 \\ 5 \\ 4 \\ 3 \\ 5 \\ 6 \end{bmatrix}$$

### Langkah B: Menghitung Perkalian Matriks Transpose $X^T X$

Transpose dari matriks $X$ (mengubah baris menjadi kolom) dikalikan dengan matriks $X$ asli:


$$X^T X = \begin{bmatrix} 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 2 & 4 & 5 & 3 & 3 & 4 & 5 \end{bmatrix} \begin{bmatrix} 1 & 2 \\ 1 & 4 \\ 1 & 5 \\ 1 & 3 \\ 1 & 3 \\ 1 & 4 \\ 1 & 5 \end{bmatrix}$$

$$X^T X = \begin{bmatrix} (1+1+1+1+1+1+1) & (2+4+5+3+3+4+5) \\ (2+4+5+3+3+4+5) & (2^2+4^2+5^2+3^2+3^2+4^2+5^2) \end{bmatrix}$$

$$X^T X = \begin{bmatrix} 7 & 26 \\ 26 & 104 \end{bmatrix}$$

### Langkah C: Menghitung Perkalian Matriks Transpose $X^T Y$

$$X^T Y = \begin{bmatrix} 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 2 & 4 & 5 & 3 & 3 & 4 & 5 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \\ 5 \\ 4 \\ 3 \\ 5 \\ 6 \end{bmatrix}$$

$$X^T Y = \begin{bmatrix} (2+3+5+4+3+5+6) \\ (2\times2 + 4\times3 + 5\times5 + 3\times4 + 3\times3 + 4\times5 + 5\times6) \end{bmatrix}$$

$$X^T Y = \begin{bmatrix} 28 \\ (4 + 12 + 25 + 12 + 9 + 20 + 30) \end{bmatrix} = \begin{bmatrix} 28 \\ 112 \end{bmatrix}$$

### Langkah D: Menghitung Invers dari Matriks $(X^T X)^{-1}$

Sebelum mencari invers, kita hitung nilai determinan dari matriks $X^T X$:


$$\text{Determinant } (X^T X) = (7 \times 104) - (26 \times 26) = 728 - 676 = 52$$

Rumus invers matriks $2 \times 2$:


$$(X^T X)^{-1} = \frac{1}{52} \begin{bmatrix} 104 & -26 \\ -26 & 7 \end{bmatrix}$$

### Langkah E: Menghitung Nilai Koefisien $\hat{\beta}$

$$\hat{\beta} = \frac{1}{52} \begin{bmatrix} 104 & -26 \\ -26 & 7 \end{bmatrix} \begin{bmatrix} 28 \\ 112 \end{bmatrix}$$

$$\hat{\beta} = \frac{1}{52} \begin{bmatrix} (104 \times 28) + (-26 \times 112) \\ (-26 \times 28) + (7 \times 112) \end{bmatrix}$$

$$\hat{\beta} = \frac{1}{52} \begin{bmatrix} 2912 - 2912 \\ -728 + 784 \end{bmatrix} = \frac{1}{52} \begin{bmatrix} 0 \\ 56 \end{bmatrix}$$

$$\hat{\beta} = \begin{bmatrix} 0 \\ \frac{56}{52} \end{bmatrix} = \begin{bmatrix} 0 \\ 1.076923 \end{bmatrix}$$

Dari hasil matriks $\hat{\beta}$ di atas, kita dapatkan:

* **$\beta_0$ (Intercept)** = $0$
* **$\beta_1$ (Slope)** = $1.076923$

Sehingga, persamaan garis regresi linier secara analitik adalah:


$$y = 1.076923x$$
![Struktur Workflow KNIME](images/linear_regresion/grafik.png)

**Gambar 1.** grafik .
---

## 2. Pembuatan Program Python Menggunakan `sklearn`

Berikut adalah skrip Python lengkap yang memuat pembuatan model menggunakan library `sklearn` dan sekaligus validasi menggunakan hitungan matriks NumPy agar Anda bisa membandingkan kedua hasilnya secara langsung.

```python
import numpy as np
from sklearn.linear_model import LinearRegression

# 1. Menginputkan data koordinat x dan y dari grafik GeoGebra
# Titik: A(2,2), B(4,3), C(5,5), D(3,4), E(3,3), F(4,5), G(5,6)
X_features = np.array([2, 4, 5, 3, 3, 4, 5]).reshape(-1, 1)
Y_targets = np.array([2, 3, 5, 4, 3, 5, 6])

print("=====================================================")
print("PROSES 1: MENGGUNAKAN LIBRARY SKLEARN (LinearRegression)")
print("=====================================================")

# Inisialisasi model regresi linier dari sklearn
lr_model = LinearRegression()

# Melatih model dengan data yang tersedia
lr_model.fit(X_features, Y_targets)

# Mengambil nilai parameter koefisien hasil training
b0_sklearn = lr_model.intercept_
b1_sklearn = lr_model.coef_[0]

print(f"Hasil Nilai Intercept (beta_0) : {b0_sklearn:.10f}")
print(f"Hasil Nilai Slope     (beta_1) : {b1_sklearn:.10f}")
print(f"Persamaan Garis Regresi        : y = {b1_sklearn:.10f}x")


print("\n=====================================================")
print("PROSES 2: MENGGUNAKAN PERHITUNGAN ANALITIK MATRIKS (NumPy)")
print("=====================================================")

# Membuat matriks X_bias dengan menambahkan kolom angka 1 untuk intercept
kolom_bias = np.ones((X_features.shape[0], 1))
X_matriks = np.hstack((kolom_bias, X_features))

# Menerapkan rumus Normal Equation: beta = inv(X^T * X) * X^T * Y
XT_X = np.dot(X_matriks.T, X_matriks)
XT_Y = np.dot(X_matriks.T, Y_targets)
beta_analitik = np.dot(np.linalg.inv(XT_X), XT_Y)

b0_analitik = beta_analitik[0]
b1_analitik = beta_analitik[1]

print(f"Hasil Nilai Intercept (beta_0) : {b0_analitik:.10f}")
print(f"Hasil Nilai Slope     (beta_1) : {b1_analitik:.10f}")
print(f"Persamaan Garis Regresi        : y = {b1_analitik:.10f}x")

```

---

## 3. Kesimpulan Analisis Perbandingan

Berdasarkan tiga metode pengerjaan yang telah dilakukan, diperoleh kesimpulan nilai koefisien regresi sebagai berikut:

| Parameter | Hasil GeoGebra | Perhitungan Analitik (Matriks) | Hasil `sklearn` (Python) |
| --- | --- | --- | --- |
| **Intercept ($\beta_0$)** | $0$ | $0$ | $0.0000000000$ |
| **Slope ($\beta_1$)** | $1.0769230769$ | $1.0769230769$ | $1.0769230769$ |

> **Kesimpulan Akhir:** Nilai koefisien regresi linier yang didapatkan melalui rumus analitik matriks murni, fungsi otomatis pada library `sklearn`, serta visualisasi fitur `FitLine` pada aplikasi GeoGebra menunjukkan hasil yang **identik dan sinkron secara konsisten**. Garis regresi memotong tepat di titik pusat koordinat $(0,0)$ dengan tingkat kemiringan grafik sebesar $1.076923$.