# SiLa Gesture Recognition Model

Sila_Model adalah berbagai file yang berisi proses pembuatan data landmark, membuat model dan pelatihan model gesture recognition untuk **Bahasa Isyarat Indonesia (SIBI)** menggunakan MediaPipe dan TensorFlow.

Link Dataset : https://www.kaggle.com/datasets/andresaputraginting/gesturesibi

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

### 1. Data Collection *(Dilakukan pada folder `collect_landmark_data.py`)*

Gesture tangan huruf A–Z dan spasi direkam secara real-time menggunakan webcam melalui script `collect_landmark_data.py`.
Setiap sampel menghasilkan 42 nilai (x dan y dari 21 titik landmark) yang disimpan dalam file CSV sesuai labelnya (contoh: `A.csv`, `B.csv`, `space.csv`).

### 2. Load & Merge Data

Setelah proses perekaman gesture selesai, seluruh file CSV hasil perekaman (misalnya `A.csv`, `B.csv`, `space.csv`) dimuat dan digabung menggunakan fungsi `combine_all_csv()` dari script Python. Fungsi ini membaca semua file di dalam folder `dataset/`, menggabungkannya menjadi satu DataFrame besar, lalu menyimpannya ke dalam file utama `dataset_all.csv`. File inilah yang akan digunakan sebagai input untuk proses preprocessing dan pelatihan model Machine Learning.

### 3. Data Preprocessing
Setelah dilakukan merge data, dilanjutkan dengan melalukan preprocessing data.
Langkah preprocessing meliputi:

* Pengecekan dan pembersihan missing values dan duplikat
* Label encoding untuk gesture (A–Z dan spasi)
* Normalisasi fitur agar model lebih stabil
  Dataset kemudian dibagi menjadi data latih dan uji (80:20) secara stratifikasi.

### 4. Model Training

Model dilatih menggunakan arsitektur **Multi-Layer Perceptron (MLP)**:

* 2 hidden layer: 512 & 256 neuron, aktivasi ReLU + Dropout
* Output layer: Softmax
  Model dilatih selama 50 epoch dan divalidasi menggunakan 20% data latih.

### 5. Model Evaluation

Model dievaluasi pada data uji menggunakan metrik akurasi, loss, dan confusion matrix.
Visualisasi performa dilihat melalui grafik training vs. validation accuracy dan loss.

### 6. Model Saving

Model disimpan dalam format `.h5` (`gesture_mlp_model.h5`), dan label mapping disimpan sebagai `label.json`.
Keduanya disimpan di folder `model/` untuk kebutuhan deployment.

### 7. Model Testing

Sebelum digunakan dalam backend, dilakukan pengujian lokal melalui script `test.py` di folder `model/` untuk memastikan model dan label encoder bekerja dengan baik.

---

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
