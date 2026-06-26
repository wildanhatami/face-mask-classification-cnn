# 😷 Klasifikasi Kondisi Penggunaan Masker pada Citra Wajah

> **Mata Kuliah:** Pengenalan Citra Digital (PCD)  
> **Topik:** Topik 5 — Klasifikasi Penggunaan Masker atau Kondisi Wajah Berdasarkan Citra  
> **Institusi:** Ilmu Komputer, Sekolah Sains Data Matematika dan Informatika, Institut Pertanian Bogor (IPB)  
> **Tahun:** 2026

---

## 📌 Deskripsi Proyek

Proyek ini membangun sistem klasifikasi kondisi penggunaan masker pada citra wajah menggunakan dua pendekatan utama:

1. **Machine Learning Klasik** — Ekstraksi fitur *Histogram of Oriented Gradients* (HOG) + *Support Vector Machine* (SVM)
2. **Deep Learning** — *Convolutional Neural Network* (CNN) sederhana yang dibangun dari awal (*from scratch*)

Pipeline yang diterapkan meliputi akuisisi data, eksplorasi dataset, praproses citra, *image enhancement* menggunakan CLAHE, ekstraksi fitur, pelatihan model, evaluasi, dan analisis perbandingan skenario.

---

## 👥 Anggota Kelompok

| No. | Nama | NIM |
|-----|------|-----|
| 1 | Muhammad Wildan Hatami | G6401231009 |
| 2 | Emi Purmawadi | G6401231032 |
| 3 | M. Reyhan Hermawan | G6401231042 |
| 4 | Hannan Azhari Batubara | G6401231052 |
| 5 | Revandra Athaya Rizkika | G6401231125 |

---

## 📂 Struktur Proyek

```
Project Akhir/
├── CODE.ipynb                                          # Notebook utama
├── LAPORAN AKHIR PROYEK PENGENALAN CITRA DIGITAL.docx # Laporan lengkap
├── PROPOSAL.docx                                       # Proposal proyek
├── Salinan dari PCD Kuliah_Project Akhir_Topik 5_Kelompok.pdf  # Slide presentasi
└── README.md
```

---

## 🗂️ Dataset

- **Sumber:** [Face Mask Detection — Kaggle](https://www.kaggle.com/datasets/vijaykumar1799/face-mask-detection) oleh Vijaykumar1799
- **Jumlah kelas:** 3 kelas
  - ✅ Menggunakan masker dengan **benar**
  - ⚠️ Menggunakan masker dengan **tidak tepat**
  - ❌ **Tidak** menggunakan masker
- **Split data:**
  - Training: 70%
  - Validasi: 15%
  - Testing: 15% (1.348 citra, distribusi kelas seimbang)

---

## 🔬 Metodologi

### 1. Praproses Citra
- Resize citra ke **128 × 128 piksel**
- Konversi ke format RGB

### 2. Image Enhancement — CLAHE
- *Contrast Limited Adaptive Histogram Equalization* (CLAHE)
- Diterapkan pada kanal Luminance (L) dalam ruang warna LAB
- Parameter: `clipLimit=2.0`, `tileGridSize=(8, 8)`

### 3. Ekstraksi Fitur — HOG
- `orientations=9`, `pixels_per_cell=(8, 8)`, `cells_per_block=(2, 2)`
- `block_norm='L2-Hys'`, `transform_sqrt=True`
- Menghasilkan vektor fitur sepanjang **8.100 elemen** per citra

### 4. Model Klasifikasi

#### Support Vector Machine (SVM)
- Kernel: **Linear**, `C=1.0`, `random_state=42`
- Input: vektor fitur HOG

#### Convolutional Neural Network (CNN)
| Komponen | Konfigurasi |
|----------|-------------|
| Input | RGB 128 × 128 × 3 |
| Augmentasi | RandomFlip, RandomRotation 0.1, RandomZoom 0.1 |
| Blok Konvolusi | Conv2D 32 → Conv2D 64 → Conv2D 128 + MaxPooling2D |
| Klasifikasi | Flatten → Dense 128 (ReLU) → Dropout 0.5 → Dense 3 (Softmax) |
| Training | Adam, sparse categorical crossentropy, batch size 32, maks. 15 epoch + Early Stopping |

---

## 📊 Hasil Evaluasi

| Peringkat | Model | Enhancement | Accuracy | Precision | Recall | F1 Macro | Waktu (s) |
|-----------|-------|-------------|----------|-----------|--------|----------|-----------|
| 🥇 1 | CNN | ❌ Tidak | **0.9889** | 0.9890 | 0.9889 | **0.9889** | 3.093 |
| 🥈 2 | HOG-SVM | ❌ Tidak | 0.9577 | 0.9576 | 0.9577 | 0.9576 | 58.92 |
| 🥉 3 | HOG-SVM | ✅ Ya | 0.9510 | 0.9510 | 0.9511 | 0.9509 | 59.58 |
| 4 | CNN | ✅ Ya | 0.9036 | 0.9059 | 0.9035 | 0.9026 | 1.035 |

### 🏆 Skenario Terbaik: CNN tanpa Enhancement

| Kelas | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| Menggunakan masker dengan benar | 0.99 | 0.98 | 0.99 | 449 |
| Menggunakan masker dengan tidak tepat | 0.98 | 1.00 | 0.99 | 449 |
| Tidak menggunakan masker | 1.00 | 0.98 | 0.99 | 450 |
| **Macro Average** | **0.99** | **0.99** | **0.99** | **1.348** |

---

## 💡 Temuan Utama

- **CNN tanpa enhancement** memberikan performa terbaik (accuracy ~98.9%)
- Penggunaan CLAHE justru **menurunkan** performa CNN secara signifikan (~8.5 poin persentase)
- HOG-SVM tetap menjadi baseline yang kuat dan efisien (training hanya ~59 detik vs ~3.093 detik untuk CNN)
- Enhancement yang tampak lebih baik secara visual tidak selalu meningkatkan performa model

---

## 🛠️ Teknologi yang Digunakan

| Kategori | Library / Tools |
|----------|-----------------|
| Bahasa | Python 3 |
| Notebook | Jupyter / Google Colab |
| Dataset | `kagglehub` |
| Pengolahan Citra | `OpenCV (cv2)`, `Pillow (PIL)`, `scikit-image` |
| Machine Learning | `scikit-learn` (SVM, metrics) |
| Deep Learning | `TensorFlow / Keras` |
| Visualisasi | `matplotlib`, `seaborn`, `graphviz` |
| Utilitas | `numpy`, `pandas`, `joblib` |

---

## 🚀 Cara Menjalankan

### Prasyarat

Pastikan Anda memiliki Python 3 dan Jupyter Notebook terinstal. Disarankan menggunakan **Google Colab** untuk memanfaatkan GPU.

### Langkah-langkah

1. **Clone repositori ini:**
   ```bash
   git clone https://github.com/<username>/<repo-name>.git
   cd "<repo-name>"
   ```

2. **Instal dependensi:**
   ```bash
   pip install kagglehub opencv-python scikit-image scikit-learn tensorflow matplotlib seaborn graphviz pandas numpy joblib pillow
   ```

3. **Siapkan Kaggle API Key:**
   - Buat akun di [Kaggle](https://www.kaggle.com/) dan unduh `kaggle.json`
   - Letakkan file di `~/.kaggle/kaggle.json`

4. **Buka notebook:**
   ```bash
   jupyter notebook CODE.ipynb
   ```
   Atau upload ke Google Colab dan jalankan sel secara berurutan.

---

## ⚠️ Keterbatasan Sistem

- Dataset berasal dari satu sumber, sehingga generalisasi pada kondisi nyata perlu diuji lebih lanjut
- Ukuran input 128×128 dapat menghilangkan detail kecil (tali masker, lipatan masker)
- Sistem belum menggunakan deteksi wajah (*face detection*) secara eksplisit
- Model CNN dibangun *from scratch* tanpa *transfer learning*
- Evaluasi belum mencakup data kamera real-time atau dataset eksternal

---

## 🔮 Saran Pengembangan Lanjutan

- [ ] Tambahkan **face detection/alignment** sebelum klasifikasi
- [ ] Terapkan **transfer learning** (MobileNetV2, EfficientNet, ResNet)
- [ ] Lakukan **hyperparameter tuning** pada CNN
- [ ] Kembangkan **enhancement adaptif** (hanya CLAHE pada citra berkontras rendah)
- [ ] Deploy model ke **TensorFlow Lite** untuk perangkat terbatas
- [ ] Bangun **demo real-time** berbasis webcam atau web app
- [ ] Tambahkan **analisis fairness** terhadap variasi pencahayaan, pose, dan jenis masker

---

## 📚 Referensi

- Agustina, L., & Hakim, L. (2025). *Praktik Analisis Citra Digital dan Pembelajaran Mesin*. Bogor: Departemen Ilmu Komputer IPB Press.
- Dalal, N., & Triggs, B. (2005). Histograms of oriented gradients for human detection. *CVPR 2005*. IEEE Computer Society.
- Gonzalez, R. C., & Woods, R. E. (2018). *Digital Image Processing* (4th ed.). Pearson.
- LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. *Nature*, 521(7553), 436–444.
- Vijaykumar1799. (n.d.). Face Mask Detection. Kaggle. https://www.kaggle.com/datasets/vijaykumar1799/face-mask-detection
- Zuiderveld, K. (1994). Contrast Limited Adaptive Histogram Equalization. *Graphics Gems IV*, 474–485.

---

<div align="center">
  <sub>Dibuat sebagai Proyek Akhir Mata Kuliah Pengenalan Citra Digital — Institut Pertanian Bogor, 2026</sub>
</div>
