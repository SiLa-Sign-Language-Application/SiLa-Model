# SiLa Gesture Recognition Model

Sila_Model adalah berbagai file yang berisi proses pembuatan data landmark, membuat model dan pelatihan model gesture recognition untuk **Bahasa Isyarat Indonesia (SIBI)** menggunakan MediaPipe dan TensorFlow.

## 📁 Struktur File

```bash
├── collect_landmark/                 # Folder untuk skrip pengumpulan data
├── dataset/                          # Folder untuk menyimpan dataset mentah atau hasil ekstrak
│   └── [gesture_label].csv           # Folder isi CSV per gesture
├── model/                            # Folder untuk menyimpan model dan label
│   ├── gesture_mlp_model.h5         # ✅ Model hasil pelatihan (akan dihasilkan setelah training)
│   └── label.json                   # ✅ Label encoder hasil training
    └── test.py                      # Melakukan pengetesan hasil model sebelum dideploy ke backend   
├── dataset_all.csv                  # Dataset gabungan hasil merge semua CSV
├── sila_model.py                    # Skrip utama untuk training model
├── SiLa_Model.ipynb                 # Notebook versi Colab
├── requirements.txt                 # Daftar dependensi Python
├── README.md                        # Dokumentasi proyek

```
Berikut adalah penjelasan singkat terkait penggunaan setiap file

## ✋ collect_landmark
Folder collect_landmark berisi script untuk mengumpulkan data gesture tangan secara real-time menggunakan webcam dan MediaPipe Hands. Data yang dikumpulkan berupa koordinat 21 titik landmark tangan (x, y) dan disimpan dalam format CSV.

## 📁 Dataset
Folder dataset berisi kumpulan file CSV hasil perekaman gesture tangan menggunakan script dari folder collect_landmark. Setiap file merepresentasikan satu label gesture, misalnya huruf A–Z atau gesture spesial seperti spasi (space).

## 🧠 Model
Folder model/ digunakan untuk menyimpan semua file penting terkait model Machine Learning hasil pelatihan. File gesture_mlp_model.h5 merupakan model MLP yang telah dilatih untuk mengenali gesture tangan berdasarkan 42 koordinat landmark (x dan y dari 21 titik), dan digunakan dalam proses inferensi di backend. File label.json berisi mapping label karakter gesture ke indeks numerik yang diperlukan untuk interpretasi output model secara akurat. Selain itu, terdapat test.py, yaitu skrip pengujian lokal yang digunakan untuk memastikan model dan label bekerja dengan benar sebelum diintegrasikan ke dalam API FastAPI.

## 📚 SiLa_Model.ipynb / sila_model.py 

### Load data dari Google Drive
Dataset berupa file CSV yang berisi koordinat landmark tangan dikumpulkan sebelumnya dan disimpan di Google Drive. Notebook ini menggunakan integrasi `drive.mount()` untuk mengakses folder tersebut dan memuat seluruh file CSV secara otomatis.

💻 Untuk Lokal:

Jika kamu menjalankan di lokal, gunakan path ke folder dataset/ yang sudah ada:
```bash
folder_path = "dataset/Dataset A-Z & Space"
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

- **Training Accuracy**: 91.12%
- **Validation Accuracy**: 93.98%
- **Test Accuracy**: 93.33%

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
