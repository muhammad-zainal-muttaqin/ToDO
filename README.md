# TO DO LIST - PROYEK DETEKSI BUAH SAWIT

---

## Daftar Pertanyaan & Dokumentasi

| No | Pertanyaan | File | Status |
|----|------------|------|--------|
| 1 | Persiapan Data Collection | [Q1_Persiapan_Data_Collection.md](Q1_Persiapan_Data_Collection.md) | Draft |
| 2 | Data: QC, Labeling, Anotasi | [Q2_Data_QC_Labeling_Anotasi.md](Q2_Data_QC_Labeling_Anotasi.md) | Draft |
| 3 | Rekomendasi Spesifikasi Alat | [Q3_Rekomendasi_Spesifikasi_Alat.md](Q3_Rekomendasi_Spesifikasi_Alat.md) | Draft |
| 4 | Permasalahan Black Bunch | [Q4_Permasalahan_Black_Bunch.md](Q4_Permasalahan_Black_Bunch.md) | Draft |
| 5 | Permasalahan Counting | [Q5_Permasalahan_Counting.md](Q5_Permasalahan_Counting.md) | Draft |

---

## Ringkasan Isi Setiap File

### Q1: Persiapan Data Collection

**A. Data Reguler (800-1000 Pohon)**
- 10 kelompok surveyor
- Protokol: 4 sisi per pohon (U-T-S-B, clockwise)
- Spesifikasi: 12MP, 4:3, Portrait, AI/Beauty OFF
- Posisi: 2-3m dari batang, tinggi 150cm
- Frame: harus mencakup batang + tajuk

**B. Data Tambahan (50-100 Pohon)**
- PIC: Zainal (Tim 11)
- Pengukuran fisik: keliling batang @150cm, dimensi tandan
- Pencatatan manual (logbook)
- Tujuan: kalibrasi ukuran, validasi posisi

---

### Q2: Data - QC, Labeling, Anotasi

**A. Quality Control**
- Kriteria reject: blur, backlight, framing salah
- Estimasi rejection rate: 10-15%

**B. Auto-Labeling**
- Tool: AnyLabeling (offline, gratis)
- Workflow: seed data → train YOLOv8-Nano → auto-label → validasi pakar

**C. Labeling dengan Pakar**
- Setup: komputer + pakar + supervisor
- Target: 15-20 gambar/menit jika auto-label akurat

**D. Anotasi**
- Kelas utama: M1, M2, M3, M4 (estimasi panen)
- Kelas tambahan: Bunga, Batang
- Format: YOLO (.txt)

---

### Q3: Rekomendasi Spesifikasi Alat

**Kamera Depth:**
- Rekomendasi: Intel RealSense D455 (Rp 10-16 juta)
- Alternatif: Intel RealSense D435i (Rp 7-12 juta)

**Tablet:**
- Syarat: USB 3.0+, Snapdragon, baterai >7000mAh
- Rekomendasi: Xiaomi Pad 6 (Rp 4.5-5.5 juta)
- Alternatif: Samsung Tab S8

**Budget Total:**
- Opsi Rekomendasi: ~Rp 18 juta
- Opsi Budget: ~Rp 15 juta

---

### Q4: Permasalahan Black Bunch

**Masalah:** Tandan pra-matang dan mentah sama-sama hitam

**Aspek pembeda (menurut pakar):**
1. Ukuran (relatif) - KUAT
2. Posisi vertikal - KUAT
3. Tekstur - SEDANG
4. Warna - LEMAH

**Rekomendasi:** Ya, perlu preprocessing/ekstraksi fitur eksplisit
- Ukuran relatif (vs batang)
- Posisi vertikal dalam frame
- Fitur tekstur (edge density, LBP)
- Enhanced color space (HSV, LAB)
- Depth (jika tersedia)

---

### Q5: Permasalahan Counting

**Teknik TANPA Depth:**
1. Simple Aggregation (baseline)
2. Multi-View NMS (recommended)
3. Video Tracking (untuk video 360°)
4. Density Estimation

**Teknik DENGAN Depth:**
5. 3D Reconstruction
6. Depth-assisted NMS (recommended)
7. Multi-View Stereo

**Rekomendasi:**
- Tanpa depth: Multi-View NMS
- Dengan depth: Depth-assisted NMS

---

## File Legacy (Backup)

File-file berikut adalah versi sebelumnya yang telah digantikan oleh struktur baru:

| File | Isi |
|------|-----|
| ToDO.md | Overview lama |
| 1_SOP_Pengambilan_Gambar_RGB.md | SOP pengambilan gambar |
| 2_Tugas_Spesifik_Zainal_Tim11.md | Tugas pengukuran fisik |
| 3_Metodologi_Pelabelan_AutoLabeling.md | Metodologi labeling |
| 4_Spesifikasi_Hardware_Depth_Tablet.md | Spesifikasi hardware detail |
| 5_Rencana_Penelitian_Novelty.md | Rencana penelitian |
| 6_Khusus_Tugas_Zainal.md | Checklist Zainal |
