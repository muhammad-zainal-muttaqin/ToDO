# 2. DATA: QUALITY CONTROL, LABELING, DAN ANOTASI

## 2.1 Ikhtisar

Dokumen ini menjelaskan prosedur pengecekan kualitas data, strategi auto-labeling untuk efisiensi, teknis pelaksanaan labeling dengan pakar, dan spesifikasi anotasi.

---

## 2.2 Quality Control (QC)

### 2.2.1 Kriteria Penerimaan

| Kriteria | Deskripsi |
|----------|-----------|
| Fokus | Objek utama (tandan, batang) tajam |
| Pencahayaan | Merata, tidak backlight |
| Komposisi | Batang dan tajuk terlihat |
| Urutan | Sesuai protokol (U-T-S-B) |
| Metadata | ID pohon terdokumentasi |

### 2.2.2 Kriteria Penolakan

| Kategori | Deskripsi | Estimasi % |
|----------|-----------|------------|
| Blur | Fokus tidak tajam | 5% |
| Backlight | Objek gelap karena cahaya belakang | 3% |
| Komposisi salah | Batang atau tajuk tidak terlihat | 2% |
| Lainnya | Corrupt file, salah orientasi | 2% |
| **Total Reject** | | **10-15%** |

### 2.2.3 Prosedur QC

```
INPUT
+------------------------------------------+
| ~4000 foto (1000 pohon x 4 sisi)         |
+------------------------------------------+
                    |
                    v
+------------------------------------------+
| SORTIR                                   |
| - Cek kriteria penolakan                 |
| - Klasifikasi: PASS / REJECT             |
+------------------------------------------+
                    |
          +---------+---------+
          |                   |
          v                   v
+------------------+   +------------------+
| PASS             |   | REJECT           |
| -> Dataset utama |   | -> Dipisahkan    |
|                  |   | -> Tetap dilabeli|
|                  |   | -> Untuk training|
|                  |   |    tambahan      |
+------------------+   +------------------+
          |                   |
          +---------+---------+
                    |
                    v
OUTPUT
+------------------------------------------+
| Dataset utama: ~3400-3600 foto           |
| Dataset tambahan: ~400-600 foto (reject) |
+------------------------------------------+
```

**Catatan:** Data yang kurang ideal (reject) tetap dipisahkan dan dilabeli untuk dapat digunakan sebagai data training/pengujian tambahan.

---

## 2.3 Auto-Labeling

### 2.3.1 Tujuan

Auto-labeling bertujuan untuk menghasilkan anotasi awal (pre-label) yang akan divalidasi oleh pakar. Pendekatan ini mengurangi waktu labeling dibandingkan pembuatan anotasi dari awal.

### 2.3.2 Perbandingan Metode

| Aspek | Manual dari Awal | Dengan Auto-Label |
|-------|------------------|-------------------|
| Tugas pakar | Gambar bounding box | Validasi dan koreksi |
| Waktu per gambar | 2-5 menit | 15-30 detik |
| Throughput | 12-30 gambar/jam | 120-240 gambar/jam |

### 2.3.3 Tools

#### Opsi Utama: AnyLabeling (Offline)

| Aspek | Spesifikasi |
|-------|-------------|
| Repository | [github.com/vietanhdev/anylabeling](https://github.com/vietanhdev/anylabeling) |
| Lisensi | Open Source |
| Platform | Windows, macOS, Linux |
| Fitur | YOLO, SAM, batch processing |
| Instalasi | `pip install anylabeling` |

#### Opsi Alternatif: Roboflow (Online)

| Aspek | Spesifikasi |
|-------|-------------|
| Website | roboflow.com |
| Lisensi | Freemium |
| Platform | Web-based |
| Fitur | Auto-label, augmentasi, kolaborasi, pretrained models |

### 2.3.4 Workflow Auto-Labeling (Alternatif dengan Pretrained Model)

Karena seed data awal (200 gambar) mungkin terlalu sedikit untuk training detector yang akurat, berikut workflow alternatif menggunakan pretrained model:

```
FASE 1: PRELABEL DETEKSI TANDAN (Bounding Box Only)
+----------------------------------------------------------+
| Gunakan model pretrained deteksi tandan sawit dari       |
| Roboflow (cari model dengan mAP tinggi)                  |
| - Ignore/buang kelas dari model pretrained               |
| - Ambil hanya output bounding box                        |
| - Hasil: semua tandan terdeteksi tanpa kelas             |
+----------------------------------------------------------+
                         |
                         v
FASE 2: LABEL KELAS (SEED) - 100 Pohon / 400 Images
+----------------------------------------------------------+
| Pakar memberikan label KELAS pada 100 pohon              |
| - Tidak perlu menggambar kotak baru                      |
| - Hanya assign kelas: M1/M2/M3/M4/Bunga                  |
| - Koreksi bounding box jika posisi salah                 |
+----------------------------------------------------------+
                         |
                         v
FASE 3: TRAIN CLASSIFIER
+----------------------------------------------------------+
| Train model klasifikasi dari 100 pohon (400 images)      |
| - Input: crop tandan dari bounding box                   |
| - Output: classifier M1/M2/M3/M4/Bunga                   |
+----------------------------------------------------------+
                         |
                         v
FASE 4: AUTO-LABEL KELAS (Pohon 101 dst)
+----------------------------------------------------------+
| Untuk pohon 101 dst:                                     |
| - Deteksi bounding box dengan pretrained model           |
| - Assign kelas dengan trained classifier                 |
| - Hasil: prelabel lengkap (box + kelas)                  |
+----------------------------------------------------------+
                         |
                         v
FASE 5: VALIDASI/PENGECEKAN KELAS OLEH PAKAR
+----------------------------------------------------------+
| Pakar mereview hasil auto-label kelas                    |
| - Koreksi kelas jika salah                               |
| - Koreksi bounding box jika perlu                        |
| - Tandai ketidakyakinan (lihat 2.4.5)                    |
+----------------------------------------------------------+
                         |
                         v
FASE 6: EXPORT DATASET FINAL
+----------------------------------------------------------+
| Export dalam format YOLO                                 |
| - Split: train (80%) / val (20%)                         |
+----------------------------------------------------------+
```

### 2.3.5 Script Training YOLOv8 (Untuk Classifier)

```python
from ultralytics import YOLO

# Load pretrained model
model = YOLO('yolov8n-cls.pt')  # classifier

# Training configuration
results = model.train(
    data='dataset_crops/',  # folder dengan crop tandan
    epochs=50,
    imgsz=224,
    batch=32,
    name='sawit_classifier_v1'
)
```

---

## 2.4 Teknis Labeling dengan Pakar

### 2.4.1 Konfigurasi Tim

| Parameter | Spesifikasi |
|-----------|-------------|
| Tim | 2 tim paralel |
| Per tim | 1 asisten + 2 pakar |
| Total pakar | 4 orang (dari GMK) |
| Opsi kerja | 2 pakar sekaligus (diskusi) atau bergantian |
| Alokasi waktu | 3-5 hari x 7 jam/hari |
| Lokasi | Kantor GMK atau sewa meeting space (??) |

### 2.4.2 Analisis Feasibility & Jadwal Harian

**Analisis Feasibility:**
Workflow ini dinilai **sangat layak (feasible)** karena:
1.  **Deteksi Objek:** Tandan buah sawit memiliki fitur visual yang kontras terhadap batang dan daun, sehingga model *pretrained* (YOLOv8) dapat mendeteksi *bounding box* dengan akurasi tinggi meskipun belum dilatih khusus pada dataset ini.
2.  **Klasifikasi Pakar:** Tugas tersulit bagi AI (membedakan kematangan M1-M4) adalah tugas termudah bagi pakar. Dengan otomatisasi *bounding box*, pakar hanya perlu fokus pada konfirmasi kelas.
3.  **Efisiensi:** Mengurangi beban kognitif pakar dari "menggambar" menjadi "memilih", yang secara signifikan mempercepat proses tanpa mengurangi kualitas.

**Estimasi Jadwal Harian (8 Jam Total):**

| Waktu | Durasi | Aktivitas |
|-------|--------|-----------|
| 08:30 - 09:00 | 30 menit | Persiapan sistem, *load* dataset, dan briefing harian |
| 09:00 - 10:30 | 90 menit | **Sesi Labeling I** |
| 10:30 - 10:45 | 15 menit | *Coffee Break* I (Istirahat sejenak) |
| 10:45 - 12:00 | 75 menit | **Sesi Labeling II** |
| 12:00 - 13:00 | 60 menit | **Istirahat, Makan Siang, & Sholat** |
| 13:00 - 15:00 | 120 menit | **Sesi Labeling III** |
| 15:00 - 15:15 | 15 menit | *Coffee Break* II (Istirahat sejenak) |
| 15:15 - 16:15 | 60 menit | **Sesi Labeling IV** |
| 16:15 - 16:30 | 15 menit | *Backup* data harian dan evaluasi progres |

**Ringkasan Waktu Efektif:**
- **Total Durasi:** 8 jam
- **Waktu Kerja Efektif:** **~6 jam 45 menit (405 menit)** per hari.
- **Istirahat/Break:** 1 jam 15 menit.
- **Estimasi Output:** Dengan asumsi moderat 10 gambar/menit (Akurasi AI 50-80%), satu tim dapat menyelesaikan **~4.000 gambar per hari**. Dengan 2 tim paralel, target 3.000 - 4.000 gambar total dapat diselesaikan dengan sangat nyaman dalam 1-2 hari kerja.

### 2.4.3 Pembagian Peran

| Peran | Tugas |
|-------|-------|
| Pakar | Validasi label, koreksi kelas, koreksi posisi |
| Asisten | Navigasi gambar, operasi teknis, dokumentasi |

### 2.4.3 Alur Validasi per Gambar

```
+--------------------------------+
| 1. Tampilkan gambar            |
+--------------------------------+
              |
              v
+--------------------------------+
| 2. Cek bounding box            |
|    - Posisi sudah benar?       |
|    - Ukuran sudah sesuai?      |
+--------------------------------+
              |
              v
+--------------------------------+
| 3. Cek klasifikasi             |
|    - Kelas sudah benar?        |
|    - M1/M2/M3/M4/Bunga/Batang? |
+--------------------------------+
              |
              v
+--------------------------------+
| 4. Koreksi jika diperlukan     |
|    - Geser/resize bounding box |
|    - Ubah kelas                |
|    - Hapus false positive      |
|    - Tambah false negative     |
+--------------------------------+
              |
              v
+--------------------------------+
| 5. Validasi total count        |
|    - Cek jumlah total tandan   |
|      per view/pohon            |
+--------------------------------+
              |
              v
+--------------------------------+
| 6. Tandai ketidakyakinan       |
|    (jika pakar tidak yakin)    |
+--------------------------------+
              |
              v
+--------------------------------+
| 7. Simpan dan lanjut           |
+--------------------------------+
```

### 2.4.4 Keyboard Shortcuts (AnyLabeling)

| Shortcut | Fungsi |
|----------|--------|
| D | Gambar berikutnya |
| A | Gambar sebelumnya |
| W | Buat bounding box baru |
| Delete | Hapus bounding box terpilih |
| Ctrl+S | Simpan |
| Ctrl+Z | Undo |

### 2.4.5 Fitur Ketidakyakinan Pakar

Saat validasi kelas, jika pakar kurang yakin dengan labelnya, informasi ini perlu didokumentasikan.

**Implementasi:**

| Metode | Deskripsi |
|--------|-----------|
| Atribut tambahan | Tambahkan field "confidence" pada anotasi |
| Kelas khusus | Gunakan suffix "_uncertain" (contoh: M2_uncertain) |
| File terpisah | Catat ID gambar dan box yang tidak pasti |

**Format dengan atribut:**
```
# class_id x_center y_center width height confidence
0 0.5000 0.4000 0.1000 0.1500 1.0
1 0.3000 0.6000 0.1200 0.1800 0.7  # uncertain
```

**Kegunaan:**
- Dapat digunakan sebagai sample weight saat training
- Analisis kasus-kasus yang sulit
- Evaluasi konsistensi antar pakar

### 2.4.6 Target Kecepatan

Tabel di bawah menunjukkan estimasi throughput (jumlah gambar yang divalidasi) oleh pakar berdasarkan kualitas hasil estimasi awal dari model AI (Auto-Label):

| Akurasi Auto-Label | Target per Menit | Beban Kerja Pakar |
|--------------------|------------------|-------------------|
| **Sangat Baik (> 80%)** | 15-20 gambar | Pakar hanya melakukan konfirmasi cepat dan sesekali koreksi minor. |
| **Cukup (50-80%)** | 8-12 gambar | Pakar perlu memperbaiki beberapa bounding box dan mengubah label kelas yang salah. |
| **Rendah (< 50%)** | 4-6 gambar | Pakar praktis melakukan labeling ulang karena hasil AI terlalu banyak error. |

**Penjelasan Logika:**
- **Akurasi Tinggi:** Semakin akurat model pre-labeling, semakin sedikit interaksi manual yang diperlukan. Fokus pakar beralih dari "menggambar" menjadi "memverifikasi".
- **Validasi Tree Count:** Penambahan langkah validasi jumlah total tandan per pohon (langkah ke-5 pada alur 2.4.3) mungkin akan sedikit menurunkan throughput ini, namun sangat kritikal untuk memastikan tidak ada tandan yang terlewat (False Negative) di setiap sudut pandang.
- **Konsistensi vs Kecepatan:** Meskipun ada target kecepatan, akurasi klasifikasi (M1-M4) dan ketepatan jumlah tandan tetap menjadi prioritas utama.

---

## 2.5 Spesifikasi Anotasi

### 2.5.1 Definisi Kelas

#### Kelas Utama (Estimasi Panen)

| Kode | Label | Definisi |
|------|-------|----------|
| 0 | M1 | Tandan panen dalam 1 bulan |
| 1 | M2 | Tandan panen dalam 2 bulan |
| 2 | M3 | Tandan panen dalam 3 bulan |
| 3 | M4 | Tandan panen dalam 4 bulan |

Label mengacu pada estimasi waktu panen, bukan kondisi visual saat ini.

#### Kelas Tambahan

| Kode | Label | Definisi |
|------|-------|----------|
| 4 | Bunga | Bunga sawit (prediksi > 4 bulan) |
| 5 | Batang | Batang pohon (referensi ukuran) |

### 2.5.2 Format Anotasi YOLO

Struktur folder:
```
dataset/
    images/
        train/
            IMG_001.jpg
            IMG_002.jpg
            ...
        val/
            IMG_100.jpg
            ...
    labels/
        train/
            IMG_001.txt
            IMG_002.txt
            ...
        val/
            IMG_100.txt
            ...
    dataset.yaml
```

Format file label (.txt):
```
# class_id x_center y_center width height
# Nilai dinormalisasi (0-1)

0 0.5000 0.4000 0.1000 0.1500
1 0.3000 0.6000 0.1200 0.1800
5 0.5000 0.8500 0.1500 0.3000
```

File dataset.yaml:
```yaml
path: ./dataset
train: images/train
val: images/val

names:
  0: M1
  1: M2
  2: M3
  3: M4
  4: Bunga
  5: Batang
```

### 2.5.3 Kelas Opsional (Pertimbangan)

| Kelas | Kegunaan |
|-------|----------|
| Pelepah | Segmentasi struktur pohon |
| Tandan_Kosong | Tandan pasca-panen |
| Rusak | Tandan yang rusak/sakit |

Penambahan kelas opsional tergantung pada kebutuhan analisis.

---

## 2.6 Checklist

### Persiapan

- [ ] Install AnyLabeling
- [ ] Install Ultralytics (`pip install ultralytics`)
- [ ] Siapkan struktur folder dataset
- [ ] Siapkan dataset.yaml
- [ ] Cari model pretrained tandan sawit di Roboflow (mAP tinggi)

### Pre-Labeling

- [ ] Prelabel deteksi dengan model pretrained (box only)
- [ ] Pakar label kelas untuk 100 pohon (400 images)
- [ ] Train classifier dari seed data
- [ ] Auto-label kelas untuk sisa dataset
- [ ] Test hasil auto-label pada 10 gambar

### Labeling dengan Pakar

- [ ] Koordinasi jadwal dengan 4 pakar GMK
- [ ] Siapkan 2 tim paralel
- [ ] Jalankan sesi validasi (3-5 hari x 7 jam)
- [ ] Dokumentasi ketidakyakinan pakar
- [ ] Export dataset final

### Verifikasi

- [ ] Cek distribusi label per kelas
- [ ] Analisis label dengan ketidakyakinan
- [ ] Verifikasi format file
- [ ] Split train/val (80/20)
- [ ] Backup dataset
