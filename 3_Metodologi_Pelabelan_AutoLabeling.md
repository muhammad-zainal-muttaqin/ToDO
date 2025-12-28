# BAGIAN 3: METODOLOGI PELABELAN & AUTO-LABELING

**PIC:** Zainal (Supervisor)

---

## 1. Definisi Kelas BBC (Estimasi Panen)

### Logika Pelabelan

> **⚠️ PENTING:** Label bukan deskripsi kondisi saat ini, tapi **KAPAN PANEN**

| Label | Artinya | Contoh (Survey Desember) |
|-------|---------|--------------------------|
| **M1 (1 Bulan)** | Panen n+1 | Panen Januari |
| **M2 (2 Bulan)** | Panen n+2 | Panen Februari |
| **M3 (3 Bulan)** | Panen n+3 | Panen Maret |
| **M4 (4 Bulan)** | Panen n+4 | Panen April |

### Kelas Tambahan

| Kelas | Fungsi |
|-------|--------|
| **Bunga** | Prediksi jangka panjang (>4 bulan) |
| **Batang** | Referensi ukuran untuk AI |

### Siklus Survey BBC

```
┌──────────────────────────────────────────────────────────────┐
│                    SIKLUS SURVEY BBC                          │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│   Survey (Des)  →  Estimasi 4 bulan ke depan                 │
│       │                                                       │
│       ├── M1 → Panen Jan                                     │
│       ├── M2 → Panen Feb                                     │
│       ├── M3 → Panen Mar                                     │
│       └── M4 → Panen Apr                                     │
│                                                               │
│   [Panen tiap 2 minggu - Continue]                           │
│                                                               │
│   Survey (Apr)  →  Estimasi 4 bulan ke depan                 │
│       │                                                       │
│       ├── M1 → Panen Mei                                     │
│       └── ... dan seterusnya                                 │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

---

## 2. Strategi Auto-Labeling

### 2.1 Tujuan Auto-Labeling

```
TANPA Auto-Label:                DENGAN Auto-Label:
─────────────────                ─────────────────────
Pakar menggambar kotak    →     Pakar hanya VALIDASI
dari NOL untuk setiap           (koreksi jika salah)
objek                           

Waktu: ████████████████   →     Waktu: ████░░░░░░░░░░
       (Sangat Lama)                   (Jauh Lebih Cepat)
```

### 2.2 Tools Rekomendasi

#### 🥇 UTAMA: Tanpa Internet (OFFLINE)

**Tool: AnyLabeling**

| Aspek | Detail |
|-------|--------|
| **Website** | https://github.com/vietanhdev/anylabeling |
| **Lisensi** | Gratis (Open Source) |
| **Platform** | Windows, Mac, Linux |
| **Fitur Utama** | Support YOLO, Segment Anything Model (SAM) |
| **Keunggulan** | Desktop based, tidak perlu internet setelah install |

**Cara Install:**
```bash
# Menggunakan pip
pip install anylabeling

# Atau download executable dari GitHub releases
```

**Fitur Auto-Label di AnyLabeling:**
1. ✅ Load custom YOLO model
2. ✅ SAM (Segment Anything) untuk segmentasi otomatis
3. ✅ Batch processing
4. ✅ Export ke format YOLO

#### 🥈 ALTERNATIF: Dengan Internet (ONLINE)

**Tool: Roboflow**

| Aspek | Detail |
|-------|--------|
| **Website** | https://roboflow.com |
| **Lisensi** | Freemium (ada tier gratis) |
| **Platform** | Web-based |
| **Fitur Utama** | Auto-label, Kolaborasi tim, Augmentasi |
| **Keunggulan** | Mudah digunakan, banyak model pre-trained |

---

## 3. Workflow Teknis Auto-Labeling

### 3.1 Diagram Alur

```
┌─────────────────────────────────────────────────────────────┐
│ STEP 1: LABELING MANUAL (SEED DATA)                         │
│ ─────────────────────────────────────                       │
│ • Zainal melabeli manual 50-100 gambar (batch kecil)        │
│ • Gunakan AnyLabeling dengan mode manual                    │
│ • Label: M1, M2, M3, M4, Bunga, Batang                      │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ STEP 2: TRAINING MODEL AWAL                                 │
│ ─────────────────────────────                               │
│ • Train YOLOv8-Nano di laptop Zainal                        │
│ • Dataset: 50-100 gambar yang sudah dilabel                 │
│ • Output: Model .pt (pre-trained weights)                   │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ STEP 3: LOAD MODEL KE ANYLABELING                           │
│ ─────────────────────────────────                           │
│ • Import model YOLOv8 custom ke AnyLabeling                 │
│ • Test dengan beberapa gambar untuk verifikasi              │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ STEP 4: AUTO-LABEL BATCH BESAR                              │
│ ────────────────────────────                                │
│ • Jalankan "Auto-Label" pada sisa dataset (~900 gambar)     │
│ • Model akan membuat bounding box otomatis                  │
│ • Export hasil ke format YOLO                               │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ STEP 5: VALIDASI PAKAR                                      │
│ ─────────────────────                                       │
│ • Setup: Komputer + Pakar + Zainal (Supervisor)             │
│ • Pakar mengecek hasil auto-label satu per satu             │
│ • Jika kotak salah → geser/hapus/tambah                     │
│ • Jika kelas salah → ubah kelas                             │
│ • ⚡ INI JAUH LEBIH CEPAT daripada dari nol!                │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ STEP 6: EXPORT FINAL DATASET                                │
│ ────────────────────────────                                │
│ • Export dataset yang sudah divalidasi                      │
│ • Format: YOLO (.txt annotations)                           │
│ • Siap untuk training model final                           │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Script Training YOLOv8-Nano (Python)

```python
from ultralytics import YOLO

# Load model
model = YOLO('yolov8n.pt')  # nano version untuk kecepatan

# Training dengan dataset awal
results = model.train(
    data='dataset.yaml',
    epochs=50,
    imgsz=640,
    batch=16,
    name='sawit_autolab_v1'
)

# Export untuk AnyLabeling
model.export(format='onnx')
```

### 3.3 Struktur Dataset YOLO

```
dataset/
├── images/
│   ├── train/
│   │   ├── IMG_001.jpg
│   │   ├── IMG_002.jpg
│   │   └── ...
│   └── val/
│       ├── IMG_100.jpg
│       └── ...
├── labels/
│   ├── train/
│   │   ├── IMG_001.txt
│   │   ├── IMG_002.txt
│   │   └── ...
│   └── val/
│       ├── IMG_100.txt
│       └── ...
└── dataset.yaml
```

### 3.4 Format Label YOLO (contoh .txt)

```
# Format: class_id x_center y_center width height
# Nilai normalized (0-1)

0 0.5 0.4 0.1 0.15    # M1 (1 bulan)
1 0.3 0.6 0.12 0.18   # M2 (2 bulan)
2 0.7 0.5 0.08 0.12   # M3 (3 bulan)
4 0.4 0.8 0.05 0.06   # Bunga
5 0.5 0.9 0.15 0.25   # Batang
```

### 3.5 dataset.yaml

```yaml
path: ./dataset
train: images/train
val: images/val

names:
  0: M1_1bulan
  1: M2_2bulan
  2: M3_3bulan
  3: M4_4bulan
  4: Bunga
  5: Batang
```

---

## 4. Tips Validasi dengan Pakar

### Keyboard Shortcuts di AnyLabeling

| Shortcut | Fungsi |
|----------|--------|
| `D` | Next image |
| `A` | Previous image |
| `W` | Create rectangle |
| `Delete` | Delete selected box |
| `Ctrl+S` | Save |

### Workflow Validasi Efisien

1. **Pakar fokus lihat** → Apakah bounding box sudah benar posisinya?
2. **Pakar fokus lihat** → Apakah kelasnya sudah benar?
3. **Zainal (Supervisor)** → Navigasi gambar, handle technical issues
4. **Target** → 10-20 gambar per menit (jika auto-label akurat 80%+)

---

## 5. Checklist Auto-Labeling

### Persiapan
- [ ] Install AnyLabeling di laptop
- [ ] Install Ultralytics (pip install ultralytics)
- [ ] Siapkan folder struktur dataset

### Fase 1: Seed Data
- [ ] Label manual 50 gambar pertama
- [ ] Review kualitas label
- [ ] Export ke format YOLO

### Fase 2: Training
- [ ] Training YOLOv8-Nano
- [ ] Evaluasi mAP (target >50%)
- [ ] Export ke ONNX untuk AnyLabeling

### Fase 3: Auto-Label
- [ ] Load model ke AnyLabeling
- [ ] Test dengan 10 gambar
- [ ] Jalankan batch auto-label

### Fase 4: Validasi
- [ ] Koordinasi jadwal dengan Pakar
- [ ] Siapkan workstation
- [ ] Jalankan sesi validasi
- [ ] Export final dataset
