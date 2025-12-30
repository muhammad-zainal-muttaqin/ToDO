# 1. PERSIAPAN DATA COLLECTION

## 1.1 Ikhtisar

Dokumen ini menjelaskan spesifikasi dan protokol pengumpulan data untuk proyek deteksi tandan buah sawit. Pengumpulan data dibagi menjadi beberapa kategori: data reguler, data tambahan untuk kalibrasi, dan data eksperimen untuk perbandingan metode.

---

## 1.2 Tim Pelaksana

| Kategori | Personel | Target | Tugas |
|----------|----------|--------|-------|
| Data Reguler | 10 orang (5 tim, 2 orang/tim) | 150-200 pohon/tim | Pengambilan foto 4 sisi |
| Data Tambahan | 1 tim | 50 pohon | Pengukuran fisik + foto |
| Data Eksperimen | Tim khusus | 50 pohon | 3 metode (4 sisi, 8 sisi, video) |
| **Total** | **6 tim** | **750-1000 pohon** | - |

---

## 1.3 Pengumpulan Data Reguler (750-1000 Pohon)

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

**Catatan:** Penentuan arah "Utara" dapat menggunakan referensi relatif (misalnya arah jalan masuk) jika kompas tidak tersedia. Yang terpenting adalah konsistensi urutan clockwise.

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
| Waktu | Sesuai jam kerja survei (08:00-16:00, dengan istirahat) |
| Cuaca | Catat kondisi cuaca pada setiap sesi |
| Pencahayaan | Hindari backlight (matahari di belakang pohon) |
| Sudut kamera | Lurus ke depan (eye-level) |
| Metadata | Catat waktu dan kondisi cuaca per batch |

### 1.3.7 Jadwal Batch

| Batch | Waktu | Tipe Data | Target |
|-------|-------|-----------|--------|
| 1 | Januari | RGB 4 sisi | 800-1000 pohon |
| 2 | April | RGB + Depth | TBD |
| Eksperimen | Bersamaan Batch 1 | 4 sisi + 8 sisi + video | 50 pohon |

---

## 1.4 Pengumpulan Data Tambahan (Pengukuran Fisik)

### 1.4.1 Revisi Sampling

Berdasarkan revisi, pengukuran fisik dibagi menjadi dua komponen:

| Komponen | Jumlah Pohon | Parameter |
|----------|--------------|-----------|
| Pengukuran keliling batang | 50 pohon | Keliling batang @150cm |
| Pengukuran tandan | 10 pohon | 3 tandan per pohon |

### 1.4.2 Tujuan

| Tujuan | Penjelasan |
|--------|------------|
| Kalibrasi ukuran | Data ukuran fisik untuk membedakan tandan hitam |
| Validasi hipotesis | Posisi vertikal tandan (tua di bawah, muda di atas) |
| Ground truth | Referensi validasi model AI |

### 1.4.3 Hipotesis Biologis

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

### 1.4.4 Parameter Pengukuran

#### A. Keliling Batang (50 Pohon)

| Parameter | Spesifikasi |
|-----------|-------------|
| Pengukuran | Keliling lingkar batang |
| Ketinggian | 150 cm dari tanah |
| Alat | Meteran pita |
| Syarat | Titik ukur harus masuk dalam frame foto |

#### B. Dimensi Tandan (10 Pohon x 3 Tandan = 30 Tandan)

| Parameter | Cara Ukur |
|-----------|-----------|
| Tinggi tandan | Vertikal, ujung bawah ke ujung atas |
| Keliling tandan | Horizontal, lingkar terbesar |
| Posisi pelepah | Atas / Tengah / Bawah |

### 1.4.5 Template Pencatatan

```
================================================================
FORM PENGUKURAN POHON - TIM 11
================================================================
Tanggal     : ___/___/______
Waktu       : ___:___
Lokasi      : _______________________________
Nomor Pohon : _____ / 50
Cuaca       : [ ] Cerah  [ ] Berawan  [ ] Hujan

BATANG
----------------------------------------------------------------
Keliling @150cm : _______ cm

TANDAN SAMPEL (isi jika pohon 1-10)
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

Foto ID: ___________________________________________________
================================================================
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

## 1.5 Pengumpulan Data Eksperimen (50 Pohon)

Data eksperimen diambil untuk membandingkan tiga metode pengambilan gambar:

### 1.5.1 Tiga Metode yang Dibandingkan

| Metode | Deskripsi | Jumlah Data per Pohon |
|--------|-----------|----------------------|
| A: 4 Sisi | Standar (U-T-S-B) | 4 foto |
| B: 8 Sisi | Tambah diagonal (NE, SE, SW, NW) | 8 foto |
| C: Video 360 | Video keliling pohon | 1 video |

### 1.5.2 Metode A: 4 Sisi (Standar)

```
        N (Utara)
           |
           v
    +--------------+
    |              |
W --|    POHON     |-- E
    |              |
    +--------------+
           |
           v
        S (Selatan)

Urutan: U -> T -> S -> B (Clockwise)
```

### 1.5.3 Metode B: 8 Sisi (Detail)

```
          N
          |
    NW    |    NE
      \   |   /
       \  |  /
    +----------+
    |          |
W --|  POHON   |-- E
    |          |
    +----------+
       /  |  \
      /   |   \
    SW    |    SE
          |
          S

Urutan: N -> NE -> E -> SE -> S -> SW -> W -> NW (Clockwise)
Hipotesis: Mengurangi blind spot karena pelepah
```

### 1.5.4 Metode C: Video 360

```
                Start/End
                    |
                    v
    +---------------------------+
    |                           |
    |         POHON             |
    |           X               |
    |                           |
    |    <-----------------     |
    |    |                 |    |
    |    |   SURVEYOR      |    |
    |    |   berjalan      |    |
    |    |   keliling      |    |
    |    |                 |    |
    |    ------------------>    |
    |                           |
    +---------------------------+

Spesifikasi Video:
- Resolusi: 1080p minimum
- Durasi: 15-30 detik per pohon
- Gerakan: Smooth, tanpa goyangan berlebih
- Jarak: Konsisten 2-3 meter dari batang

Fungsi:
- Ground truth untuk counting
- Data paling lengkap (tidak ada sisi terlewat)
- Untuk verifikasi akurasi metode A dan B
```

### 1.5.5 Desain Eksperimen

```
+---------------------------------------------------+
| 50 pohon yang SAMA diambil dengan KETIGA metode   |
+---------------------------------------------------+
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
    +-----------+  +-----------+  +-----------+
    | Metode A  |  | Metode B  |  | Metode C  |
    | 4 Sisi    |  | 8 Sisi    |  | Video     |
    | 200 foto  |  | 400 foto  |  | 50 video  |
    +-----------+  +-----------+  +-----------+
          |              |              |
          +--------------+--------------+
                         |
                         v
    +---------------------------------------------------+
    | Ground Truth: Hitungan manual per pohon           |
    +---------------------------------------------------+
```

### 1.5.6 Hipotesis yang Diuji

| ID | Hipotesis | Prediksi |
|----|-----------|----------|
| H1 | 8 sisi > 4 sisi | 8 sisi lebih akurat karena mengurangi blind spot |
| H2 | Video > 8 sisi | Video paling akurat karena continuous coverage |
| H3 | Depth membantu | Dengan depth, estimasi ukuran lebih akurat |

---

## 1.6 Output Data

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
| Pohon dengan keliling batang | 50 |
| Pohon dengan ukuran tandan | 10 |
| Total tandan diukur | 30 |

### Data Eksperimen

| Item | Estimasi |
|------|----------|
| Pohon eksperimen | 50 |
| Total foto 4 sisi | 200 |
| Total foto 8 sisi | 400 |
| Total video | 50 |

---

## 1.7 Checklist Persiapan

### Surveyor Reguler

- [ ] Smartphone dengan baterai penuh
- [ ] Power bank
- [ ] Tripod (opsional)
- [ ] Memahami protokol pengambilan

### Tim Data Tambahan

- [ ] Form pengukuran cetak (60+ lembar)
- [ ] Meteran pita minimal 3 meter
- [ ] Meteran rigid
- [ ] Smartphone dengan baterai penuh
- [ ] Power bank
- [ ] Clipboard
- [ ] Alat tulis

### Tim Eksperimen

- [ ] Smartphone dengan mode video 1080p
- [ ] Storage cukup untuk ~50 video
- [ ] Power bank
- [ ] Tripod/stabilizer (opsional)
- [ ] Memahami ketiga metode
