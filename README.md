# 🚗🚌🏍️ Deteksi Objek Kendaraan dengan YOLO26

Proyek **computer vision** untuk mendeteksi dan mengklasifikasikan **6 kelas kendaraan** menggunakan **YOLO26 Nano**. Proyek ini mencakup proses audit dataset, fine-tuning model, evaluasi performa, serta analisis visual terhadap hasil deteksi objek.

Model dilatih untuk mendeteksi:

- 🚌 Bus
- 🚗 Car
- 🏍️ Motorbike
- 🛺 Rickshaw
- 🚚 Truck
- 🚐 Van

---

## 📌 Gambaran Umum Proyek

Proyek ini mengimplementasikan pipeline deteksi objek menggunakan **YOLO26 Nano (`yolo26n.pt`)** dengan dataset kendaraan khusus.

Alur yang digunakan meliputi:

1. Audit dan validasi dataset
2. Persiapan dataset dalam format YOLO
3. Transfer learning dari bobot YOLO26 yang telah dilatih sebelumnya
4. Fine-tuning model
5. Validasi dan evaluasi performa
6. Analisis performa per kelas
7. Pemeriksaan visual terhadap hasil deteksi
8. Analisis performa inferensi

Tujuan proyek ini adalah membangun model deteksi kendaraan yang ringan dan mampu mengenali berbagai kategori kendaraan dari gambar.

---

## 🎯 Ringkasan Utama Proyek

- **6 kelas kendaraan**
- **2.062 gambar untuk training**
- **873 gambar untuk validation**
- **Model YOLO26 Nano pretrained**
- **Transfer learning / fine-tuning**
- **mAP@0.50: 98,34%**
- **mAP@0.50:0.95: 91,57%**
- **Precision: 98,05%**
- **Recall: 94,30%**
- **~158,5 FPS** pada konfigurasi inferensi yang diuji
- Audit dataset dan validasi anotasi
- Analisis performa per kelas
- Analisis menggunakan confusion matrix, Precision-Recall, dan F1

---

## 📊 Dataset

Proyek ini menggunakan dataset deteksi objek kendaraan yang tersedia secara publik dalam **format YOLO**.

| Komponen | Detail |
|---|---|
| Jenis Dataset | Vehicle Object Detection |
| Format | YOLO |
| Training Images | 2.062 |
| Validation Images | 873 |
| Training Objects | 2.637 |
| Validation Objects | 1.114 |
| Jumlah Kelas | 6 |

### 🔗 Sumber Dataset

Dataset tersedia secara publik melalui Roboflow Universe:

**[Vehicle Object Dataset – Roboflow](https://universe.roboflow.com/nadin-pethiyagoda/vehicle-dataset-for-yolo)**

### Pemetaan Kelas

| Class ID | Kelas |
|---:|---|
| 0 | Bus |
| 1 | Car |
| 2 | Motorbike |
| 3 | Rickshaw |
| 4 | Truck |
| 5 | Van |

---

## 🧹 Persiapan & Audit Dataset

Sebelum proses training, dataset diaudit untuk memverifikasi struktur dan konsistensi anotasi.

Proses persiapan dan audit meliputi:

- Memeriksa jumlah gambar training dan validation
- Memverifikasi ketersediaan gambar dan label
- Memeriksa struktur anotasi YOLO
- Meninjau distribusi kelas
- Memvalidasi konfigurasi `data.yaml`
- Memverifikasi enam kelas kendaraan
- Memvisualisasikan sampel yang mewakili setiap kelas dalam dataset

Audit dataset dilakukan sebelum training untuk mengurangi potensi permasalahan data dan anotasi yang dapat memengaruhi performa model.

---

## 🧠 Model

Proyek ini menggunakan **YOLO26 Nano**, yaitu arsitektur deteksi objek yang ringan, yang diinisialisasi menggunakan bobot pretrained dan kemudian dilakukan fine-tuning pada dataset kendaraan.

| Parameter | Konfigurasi |
|---|---|
| Model | YOLO26 Nano |
| Bobot | `yolo26n.pt` |
| Tugas | Object Detection |
| Framework | Ultralytics |
| Ukuran Gambar | 640 × 640 |
| Maksimum Epoch | 30 |
| Early Stopping | Patience = 8 |
| Optimizer | Auto |
| Seed | 42 |
| Hardware | NVIDIA T4 GPU |
| Environment | Google Colab |
| Bahasa | Python |

### Transfer Learning

Model dimulai dari bobot pretrained **YOLO26 Nano** dan selanjutnya dilakukan fine-tuning menggunakan dataset kendaraan khusus.

Konfigurasi training menggunakan **early stopping** dengan nilai patience sebesar **8 epoch**, sehingga proses training dapat dihentikan ketika performa validasi tidak lagi mengalami peningkatan.

---

## 📈 Performa Model

Model final dievaluasi menggunakan dataset validation dengan metrik standar untuk object detection.

| Metrik | Hasil |
|---|---:|
| Precision | **0,9805** |
| Recall | **0,9430** |
| mAP@0.50 | **0,9834** |
| mAP@0.50:0.95 | **0,9157** |
| Waktu Inferensi | **6,31 ms/gambar** |
| Perkiraan FPS | **158,5 FPS** |

### Interpretasi Metrik

- **Precision** mengukur proporsi hasil deteksi yang diprediksi model dan benar.
- **Recall** mengukur proporsi objek yang sebenarnya ada dan berhasil dideteksi oleh model.
- **mAP@0.50** mengevaluasi Mean Average Precision menggunakan ambang IoU sebesar 0,50.
- **mAP@0.50:0.95** mengevaluasi performa deteksi pada berbagai ambang IoU dari 0,50 hingga 0,95.

Hasil tersebut menunjukkan performa validasi keseluruhan yang kuat, sementara analisis per kelas memberikan gambaran tambahan mengenai perbedaan performa antar kategori kendaraan.

---

## 📊 Performa Per Kelas

Hasil validation menunjukkan nilai Average Precision (AP) berikut untuk setiap kelas kendaraan:

| Kelas Kendaraan | AP |
|---|---:|
| Van | **0,9661** |
| Bus | **0,9595** |
| Car | **0,9342** |
| Truck | **0,9252** |
| Rickshaw | **0,9148** |
| Motorbike | **0,7943** |

### Temuan Utama

Sebagian besar kelas memperoleh nilai AP di atas **0,90**.

Kelas **Motorbike** memperoleh AP terendah sebesar **0,7943**, sehingga menjadi kandidat utama untuk pengembangan lebih lanjut melalui penambahan data, peninjauan anotasi, augmentasi, maupun eksperimen model.

---

## 📉 Metrik Training

Proses training dipantau menggunakan metrik loss dan performa deteksi selama proses training dan validation.

![Metrik Training](Images/training_metrics.png)

Kurva training memberikan gambaran mengenai proses pembelajaran model dan perubahan performa validasi sepanjang proses training.

---

## 🔍 Confusion Matrix

Confusion matrix yang telah dinormalisasi digunakan untuk menganalisis perilaku prediksi pada enam kelas kendaraan.

![Confusion Matrix Ternormalisasi](Images/confusion_matrix_normalized.png)

Visualisasi ini membantu mengidentifikasi prediksi yang benar serta potensi kesalahan atau kebingungan antara kategori kendaraan yang berbeda.

---

## 🎯 Kurva Precision-Recall

Kurva Precision-Recall menunjukkan hubungan antara precision dan recall pada berbagai confidence threshold.

![Kurva Precision-Recall](Images/precision_recall_curve.png)

Visualisasi ini membantu mengevaluasi performa deteksi model pada berbagai nilai threshold operasional.

---

## 📐 Kurva F1

Kurva F1 menunjukkan keseimbangan antara precision dan recall pada berbagai confidence threshold.

![Kurva F1](Images/f1_curve.png)

Nilai F1 dapat digunakan untuk mengidentifikasi confidence threshold yang memberikan keseimbangan praktis antara precision dan recall.

---

## 🖼️ Hasil Deteksi

Contact sheet berikut menampilkan beberapa gambar validation yang mewakili hasil deteksi model, lengkap dengan bounding box, label kelas, dan confidence score.

![Hasil Deteksi Kendaraan](Images/detection_contact_sheet.png)

Model mampu mendeteksi berbagai kategori kendaraan, termasuk:

- Car
- Bus
- Motorbike
- Rickshaw
- Truck
- Van

Contoh tersebut memberikan gambaran kualitatif mengenai kemampuan deteksi model sebagai pelengkap terhadap hasil evaluasi numerik.

---

## ⚡ Performa Inferensi

Berdasarkan hasil inferensi pada konfigurasi validation yang diuji, model memiliki waktu inferensi rata-rata sekitar:

**6,31 ms per gambar**

atau sekitar:

**158,5 FPS**

Hasil tersebut menunjukkan bahwa model Nano memiliki efisiensi komputasi yang baik pada lingkungan pengujian yang digunakan.

> Kecepatan inferensi dapat berbeda bergantung pada hardware GPU, ukuran gambar, batch size, environment runtime, dan konfigurasi deployment. Oleh karena itu, nilai FPS yang dilaporkan merupakan hasil pengukuran pada konfigurasi pengujian ini dan bukan nilai performa yang berlaku secara universal.

---

## 🛠️ Teknologi & Tools

### Pemrograman & Framework

- **Python**
- **PyTorch**
- **Ultralytics**

### Computer Vision

- **YOLO26**
- **OpenCV**
- **Object Detection**

### Analisis Data & Visualisasi

- **Matplotlib**

### Environment Pengembangan

- **Google Colab**
- **NVIDIA T4 GPU**

---

## 📁 Struktur Proyek

```text
YOLO26-Vehicle-Object-Detection/
│
├── Images/
│   ├── confusion_matrix_normalized.png
│   ├── detection_contact_sheet.png
│   ├── f1_curve.png
│   ├── precision_recall_curve.png
│   └── training_metrics.png
│
├── README.md
│
└── YOLO26_Vehicle_Object_Detection.ipynb
```

---

## 🚀 Cara Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/Febriyan2005/YOLO26-Vehicle-Object-Detection.git
cd YOLO26-Vehicle-Object-Detection
```

### 2. Instal Dependensi

```bash
pip install ultralytics opencv-python matplotlib
```

### 3. Buka Notebook

Buka file:

```text
YOLO26_Vehicle_Object_Detection.ipynb
```

Notebook berisi keseluruhan alur kerja mulai dari persiapan dan audit dataset hingga evaluasi model dan inferensi.

> Dataset asli tidak disertakan di dalam repository ini. Dataset dapat diperoleh melalui sumber Roboflow yang tercantum di atas.

---

## 📌 Keterbatasan & Pengembangan Selanjutnya

Meskipun model menunjukkan performa validasi keseluruhan yang kuat, masih terdapat beberapa aspek yang dapat dikembangkan lebih lanjut.

### Keterbatasan Saat Ini

- Ukuran dataset relatif terbatas untuk tugas object detection multi-kelas.
- Performa model berbeda pada setiap kategori kendaraan.
- Kelas Motorbike memiliki AP yang lebih rendah dibandingkan kelas lainnya.
- Kecepatan inferensi bergantung pada hardware dan konfigurasi runtime yang digunakan.

### Pengembangan Selanjutnya

Eksperimen berikut dapat dilakukan untuk meningkatkan sistem:

- Menambah jumlah data training
- Meninjau dan memperbaiki anotasi
- Menerapkan augmentasi data yang lebih terarah
- Melakukan tuning hyperparameter lebih lanjut
- Membandingkan YOLO26 Nano dengan varian model yang lebih besar
- Menguji model pada dataset eksternal yang independen
- Mengembangkan model menjadi aplikasi real-time

---

## 👤 Pengembang

**Febriyan**

Mahasiswa Informatika / Computer Science  
Universitas Esa Unggul

Memiliki minat pada:

- Machine Learning
- Artificial Intelligence
- Computer Vision
- Deep Learning
- Data Analysis

---

## 📄 Lisensi

Proyek ini ditujukan untuk **keperluan pembelajaran dan portfolio**.

Dataset yang digunakan tetap mengikuti ketentuan lisensi dan penggunaan dari penyedia aslinya.

---
