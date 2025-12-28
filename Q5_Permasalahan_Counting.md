# 5. PERMASALAHAN COUNTING: TEKNIK PENGHITUNGAN TANDAN

---

## A. Deskripsi Masalah

Menghitung jumlah tandan buah sawit per pohon dari gambar/video multi-sisi dengan tantangan:

| Tantangan | Deskripsi |
|-----------|-----------|
| **Oklusi** | Tandan tertutup pelepah dari sisi tertentu |
| **Double counting** | Tandan yang sama terlihat dari 2+ sisi |
| **Variasi sudut** | Tampilan tandan berbeda dari sudut berbeda |
| **Multi-view fusion** | Menggabungkan deteksi dari 4/8 sisi |

---

## B. Teknik Counting TANPA Depth

### B.1 Teknik 1: Simple Aggregation

**Konsep:** Deteksi per sisi, lalu agregasi dengan heuristik.

```
Sisi Utara  → Deteksi: 5 tandan
Sisi Timur  → Deteksi: 6 tandan
Sisi Selatan → Deteksi: 4 tandan
Sisi Barat  → Deteksi: 5 tandan
─────────────────────────────────
Total Deteksi: 20 tandan
Faktor Overlap: 0.5 (asumsi)
─────────────────────────────────
Estimasi Final: 10 tandan
```

**Kelebihan:**
- Implementasi sederhana
- Tidak butuh kalibrasi kompleks

**Kekurangan:**
- Faktor overlap harus dikalibrasi
- Tidak akurat untuk pohon dengan distribusi tandan tidak merata

---

### B.2 Teknik 2: NMS (Non-Maximum Suppression) Multi-View

**Konsep:** Proyeksikan deteksi ke koordinat 3D virtual, lalu NMS untuk menghilangkan duplikat.

```
┌──────────────────────────────────────────────────┐
│ STEP 1: Deteksi per Sisi                          │
│ - Jalankan YOLO pada setiap gambar                │
│ - Output: bounding box + confidence per sisi      │
└──────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────┐
│ STEP 2: Proyeksi ke Koordinat Silinder           │
│ - Asumsikan pohon adalah silinder                 │
│ - Konversi (x, y)_sisi → (θ, z)_silinder         │
│ - θ = sudut keliling (0°-360°)                   │
│ - z = tinggi vertikal                            │
└──────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────┐
│ STEP 3: Clustering / NMS                          │
│ - Kelompokkan deteksi yang berdekatan di (θ, z)  │
│ - Gunakan IoU threshold di ruang silinder        │
│ - Pilih deteksi dengan confidence tertinggi      │
└──────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────┐
│ STEP 4: Count Unique Objects                      │
│ - Hitung jumlah cluster                          │
│ - Output: estimasi tandan unik                   │
└──────────────────────────────────────────────────┘
```

**Implementasi:**

```python
import numpy as np
from scipy.cluster.hierarchy import fcluster, linkage

def cylindrical_projection(bbox, side_angle, image_width, image_height):
    """
    Proyeksikan bounding box ke koordinat silinder
    
    side_angle: 0 (Utara), 90 (Timur), 180 (Selatan), 270 (Barat)
    """
    x_center = (bbox[0] + bbox[2]) / 2 / image_width  # normalized 0-1
    y_center = (bbox[1] + bbox[3]) / 2 / image_height
    
    # Konversi x ke sudut (asumsi FOV ~60 derajat)
    fov = 60
    theta = side_angle + (x_center - 0.5) * fov
    theta = theta % 360
    
    z = y_center  # tinggi relatif
    
    return theta, z

def multi_view_nms(detections_all_sides, iou_threshold=0.3):
    """
    NMS di ruang silinder
    
    detections_all_sides: list of (theta, z, confidence, class)
    """
    if len(detections_all_sides) == 0:
        return []
    
    # Hierarchical clustering
    coords = np.array([[d[0], d[1]*360] for d in detections_all_sides])  # scale z
    Z = linkage(coords, method='average')
    clusters = fcluster(Z, t=30, criterion='distance')  # threshold 30 degrees
    
    # Ambil deteksi dengan confidence tertinggi per cluster
    unique_detections = []
    for cluster_id in np.unique(clusters):
        cluster_dets = [d for d, c in zip(detections_all_sides, clusters) if c == cluster_id]
        best = max(cluster_dets, key=lambda x: x[2])  # highest confidence
        unique_detections.append(best)
    
    return unique_detections
```

**Kelebihan:**
- Lebih akurat dari simple aggregation
- Menangani overlap secara eksplisit

**Kekurangan:**
- Butuh kalibrasi FOV dan threshold
- Asumsi silinder mungkin tidak sempurna

---

### B.3 Teknik 3: Tracking-based Counting (untuk Video 360°)

**Konsep:** Gunakan object tracking pada video keliling pohon.

```
Frame 1 → Frame 2 → Frame 3 → ... → Frame N
   ↓         ↓         ↓              ↓
 [Det]    [Track]   [Track]       [Track]
   ↓         ↓         ↓              ↓
 ID:1,2   ID:1,2,3  ID:1,2,3,4    ID:...
                                      ↓
                              Total Unique IDs
```

**Implementasi dengan ByteTrack/SORT:**

```python
from ultralytics import YOLO

# Load model
model = YOLO('best.pt')

# Video tracking
results = model.track(
    source='video_360.mp4',
    tracker='bytetrack.yaml',
    persist=True
)

# Count unique IDs
unique_ids = set()
for frame_result in results:
    if frame_result.boxes.id is not None:
        unique_ids.update(frame_result.boxes.id.tolist())

print(f"Total tandan unik: {len(unique_ids)}")
```

**Kelebihan:**
- Sangat akurat jika tracking stabil
- Natural untuk data video

**Kekurangan:**
- Butuh video (bukan foto)
- Tracking bisa fail jika oklusi besar

---

### B.4 Teknik 4: Learned Counting (Density Estimation)

**Konsep:** Model langsung memprediksi density map, lalu integrasi untuk count.

```
RGB Image → CNN → Density Map → Integral → Count
```

**Network Architecture:**

```python
import torch
import torch.nn as nn

class DensityCounter(nn.Module):
    def __init__(self, backbone='resnet50'):
        super().__init__()
        self.backbone = torch.hub.load('pytorch/vision', backbone, pretrained=True)
        self.backbone.fc = nn.Identity()
        
        self.decoder = nn.Sequential(
            nn.ConvTranspose2d(2048, 512, 4, stride=2, padding=1),
            nn.ReLU(),
            nn.ConvTranspose2d(512, 128, 4, stride=2, padding=1),
            nn.ReLU(),
            nn.ConvTranspose2d(128, 1, 4, stride=2, padding=1),
            nn.ReLU()  # Density harus positif
        )
    
    def forward(self, x):
        features = self.backbone(x)
        density = self.decoder(features)
        count = density.sum(dim=(2,3))  # Integral
        return density, count
```

**Kelebihan:**
- End-to-end learning
- Bisa handle crowded scenes

**Kekurangan:**
- Butuh annotation titik (bukan box)
- Training lebih sulit

---

## C. Teknik Counting DENGAN Depth

### C.1 Teknik 5: 3D Reconstruction + Counting

**Konsep:** Bangun point cloud 3D dari RGB-D, lalu deteksi dan count di 3D.

```
RGB-D (4 sisi) → Point Cloud Fusion → 3D Object Detection → Count
```

**Pipeline:**

```python
import open3d as o3d
import numpy as np

def create_point_cloud(rgb, depth, intrinsics):
    """
    Buat point cloud dari RGB-D
    """
    rgbd = o3d.geometry.RGBDImage.create_from_color_and_depth(
        o3d.geometry.Image(rgb),
        o3d.geometry.Image(depth),
        depth_scale=1000.0,
        convert_rgb_to_intensity=False
    )
    
    pcd = o3d.geometry.PointCloud.create_from_rgbd_image(
        rgbd, intrinsics
    )
    
    return pcd

def fuse_point_clouds(pcds, transformations):
    """
    Gabungkan point cloud dari multiple views
    """
    combined = o3d.geometry.PointCloud()
    for pcd, transform in zip(pcds, transformations):
        pcd_transformed = pcd.transform(transform)
        combined += pcd_transformed
    
    # Downsample untuk menghilangkan duplikat
    combined = combined.voxel_down_sample(voxel_size=0.02)
    
    return combined

def cluster_and_count(pcd, eps=0.1, min_points=100):
    """
    Clustering untuk counting object
    """
    labels = np.array(pcd.cluster_dbscan(eps=eps, min_points=min_points))
    n_clusters = labels.max() + 1
    
    return n_clusters
```

**Kelebihan:**
- Mengeliminasi double counting secara alami
- Akurasi tinggi

**Kekurangan:**
- Butuh depth sensor
- Komputasi berat
- Perlu kalibrasi kamera

---

### C.2 Teknik 6: Depth-assisted NMS

**Konsep:** Gunakan depth untuk estimasi posisi 3D, lalu NMS di ruang 3D.

```
RGB → YOLO Detection → (x, y) pixel
                           ↓
Depth Map ──────────→ z (kedalaman)
                           ↓
                     (X, Y, Z) world
                           ↓
                     3D NMS
                           ↓
                     Unique Count
```

**Implementasi:**

```python
def pixel_to_3d(u, v, depth, intrinsics):
    """
    Konversi koordinat pixel ke 3D
    """
    fx, fy = intrinsics['fx'], intrinsics['fy']
    cx, cy = intrinsics['cx'], intrinsics['cy']
    
    Z = depth
    X = (u - cx) * Z / fx
    Y = (v - cy) * Z / fy
    
    return X, Y, Z

def depth_nms(detections_2d, depth_map, intrinsics, distance_threshold=0.3):
    """
    NMS menggunakan jarak 3D
    """
    detections_3d = []
    
    for det in detections_2d:
        x_center = (det['bbox'][0] + det['bbox'][2]) / 2
        y_center = (det['bbox'][1] + det['bbox'][3]) / 2
        u, v = int(x_center), int(y_center)
        
        depth = depth_map[v, u]
        if depth > 0:
            X, Y, Z = pixel_to_3d(u, v, depth, intrinsics)
            detections_3d.append({
                '3d_pos': (X, Y, Z),
                'confidence': det['confidence'],
                'class': det['class']
            })
    
    # NMS based on 3D distance
    unique = []
    for det in sorted(detections_3d, key=lambda x: -x['confidence']):
        is_duplicate = False
        for u in unique:
            dist = np.linalg.norm(np.array(det['3d_pos']) - np.array(u['3d_pos']))
            if dist < distance_threshold:
                is_duplicate = True
                break
        if not is_duplicate:
            unique.append(det)
    
    return unique
```

**Kelebihan:**
- Lebih akurat dari 2D NMS
- Tidak perlu full 3D reconstruction

**Kekurangan:**
- Butuh depth per frame
- Perlu kalibrasi

---

### C.3 Teknik 7: Multi-View Stereo (tanpa depth sensor)

**Konsep:** Estimasi depth dari multiple RGB images menggunakan stereo matching.

```
RGB (4 sisi) → Feature Matching → Triangulasi → Pseudo Depth → 3D Counting
```

**Tools:** OpenCV StereoSGBM, COLMAP, atau learned MVS seperti MVSNet

**Kelebihan:**
- Tidak butuh depth sensor
- Bisa digunakan dengan kamera biasa

**Kekurangan:**
- Akurasi lebih rendah dari sensor depth
- Komputasi berat

---

## D. Perbandingan Teknik

| Teknik | Depth Required | Akurasi | Kompleksitas | Rekomendasi |
|--------|----------------|---------|--------------|-------------|
| Simple Aggregation | Tidak | Rendah | Rendah | Baseline |
| Multi-View NMS | Tidak | Sedang | Sedang | Recommended |
| Video Tracking | Tidak | Tinggi | Sedang | Untuk video |
| Density Estimation | Tidak | Sedang | Tinggi | Opsional |
| 3D Reconstruction | Ya | Tinggi | Tinggi | Advanced |
| Depth-assisted NMS | Ya | Tinggi | Sedang | Recommended |
| Multi-View Stereo | Tidak* | Sedang | Tinggi | Alternatif |

---

## E. Rekomendasi Eksperimen

### Tanpa Depth Sensor

| Prioritas | Teknik | Alasan |
|-----------|--------|--------|
| 1 | Multi-View NMS | Balance akurasi dan kompleksitas |
| 2 | Video Tracking | Jika ada data video 360° |
| 3 | Simple Aggregation | Sebagai baseline |

### Dengan Depth Sensor

| Prioritas | Teknik | Alasan |
|-----------|--------|--------|
| 1 | Depth-assisted NMS | Relatif sederhana, akurat |
| 2 | 3D Reconstruction | Akurasi tertinggi, tapi kompleks |

---

## F. Eksperimen Komparatif yang Diusulkan

| Metode Data | Teknik Counting | Metric |
|-------------|-----------------|--------|
| 4 Sisi RGB | Simple Aggregation | MAE vs Ground Truth |
| 4 Sisi RGB | Multi-View NMS | MAE vs Ground Truth |
| 8 Sisi RGB | Multi-View NMS | MAE vs Ground Truth |
| Video 360° | Tracking | MAE vs Ground Truth |
| 4 Sisi RGB-D | Depth NMS | MAE vs Ground Truth |
| 4 Sisi RGB-D | 3D Reconstruction | MAE vs Ground Truth |

**Ground Truth:** Hitungan manual oleh pakar pada 50 pohon sampel.

---

## G. Kesimpulan

1. **Tanpa Depth:** Multi-View NMS adalah teknik paling praktis dengan akurasi memadai.

2. **Dengan Depth:** Depth-assisted NMS memberikan peningkatan signifikan dengan kompleksitas terkontrol.

3. **Video:** Tracking-based counting sangat efektif jika data video tersedia.

4. **Eksperimen penting:** Bandingkan semua teknik pada ground truth yang sama untuk menentukan metode terbaik.
