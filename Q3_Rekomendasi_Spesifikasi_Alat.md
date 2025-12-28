# 3. REKOMENDASI SPESIFIKASI ALAT

## 3.1 Ikhtisar

Dokumen ini menyediakan rekomendasi spesifikasi hardware untuk pengumpulan data depth dan tablet sebagai viewfinder. Spesifikasi disusun berdasarkan kebutuhan teknis proyek dan ketersediaan di pasar Indonesia.

---

## 3.2 Kamera Depth

### 3.2.1 Spesifikasi Minimal

| Parameter | Spesifikasi | Alasan |
|-----------|-------------|--------|
| IMU | Tersedia | Deteksi orientasi kamera |
| Jangkauan | 3-6 meter | Jarak pengambilan dari pohon |
| Koneksi | USB 3.0+ | Bandwidth transfer data depth |
| Outdoor | Didukung | Penggunaan di perkebunan |

### 3.2.2 Rekomendasi: Intel RealSense D455

| Parameter | Spesifikasi |
|-----------|-------------|
| Part Number | 82635DSD455 |
| Teknologi | Active Stereo |
| Jangkauan optimal | 0.6m - 6m |
| Jangkauan maksimum | 20m |
| Akurasi | < 2% error pada 4 meter |
| Resolusi depth | 1280 x 720 @ 90 fps |
| FOV depth | 86 x 57 derajat |
| Resolusi RGB | 1280 x 800 @ 30 fps |
| Shutter RGB | Global shutter |
| Baseline stereo | 95mm |
| IMU | Bosch BMI055 (6DoF) |
| Koneksi | USB 3.1 Gen 1 Type-C |
| Dimensi | 124 x 26 x 29 mm |
| Harga (Indonesia) | Rp 10.000.000 - 16.000.000 |

### 3.2.3 Alternatif: Intel RealSense D435i

| Parameter | Spesifikasi |
|-----------|-------------|
| Part Number | 82635D435I |
| Jangkauan optimal | 0.3m - 3m |
| Jangkauan maksimum | 10m |
| Resolusi depth | 1280 x 720 @ 90 fps |
| Shutter RGB | Rolling shutter |
| Baseline stereo | 50mm |
| IMU | Bosch BMI055 (6DoF) |
| Dimensi | 90 x 25 x 25 mm |
| Harga (Indonesia) | Rp 7.000.000 - 12.000.000 |

Catatan: Pastikan model memiliki suffix "i" (D435**i**) yang menandakan ketersediaan IMU.

### 3.2.4 Perbandingan

| Parameter | D455 | D435i |
|-----------|------|-------|
| Jangkauan optimal | 6m | 3m |
| Baseline | 95mm | 50mm |
| Akurasi jarak jauh | Lebih baik | Kurang |
| RGB shutter | Global | Rolling |
| Ukuran | Lebih besar | Lebih compact |
| Harga | Lebih tinggi | Lebih rendah |

D455 lebih sesuai untuk jarak pengambilan 2-3 meter dari pohon karena jangkauan optimal yang lebih lebar.

---

## 3.3 Tablet / Perangkat Android

### 3.3.1 Spesifikasi Minimal

| Parameter | Spesifikasi | Alasan |
|-----------|-------------|--------|
| USB | 3.0+ (3.2 Gen 1) | Bandwidth transfer depth |
| USB OTG | Didukung | Koneksi kamera eksternal |
| Prosesor | Qualcomm Snapdragon | Kompatibilitas RealSense SDK |
| Baterai | 7000mAh+ | Supply daya ke kamera |
| OS | Android 10+ | Dukungan SDK |

### 3.3.2 Tabel Kompatibilitas USB

| Brand | Model | Versi USB | Kompatibel |
|-------|-------|-----------|------------|
| Xiaomi | Pad 6 | USB 3.2 Gen 1 | Ya |
| Xiaomi | Pad 5 Pro 12.4 | USB 3.2 Gen 1 | Ya |
| Xiaomi | Pad 5 | USB 2.0 | Tidak |
| Poco | Poco Pad | USB 2.0 | Tidak |
| Redmi | Pad Pro | USB 2.0 | Tidak |
| Samsung | Tab S8 / S8+ / S8 Ultra | USB 3.2 Gen 1 | Ya |
| Samsung | Tab S9 / S9+ / S9 Ultra | USB 3.2 Gen 1 | Ya |
| Samsung | Tab S6 Lite (2024) | USB 2.0 | Tidak |
| Samsung | Tab S9 FE / FE+ | USB 2.0 | Tidak |
| Samsung | Tab A series | USB 2.0 | Tidak |
| Lenovo | Tab P12 Pro | USB 3.2 Gen 2 | Ya |

Catatan: Port USB Type-C tidak menjamin USB 3.0. Verifikasi spesifikasi sebelum pembelian.

### 3.3.3 Rekomendasi: Xiaomi Pad 6

| Parameter | Spesifikasi |
|-----------|-------------|
| Layar | 11 inci IPS LCD, 2880 x 1800, 144Hz |
| Prosesor | Qualcomm Snapdragon 870 |
| RAM | 8GB |
| Storage | 256GB |
| USB | Type-C 3.2 Gen 1 |
| USB OTG | Didukung |
| DisplayPort | Didukung |
| Baterai | 8840 mAh |
| OS | Android 13 |
| Harga (Indonesia) | Rp 4.500.000 - 5.500.000 |

### 3.3.4 Alternatif: Samsung Galaxy Tab S8

| Parameter | Spesifikasi |
|-----------|-------------|
| Layar | 11 inci LTPS TFT, 2560 x 1600, 120Hz |
| Prosesor | Qualcomm Snapdragon 8 Gen 1 |
| RAM | 8GB |
| Storage | 128GB / 256GB |
| USB | Type-C 3.2 Gen 1 |
| Baterai | 8000 mAh |
| OS | Android 12+ |
| Harga (Indonesia) | Rp 7.000.000 - 9.000.000 |

---

## 3.4 Smartphone untuk Batch 1 (RGB Only)

### 3.4.1 Spesifikasi Minimal

| Parameter | Spesifikasi |
|-----------|-------------|
| Kamera | 12 MP minimum |
| Rasio | Support 4:3 |
| Orientasi | Portrait |
| Fitur AI/Beauty | Dapat dinonaktifkan |

Smartphone apa pun yang memenuhi kriteria di atas dapat digunakan, termasuk Samsung A55, Xiaomi Redmi series, dan model serupa.

---

## 3.5 Aksesoris

| Item | Spesifikasi | Estimasi Harga |
|------|-------------|----------------|
| Kabel USB-C to USB-C | USB 3.1, 1-2 meter | Rp 100.000 - 300.000 |
| Tripod | Ball head, tinggi 100-180cm | Rp 300.000 - 800.000 |
| Tablet holder | Clamp mount untuk tripod | Rp 50.000 - 150.000 |
| Camera mount | 1/4 inch screw untuk RealSense | Rp 30.000 - 100.000 |
| Power bank | 20000mAh+, USB-C PD output | Rp 300.000 - 600.000 |

Catatan: Gunakan kabel USB 3.1 yang support bandwidth 10Gbps, bukan kabel charging standar.

---

## 3.6 Estimasi Budget

### Opsi A: Konfigurasi Utama

| Item | Estimasi Harga |
|------|----------------|
| Intel RealSense D455 | Rp 12.000.000 |
| Xiaomi Pad 6 (8/256GB) | Rp 5.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + mount | Rp 500.000 |
| Power bank | Rp 400.000 |
| **Total** | **Rp 18.100.000** |

### Opsi B: Konfigurasi Budget

| Item | Estimasi Harga |
|------|----------------|
| Intel RealSense D435i | Rp 9.000.000 |
| Xiaomi Pad 6 (8/256GB) | Rp 5.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + mount | Rp 400.000 |
| Power bank | Rp 300.000 |
| **Total** | **Rp 14.900.000** |

---

## 3.7 Catatan Teknis

1. Tablet dengan port USB Type-C tidak selalu menggunakan USB 3.0. Verifikasi spesifikasi perangkat sebelum pembelian.

2. RealSense D455 dan D435i memerlukan USB 3.0 untuk operasi penuh. USB 2.0 tidak menyediakan bandwidth yang cukup.

3. iPad tidak didukung oleh RealSense SDK.

4. Harga kamera depth di Indonesia lebih tinggi dari harga retail internasional karena biaya impor.

5. Untuk pengujian kompatibilitas, gunakan aplikasi RealSense Viewer setelah perangkat diterima.

---

## 3.8 Sumber Pembelian

| Item | Platform |
|------|----------|
| Intel RealSense | Tokopedia, Shopee (cari: "RealSense D455" atau "RealSense D435i") |
| Xiaomi Pad 6 | Mi Store, Tokopedia, Shopee, Lazada, Blibli |
| Samsung Tab S8 | Samsung Store, Tokopedia, Shopee |
| Aksesoris | Tokopedia, Shopee |
