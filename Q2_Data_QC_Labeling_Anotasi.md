# 2. DATA: QUALITY CONTROL, AUTO-LABELING & ANOTASI

---

## A. Pengecekan Kualitas Data (Quality Control)

### A.1 Kriteria Reject

| Kriteria | Deskripsi |
|----------|-----------|
| Blur | Gambar tidak fokus/kabur |
| Backlight | Pencahayaan dari belakang objek terlalu kuat |
| Batang tidak terlihat | Titik 150cm tidak masuk dalam frame |
| Framing salah | Tajuk/tandan tidak terlihat |
| Urutan salah | Tidak mengikuti rotasi U-T-S-B |

### A.2 Prosedur QC

```
INPUT: ~4000 foto (1000 pohon × 4 sisi)
           ↓
    [SORTIR MANUAL]
           ↓
    Buang foto yang tidak memenuhi kriteria
           ↓
OUTPUT: ~3400-3600 foto yang memenuhi standar
```

### A.3 Estimasi Rejection Rate

| Kategori | Estimasi % |
|----------|------------|
| Blur | 5% |
| Backlight | 3% |
| Framing salah | 2% |
| Lainnya | 2% |
| **Total Reject** | **~10-15%** |

---

## B. Auto-Label untuk Pre-Labeling

### B.1 Tujuan Auto-Labeling

| Tanpa Auto-Label | Dengan Auto-Label |
|-----------------|-------------------|
| Pakar menggambar bounding box dari nol | Pakar hanya validasi & koreksi |
| Waktu: sangat lama | Waktu: jauh lebih cepat |

### B.2 Rekomendasi Tools

#### Utama: AnyLabeling (Offline)

| Aspek | Detail |
|-------|--------|
| Website | https://github.com/vietanhdev/anylabeling |
| Lisensi | Gratis, Open Source |
| Platform | Windows, Mac, Linux |
| Fitur | YOLO, SAM (Segment Anything), batch processing |
| Keunggulan | Tidak perlu internet setelah install |

**Instalasi:**
```bash
pip install anylabeling
```

#### Alternatif: Roboflow (Online)

| Aspek | Detail |
|-------|--------|
| Website | https://roboflow.com |
| Lisensi | Freemium |
| Platform | Web-based |
| Keunggulan | Mudah, kolaborasi tim |

### B.3 Workflow Auto-Labeling

```
STEP 1: SEED DATA
├── Zainal label manual 50-100 gambar
├── Gunakan AnyLabeling mode manual
└── Label: M1, M2, M3, M4, Bunga, Batang

          ↓

STEP 2: TRAIN MODEL AWAL
├── Train YOLOv8-Nano dengan seed data
├── Dataset: 50-100 gambar berlabel
└── Output: model .pt

          ↓

STEP 3: AUTO-LABEL
├── Load model ke AnyLabeling
├── Jalankan auto-label pada sisa dataset
└── Export format YOLO

          ↓

STEP 4: VALIDASI PAKAR
├── Pakar cek hasil auto-label
├── Koreksi jika salah
└── Export final dataset
```

### B.4 Script Training YOLOv8 (Python)

```python
from ultralytics import YOLO

# Load model
model = YOLO('yolov8n.pt')

# Training
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

## C. Teknis Kegiatan Labeling dengan Pakar

### C.1 Setup

| Item | Detail |
|------|--------|
| Lokasi | Di depan komputer |
| Peserta | 1 Pakar + Zainal (Supervisor) |
| Software | AnyLabeling dengan model pre-trained |
| Data | Dataset yang sudah di-auto-label |

### C.2 Pembagian Tugas

| Role | Tugas |
|------|-------|
| **Pakar** | Validasi label, koreksi kelas, koreksi posisi bounding box |
| **Zainal** | Navigasi gambar, handle teknis, catat feedback |

### C.3 Workflow Validasi

1. **Cek bounding box** → Apakah posisi sudah benar?
2. **Cek kelas** → Apakah klasifikasi sudah benar (M1/M2/M3/M4)?
3. **Koreksi** → Geser/hapus/tambah bounding box jika perlu
4. **Next** → Lanjut ke gambar berikutnya

### C.4 Keyboard Shortcuts AnyLabeling

| Shortcut | Fungsi |
|----------|--------|
| D | Next image |
| A | Previous image |
| W | Create rectangle |
| Delete | Delete selected box |
| Ctrl+S | Save |

### C.5 Target Kecepatan

| Kondisi | Target per Menit |
|---------|------------------|
| Auto-label akurat >80% | 15-20 gambar |
| Auto-label akurat 50-80% | 8-12 gambar |
| Auto-label akurat <50% | 4-6 gambar |

---

## D. Anotasi: Kelas & Label Tambahan

### D.1 Kelas Utama (Estimasi Panen BBC)

| Label | Artinya | Contoh (Survey Desember) |
|-------|---------|--------------------------|
| **M1** | Panen 1 bulan lagi | Panen Januari |
| **M2** | Panen 2 bulan lagi | Panen Februari |
| **M3** | Panen 3 bulan lagi | Panen Maret |
| **M4** | Panen 4 bulan lagi | Panen April |

> **Logika:** Label bukan kondisi saat ini, tapi **KAPAN PANEN**

### D.2 Kelas Tambahan

| Kelas | Fungsi |
|-------|--------|
| **Bunga** | Prediksi jangka panjang (>4 bulan) |
| **Batang** | Referensi ukuran untuk AI |

### D.3 Kelas Opsional (untuk Pertimbangan)

| Kelas | Alasan |
|-------|--------|
| Pelepah | Untuk segmentasi struktur pohon |
| Tandan_Kosong | Tandan yang sudah dipanen (jika ada) |
| Damage | Tandan rusak/sakit (jika relevan) |

### D.4 Format Anotasi

**Struktur Folder:**
```
dataset/
├── images/
│   ├── train/
│   └── val/
├── labels/
│   ├── train/
│   └── val/
└── dataset.yaml
```

**Format Label YOLO (.txt):**
```
# class_id x_center y_center width height (normalized 0-1)
0 0.5 0.4 0.1 0.15    # M1
1 0.3 0.6 0.12 0.18   # M2
2 0.7 0.5 0.08 0.12   # M3
3 0.2 0.3 0.09 0.14   # M4
4 0.4 0.8 0.05 0.06   # Bunga
5 0.5 0.9 0.15 0.25   # Batang
```

**dataset.yaml:**
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

---

## E. Checklist Data Pipeline

### Persiapan
- [ ] Install AnyLabeling
- [ ] Install Ultralytics
- [ ] Siapkan struktur folder dataset

### Pre-Labeling
- [ ] Label manual 50-100 gambar (seed)
- [ ] Train YOLOv8-Nano
- [ ] Export model ke ONNX
- [ ] Test auto-label pada 10 gambar

### Labeling dengan Pakar
- [ ] Koordinasi jadwal dengan pakar
- [ ] Jalankan auto-label pada full dataset
- [ ] Sesi validasi dengan pakar
- [ ] Export final dataset

### Quality Check
- [ ] Verifikasi jumlah label per kelas
- [ ] Cek konsistensi format
- [ ] Split train/val (80/20)
