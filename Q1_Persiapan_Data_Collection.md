# 1. PERSIAPAN DATA COLLECTION

## 1.1 Ikhtisar

Dokumen ini menjelaskan spesifikasi dan protokol pengumpulan data untuk proyek deteksi tandan buah sawit. Pengumpulan data dibagi menjadi dua kategori: data reguler dan data tambahan untuk kalibrasi.

---

## 1.2 Tim Pelaksana

| Kategori | Kelompok | Target | Tugas |
|----------|----------|--------|-------|
| Data Reguler | 10 kelompok surveyor | 80-100 pohon/kelompok | Pengambilan foto 4 sisi |
| Data Tambahan | Tim 11 (Zainal) | 50-100 pohon | Pengukuran fisik + foto |
| **Total** | **11 kelompok** | **800-1000 pohon** | - |

---

## 1.3 Pengumpulan Data Reguler (800-1000 Pohon)

### 1.3.1 Spesifikasi Kamera

| Parameter | Nilai | Keterangan |
|-----------|-------|------------|
| Resolusi | 12 MP (tetap) | Tidak menggunakan mode auto |
| Orientasi | Portrait | Posisi vertikal |
| Rasio | 4:3 | Native sensor, hindari 16:9 |
| AI/Beauty | OFF | Mencegah manipulasi warna |
| Flash | OFF | - |
| Perangkat | Smartphone kamera 12MP+ | Contoh: Samsung A55 |

### 1.3.2 Urutan Pengambilan Gambar

Pengambilan dilakukan dari 4 sisi dengan urutan searah jarum jam:

```
                          UTARA (Foto 1)
                              |
                              |
                              v
            +-----------------+-----------------+
            |                 |                 |
            |                 |                 |
  BARAT     |                 |                 |     TIMUR
  (Foto 4)  |     [POHON]     |                 |    (Foto 2)
     <------+       X         +------>          |
            |                 |                 |
            |                 |                 |
            +-----------------+-----------------+
                              |
                              |
                              v
                        SELATAN (Foto 3)

  Urutan: U (1) -> T (2) -> S (3) -> B (4)
  Rotasi: Clockwise (searah jarum jam)
```

### 1.3.3 Posisi Pengambilan (Tampak Atas)

```
                    2-3 meter
              <----------------->

              +-------------------+
              |                   |
              |     SURVEYOR      |
              |        @          |
              |       /|\         |
              |        |          |
              +-------------------+
                       |
                       | 2-3 meter
                       |
                       v
              +-------------------+
              |                   |
              |      POHON        |
              |        #          |
              |                   |
              +-------------------+
```

### 1.3.4 Posisi Pengambilan (Tampak Samping)

```
                                    +--------------------+
                                    |                    |
                                    |   TAJUK/PELEPAH    |  <- harus masuk frame
                                    |   dengan tandan    |
                                    |                    |
                                    +--------------------+
                                    |                    |
                              150cm |     BATANG         |
    SURVEYOR                   +--->|       |            |  <- titik 150cm
       @                       |    |       |            |     harus terlihat
      /|\   [KAMERA]           |    |       |            |
       |    tinggi 150cm ------+    +-------+------------+
      / \                           |       |            |
    ------                          |  TANAH             |
                                    +--------------------+

    <------------ 2-3 meter ------------>
```

### 1.3.5 Komposisi Frame

Setiap foto harus mencakup elemen berikut:

```
    +----------------------------------+
    |                                  |
    |      TAJUK + TANDAN BUAH         |
    |      ~~~~~~~~~~~~~~~~~~~~        |
    |           |||                    |
    |           |||                    |
    |        +--|||--+                 |
    |        |  |||  |                 |
    |        | BATANG|                 |
    |   ---->| 150cm |<----            |  <- Titik referensi kalibrasi
    |        |  |||  |                 |
    |        +--|||--+                 |
    |           |||                    |
    +----------------------------------+

    Elemen wajib:
    1. Batang pohon (referensi ukuran)
    2. Titik 150cm dari tanah (kalibrasi)
    3. Tajuk/pelepah dengan tandan buah
```

### 1.3.6 Kondisi Pengambilan

| Parameter | Ketentuan |
|-----------|-----------|
| Waktu | 07:00-10:00 atau 15:00-17:00 |
| Cuaca | Cerah atau berawan tipis |
| Pencahayaan | Hindari backlight (matahari di belakang pohon) |
| Sudut kamera | Lurus ke depan (eye-level) |

### 1.3.7 Jadwal Batch

| Batch | Waktu | Tipe Data | Target |
|-------|-------|-----------|--------|
| 1 | Januari | RGB 4 sisi | 800-1000 pohon |
| 2 | April | RGB + Depth | TBD |

---

## 1.4 Pengumpulan Data Tambahan (50-100 Pohon)

### 1.4.1 Tujuan

| Tujuan | Penjelasan |
|--------|------------|
| Kalibrasi ukuran | Data ukuran fisik untuk membedakan tandan hitam |
| Validasi hipotesis | Posisi vertikal tandan (tua di bawah, muda di atas) |
| Ground truth | Referensi validasi model AI |

### 1.4.2 Hipotesis Biologis

```
    POHON SAWIT (Tampak Samping)

    +---------------------------+
    |      PELEPAH ATAS         |  <- Tandan MUDA (M3/M4)
    |       ~~O~~O~~            |     belum matang
    |          |                |
    |          |                |
    |      PELEPAH TENGAH       |  <- Tandan TRANSISI
    |       ~~O~~O~~            |
    |          |                |
    |          |                |
    |      PELEPAH BAWAH        |  <- Tandan TUA (M1/M2)
    |       ~~O~~O~~            |     mendekati matang
    |          |                |
    |      [BATANG]             |
    |     ... 150cm ...         |  <- Titik pengukuran
    |          |                |
    +----------+----------------+
             TANAH
```

### 1.4.3 Parameter Pengukuran

#### A. Batang (Trunk)

| Parameter | Spesifikasi |
|-----------|-------------|
| Pengukuran | Keliling lingkar batang |
| Ketinggian | 150 cm dari tanah |
| Alat | Meteran pita (batang melengkung) |
| Syarat | Titik ukur harus masuk dalam frame foto |

#### B. Tandan Buah Segar (TBS)

| Parameter | Cara Ukur |
|-----------|-----------|
| Tinggi tandan | Vertikal, ujung bawah ke ujung atas |
| Keliling tandan | Horizontal, lingkar terbesar |
| Posisi pelepah | Atas / Tengah / Bawah |
| Jumlah sampel | Minimal 3 tandan per pohon |

### 1.4.4 Template Pencatatan

```
================================================================
FORM PENGUKURAN POHON - TIM 11
================================================================
Tanggal     : ___/___/______
Lokasi      : _______________________________
Nomor Pohon : _____ / 100
Cuaca       : [ ] Cerah  [ ] Berawan  [ ] Hujan

BATANG
----------------------------------------------------------------
Keliling @150cm : _______ cm

TANDAN SAMPEL
----------------------------------------------------------------
#1: Posisi [ ]Atas [ ]Tengah [ ]Bawah
    Tinggi: _____ cm   Keliling: _____ cm

#2: Posisi [ ]Atas [ ]Tengah [ ]Bawah
    Tinggi: _____ cm   Keliling: _____ cm

#3: Posisi [ ]Atas [ ]Tengah [ ]Bawah
    Tinggi: _____ cm   Keliling: _____ cm

BUNGA (jika ada): [ ] Ya  [ ] Tidak
Posisi bunga    : _______________________________

CATATAN
----------------------------------------------------------------
_____________________________________________________________
_____________________________________________________________

Foto ID: ___________________________________________________
================================================================
```

### 1.4.5 Workflow Pengukuran

```
+---------------------------------------------------+
| 1. Pilih pohon sampel                             |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| 2. Ukur keliling batang pada ketinggian 150cm     |
|    - Tandai titik pengukuran untuk referensi foto |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| 3. Identifikasi 3 tandan sampel                   |
|    - 1 dari pelepah atas (muda)                   |
|    - 1 dari pelepah tengah (transisi)             |
|    - 1 dari pelepah bawah (tua)                   |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| 4. Ukur dimensi setiap tandan                     |
|    - Tinggi vertikal                              |
|    - Keliling horizontal                          |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| 5. Foto pohon dari 4 sisi                         |
|    - Pastikan titik 150cm masuk frame             |
|    - Ikuti protokol standar (U-T-S-B)             |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| 6. Catat semua data di form                       |
|    - Hubungkan dengan ID foto                     |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| 7. Lanjut ke pohon berikutnya                     |
+---------------------------------------------------+
```

### 1.4.6 Peralatan

| Alat | Fungsi |
|------|--------|
| Meteran pita (3m+) | Keliling batang dan tandan |
| Meteran rigid | Tinggi tandan |
| Form cetak | Pencatatan manual |
| Smartphone | Foto + backup digital |
| Power bank | Cadangan daya |
| Clipboard | Alas tulis di lapangan |

---

## 1.5 Output Data

### Data Reguler

| Item | Estimasi |
|------|----------|
| Total pohon | 800-1000 |
| Foto per pohon | 4 |
| Total foto | 3200-4000 |
| Setelah QC (estimasi reject 10-15%) | 2800-3600 |

### Data Tambahan

| Item | Estimasi |
|------|----------|
| Total pohon sampel | 50-100 |
| Foto per pohon | 4 |
| Record pengukuran | 50-100 |
| Data tandan per pohon | 3 |

---

## 1.6 Checklist Persiapan

### Surveyor Reguler

- [ ] Smartphone dengan baterai penuh
- [ ] Power bank
- [ ] Tripod (opsional)
- [ ] Memahami protokol pengambilan

### Tim Data Tambahan

- [ ] Form pengukuran cetak (100+ lembar)
- [ ] Meteran pita minimal 3 meter
- [ ] Meteran rigid
- [ ] Smartphone dengan baterai penuh
- [ ] Power bank
- [ ] Clipboard
- [ ] Alat tulis
