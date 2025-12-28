# 5. PERMASALAHAN COUNTING: TEKNIK PENGHITUNGAN TANDAN

## 5.1 Deskripsi Masalah

Penghitungan jumlah tandan buah sawit per pohon dari data multi-view (gambar/video) menghadapi beberapa tantangan teknis.

### 5.1.1 Tantangan Utama

| Tantangan | Deskripsi |
|-----------|-----------|
| Oklusi | Tandan tertutup oleh pelepah dari sudut tertentu |
| Double counting | Tandan yang sama terlihat dari beberapa sisi |
| Variasi tampilan | Tampilan visual tandan berbeda dari sudut berbeda |
| Multi-view fusion | Penggabungan deteksi dari 4 atau 8 sisi |

### 5.1.2 Ilustrasi Masalah Double Counting

```
        TAMPAK ATAS POHON
        
           UTARA
              |
    +---------+---------+
    |    (A)  |  (B)    |  <- Tandan A terlihat dari
    |         |         |     sisi Utara dan Barat
    |   (C)   X   (D)   |  <- Tandan D terlihat dari
    |         |         |     sisi Timur dan Selatan
    +---------+---------+
              |
           SELATAN

    Jika dihitung per sisi:
    - Utara:  A, B       = 2
    - Timur:  B, D       = 2
    - Selatan: C, D      = 2
    - Barat:  A, C       = 2
    -------------------------
    Total deteksi        = 8
    Jumlah tandan aktual = 4

    Diperlukan mekanisme de-duplikasi
```

---

## 5.2 Teknik Counting Tanpa Depth

### 5.2.1 Teknik 1: Simple Aggregation

**Konsep:** Mendeteksi tandan per sisi, kemudian menggunakan faktor koreksi untuk mengestimasi jumlah unik.

**Formula:**
```
Estimasi_Tandan = Total_Deteksi x Faktor_Koreksi
```

**Contoh:**
```
Sisi Utara   : 5 tandan
Sisi Timur   : 6 tandan
Sisi Selatan : 4 tandan
Sisi Barat   : 5 tandan
---------------------------
Total Deteksi: 20 tandan
Faktor Koreksi: 0.5
---------------------------
Estimasi Final: 10 tandan
```

| Kelebihan | Kekurangan |
|-----------|------------|
| Implementasi sederhana | Faktor koreksi perlu kalibrasi |
| Komputasi ringan | Tidak akurat untuk distribusi tidak merata |

### 5.2.2 Teknik 2: Multi-View NMS (Non-Maximum Suppression)

**Konsep:** Memproyeksikan deteksi ke koordinat silinder virtual, kemudian menerapkan clustering untuk menghilangkan duplikat.

**Alur Proses:**
```
+---------------------------------------------------+
| STEP 1: Deteksi per Sisi                          |
| - Jalankan YOLO pada setiap gambar                |
| - Output: bounding box + confidence               |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| STEP 2: Proyeksi ke Koordinat Silinder            |
| - Asumsi pohon sebagai silinder                   |
| - Konversi (x, y) pixel ke (theta, z)             |
|   - theta: sudut keliling (0-360 derajat)         |
|   - z: tinggi vertikal                            |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| STEP 3: Clustering                                |
| - Kelompokkan deteksi berdekatan di (theta, z)    |
| - Gunakan hierarchical clustering atau DBSCAN     |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| STEP 4: Seleksi Representative                    |
| - Pilih deteksi dengan confidence tertinggi       |
|   per cluster                                     |
+---------------------------------------------------+
                         |
                         v
+---------------------------------------------------+
| STEP 5: Count                                     |
| - Jumlah cluster = jumlah tandan unik             |
+---------------------------------------------------+
```

**Diagram Proyeksi Silinder:**
```
    KOORDINAT IMAGE (per sisi)      KOORDINAT SILINDER
    
    +------------------+                 360
    |                  |                  |
    | (x,y) pixel      |     theta = side_angle + (x - 0.5) * FOV
    |        *         |  -->             |
    |                  |                  *  (theta, z)
    +------------------+                  |
          |                              0
          |
          v
    z = y / height     (normalized)
```

**Implementasi:**
```python
import numpy as np
from scipy.cluster.hierarchy import fcluster, linkage

def cylindrical_projection(bbox, side_angle, image_width, image_height, fov=60):
    """
    Memproyeksikan bounding box ke koordinat silinder.
    
    Parameters:
        bbox: [x1, y1, x2, y2] koordinat bounding box
        side_angle: sudut sisi (0=Utara, 90=Timur, 180=Selatan, 270=Barat)
        image_width: lebar gambar dalam pixel
        image_height: tinggi gambar dalam pixel
        fov: field of view kamera dalam derajat
    
    Returns:
        tuple: (theta, z) koordinat silinder
    """
    x_center = (bbox[0] + bbox[2]) / 2 / image_width
    y_center = (bbox[1] + bbox[3]) / 2 / image_height
    
    theta = side_angle + (x_center - 0.5) * fov
    theta = theta % 360
    
    z = y_center
    
    return theta, z

def multi_view_nms(detections, distance_threshold=30):
    """
    NMS berbasis clustering di ruang silinder.
    
    Parameters:
        detections: list of (theta, z, confidence, class_id)
        distance_threshold: threshold clustering dalam derajat
    
    Returns:
        list: deteksi unik setelah NMS
    """
    if len(detections) == 0:
        return []
    
    coords = np.array([[d[0], d[1] * 360] for d in detections])
    Z = linkage(coords, method='average')
    clusters = fcluster(Z, t=distance_threshold, criterion='distance')
    
    unique_detections = []
    for cluster_id in np.unique(clusters):
        cluster_dets = [d for d, c in zip(detections, clusters) if c == cluster_id]
        best = max(cluster_dets, key=lambda x: x[2])
        unique_detections.append(best)
    
    return unique_detections
```

| Kelebihan | Kekurangan |
|-----------|------------|
| Menangani overlap secara eksplisit | Perlu kalibrasi FOV dan threshold |
| Lebih akurat dari simple aggregation | Asumsi silinder tidak sempurna |

### 5.2.3 Teknik 3: Tracking-based Counting (Video 360)

**Konsep:** Menggunakan object tracking pada video keliling pohon untuk menghitung objek unik.

**Alur Tracking:**
```
Frame 1    Frame 2    Frame 3    Frame 4    ...    Frame N
   |          |          |          |                 |
   v          v          v          v                 v
 [Det]     [Track]    [Track]    [Track]          [Track]
   |          |          |          |                 |
   v          v          v          v                 v
ID: 1,2    ID: 1,2,3  ID: 1,2,3,4  ID: ...        ID: ...
                                                      |
                                                      v
                                               Total Unique IDs
```

**Implementasi dengan Ultralytics:**
```python
from ultralytics import YOLO

model = YOLO('best.pt')

results = model.track(
    source='video_360.mp4',
    tracker='bytetrack.yaml',
    persist=True
)

unique_ids = set()
for frame_result in results:
    if frame_result.boxes.id is not None:
        unique_ids.update(frame_result.boxes.id.tolist())

total_count = len(unique_ids)
```

| Kelebihan | Kekurangan |
|-----------|------------|
| Akurasi tinggi jika tracking stabil | Memerlukan data video |
| Natural untuk video 360 | Tracking dapat gagal pada oklusi besar |

### 5.2.4 Teknik 4: Density Estimation

**Konsep:** Model memprediksi density map, kemudian mengintegrasikan untuk mendapatkan count.

```
RGB Image --> CNN --> Density Map --> Integration --> Count

    +----------------+     +----------------+
    |                |     |    . . .       |
    |  Input Image   | --> |   . . . .      | --> Integral = 10.2
    |                |     |    . .         |     Count = 10
    +----------------+     +----------------+
```

**Arsitektur:**
```python
import torch
import torch.nn as nn

class DensityCounter(nn.Module):
    def __init__(self):
        super().__init__()
        self.encoder = nn.Sequential(
            nn.Conv2d(3, 64, 3, padding=1), nn.ReLU(),
            nn.MaxPool2d(2),
            nn.Conv2d(64, 128, 3, padding=1), nn.ReLU(),
            nn.MaxPool2d(2),
            nn.Conv2d(128, 256, 3, padding=1), nn.ReLU(),
        )
        self.decoder = nn.Sequential(
            nn.ConvTranspose2d(256, 128, 4, stride=2, padding=1), nn.ReLU(),
            nn.ConvTranspose2d(128, 64, 4, stride=2, padding=1), nn.ReLU(),
            nn.Conv2d(64, 1, 1), nn.ReLU()
        )
    
    def forward(self, x):
        features = self.encoder(x)
        density = self.decoder(features)
        count = density.sum(dim=(2, 3))
        return density, count
```

| Kelebihan | Kekurangan |
|-----------|------------|
| End-to-end learning | Memerlukan anotasi titik |
| Dapat menangani scene padat | Training lebih sulit |

---

## 5.3 Teknik Counting Dengan Depth

### 5.3.1 Teknik 5: 3D Reconstruction

**Konsep:** Membangun point cloud 3D dari data RGB-D, kemudian melakukan clustering untuk counting.

```
RGB-D (4 sisi) --> Point Cloud per View --> Fusion --> 3D Clustering --> Count

    View 1          View 2          View 3          View 4
       |               |               |               |
       v               v               v               v
   +-------+       +-------+       +-------+       +-------+
   | * * * |       | * * * |       | * * * |       | * * * |
   |  * *  |       |  * *  |       |  * *  |       |  * *  |
   +-------+       +-------+       +-------+       +-------+
       |               |               |               |
       +---------------+---------------+---------------+
                               |
                               v
                    +-------------------+
                    |     Fused PCD     |
                    |    * * * * * *    |
                    |     * * * *       |
                    +-------------------+
                               |
                               v
                    +-------------------+
                    |     Clustering    |
                    |     (DBSCAN)      |
                    +-------------------+
                               |
                               v
                        Count = 12
```

**Implementasi:**
```python
import open3d as o3d
import numpy as np

def create_point_cloud(rgb, depth, intrinsics):
    """Membuat point cloud dari RGB-D."""
    rgbd = o3d.geometry.RGBDImage.create_from_color_and_depth(
        o3d.geometry.Image(rgb),
        o3d.geometry.Image(depth),
        depth_scale=1000.0,
        convert_rgb_to_intensity=False
    )
    pcd = o3d.geometry.PointCloud.create_from_rgbd_image(rgbd, intrinsics)
    return pcd

def fuse_point_clouds(pcds, transformations):
    """Menggabungkan point cloud dari multiple views."""
    combined = o3d.geometry.PointCloud()
    for pcd, transform in zip(pcds, transformations):
        pcd_transformed = pcd.transform(transform)
        combined += pcd_transformed
    combined = combined.voxel_down_sample(voxel_size=0.02)
    return combined

def count_clusters(pcd, eps=0.1, min_points=100):
    """Menghitung jumlah cluster."""
    labels = np.array(pcd.cluster_dbscan(eps=eps, min_points=min_points))
    n_clusters = labels.max() + 1
    return n_clusters
```

| Kelebihan | Kekurangan |
|-----------|------------|
| Eliminasi double counting secara alami | Komputasi berat |
| Akurasi tinggi | Memerlukan kalibrasi kamera |

### 5.3.2 Teknik 6: Depth-assisted NMS

**Konsep:** Menggunakan depth untuk estimasi posisi 3D, kemudian melakukan NMS berdasarkan jarak 3D.

```
RGB Image --> YOLO --> (x, y) pixel
                            |
                            +------+
                                   |
Depth Map ---------------------->  z depth
                                   |
                                   v
                            (X, Y, Z) world
                                   |
                                   v
                              3D NMS
                                   |
                                   v
                            Unique Count
```

**Transformasi Koordinat:**
```
         Image Plane                    3D World
         
         +--------+                      Z (depth)
         |        |                       ^
         |  u,v   |     Z = depth(u,v)    |
         |   *    |     X = (u-cx)*Z/fx   +---> X
         |        |     Y = (v-cy)*Z/fy  /
         +--------+                     Y
```

**Implementasi:**
```python
def pixel_to_3d(u, v, depth, intrinsics):
    """Konversi koordinat pixel ke 3D."""
    fx, fy = intrinsics['fx'], intrinsics['fy']
    cx, cy = intrinsics['cx'], intrinsics['cy']
    
    Z = depth
    X = (u - cx) * Z / fx
    Y = (v - cy) * Z / fy
    
    return X, Y, Z

def depth_based_nms(detections_2d, depth_map, intrinsics, distance_threshold=0.3):
    """NMS berdasarkan jarak 3D."""
    detections_3d = []
    
    for det in detections_2d:
        cx = (det['bbox'][0] + det['bbox'][2]) / 2
        cy = (det['bbox'][1] + det['bbox'][3]) / 2
        u, v = int(cx), int(cy)
        
        depth = depth_map[v, u]
        if depth > 0:
            X, Y, Z = pixel_to_3d(u, v, depth, intrinsics)
            detections_3d.append({
                'pos_3d': (X, Y, Z),
                'confidence': det['confidence'],
                'class': det['class']
            })
    
    # Greedy NMS berdasarkan jarak 3D
    unique = []
    for det in sorted(detections_3d, key=lambda x: -x['confidence']):
        is_duplicate = False
        for u in unique:
            dist = np.linalg.norm(
                np.array(det['pos_3d']) - np.array(u['pos_3d'])
            )
            if dist < distance_threshold:
                is_duplicate = True
                break
        if not is_duplicate:
            unique.append(det)
    
    return unique
```

| Kelebihan | Kekurangan |
|-----------|------------|
| Lebih akurat dari 2D NMS | Memerlukan depth per frame |
| Tidak perlu full 3D reconstruction | Memerlukan kalibrasi |

### 5.3.3 Teknik 7: Multi-View Stereo (Tanpa Depth Sensor)

**Konsep:** Mengestimasi depth dari multiple RGB images menggunakan stereo matching.

```
RGB View 1 ----+
               |
RGB View 2 ----+--> Feature Matching --> Triangulation --> Pseudo Depth
               |
RGB View 3 ----+
               |
RGB View 4 ----+
```

**Tools yang dapat digunakan:**
- OpenCV StereoSGBM
- COLMAP
- MVSNet

| Kelebihan | Kekurangan |
|-----------|------------|
| Tidak memerlukan depth sensor | Akurasi lebih rendah |
| Menggunakan kamera biasa | Komputasi berat |

---

## 5.4 Perbandingan Teknik

| ID | Teknik | Depth Required | Akurasi | Kompleksitas |
|----|--------|----------------|---------|--------------|
| T1 | Simple Aggregation | Tidak | Rendah | Rendah |
| T2 | Multi-View NMS | Tidak | Sedang | Sedang |
| T3 | Video Tracking | Tidak | Tinggi | Sedang |
| T4 | Density Estimation | Tidak | Sedang | Tinggi |
| T5 | 3D Reconstruction | Ya | Tinggi | Tinggi |
| T6 | Depth-assisted NMS | Ya | Tinggi | Sedang |
| T7 | Multi-View Stereo | Tidak* | Sedang | Tinggi |

---

## 5.5 Desain Eksperimen

| ID | Data | Teknik | Metrik |
|----|------|--------|--------|
| E1 | 4 sisi RGB | Simple Aggregation | MAE, RMSE |
| E2 | 4 sisi RGB | Multi-View NMS | MAE, RMSE |
| E3 | 8 sisi RGB | Multi-View NMS | MAE, RMSE |
| E4 | Video 360 | Tracking | MAE, RMSE |
| E5 | 4 sisi RGB-D | Depth-assisted NMS | MAE, RMSE |
| E6 | 4 sisi RGB-D | 3D Reconstruction | MAE, RMSE |

### Ground Truth

- Hitungan manual oleh pakar pada 50 pohon sampel
- Dokumentasi foto dari setiap sisi untuk verifikasi

### Metrik Evaluasi

| Metrik | Formula |
|--------|---------|
| MAE (Mean Absolute Error) | mean(abs(predicted - actual)) |
| RMSE (Root Mean Square Error) | sqrt(mean((predicted - actual)^2)) |
| Relative Error | abs(predicted - actual) / actual |

---

## 5.6 Rekomendasi

### Tanpa Depth Sensor

| Prioritas | Teknik | Alasan |
|-----------|--------|--------|
| 1 | Multi-View NMS | Balance antara akurasi dan kompleksitas |
| 2 | Video Tracking | Akurasi tinggi jika data video tersedia |
| 3 | Simple Aggregation | Baseline untuk perbandingan |

### Dengan Depth Sensor

| Prioritas | Teknik | Alasan |
|-----------|--------|--------|
| 1 | Depth-assisted NMS | Akurasi tinggi dengan kompleksitas terkontrol |
| 2 | 3D Reconstruction | Akurasi tertinggi untuk kasus kompleks |

---

## 5.7 Ringkasan

### Permasalahan

Penghitungan tandan dari data multi-view menghadapi tantangan double counting dan oklusi.

### Solusi Tersedia

1. **Tanpa Depth:** Multi-View NMS dan Video Tracking memberikan hasil yang memadai
2. **Dengan Depth:** Depth-assisted NMS memberikan peningkatan akurasi signifikan

### Langkah Selanjutnya

- Implementasi Multi-View NMS sebagai baseline
- Evaluasi pada 50 pohon sampel dengan ground truth
- Perbandingan dengan teknik lain jika hasil baseline tidak memadai
