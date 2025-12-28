# BAGIAN 5: RENCANA PENELITIAN & NOVELTY (KEBARUAN)

**Fokus:** Eksperimen Akademis & Kebaruan Penelitian

---

## 1. Usulan Metode Utama

### 1.1 Arsitektur Model

```
INPUT:                    MODEL:              OUTPUT:
┌─────────────┐         ┌─────────┐        ┌──────────────┐
│ RGB (3ch)   │────┐    │         │        │ Bounding Box │
│ R, G, B     │    │    │ YOLOv8  │───────▶│ + Kelas BBC  │
└─────────────┘    ├───▶│ (4ch)   │        │ (M1-M4)      │
┌─────────────┐    │    │         │        └──────────────┘
│ Depth (1ch) │────┘    └─────────┘
└─────────────┘
```

### 1.2 Detail Teknis

| Aspek | Spesifikasi |
|-------|-------------|
| **Model Base** | YOLOv8-Nano (untuk kecepatan) |
| **Input Channel** | 4 (R-G-B + Depth) |
| **Transfer Learning** | Pre-train weights COCO → Fine-tune sawit |
| **Output** | Detection + Classification (M1, M2, M3, M4, Bunga, Batang) |

### 1.3 Modifikasi Input Layer

```python
# Mengubah input dari 3 channel ke 4 channel
from ultralytics import YOLO

# Load pretrained model
model = YOLO('yolov8n.pt')

# Modifikasi untuk 4 channel (perlu custom implementation)
# Option 1: Fusion RGB + Depth di early layer
# Option 2: Late fusion dengan 2 backbone terpisah
```

---

## 2. Eksperimen Komparasi

### 2.1 Tiga Metode yang Dibandingkan

| Metode | Deskripsi | Jumlah Gambar |
|--------|-----------|---------------|
| **A (Standar)** | 4 Sisi (U-T-S-B) | 4 per pohon |
| **B (Detailed)** | 8 Sisi (tambah diagonal) | 8 per pohon |
| **C (Video)** | Video 360° mengelilingi | 1 video per pohon |

### 2.2 Metode A: 4 Sisi (Standar BBC)

```
        N (Utara)
           │
           ▼
    ┌──────────────┐
    │              │
W ──│    POHON     │── E
    │              │
    └──────────────┘
           │
           ▼
        S (Selatan)

Arah: U → T → S → B (Clockwise)
```

### 2.3 Metode B: 8 Sisi (Detailed)

```
          N
          │
    NW    │    NE
      \   │   /
       \  │  /
    ┌─────────────┐
    │             │
W ──│   POHON     │── E
    │             │
    └─────────────┘
       /  │  \
      /   │   \
    SW    │    SE
          │
          S

Arah: U → TL → T → TG → S → BD → B → BL (Clockwise)
Hipotesis: Mengurangi blind spot (buah tertutup pelepah)
```

### 2.4 Metode C: Video 360°

```
                Start
                  │
                  ▼
    ┌─────────────────────────┐
    │         POHON           │
    │                         │
    │    ←───────────────     │
    │    │               │    │
    │    │   Record      │    │
    │    │   Video       │    │
    │    │               │    │
    │    └───────────────→    │
    │                         │
    └─────────────────────────┘

Fungsi: Ground Truth (Kunci Jawaban)
- Tidak ada sisi terlewat
- Data paling lengkap
- Untuk verifikasi akurasi metode A dan B
```

---

## 3. Rencana Eksperimen

### 3.1 Desain Eksperimen

```
TAHAP 1: Pengumpulan Data
─────────────────────────
• Ambil data dengan KETIGA metode pada 50 pohon yang SAMA
• Catat ground truth manual (hitungan asli)

TAHAP 2: Processing
───────────────────
• Train model untuk masing-masing metode
• Metode A: YOLOv8 + 4 gambar
• Metode B: YOLOv8 + 8 gambar
• Metode C: Video → Frame extraction → YOLOv8

TAHAP 3: Evaluasi
─────────────────
• Bandingkan akurasi counting dengan ground truth
• Metrik: Accuracy, Precision, Recall, F1
• Tentukan metode terbaik
```

### 3.2 Hipotesis yang Diuji

| Hipotesis | Prediksi |
|-----------|----------|
| H1: 8 sisi > 4 sisi | 8 sisi lebih akurat karena mengurangi blind spot |
| H2: Video > 8 sisi | Video paling akurat karena continuous coverage |
| H3: Depth membantu | Dengan depth, ukuran estimasi lebih akurat |

---

## 4. Output Klasifikasi

### 4.1 Sistem Counting Otomatis

```
Per Pohon:
┌────────────────────────────────────────────┐
│ Gambar/Video → Model → Deteksi Tandan      │
│                           │                │
│                           ▼                │
│                   ┌───────────────┐        │
│                   │ Agregasi 4/8  │        │
│                   │ sisi gambar   │        │
│                   └───────────────┘        │
│                           │                │
│                           ▼                │
│            ┌──────────────────────────┐    │
│            │ Output per pohon:        │    │
│            │ • M1: 2 tandan           │    │
│            │ • M2: 3 tandan           │    │
│            │ • M3: 5 tandan           │    │
│            │ • M4: 1 tandan           │    │
│            │ • Bunga: 4               │    │
│            └──────────────────────────┘    │
└────────────────────────────────────────────┘
```

### 4.2 Estimasi Hasil Panen

```
Menggunakan data kalibrasi dari Tugas Pengukuran Fisik:

Estimasi Panen = Σ (Jumlah Tandan × Rata-rata Berat per Kelas)

Contoh:
M1: 100 tandan × 20kg = 2000 kg (panen bulan depan)
M2: 150 tandan × 15kg = 2250 kg (panen 2 bulan)
M3: 200 tandan × 10kg = 2000 kg (panen 3 bulan)
M4: 180 tandan × 5kg  = 900 kg  (panen 4 bulan)
```

---

## 5. Checklist Penelitian

### Fase Persiapan
- [ ] Finalisasi desain eksperimen
- [ ] Persiapan hardware (Batch 1: RGB, Batch 2+: Depth)
- [ ] Koordinasi dengan surveyor

### Fase Pengumpulan Data
- [ ] Batch 1: RGB 4 sisi (10 kelompok)
- [ ] Batch khusus: Pengukuran fisik 100 pohon (Zainal)
- [ ] Batch eksperimen: 50 pohon dengan 3 metode (A, B, C)

### Fase Processing
- [ ] Sortir dan QC gambar
- [ ] Auto-labeling + Validasi pakar
- [ ] Training model

### Fase Evaluasi
- [ ] Bandingkan 3 metode
- [ ] Analisis statistik
- [ ] Penulisan hasil
