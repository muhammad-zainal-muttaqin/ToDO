# Dokumentasi Proyek Deteksi Tandan Buah Sawit

## Ikhtisar

Dokumen ini menyediakan panduan teknis untuk proyek deteksi dan klasifikasi tandan buah sawit menggunakan computer vision. Proyek ini mencakup pengumpulan data, labeling, dan pengembangan model untuk estimasi waktu panen.

---

## Daftar Dokumen

| No | Dokumen | Deskripsi |
|----|---------|----------|
| 1 | [Q1_Persiapan_Data_Collection.md](Q1_Persiapan_Data_Collection.md) | Protokol pengumpulan data reguler, tambahan, dan eksperimen |
| 2 | [Q2_Data_QC_Labeling_Anotasi.md](Q2_Data_QC_Labeling_Anotasi.md) | Prosedur QC, auto-labeling, dan spesifikasi anotasi |
| 3 | [Q3_Rekomendasi_Spesifikasi_Alat.md](Q3_Rekomendasi_Spesifikasi_Alat.md) | Spesifikasi hardware (kamera depth, tablet, GPS, tongkat) |
| 4 | [Q4_Permasalahan_Black_Bunch.md](Q4_Permasalahan_Black_Bunch.md) | Analisis diferensiasi tandan hitam |
| 5 | [Q5_Permasalahan_Counting.md](Q5_Permasalahan_Counting.md) | Teknik penghitungan tandan multi-view |

---

## Ringkasan Isi

### Q1: Persiapan Data Collection

Spesifikasi dan protokol pengumpulan data:

**Data Reguler (800-1000 Pohon)**
- 10 kelompok surveyor, 80-100 pohon per kelompok
- Foto 4 sisi per pohon (U-T-S-B, clockwise)
- Spesifikasi: 12MP, 4:3, Portrait
- Jarak: 2-3 meter, tinggi kamera: 150cm
- Waktu: sesuai jam kerja survei (08:00-16:00)

**Data Tambahan (Pengukuran Fisik)**
- PIC: Tim 11 (Zainal)
- 50 pohon: pengukuran keliling batang @150cm
- 10 pohon: pengukuran 3 tandan per pohon

**Data Eksperimen (50 Pohon)**
- 3 metode dibandingkan:
  - Metode A: 4 sisi (standar)
  - Metode B: 8 sisi (detail)
  - Metode C: Video 360 derajat

---

### Q2: Data - QC, Labeling, Anotasi

**Quality Control**
- Kriteria reject: blur, backlight, komposisi salah
- Data reject dipisahkan tetapi tetap dilabeli untuk training tambahan

**Auto-Labeling (Alternatif dengan Pretrained)**
1. Prelabel deteksi dengan model pretrained dari Roboflow (box only)
2. Pakar label kelas untuk 100 pohon (400 images)
3. Train classifier
4. Auto-label kelas untuk sisa dataset
5. Validasi oleh pakar dengan dokumentasi ketidakyakinan

**Tim Labeling**
- 4 pakar dari GMK
- 2 tim paralel (1 asisten + 2 pakar per tim)
- Alokasi: 3-5 hari x 7 jam/hari

**Fitur Ketidakyakinan**
- Jika pakar kurang yakin, ketidakyakinan didokumentasikan
- Informasi ini dapat digunakan untuk training/analisis

---

### Q3: Rekomendasi Spesifikasi Alat

**Budget per Set:** Rp 21.000.000

**Kamera Depth**
- Utama: Intel RealSense D455 (Rp 10-16 juta)
- Alternatif: Intel RealSense D435i (Rp 7-12 juta)

**Tablet**
- Syarat: USB 3.0+, Qualcomm Snapdragon, baterai 7000mAh+
- Rekomendasi: Xiaomi Pad 6 (Rp 4.5-5.5 juta)

**Smartphone (Opsi Non-Tablet)**
- Harus USB 3.0+ untuk kompatibilitas RealSense
- Samsung A54/A55 menggunakan USB 2.0 (tidak kompatibel depth)

**GPS**
- GPS bawaan smartphone umumnya memadai
- Opsi external untuk akurasi lebih tinggi

**Tongkat Teleskopik (Pole)**
- Panjang: 3-5 meter
- Material: Carbon fiber atau aluminum (ringan dan kuat)
- Opsi: Commercial atau DIY

---

### Q4: Permasalahan Black Bunch

**Masalah:** Tandan M2, M3, M4 memiliki warna serupa (hitam)

**Aspek pembeda menurut pakar:**

| Aspek | Keandalan |
|-------|-----------|
| Ukuran relatif | Tinggi |
| Posisi vertikal | Tinggi |
| Tekstur | Sedang |
| Warna | Rendah |

**Rekomendasi:** Ekstraksi fitur tambahan (ukuran relatif, posisi vertikal, tekstur)

---

### Q5: Permasalahan Counting

**Tantangan:** Double counting dan oklusi pada data multi-view

**Teknik tanpa depth:**
1. Simple Aggregation (baseline)
2. Multi-View NMS (rekomendasi)
3. Video Tracking (untuk data video 360)
4. Density Estimation

**Teknik dengan depth:**
5. 3D Reconstruction
6. Depth-assisted NMS (rekomendasi)
7. Multi-View Stereo

---

## Referensi

### Tools

| Tool | Fungsi | URL |
|------|--------|-----|
| AnyLabeling | Labeling + auto-label | github.com/vietanhdev/anylabeling |
| Ultralytics | YOLOv8 training | github.com/ultralytics/ultralytics |
| Roboflow | Dataset management + pretrained models | roboflow.com |

### Hardware

| Item | Vendor |
|------|--------|
| Intel RealSense | Tokopedia, Shopee |
| Xiaomi Pad | Mi Store, Tokopedia, Shopee |
| Samsung Tab | Samsung Store, Tokopedia, Shopee |
