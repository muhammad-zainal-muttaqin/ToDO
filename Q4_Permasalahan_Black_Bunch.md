# 4. PERMASALAHAN BLACK BUNCH: DIFERENSIASI TANDAN HITAM

## 4.1 Deskripsi Masalah

Tandan sawit pada tahap pra-matang dan mentah memiliki warna visual yang serupa (hitam), sehingga diferensiasi berdasarkan warna RGB memiliki keterbatasan.

### 4.1.1 Karakteristik Visual Tandan

| Kategori | Warna | Ukuran Relatif | Estimasi Panen |
|----------|-------|----------------|----------------|
| Matang (M1) | Merah/oranye | Besar | 1 bulan |
| Pra-Matang (M2) | Hitam | Besar | 2 bulan |
| Mentah (M3) | Hitam | Sedang | 3 bulan |
| Mentah (M4) | Hitam | Kecil | 4 bulan |
| Bunga | Kuning/cream | Sangat kecil | > 4 bulan |

### 4.1.2 Persoalan Utama

Klasifikasi M2, M3, dan M4 berdasarkan fitur warna saja tidak memadai karena ketiganya memiliki warna serupa (hitam). Diperlukan fitur tambahan untuk meningkatkan akurasi klasifikasi.

---

## 4.2 Aspek Pembeda Menurut Observasi Pakar

Berdasarkan pengamatan pakar di lapangan, berikut aspek yang digunakan untuk membedakan tandan hitam:

| Aspek | Deskripsi | Tingkat Keandalan |
|-------|-----------|-------------------|
| Ukuran relatif | Tandan matang berukuran lebih besar | Tinggi |
| Posisi vertikal | Tandan tua berada di pelepah bawah | Tinggi |
| Tekstur | Tandan matang memiliki duri lebih mekar | Sedang |
| Warna | Variasi nuansa hitam | Rendah |

### 4.2.1 Diagram Posisi Tandan pada Pohon

```
    POHON SAWIT (Tampak Samping)

    +----------------------------------+
    |                                  |
    |   PELEPAH ATAS                   |
    |   ~~~~~~~~                       |
    |      (O)  <- Tandan Muda (M3/M4) |
    |       |      Ukuran: Kecil       |
    |       |      Warna: Hitam        |
    |                                  |
    +----------------------------------+
    |                                  |
    |   PELEPAH TENGAH                 |
    |   ~~~~~~~~~~                     |
    |      (O)  <- Tandan Transisi     |
    |       |      Ukuran: Sedang      |
    |       |      Warna: Hitam        |
    |                                  |
    +----------------------------------+
    |                                  |
    |   PELEPAH BAWAH                  |
    |   ~~~~~~~~~~                     |
    |      (O)  <- Tandan Tua (M1/M2)  |
    |       |      Ukuran: Besar       |
    |       |      Warna: Merah/Hitam  |
    |                                  |
    +----------------------------------+
    |         |                        |
    |      BATANG                      |
    |    ...150cm...  <- Titik ukur    |
    |         |                        |
    +---------+------------------------+
            TANAH
```

---

## 4.3 Usulan Ekstraksi Fitur Tambahan

### 4.3.1 Fitur Berbasis Ukuran Relatif

Menggunakan batang sebagai referensi ukuran karena batang selalu ada dalam setiap frame.

**Formula:**
```
Rasio_Ukuran = Luas_BoundingBox_Tandan / Luas_BoundingBox_Batang
```

**Implementasi:**
```python
def calculate_relative_size(tandan_box, batang_box):
    """
    Menghitung rasio ukuran tandan relatif terhadap batang.
    
    Parameters:
        tandan_box: [x1, y1, x2, y2] koordinat bounding box tandan
        batang_box: [x1, y1, x2, y2] koordinat bounding box batang
    
    Returns:
        float: rasio ukuran
    """
    tandan_area = (tandan_box[2] - tandan_box[0]) * (tandan_box[3] - tandan_box[1])
    batang_area = (batang_box[2] - batang_box[0]) * (batang_box[3] - batang_box[1])
    
    if batang_area > 0:
        return tandan_area / batang_area
    return 0
```

### 4.3.2 Fitur Berbasis Posisi Vertikal

Menggunakan posisi Y dalam frame sebagai indikator usia tandan.

**Formula:**
```
Posisi_Relatif = Y_Center_Tandan / Height_Frame
```

**Interpretasi:**

| Nilai | Posisi | Interpretasi |
|-------|--------|--------------|
| < 0.3 | Atas | Tandan muda |
| 0.3 - 0.6 | Tengah | Transisi |
| > 0.6 | Bawah | Tandan tua |

**Implementasi:**
```python
def calculate_vertical_position(tandan_box, frame_height):
    """
    Menghitung posisi vertikal relatif tandan dalam frame.
    
    Parameters:
        tandan_box: [x1, y1, x2, y2] koordinat bounding box
        frame_height: tinggi frame dalam pixel
    
    Returns:
        float: posisi relatif (0-1)
    """
    y_center = (tandan_box[1] + tandan_box[3]) / 2
    return y_center / frame_height
```

### 4.3.3 Fitur Berbasis Tekstur

#### Edge Density

Tandan matang memiliki duri yang lebih mekar, menghasilkan densitas edge yang lebih tinggi.

```python
import cv2
import numpy as np

def extract_edge_density(image_crop):
    """
    Mengekstrak densitas edge dari crop tandan.
    
    Parameters:
        image_crop: array numpy, crop gambar tandan
    
    Returns:
        float: densitas edge (0-1)
    """
    gray = cv2.cvtColor(image_crop, cv2.COLOR_BGR2GRAY)
    edges = cv2.Canny(gray, 50, 150)
    edge_density = np.sum(edges > 0) / edges.size
    return edge_density
```

#### Local Binary Patterns (LBP)

```python
from skimage.feature import local_binary_pattern

def extract_lbp_features(image_crop, radius=3, n_points=24):
    """
    Mengekstrak fitur LBP untuk analisis tekstur.
    
    Parameters:
        image_crop: array numpy, crop gambar tandan
        radius: radius LBP
        n_points: jumlah titik sampling
    
    Returns:
        array: histogram LBP ternormalisasi
    """
    gray = cv2.cvtColor(image_crop, cv2.COLOR_BGR2GRAY)
    lbp = local_binary_pattern(gray, n_points, radius, method='uniform')
    hist, _ = np.histogram(lbp.ravel(), bins=n_points+2, range=(0, n_points+2))
    return hist / hist.sum()
```

### 4.3.4 Fitur Berbasis Warna (Enhanced)

Transformasi color space untuk meningkatkan sensitivitas terhadap variasi warna gelap.

```python
def extract_color_features(image_crop):
    """
    Mengekstrak fitur warna dari berbagai color space.
    
    Parameters:
        image_crop: array numpy, crop gambar tandan
    
    Returns:
        array: vektor fitur warna
    """
    # RGB mean
    rgb_mean = image_crop.mean(axis=(0,1))
    
    # HSV mean
    hsv = cv2.cvtColor(image_crop, cv2.COLOR_BGR2HSV)
    hsv_mean = hsv.mean(axis=(0,1))
    
    # LAB mean (L channel sensitif terhadap luminance)
    lab = cv2.cvtColor(image_crop, cv2.COLOR_BGR2LAB)
    lab_mean = lab.mean(axis=(0,1))
    
    return np.concatenate([rgb_mean, hsv_mean, lab_mean])
```

#### Contrast Enhancement

```python
def enhance_contrast(image):
    """
    Meningkatkan kontras menggunakan CLAHE.
    
    Parameters:
        image: array numpy, gambar BGR
    
    Returns:
        array: gambar dengan kontras yang ditingkatkan
    """
    lab = cv2.cvtColor(image, cv2.COLOR_BGR2LAB)
    l, a, b = cv2.split(lab)
    
    clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8,8))
    l_enhanced = clahe.apply(l)
    
    enhanced = cv2.merge([l_enhanced, a, b])
    return cv2.cvtColor(enhanced, cv2.COLOR_LAB2BGR)
```

### 4.3.5 Fitur Berbasis Depth

Jika data depth tersedia, ukuran absolut dapat diestimasi.

**Formula:**
```
Ukuran_Absolut = Ukuran_Pixel x Depth_Value x Faktor_Kalibrasi
```

```python
def calculate_absolute_size(tandan_box, depth_map, intrinsics):
    """
    Mengestimasi ukuran absolut tandan menggunakan data depth.
    
    Parameters:
        tandan_box: [x1, y1, x2, y2] koordinat bounding box
        depth_map: array numpy, depth map
        intrinsics: parameter intrinsik kamera
    
    Returns:
        tuple: (width_meters, height_meters)
    """
    x1, y1, x2, y2 = tandan_box
    cx, cy = (x1 + x2) / 2, (y1 + y2) / 2
    
    depth = depth_map[int(cy), int(cx)]
    
    # Konversi pixel ke meter
    width_pixels = x2 - x1
    height_pixels = y2 - y1
    
    width_meters = width_pixels * depth / intrinsics['fx']
    height_meters = height_pixels * depth / intrinsics['fy']
    
    return width_meters, height_meters
```

---

## 4.4 Arsitektur Model

### 4.4.1 Opsi 1: Feature Engineering + Classifier

```
+----------------+     +------------------+     +-------------------+
|    RGB Image   | --> |  YOLO Detection  | --> |   Crop Tandan     |
+----------------+     +------------------+     +-------------------+
                                                         |
                                                         v
                                          +----------------------------+
                                          |   Feature Extraction       |
                                          |   - Ukuran relatif         |
                                          |   - Posisi vertikal        |
                                          |   - Tekstur (edge/LBP)     |
                                          |   - Warna (HSV/LAB)        |
                                          +----------------------------+
                                                         |
                                                         v
                                          +----------------------------+
                                          |   Classifier               |
                                          |   - SVM                    |
                                          |   - Random Forest          |
                                          |   - MLP                    |
                                          +----------------------------+
                                                         |
                                                         v
                                          +----------------------------+
                                          |   Output: M1/M2/M3/M4      |
                                          +----------------------------+
```

### 4.4.2 Opsi 2: Multi-Input Fusion

```
Input 1: RGB Image ────────────┐
                               |
Input 2: Depth Map ────────────+──> Fusion Layer ──> CNN ──> Classification
                               |
Input 3: Texture Map ──────────┘
```

### 4.4.3 Opsi 3: RGB-D 4-Channel

```
+----------------+     +----------------+
|  RGB (3 ch)    |     |  Depth (1 ch)  |
|  R, G, B       |     |       D        |
+-------+--------+     +-------+--------+
        |                      |
        +----------+-----------+
                   |
                   v
        +----------------------+
        |  Concatenate         |
        |  4 Channel Input     |
        +----------------------+
                   |
                   v
        +----------------------+
        |  Modified YOLOv8     |
        |  (4-channel input)   |
        +----------------------+
                   |
                   v
        +----------------------+
        |  Detection +         |
        |  Classification      |
        +----------------------+
```

---

## 4.5 Rekomendasi Pendekatan

| Prioritas | Fitur | Kompleksitas Implementasi | Potensi Dampak |
|-----------|-------|---------------------------|----------------|
| 1 | Ukuran relatif | Rendah | Tinggi |
| 2 | Posisi vertikal | Rendah | Tinggi |
| 3 | Depth (jika tersedia) | Sedang | Tinggi |
| 4 | Edge density | Sedang | Sedang |
| 5 | Color space (HSV/LAB) | Rendah | Rendah |
| 6 | LBP/Gabor | Tinggi | Sedang |

### Tahapan Implementasi

**Fase 1 (Dasar):**
- Implementasi fitur ukuran relatif
- Implementasi fitur posisi vertikal
- Evaluasi baseline

**Fase 2 (Menengah):**
- Tambahkan fitur tekstur (edge density)
- Eksperimen dengan color space transformation
- Evaluasi peningkatan akurasi

**Fase 3 (Lanjutan):**
- Implementasi RGB-D jika hardware tersedia
- Eksperimen dengan arsitektur multi-input
- Optimasi final

---

## 4.6 Desain Eksperimen

| ID | Eksperimen | Input | Fitur Tambahan |
|----|------------|-------|----------------|
| E0 | Baseline | RGB | Tidak ada |
| E1 | Size | RGB | Ukuran relatif |
| E2 | Position | RGB | Posisi vertikal |
| E3 | Size + Position | RGB | Kedua fitur |
| E4 | Texture | RGB | Edge density |
| E5 | RGB-D | RGB + Depth | 4-channel |
| E6 | Kombinasi | RGB + Depth | Semua fitur |

### Metrik Evaluasi

| Metrik | Deskripsi |
|--------|-----------|
| mAP@0.5 | Mean Average Precision pada IoU 0.5 |
| mAP@0.5:0.95 | mAP pada range IoU |
| Per-class Accuracy | Akurasi per kelas (M1, M2, M3, M4) |
| Confusion Matrix | Matriks kesalahan klasifikasi |

---

## 4.7 Ringkasan

### Permasalahan

Diferensiasi tandan sawit kategori M2, M3, dan M4 menghadapi tantangan karena ketiganya memiliki warna visual serupa (hitam).

### Solusi

Penggunaan fitur tambahan di luar warna RGB:

1. **Ukuran relatif** - menggunakan batang sebagai referensi
2. **Posisi vertikal** - berdasarkan hipotesis biologis
3. **Tekstur** - densitas edge dan pola tekstur
4. **Depth** - estimasi ukuran absolut (jika hardware tersedia)

### Prioritas Implementasi

Fitur ukuran relatif dan posisi vertikal memiliki rasio dampak terhadap kompleksitas yang tertinggi dan direkomendasikan untuk implementasi awal.
