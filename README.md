# Dokumentasi Proyek Deteksi Tandan Buah Sawit

## Ikhtisar

Dokumen ini menyediakan panduan teknis untuk proyek deteksi dan klasifikasi tandan buah sawit menggunakan computer vision. Proyek ini mencakup pengumpulan data, labeling, dan pengembangan model untuk estimasi waktu panen.

---

## Daftar Dokumen

| No | Dokumen | Deskripsi | Status |
|----|---------|-----------|--------|
| 1 | [Q1_Persiapan_Data_Collection.md](Q1_Persiapan_Data_Collection.md) | Protokol pengumpulan data reguler dan tambahan | Draft |
| 2 | [Q2_Data_QC_Labeling_Anotasi.md](Q2_Data_QC_Labeling_Anotasi.md) | Prosedur QC, auto-labeling, dan spesifikasi anotasi | Draft |
| 3 | [Q3_Rekomendasi_Spesifikasi_Alat.md](Q3_Rekomendasi_Spesifikasi_Alat.md) | Spesifikasi hardware (kamera depth, tablet) | Draft |
| 4 | [Q4_Permasalahan_Black_Bunch.md](Q4_Permasalahan_Black_Bunch.md) | Analisis diferensiasi tandan hitam | Draft |
| 5 | [Q5_Permasalahan_Counting.md](Q5_Permasalahan_Counting.md) | Teknik penghitungan tandan multi-view | Draft |

---

## Ringkasan Isi

### Q1: Persiapan Data Collection

Spesifikasi dan protokol pengumpulan data:

**Data Reguler (800-1000 Pohon)**
- 10 kelompok surveyor, 80-100 pohon per kelompok
- Foto 4 sisi per pohon (U-T-S-B, clockwise)
- Spesifikasi: 12MP, 4:3, Portrait
- Jarak: 2-3 meter, tinggi kamera: 150cm

**Data Tambahan (50-100 Pohon)**
- PIC: Tim 11 (Zainal)
- Pengukuran fisik: keliling batang @150cm, dimensi tandan
- Tujuan: kalibrasi ukuran, validasi hipotesis posisi

---

### Q2: Data - QC, Labeling, Anotasi

**Quality Control**
- Kriteria reject: blur, backlight, komposisi salah
- Estimasi rejection rate: 10-15%

**Auto-Labeling**
- Tool: AnyLabeling (offline, open source)
- Workflow: seed data (50-100) -> train YOLOv8-Nano -> auto-label -> validasi pakar

**Spesifikasi Anotasi**
- Kelas utama: M1, M2, M3, M4 (estimasi waktu panen)
- Kelas tambahan: Bunga, Batang
- Format: YOLO (.txt)

---

### Q3: Rekomendasi Spesifikasi Alat

**Kamera Depth**
- Utama: Intel RealSense D455 (Rp 10-16 juta)
- Alternatif: Intel RealSense D435i (Rp 7-12 juta)

**Tablet**
- Syarat: USB 3.0+, Qualcomm Snapdragon, baterai 7000mAh+
- Rekomendasi: Xiaomi Pad 6 (Rp 4.5-5.5 juta)
- Alternatif: Samsung Tab S8 (Rp 7-9 juta)

**Estimasi Budget**
- Konfigurasi utama: Rp 18.1 juta
- Konfigurasi budget: Rp 14.9 juta

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

**Rekomendasi:** Ekstraksi fitur tambahan (ukuran relatif, posisi vertikal, tekstur) untuk meningkatkan akurasi klasifikasi

---

### Q5: Permasalahan Counting

**Tantangan:** Double counting dan oklusi pada data multi-view

**Teknik tanpa depth:**
1. Simple Aggregation (baseline)
2. Multi-View NMS (rekomendasi)
3. Video Tracking (untuk data video)
4. Density Estimation

**Teknik dengan depth:**
5. 3D Reconstruction
6. Depth-assisted NMS (rekomendasi)
7. Multi-View Stereo

---

## Struktur Folder

```
ToDO/
    README.md                           <- Dokumen ini
    Q1_Persiapan_Data_Collection.md
    Q2_Data_QC_Labeling_Anotasi.md
    Q3_Rekomendasi_Spesifikasi_Alat.md
    Q4_Permasalahan_Black_Bunch.md
    Q5_Permasalahan_Counting.md
    legacy/                             <- Arsip dokumen versi sebelumnya
```

---

## Referensi

### Tools

| Tool | Fungsi | URL |
|------|--------|-----|
| AnyLabeling | Labeling + auto-label | github.com/vietanhdev/anylabeling |
| Ultralytics | YOLOv8 training | github.com/ultralytics/ultralytics |
| Roboflow | Dataset management | roboflow.com |

### Hardware

| Item | Vendor |
|------|--------|
| Intel RealSense | Tokopedia, Shopee |
| Xiaomi Pad | Mi Store, Tokopedia, Shopee |
| Samsung Tab | Samsung Store, Tokopedia, Shopee |

---

## Changelog

| Tanggal | Perubahan |
|---------|-----------|
| 2024-12 | Restrukturisasi dokumen menjadi 5 topik utama |
| 2024-12 | Penambahan visualisasi ASCII dan diagram |
| 2024-12 | Revisi format menjadi lebih profesional |
