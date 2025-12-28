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
| SORTIR MANUAL                            |
| - Cek kriteria penolakan                 |
| - Tandai foto yang tidak memenuhi standar|
+------------------------------------------+
                    |
                    v
+------------------------------------------+
| KLASIFIKASI                              |
|  [PASS] -> Masuk dataset                 |
|  [REJECT] -> Dokumentasi alasan          |
+------------------------------------------+
                    |
                    v
OUTPUT
+------------------------------------------+
| ~3400-3600 foto yang memenuhi standar    |
+------------------------------------------+
```

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
| Repository | github.com/vietanhdev/anylabeling |
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
| Fitur | Auto-label, augmentasi, kolaborasi |

### 2.3.4 Workflow Auto-Labeling

```
FASE 1: SEED DATA
+--------------------------------------------------+
| Zainal melabel manual 50-100 gambar              |
| - Menggunakan AnyLabeling mode manual            |
| - Label: M1, M2, M3, M4, Bunga, Batang           |
+--------------------------------------------------+
                         |
                         v
FASE 2: TRAINING MODEL AWAL
+--------------------------------------------------+
| Train YOLOv8-Nano dengan seed data               |
| - Dataset: 50-100 gambar berlabel                |
| - Output: model weights (.pt)                    |
+--------------------------------------------------+
                         |
                         v
FASE 3: EXPORT MODEL
+--------------------------------------------------+
| Export model ke format ONNX                      |
| - Untuk kompatibilitas dengan AnyLabeling        |
+--------------------------------------------------+
                         |
                         v
FASE 4: AUTO-LABEL
+--------------------------------------------------+
| Load model ke AnyLabeling                        |
| Jalankan inferensi pada sisa dataset             |
| - Input: ~3400 gambar                            |
| - Output: anotasi otomatis                       |
+--------------------------------------------------+
                         |
                         v
FASE 5: VALIDASI PAKAR
+--------------------------------------------------+
| Pakar mereview hasil auto-label                  |
| - Koreksi bounding box jika salah posisi         |
| - Koreksi kelas jika salah klasifikasi           |
| - Tambah deteksi yang terlewat                   |
+--------------------------------------------------+
                         |
                         v
FASE 6: EXPORT DATASET FINAL
+--------------------------------------------------+
| Export dalam format YOLO                         |
| - Split: train (80%) / val (20%)                 |
+--------------------------------------------------+
```

### 2.3.5 Script Training YOLOv8

```python
from ultralytics import YOLO

# Load pretrained model
model = YOLO('yolov8n.pt')

# Training configuration
results = model.train(
    data='dataset.yaml',
    epochs=50,
    imgsz=640,
    batch=16,
    name='sawit_prelabel_v1'
)

# Export untuk AnyLabeling
model.export(format='onnx')
```

---

## 2.4 Teknis Labeling dengan Pakar

### 2.4.1 Konfigurasi Sesi

| Parameter | Spesifikasi |
|-----------|-------------|
| Lokasi | Ruangan dengan komputer |
| Durasi sesi | 2-3 jam per sesi |
| Peserta | 1 pakar + 1 operator (Zainal) |
| Software | AnyLabeling dengan model pre-trained |
| Input | Dataset dengan pre-label |

### 2.4.2 Pembagian Peran

| Peran | Tugas |
|-------|-------|
| Pakar | Validasi label, koreksi kelas, koreksi posisi |
| Operator | Navigasi gambar, operasi teknis, dokumentasi |

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
| 5. Simpan dan lanjut           |
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

### 2.4.5 Target Kecepatan

| Akurasi Auto-Label | Target per Menit |
|--------------------|------------------|
| > 80% | 15-20 gambar |
| 50-80% | 8-12 gambar |
| < 50% | 4-6 gambar |

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

### Pre-Labeling

- [ ] Label manual 50-100 gambar (seed data)
- [ ] Training YOLOv8-Nano
- [ ] Evaluasi mAP pada validation set
- [ ] Export model ke ONNX
- [ ] Test auto-label pada 10 gambar

### Labeling dengan Pakar

- [ ] Koordinasi jadwal dengan pakar
- [ ] Jalankan auto-label pada full dataset
- [ ] Sesi validasi dengan pakar
- [ ] Export dataset final

### Verifikasi

- [ ] Cek distribusi label per kelas
- [ ] Verifikasi format file
- [ ] Split train/val (80/20)
- [ ] Backup dataset
