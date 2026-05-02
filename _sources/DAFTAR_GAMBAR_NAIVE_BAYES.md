# Daftar Gambar yang Perlu Dibuat untuk naive_bayes.md

## Checklist Screenshot KNIME

Berikut adalah daftar gambar yang perlu Anda buat dari workflow KNIME Anda:

### ✅ Sudah Ada
- [x] `workflow_final.png` - Screenshot keseluruhan workflow

### ❌ Perlu Dibuat

1. **node_excel_reader.png**
   - Screenshot: Klik kanan node Excel Reader → Configure
   - Tampilkan: Panel konfigurasi dengan file path dan pengaturan sheet

2. **node_normalizer.png**
   - Screenshot: Klik kanan node Normalizer → Configure
   - Tampilkan: Panel dengan kolom yang dipilih (jam_belajar, kehadiran, nilai_tugas)
   - Pastikan terlihat: Min-Max normalization method

3. **node_partitioning.png**
   - Screenshot: Klik kanan node Partitioning → Configure
   - Tampilkan: Pengaturan 80% training, 20% testing
   - Pastikan terlihat: Stratified sampling option

4. **node_python_learner.png**
   - Screenshot: Klik kanan Python Script (Learner) → Configure
   - Tampilkan: Editor kode Python dengan script lengkap
   - Pastikan terlihat: Input/Output port configuration

5. **node_python_predictor.png**
   - Screenshot: Klik kanan Python Script (Predictor) → Configure
   - Tampilkan: Editor kode Python dengan script lengkap
   - Pastikan terlihat: Input ports (Object + Table) dan Output port

6. **node_scorer.png**
   - Screenshot: Klik kanan node Scorer → Configure
   - Tampilkan: Pemilihan kolom First (lulus) dan Second (Prediksi_Lulus)

7. **node_confusion_matrix.png**
   - Screenshot: Klik kanan Scorer → View: Confusion Matrix
   - Tampilkan: Matriks confusion dengan angka TP, TN, FP, FN

8. **hasil_scorer.png**
   - Screenshot: Klik kanan Scorer → View: Statistics
   - Tampilkan: Tabel dengan Accuracy, Precision, Recall, F1-Score

## Cara Membuat Screenshot

### Untuk Node Configuration:
1. Klik kanan pada node
2. Pilih "Configure..."
3. Screenshot panel konfigurasi
4. Crop agar fokus pada bagian penting

### Untuk View Results:
1. Klik kanan pada node yang sudah dieksekusi (hijau)
2. Pilih menu "View: ..."
3. Screenshot hasil visualisasi
4. Crop jika perlu

## Lokasi Penyimpanan
Simpan semua gambar di folder: `images/`

## Tips Screenshot
- Gunakan resolusi yang cukup tinggi (minimal 1920x1080)
- Pastikan teks terbaca dengan jelas
- Crop bagian yang tidak relevan (menu bar, taskbar, dll)
- Format: PNG (untuk kualitas terbaik)
- Nama file: sesuai dengan daftar di atas (huruf kecil, underscore)
