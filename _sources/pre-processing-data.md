# Pre-Processing Data

Dalam siklus penambangan data (*data mining*), **Pre-Processing Data** (Pra-pemrosesan Data) adalah salah satu tahapan yang paling awal dan paling krusial. Secara sederhana, pre-processing adalah serangkaian proses untuk membersihkan, mengubah, dan mengintegrasikan data mentah (yang biasanya berantakan, tidak konsisten, atau memiliki nilai yang hilang) menjadi format yang lebih rapi dan terstruktur sebelum diproses oleh algoritma.

**Mengapa tahap ini sangat krusial?**
Data mentah di dunia nyata seringkali tidak sempurna. Sering ditemukan *noise* (data error/tidak wajar), *missing value* (data kosong/hilang), atau atribut dengan rentang skala yang berbeda-beda jauh. Berlaku prinsip analitik fundamental: *"Garbage In, Garbage Out"* (Sampah Masuk, Sampah Keluar). Jika data mentah yang buruk langsung digunakan untuk pemodelan, maka akurasi algoritma akan anjlok drastis dan menghasilkan kesimpulan yang menyesatkan. Oleh karena itu, kualitas model data mining sangat bergantung pada kualitas data yang dipersiapkan.

**Langkah-langkah Umum Pre-Processing:**
1. **Data Cleaning (Pembersihan Data):** Proses untuk menangani *missing value*, menghaluskan *noisy data*, mengidentifikasi atau membuang temuan *outliers* (pencilan), serta memperbaiki ketidakkonsistenan isi data.
2. **Data Integration (Integrasi Data):** Menggabungkan data yang berasal dari berbagai sumber yang terpisah (multipel *database* atau ragam file teks) menjadi satu gudang data (dataset) yang koheren.
3. **Data Transformation (Transformasi Data):** Proses mengubah nilai skala dan format data agar optimal saat fase *mining*, salah satu metode yang paling populer adalah **Normalisasi Data** (menyamakan jangkauan/rentang data numerik).
4. **Data Reduction (Reduksi Data):** Taktik menyusutkan ukuran volume asal data secara masif (agar komputasi lebih efisien) namun tetap berupaya keras mempertahankan jejak representasi/karakteristik analitik aslinya (misal dengan reduksi dimensi data/seleksi fitur).
