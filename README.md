# SiLa Gesture Recognition Model

Notebook ini berisi proses pembuatan dan pelatihan model gesture recognition untuk **Bahasa Isyarat Indonesia (SIBI)** menggunakan MediaPipe dan TensorFlow.

## 📚 Deskripsi Notebook

### Load data dari Google Drive
Dataset berupa file CSV yang berisi koordinat landmark tangan dikumpulkan sebelumnya dan disimpan di Google Drive. Notebook ini menggunakan integrasi `drive.mount()` untuk mengakses folder tersebut dan memuat seluruh file CSV secara otomatis.

💻 Untuk Lokal:

Jika kamu menjalankan di lokal, gunakan path ke folder dataset/ yang sudah ada:
```bash
folder_path = 'dataset'
```

### Merge Data
Setiap file CSV merepresentasikan satu gesture tertentu. Semua file digabung menjadi satu DataFrame besar menggunakan `pandas.concat()`. Hasilnya disimpan sebagai `dataset_all.csv` yang menjadi basis data untuk pelatihan model.

## ⚙️ Teknologi yang Digunakan

- TensorFlow / Keras
- MediaPipe
- Pandas, NumPy
- Matplotlib

## 🧪 Langkah-Langkah Utama

1. Load dataset hasil ekstraksi koordinat landmark tangan.
2. Preprocessing data: normalisasi, encoding label.
3. Bangun model: MLP.
4. Training model dan evaluasi.
5. Simpan model ke format `.h5` untuk deployment.

## 📈 Hasil Pelatihan

- **Training Accuracy**: 93.14%
- **Validation Accuracy**: 94.21%
- **Test Accuracy**: 94.91%

## 📁 Struktur Output

```bash
├── gesture_mlp_model.h5       # Model hasil pelatihan
├── label.json          # Encoder label gesture
```

## 🚀 Cara Menjalankan

### 🧪 Opsi 1: Jalankan di Google Colab atau Jupyter Notebook

1. Buka file `.ipynb` (notebook) menggunakan:
   - [Google Colab](https://colab.research.google.com/)  
     atau
   - Jupyter Notebook (dari Anaconda atau `jupyter notebook` CLI)

2. Jalankan cell secara berurutan dari atas ke bawah.

> Pastikan runtime sudah diaktifkan dan dependensi sudah terinstall.

---

### 💻 Opsi 2: Jalankan di Visual Studio Code (VS Code)

1. **Install Python extension** di VS Code (jika belum ada).
2. Buka folder proyek yang berisi file `.ipynb`.
3. Klik file notebook `.ipynb` untuk membukanya.
4. Akan muncul tombol ▶️ (Run Cell) di setiap cell.
5. Jalankan cell satu per satu dari atas ke bawah.

> Jika VS Code tidak mengenali Python environment, pastikan kamu sudah menginstall Python dan environment-nya telah terdeteksi (`Ctrl+Shift+P` → “Python: Select Interpreter”).

---

### ⚙️ Jika notebook butuh environment tambahan

Buat virtual environment dan install dependensi:

```bash
python -m venv venv
source venv/bin/activate  # atau venv\Scripts\activate di Windows
pip install -r requirements.txt
```

## 📝 Catatan

- Dataset harus sudah tersedia dalam format CSV dengan kolom koordinat landmark dan label gesture.
- Gunakan MediaPipe Hands untuk membuat dataset gesture jika belum tersedia. Untuk membuatnya bisa langsung ke folder `collect_landmark` untuk membuat dataset gesture.
