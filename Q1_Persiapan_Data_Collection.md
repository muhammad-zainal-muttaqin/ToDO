# 1. PERSIAPAN DATA COLLECTION

---

## A. Spesifikasi & Protokol Pengumpulan Data Reguler (800-1000 Pohon)

### A.1 Tim Pelaksana
- **Jumlah Kelompok:** 10 kelompok surveyor
- **Target per Kelompok:** 80-100 pohon
- **Total Target:** 800-1000 pohon

### A.2 Spesifikasi Kamera

| Parameter | Nilai |
|-----------|-------|
| Device | Smartphone dengan kamera ≥12MP |
| Resolusi | 12 MP (Fix) |
| Orientasi | Portrait (Tegak) |
| Rasio | 4:3 (Native Sensor) |
| Fitur AI/Beauty | OFF |
| Flash | OFF |

### A.3 Protokol Pengambilan Gambar

#### Urutan Pengambilan (Per Pohon)
1. Foto 1: Menghadap **Utara**
2. Foto 2: Menghadap **Timur**
3. Foto 3: Menghadap **Selatan**
4. Foto 4: Menghadap **Barat**

> **Rotasi:** Searah Jarum Jam (Clockwise)

#### Posisi Pengambilan

| Parameter | Nilai |
|-----------|-------|
| Jarak dari batang | 2-3 meter |
| Tinggi kamera | 150 cm (setinggi dada) |
| Sudut | Lurus ke depan (eye-level) |

#### Komposisi Frame

Setiap foto harus mencakup:
1. **Batang pohon** (termasuk titik 150cm dari tanah)
2. **Pangkal pelepah/tajuk** (tempat tandan buah berada)

```
┌─────────────────────────────┐
│                             │
│     [TAJUK/PELEPAH]         │
│        dengan tandan        │
│                             │
│                             │
│       [BATANG POHON]        │
│           150cm ← ──────────│← Harus terlihat
│                             │
└─────────────────────────────┘
```

#### Kondisi Pengambilan

| Aspek | Ketentuan |
|-------|-----------|
| Cuaca | Cek sebelum pengambilan, hindari backlight parah |
| Waktu | Pagi (07:00-10:00) atau sore (15:00-17:00) untuk pencahayaan optimal |
| Konsistensi | Semua surveyor mengikuti protokol yang sama |

### A.4 Output Data Reguler

| Item | Jumlah |
|------|--------|
| Total pohon | 800-1000 pohon |
| Foto per pohon | 4 sisi |
| Total foto | 3200-4000 foto |
| Setelah QC | ~2800-3600 foto (estimasi 10-15% reject) |

---

## B. Spesifikasi Pengumpulan Data Tambahan (50-100 Pohon)

### B.1 Tim Pelaksana
- **PIC:** Zainal (Supervisor / Tim 11)
- **Status:** Terpisah dari 10 kelompok surveyor reguler
- **Target:** 50-100 pohon sampel

### B.2 Tujuan Data Tambahan

| Tujuan | Penjelasan |
|--------|------------|
| **Kalibrasi Ukuran** | Untuk membedakan tandan pra-matang dan mentah yang warnanya mirip |
| **Validasi Posisi** | Membuktikan hipotesis posisi tandan (tua di bawah, muda di atas) |
| **Ground Truth** | Data pembanding untuk validasi model AI |

### B.3 Protokol Pengukuran Fisik

#### Parameter 1: Keliling Batang

| Item | Spesifikasi |
|------|-------------|
| Apa yang diukur | Keliling lingkar batang |
| Ketinggian | 150 cm dari tanah |
| Alat | Meteran pita |
| Syarat | Titik pengukuran harus terlihat dalam foto |

#### Parameter 2: Dimensi Tandan Buah

| Item | Spesifikasi |
|------|-------------|
| Tinggi tandan | Ukur vertikal (cm) |
| Keliling tandan | Ukur horizontal (cm) |
| Posisi pelepah | Catat: atas/tengah/bawah |
| Sample | Minimal 3 tandan per pohon (mewakili ukuran berbeda) |

### B.4 Pencatatan Data

Karena aplikasi custom belum memiliki fitur input angka, pencatatan dilakukan **manual**:

| Metode | Detail |
|--------|--------|
| Logbook | Form cetak yang dibawa ke lapangan |
| HP | Excel/Notes sebagai backup |
| Foto ID | Setiap catatan dihubungkan dengan ID foto |

### B.5 Template Form Pengukuran

```
═══════════════════════════════════════════════════
FORM PENGUKURAN POHON - DATA TAMBAHAN
═══════════════════════════════════════════════════
Tanggal     : ___/___/______
Pohon No    : _____ / 100
Cuaca       : Cerah / Berawan / Hujan

BATANG:
Keliling @150cm : _______ cm

TANDAN SAMPEL:
#1: Posisi [Atas/Tengah/Bawah] | Tinggi: ___cm | Keliling: ___cm
#2: Posisi [Atas/Tengah/Bawah] | Tinggi: ___cm | Keliling: ___cm
#3: Posisi [Atas/Tengah/Bawah] | Tinggi: ___cm | Keliling: ___cm

Foto ID: _______________________________________
═══════════════════════════════════════════════════
```

### B.6 Output Data Tambahan

| Item | Jumlah |
|------|--------|
| Total pohon sample | 50-100 pohon |
| Foto per pohon | 4 sisi |
| Data ukuran per pohon | 1 keliling batang + 3 ukuran tandan |
| Total dataset | 200-400 foto + 50-100 record pengukuran |

---

## C. Jadwal Pengambilan Data

| Batch | Waktu | Jenis | Target |
|-------|-------|-------|--------|
| Batch 1 | Januari | RGB reguler + data tambahan | 800-1000 pohon |
| Batch 2 | April | RGB + Depth (eksperimen) | TBD |

---

## D. Checklist Persiapan Lapangan

### Untuk Surveyor Reguler (10 Kelompok)
- [ ] Smartphone dengan baterai penuh
- [ ] Power bank
- [ ] Tripod (jika tersedia)
- [ ] Briefing protokol pengambilan

### Untuk Tim Data Tambahan (Zainal)
- [ ] Form pengukuran cetak
- [ ] Meteran pita (min 3 meter)
- [ ] Smartphone dengan baterai penuh
- [ ] Power bank
- [ ] Logbook/clipboard
