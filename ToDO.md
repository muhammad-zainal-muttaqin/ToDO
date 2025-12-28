# DOKUMEN RENCANA KERJA & SPESIFIKASI TEKNIS

**Proyek:** Deteksi dan Klasifikasi Buah Sawit dengan AI  
**Metode:** Pengambilan Gambar Multi-Sisi + Depth Sensor

---

## DAFTAR ISI

| No | Bagian | Deskripsi |
|----|--------|-----------|
| 1 | Data Collection (Batch 1 - RGB) | SOP pengambilan gambar, setting kamera |
| 2 | Tugas Spesifik Supervisor | Pengukuran fisik 100 pohon |
| 3 | Metodologi Pelabelan | Kelas BBC, auto-labeling |
| 4 | Spesifikasi Hardware | Kamera depth, tablet |
| 5 | Rencana Penelitian & Novelty | Eksperimen perbandingan metode |

---

## BAGIAN 1: DATA COLLECTION (BATCH 1 - RGB)

**Tujuan:** Standarisasi input data agar model AI bisa dikembangkan dengan benar.

### 1. SOP Pengambilan Gambar

**Urutan Pengambilan (Sequence):**
- **Aturan:** Wajib berurutan dan **konsisten**
- **Arah:** Utara → Timur → Selatan → Barat
- **Rotasi:** Disepakati **Searah Jarum Jam (Clockwise)** untuk keseragaman seluruh surveyor
- *Alasan:* Agar saat *stitching* atau *counting*, algoritma tahu gambar ke-2 adalah sisi kanan dari gambar ke-1

**Posisi & Jarak:**
- **Jarak:** 2 - 3 meter dari batang
- **Tinggi Kamera:** 150 cm (Setinggi dada rata-rata, gunakan tripod jika memungkinkan)
- **Angle:** Lurus ke depan (*Frontal/Eye-level*). Tidak boleh terlalu mendongak

**Komposisi Frame (Wajib):**
- **Cakupan:** Harus terlihat **Batang Pohon** (sebagai referensi ukuran) dan **Pangkal Pelepah/Tajuk** (tempat buah berada)
- *Alasan:* Tanpa batang, AI tidak punya pembanding untuk membedakan "Buah Kecil difoto dekat" vs "Buah Besar difoto jauh"

### 2. Setting Kamera

| Parameter | Pengaturan |
|-----------|------------|
| **Device** | Samsung A55 (atau setara) |
| **Orientasi** | Portrait (Tegak) |
| **Rasio** | 4:3 (Native Sensor). Jangan 16:9 |
| **Fitur AI/Beauty** | **OFF** (Wajib dimatikan) |
| **Flash** | OFF |
| **Resolusi** | Highest Available |

### 3. Quality Control (Sortir Batch 1)

- Total target: ~1000 gambar
- Lakukan sortir manual: Buang gambar blur, backlight parah, atau batang tidak terlihat
- Hanya gambar "Sempurna" yang masuk dataset (target: ~900 gambar)

---

## BAGIAN 2: TUGAS SPESIFIK SUPERVISOR (ZAINAL) - TIM 11

**Tugas Utama:** Pengumpulan Data "Ground Truth" Fisik pada **100 Pohon Sampel**  
*Catatan: Dilakukan terpisah dari 10 kelompok surveyor biasa*

### 1. Tujuan & Logika

- **Kalibrasi Ukuran:** Membedakan buah Pra-Matang (M2) dan Mentah (M3) yang warnanya sama-sama hitam. Dimensi fisik menjadi kunci kalibrasi
- **Validasi Posisi Biologis:** Membuktikan hipotesis bahwa "Buah Tua posisinya di pelepah bawah, Buah Muda di pelepah atas"

### 2. Protokol Pengukuran Manual

**Target:** 100 Pohon terpilih

**Parameter 1: Batang (Trunk)**
- Ukur **Keliling Batang** pada ketinggian **150 cm** dari tanah
- *Syarat:* Saat pohon difoto, titik 150cm ini harus masuk dalam frame foto

**Parameter 2: Buah Sawit (TBS)**
- Pilih sampel buah (Mewakili ukuran Besar, Sedang, Kecil/Bunga)
- Ukur **Tinggi Buah** (Vertikal) dan **Keliling Buah** (Horizontal)
- Catat **Posisi Pelepah**: Lingkar bawah (Tua) atau lingkar atas (Muda)

**Alat:** Meteran pita  
**Pencatatan:** Manual (Logbook/Excel di HP)

---

## BAGIAN 3: METODOLOGI PELABELAN & AUTO-LABELING

### 1. Definisi Kelas BBC (Estimasi Panen)

> **Logika:** Label bukan deskripsi kondisi saat ini, tapi **kapan panen**

| Label | Artinya |
|-------|---------|
| **M1 (1 Bulan)** | Panen n+1 |
| **M2 (2 Bulan)** | Panen n+2 |
| **M3 (3 Bulan)** | Panen n+3 |
| **M4 (4 Bulan)** | Panen n+4 |

**Tambahan:** Labeli juga **Bunga** (prediksi jangka panjang) dan **Batang** (referensi AI)

### 2. Strategi Auto-Labeling

**Tujuan:** Membantu Pakar. Pakar hanya memvalidasi (koreksi), bukan menggambar kotak dari nol.

**Tools Rekomendasi:**
- **Utama (Offline):** AnyLabeling - Support YOLO/SAM, Gratis, Desktop based
- **Alternatif (Online):** Roboflow - Jika butuh kolaborasi tim

**Workflow Teknis:**
1. Zainal melabeli manual 50-100 gambar (Batch kecil)
2. Train model YOLOv8-Nano (Pre-train)
3. Load model ke AnyLabeling
4. Jalankan "Auto-Label" pada sisa dataset (~900 gambar)
5. **Validasi Pakar:** Pakar mengecek hasil auto-label, koreksi jika salah

---

## BAGIAN 4: SPESIFIKASI HARDWARE (DEPTH & TABLET)

*Untuk Batch Selanjutnya & Eksperimen*

### 1. Kamera Depth (Sensor Kedalaman)

**Syarat Mutlak:**
- Harus bisa mendeteksi **Angle/Derajat Kemiringan** (IMU)
- Tahan Cahaya Matahari Siang

**Rekomendasi:**
| Model | Keterangan |
|-------|------------|
| **Intel RealSense D455** | Disarankan untuk outdoor (jangkauan jauh, global shutter) |
| **Intel RealSense D435i** | Opsi lebih murah, pastikan ada huruf "i" (IMU) |

> **⚠️ Huruf "i" WAJIB ada** - menandakan memiliki IMU (Inertial Measurement Unit) untuk deteksi orientasi kamera

### 2. Tablet/Monitor

**Fungsi:** Viewfinder kamera depth

**Syarat Kritis:**
- Port: **USB Type-C 3.0 / 3.1** (Wajib, USB 2.0 tidak cukup bandwidth)
- OS: Android (Kompatibel dengan RealSense SDK)
- Baterai: Besar (7000mAh+) karena menyuplai daya ke kamera

**Rekomendasi:** Samsung Galaxy Tab Active 4 Pro atau Samsung Tab S6 Lite

---

## BAGIAN 5: RENCANA PENELITIAN & NOVELTY (KEBARUAN)

### 1. Usulan Metode Utama

| Aspek | Spesifikasi |
|-------|-------------|
| **Model** | YOLOv8 (Transfer Learning) |
| **Input** | 4 Channel (R-G-B + Depth) |
| **Tujuan** | Menggunakan depth untuk membedakan buah besar/kecil dan menghitung estimasi panen |

### 2. Eksperimen Komparasi

Untuk meningkatkan nilai penelitian (Proof of Concept), bandingkan 3 metode pengambilan data:

| Metode | Deskripsi | Hipotesis |
|--------|-----------|-----------|
| **A (Standar)** | 4 Sisi (U-T-S-B) | Baseline/standar BBC |
| **B (Detailed)** | 8 Sisi (tambah diagonal) | Mengurangi blind spot |
| **C (Video)** | Video 360° mengelilingi pohon | Ground Truth paling lengkap |

### 3. Output Klasifikasi

- Sistem otomatis menghitung jumlah tandan per kelas bulan (M1, M2, M3, M4)
- Estimasi total panen dalam Kg (menggunakan rumus kalibrasi dari pengukuran fisik)

---

## DOKUMEN DETAIL

Untuk informasi lebih lengkap, lihat dokumen terpisah:

| No | File | Isi |
|----|------|-----|
| 1 | `1_SOP_Pengambilan_Gambar_RGB.md` | SOP foto, setting kamera, QC |
| 2 | `2_Tugas_Spesifik_Zainal_Tim11.md` | Pengukuran 100 pohon, template form |
| 3 | `3_Metodologi_Pelabelan_AutoLabeling.md` | Auto-labeling, workflow teknis |
| 4 | `4_Spesifikasi_Hardware_Depth_Tablet.md` | Kamera depth, tablet, budget |
| 5 | `5_Rencana_Penelitian_Novelty.md` | Eksperimen, hipotesis, evaluasi |
| 6 | `6_Khusus_Tugas_Zainal.md` | Master checklist supervisor |