# KHUSUS TUGAS ZAINAL - RINGKASAN LENGKAP

**Nama:** Zainal  
**Posisi:** Supervisor (Tim 11 - Terpisah dari 10 kelompok surveyor)

---

## Ringkasan Tugas

### Tugas 1: Pengukuran Fisik 100 Pohon
- Keliling batang @150cm
- Tinggi dan keliling buah sampel
- Pencatatan manual
- Detail: Lihat `2_Tugas_Spesifik_Zainal_Tim11.md`

### Tugas 2: Auto-Labeling
- Siapkan auto-label sebelum sesi dengan pakar
- Prioritas: tools OFFLINE (AnyLabeling)
- Detail: Lihat `3_Metodologi_Pelabelan_AutoLabeling.md`

### Tugas 3: Riset Hardware
- Rekomendasi kamera depth dengan IMU
- Rekomendasi tablet yang kompatibel
- Detail: Lihat `4_Spesifikasi_Hardware_Depth_Tablet.md`

### Tugas 4: Eksperimen Metode
- Uji coba 4 sisi vs 8 sisi vs video
- Cari metode terbaik untuk akurasi counting
- Detail: Lihat `5_Rencana_Penelitian_Novelty.md`

---

## MASTER CHECKLIST ZAINAL

### Prioritas Tinggi (Segera)

#### Persiapan Batch 1
- [ ] Siapkan meteran pita dan logbook
- [ ] Print template form pengukuran
- [ ] Koordinasi jadwal dengan kelompok lain
- [ ] Cek cuaca sebelum pengambilan

#### Pengukuran Lapangan (100 Pohon)
- [ ] Ukur keliling batang @150cm
- [ ] Ukur sample buah (tinggi & keliling)
- [ ] Catat posisi pelepah (atas/bawah)
- [ ] Foto 4 sisi dengan batang dalam frame
- [ ] Hubungkan ID foto dengan data ukuran

### Prioritas Sedang (Setelah Batch 1)

#### Persiapan Auto-Labeling
- [ ] Install AnyLabeling
- [ ] Install Ultralytics (pip install ultralytics)
- [ ] Buat struktur folder dataset
- [ ] Label manual 50-100 gambar seed

#### Training Model Awal
- [ ] Train YOLOv8-Nano dengan seed data
- [ ] Evaluasi mAP (target >50%)
- [ ] Export model ke ONNX
- [ ] Load model ke AnyLabeling

#### Auto-Label Batch
- [ ] Jalankan auto-label pada sisa dataset
- [ ] Koordinasi jadwal dengan pakar
- [ ] Sesi validasi dengan pakar
- [ ] Export final dataset

### Prioritas Rendah (Batch Selanjutnya)

#### Riset Hardware
- [ ] Finalisasi pilihan kamera depth (D455 vs D435i)
- [ ] Finalisasi pilihan tablet
- [ ] Verifikasi kompatibilitas
- [ ] Estimasi budget ke dosen
- [ ] Pengadaan hardware

#### Eksperimen Metode
- [ ] Desain eksperimen 3 metode
- [ ] Pengambilan data 50 pohon dengan 3 metode
- [ ] Perbandingan akurasi
- [ ] Dokumentasi hasil

---

## Daftar File Terkait

| No | File | Isi |
|----|------|-----|
| 1 | `1_SOP_Pengambilan_Gambar_RGB.md` | SOP foto, setting kamera, QC |
| 2 | `2_Tugas_Spesifik_Zainal_Tim11.md` | Pengukuran 100 pohon |
| 3 | `3_Metodologi_Pelabelan_AutoLabeling.md` | Auto-labeling, kelas BBC |
| 4 | `4_Spesifikasi_Hardware_Depth_Tablet.md` | Kamera depth, tablet |
| 5 | `5_Rencana_Penelitian_Novelty.md` | Eksperimen, novelty |
| 6 | `6_Khusus_Tugas_Zainal.md` | File ini - ringkasan |

---

## Kontak & Koordinasi

| Pihak | Untuk Keperluan |
|-------|-----------------|
| Dosen Pembimbing | Konsultasi research, validasi metode |
| Pakar | Sesi pelabelan data |
| 10 Kelompok Surveyor | Koordinasi pengambilan gambar |

---

## Catatan Penting

1. Zainal terpisah dari 10 kelompok - Tim 11 dengan tugas khusus
2. Pencatatan manual - Aplikasi belum punya fitur input angka
3. Prioritas tools offline - Auto-label utamakan yang tidak perlu internet
4. USB 3.0+ diperlukan - Untuk tablet yang dipakai dengan kamera depth
5. Cek cuaca - Pencahayaan konsisten perlu diperhatikan
