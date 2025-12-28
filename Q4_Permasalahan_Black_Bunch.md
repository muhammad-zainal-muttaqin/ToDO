# 4. PERMASALAHAN BLACK BUNCH: DIFERENSIASI TANDAN HITAM

---

## A. Deskripsi Masalah

Tandan sawit yang belum matang (pra-matang dan mentah) sama-sama berwarna **hitam**, sehingga sulit dibedakan hanya dari warna RGB.

| Kategori | Warna | Ukuran | Status |
|----------|-------|--------|--------|
| Matang | Merah/oranye | Besar | Siap panen |
| Pra-Matang (M2) | **Hitam** | Besar | 2 bulan lagi |
| Mentah (M3/M4) | **Hitam** | Kecil-Sedang | 3-4 bulan lagi |

**Tantangan:** Bagaimana membedakan M2, M3, dan M4 yang sama-sama hitam?

---

## B. Aspek yang Diperhatikan Pakar di Lapangan

Berdasarkan observasi pakar, berikut aspek yang digunakan untuk membedakan tandan hitam:

| Aspek | Deskripsi | Kekuatan |
|-------|-----------|----------|
| **Ukuran (relatif)** | Tandan matang lebih besar dari mentah | Kuat |
| **Posisi vertikal** | Tandan tua di pelepah bawah, muda di atas | Kuat |
| **Tekstur** | Tandan matang lebih "berisi", duri lebih mekar | Sedang |
| **Warna** | Nuansa hitam sedikit berbeda | Paling Lemah |

---

## C. Usulan Preprocessing & Ekstraksi Fitur Eksplisit

### C.1 Apakah Perlu?

**Ya, sangat direkomendasikan** untuk meningkatkan akurasi klasifikasi black bunch.

Plain RGB mungkin tidak cukup menangkap perbedaan halus. Preprocessing eksplisit dapat membantu model "melihat" fitur yang digunakan pakar.

### C.2 Usulan Fitur Berbasis Ukuran (Relatif)

#### Pendekatan 1: Rasio Ukuran Tandan vs Batang

```
Fitur = Luas_BoundingBox_Tandan / Luas_BoundingBox_Batang
```

> Karena batang harus ada dalam setiap frame, bisa digunakan sebagai referensi ukuran.

#### Pendekatan 2: Estimasi Ukuran Absolut

Jika data depth tersedia:
```
Ukuran_Absolut = Ukuran_Pixel × Depth_Value × Faktor_Kalibrasi
```

#### Implementasi

```python
def calculate_relative_size(tandan_box, batang_box):
    """
    Hitung rasio ukuran tandan relatif terhadap batang
    """
    tandan_area = (tandan_box[2] - tandan_box[0]) * (tandan_box[3] - tandan_box[1])
    batang_area = (batang_box[2] - batang_box[0]) * (batang_box[3] - batang_box[1])
    
    return tandan_area / batang_area if batang_area > 0 else 0
```

### C.3 Usulan Fitur Berbasis Posisi Vertikal

#### Pendekatan: Posisi Y Relatif dalam Frame

```
Posisi_Relatif = Y_Center_Tandan / Height_Frame
```

| Posisi | Interpretasi |
|--------|--------------|
| Atas (< 0.3) | Kemungkinan tandan muda |
| Tengah (0.3-0.6) | Transisi |
| Bawah (> 0.6) | Kemungkinan tandan tua |

#### Implementasi

```python
def calculate_vertical_position(tandan_box, frame_height):
    """
    Hitung posisi vertikal relatif tandan dalam frame
    """
    y_center = (tandan_box[1] + tandan_box[3]) / 2
    return y_center / frame_height
```

### C.4 Usulan Fitur Berbasis Tekstur

#### Pendekatan 1: Analisis Tepi (Edge Detection)

Tandan matang memiliki duri yang lebih mekar, menghasilkan lebih banyak edge.

```python
import cv2
import numpy as np

def extract_texture_features(image_crop):
    """
    Ekstrak fitur tekstur dari crop tandan
    """
    gray = cv2.cvtColor(image_crop, cv2.COLOR_BGR2GRAY)
    
    # Canny edge detection
    edges = cv2.Canny(gray, 50, 150)
    edge_density = np.sum(edges > 0) / edges.size
    
    return edge_density
```

#### Pendekatan 2: Local Binary Patterns (LBP)

```python
from skimage.feature import local_binary_pattern

def extract_lbp_features(image_crop, radius=3, n_points=24):
    """
    Ekstrak fitur LBP untuk analisis tekstur
    """
    gray = cv2.cvtColor(image_crop, cv2.COLOR_BGR2GRAY)
    lbp = local_binary_pattern(gray, n_points, radius, method='uniform')
    
    # Histogram sebagai fitur
    hist, _ = np.histogram(lbp.ravel(), bins=n_points+2, range=(0, n_points+2))
    return hist / hist.sum()
```

#### Pendekatan 3: Gabor Filters

```python
from skimage.filters import gabor

def extract_gabor_features(image_crop):
    """
    Ekstrak fitur Gabor untuk analisis tekstur multi-skala
    """
    gray = cv2.cvtColor(image_crop, cv2.COLOR_BGR2GRAY)
    features = []
    
    for frequency in [0.1, 0.2, 0.3, 0.4]:
        for theta in [0, np.pi/4, np.pi/2, 3*np.pi/4]:
            filt_real, filt_imag = gabor(gray, frequency, theta)
            features.append(filt_real.mean())
            features.append(filt_real.std())
    
    return np.array(features)
```

### C.5 Usulan Fitur Berbasis Warna (Enhanced)

Meskipun warna adalah pembeda terlemah, preprocessing bisa meningkatkan kontras:

#### Pendekatan 1: Color Space Transformation

```python
def extract_color_features(image_crop):
    """
    Ekstrak fitur warna dari berbagai color space
    """
    # RGB
    rgb_mean = image_crop.mean(axis=(0,1))
    
    # HSV - mungkin lebih sensitif terhadap nuansa hitam
    hsv = cv2.cvtColor(image_crop, cv2.COLOR_BGR2HSV)
    hsv_mean = hsv.mean(axis=(0,1))
    
    # LAB - L channel untuk luminance
    lab = cv2.cvtColor(image_crop, cv2.COLOR_BGR2LAB)
    lab_mean = lab.mean(axis=(0,1))
    
    return np.concatenate([rgb_mean, hsv_mean, lab_mean])
```

#### Pendekatan 2: Histogram Equalization

```python
def enhance_contrast(image):
    """
    Tingkatkan kontras untuk memperlihatkan nuansa hitam
    """
    lab = cv2.cvtColor(image, cv2.COLOR_BGR2LAB)
    l, a, b = cv2.split(lab)
    
    # CLAHE pada L channel
    clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8,8))
    l_enhanced = clahe.apply(l)
    
    enhanced = cv2.merge([l_enhanced, a, b])
    return cv2.cvtColor(enhanced, cv2.COLOR_LAB2BGR)
```

---

## D. Arsitektur Model dengan Fitur Tambahan

### D.1 Opsi 1: Feature Engineering + Classifier

```
RGB Image → YOLO (Detection) → Crop Tandan
                                    ↓
                        ┌───────────────────────┐
                        │ Feature Extraction:    │
                        │ - Ukuran relatif       │
                        │ - Posisi vertikal      │
                        │ - Tekstur (LBP/Gabor)  │
                        │ - Warna (HSV/LAB)      │
                        └───────────────────────┘
                                    ↓
                        Classifier (SVM/RandomForest/MLP)
                                    ↓
                                Kelas (M1/M2/M3/M4)
```

### D.2 Opsi 2: Multi-Input CNN

```
Input 1: RGB Image ──────────────┐
                                 │
Input 2: Depth Map ──────────────┼──→ Fusion CNN → Klasifikasi
                                 │
Input 3: Texture Feature Map ────┘
```

### D.3 Opsi 3: End-to-End dengan Attention

```
RGB Image → CNN Backbone → Spatial Attention (fokus tekstur)
                                    ↓
                        Classification Head
                                    ↓
                            M1/M2/M3/M4
```

---

## E. Rekomendasi Pendekatan

| Prioritas | Pendekatan | Kompleksitas | Potensi Dampak |
|-----------|------------|--------------|----------------|
| 1 | Ukuran relatif (vs batang) | Rendah | Tinggi |
| 2 | Posisi vertikal | Rendah | Sedang-Tinggi |
| 3 | Depth (jika tersedia) | Sedang | Tinggi |
| 4 | Tekstur (edge density) | Sedang | Sedang |
| 5 | Enhanced color (HSV/LAB) | Rendah | Rendah |
| 6 | LBP/Gabor | Tinggi | Sedang |

### Rekomendasi Langkah:

1. **Fase 1 (Simple):** Mulai dengan ukuran relatif + posisi vertikal
2. **Fase 2 (Medium):** Tambahkan fitur tekstur sederhana (edge density)
3. **Fase 3 (Advanced):** Eksperimen dengan depth dan multi-input model

---

## F. Eksperimen yang Diusulkan

| Eksperimen | Deskripsi |
|------------|-----------|
| Baseline | YOLO dengan plain RGB |
| Exp 1 | YOLO + fitur ukuran relatif |
| Exp 2 | YOLO + fitur posisi vertikal |
| Exp 3 | YOLO + tekstur |
| Exp 4 | YOLO + RGB-D (4 channel) |
| Exp 5 | Kombinasi terbaik |

---

## G. Kesimpulan

**Jawaban:** Ya, preprocessing dan ekstraksi fitur eksplisit **sangat direkomendasikan** untuk mengatasi permasalahan black bunch.

Fitur yang paling promising:
1. **Ukuran relatif** - mudah diimplementasi, dampak tinggi
2. **Posisi vertikal** - mudah diimplementasi, validasi hipotesis biologis
3. **Depth** - jika hardware tersedia, memberikan ukuran absolut

Plain RGB saja kemungkinan tidak cukup untuk membedakan tandan hitam dengan akurasi tinggi.
