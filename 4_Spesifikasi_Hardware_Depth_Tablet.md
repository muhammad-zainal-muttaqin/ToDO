# BAGIAN 4: SPESIFIKASI HARDWARE (DEPTH & TABLET)

**Status:** Untuk Batch Selanjutnya & Eksperimen  
**Catatan:** Batch 1 menggunakan kamera smartphone biasa (RGB). Hardware depth camera diperlukan untuk batch selanjutnya.

---

## 1. Kamera Depth

### 1.1 Spesifikasi Minimal untuk Proyek Ini

| Syarat | Alasan |
|--------|--------|
| IMU (Inertial Measurement Unit) | Mendeteksi orientasi kamera (dongak/nunduk, miring) |
| Tahan cahaya matahari (outdoor) | Pengambilan data di perkebunan siang hari |
| Jangkauan 3-6 meter | Jarak pengambilan foto dari batang pohon |
| Koneksi USB 3.0+ | Transfer data depth membutuhkan bandwidth tinggi |

---

### 1.2 Rekomendasi Utama: Intel RealSense Depth Camera D455

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Intel RealSense Depth Camera D455 |
| **Part Number** | 82635DSD455 |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| Teknologi Depth | Active Stereo |
| Jangkauan Operasional | 0.6m - 6m (optimal), hingga 20m (maksimum) |
| Akurasi Depth | < 2% error pada jarak 4 meter |
| Resolusi Depth | 1280 x 720 @ 90 fps |
| FOV Depth | 86° x 57° |
| Resolusi RGB | 1280 x 800 @ 30 fps |
| Shutter RGB | Global Shutter |
| Baseline Stereo | 95mm |
| IMU | Bosch BMI055 (6DoF) |
| Koneksi | USB 3.1 Gen 1 Type-C |
| Dimensi | 124mm x 26mm x 29mm |

#### Keunggulan untuk Proyek Ini

- IMU built-in untuk deteksi orientasi kamera
- Jangkauan 6m cukup untuk jarak 2-3m dari pohon
- Global shutter RGB tidak blur saat gerakan
- FOV lebar 86° memberikan cakupan area besar

#### Harga & Pembelian di Indonesia

| Platform | Kisaran Harga | Catatan |
|----------|---------------|---------|
| Tokopedia | Rp 10-16 juta | Cari: "Intel RealSense D455" |
| Shopee | Rp 10-16 juta | Cari: "RealSense D455" |
| AliExpress | $350-450 USD | Import, perlu waktu pengiriman |

> **Catatan:** Harga di Indonesia lebih mahal dari harga retail luar negeri ($350-400) karena ongkos import dan ketersediaan terbatas.

---

### 1.3 Alternatif: Intel RealSense Depth Camera D435i

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Intel RealSense Depth Camera D435i |
| **Part Number** | 82635D435I |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| Teknologi Depth | Active Stereo |
| Jangkauan Operasional | 0.3m - 3m (optimal), hingga 10m (maksimum) |
| Resolusi Depth | 1280 x 720 @ 90 fps |
| FOV Depth | 87° x 58° |
| Resolusi RGB | 1920 x 1080 @ 30 fps |
| Shutter RGB | Rolling Shutter |
| Baseline Stereo | 50mm |
| IMU | Bosch BMI055 (6DoF) |
| Koneksi | USB 3.1 Gen 1 Type-C |
| Dimensi | 90mm x 25mm x 25mm |

#### Catatan

- Huruf "i" pada D435**i** menandakan ada IMU
- Jangkauan lebih pendek (3m optimal)
- Lebih compact dan lebih murah
- Harga di Indonesia: Rp 7-12 juta

---

### 1.4 Perbandingan D455 vs D435i

| Fitur | D455 | D435i |
|-------|------|-------|
| Jangkauan Optimal | 0.6m - 6m | 0.3m - 3m |
| Baseline | 95mm | 50mm |
| Akurasi (4m) | < 2% error | > 2% error |
| RGB Shutter | Global | Rolling |
| IMU | Ada | Ada |
| Ukuran | Lebih besar | Lebih compact |
| Harga (ID) | Rp 10-16 juta | Rp 7-12 juta |

**Rekomendasi:** D455 lebih cocok karena jarak pengambilan 2-3m masih dalam range optimal.

---

## 2. Tablet / Android Device

### 2.1 Syarat Kompatibilitas dengan RealSense

| Syarat | Alasan |
|--------|--------|
| **USB 3.0+ (USB 3.2 Gen 1)** | Transfer data depth butuh bandwidth tinggi. USB 2.0 tidak cukup |
| **USB OTG Support** | Untuk koneksi ke kamera external |
| **Prosesor Qualcomm Snapdragon** | Direkomendasikan untuk kompatibilitas RealSense SDK |
| **Baterai > 7000mAh** | Menyuplai daya ke kamera (~2W) |

---

### 2.2 Tabel Kompatibilitas USB Tablet Populer

| Brand | Model | Versi USB | Kompatibel |
|-------|-------|-----------|------------|
| **Xiaomi** | Pad 6 | USB 3.2 Gen 1 | ✓ Ya |
| **Xiaomi** | Pad 5 Pro 12.4 | USB 3.2 Gen 1 | ✓ Ya |
| **Xiaomi** | Pad 5 | USB 2.0 | ✗ Tidak |
| **Poco** | Poco Pad | USB 2.0 | ✗ Tidak |
| **Redmi** | Pad Pro | USB 2.0 | ✗ Tidak |
| **Samsung** | Tab S8 / S8+ / S8 Ultra | USB 3.2 Gen 1 | ✓ Ya |
| **Samsung** | Tab S9 / S9+ / S9 Ultra | USB 3.2 Gen 1 | ✓ Ya |
| **Samsung** | Tab S6 Lite (2024) | USB 2.0 | ✗ Tidak |
| **Samsung** | Tab S9 FE | USB 2.0 | ✗ Tidak |
| **Lenovo** | Tab P12 Pro | USB 3.2 Gen 2 | ✓ Ya |

---

### 2.3 Rekomendasi Utama: Xiaomi Pad 6

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Xiaomi Pad 6 |
| **Varian** | 8GB/256GB |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| Layar | 11" IPS LCD, 2880 x 1800, 144Hz |
| Prosesor | Qualcomm Snapdragon 870 |
| RAM | 8GB |
| Storage | 256GB |
| USB | USB Type-C 3.2 Gen 1 |
| USB OTG | Didukung |
| DisplayPort | Didukung |
| Baterai | 8840 mAh |
| OS | Android 13 (MIUI Pad 14) |

#### Keunggulan

- USB 3.2 Gen 1 - kompatibel dengan RealSense
- Snapdragon 870 - performa tinggi
- Baterai 8840mAh - cukup untuk menyuplai kamera
- Harga terjangkau dibanding Samsung flagship
- Mudah ditemukan di Indonesia

#### Harga di Indonesia

| Platform | Kisaran Harga |
|----------|---------------|
| Tokopedia | Rp 4.5-5.5 juta |
| Shopee | Rp 4.5-5.5 juta |
| Lazada | Rp 3.4-5.1 juta |
| Blibli | Rp 3.2-5.3 juta |
| Mi Store | Rp 4.999.000 (resmi) |

---

### 2.4 Alternatif: Samsung Galaxy Tab S8

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Samsung Galaxy Tab S8 |
| **Varian** | WiFi / 5G |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| Layar | 11" LTPS TFT, 2560 x 1600, 120Hz |
| Prosesor | Qualcomm Snapdragon 8 Gen 1 |
| RAM | 8GB |
| Storage | 128GB / 256GB |
| USB | USB Type-C 3.2 Gen 1 |
| Baterai | 8000 mAh |
| OS | Android 12 (upgradable) |

#### Harga di Indonesia

| Platform | Kisaran Harga |
|----------|---------------|
| Tokopedia | Rp 7-9 juta (baru), Rp 5-7 juta (second) |
| Shopee | Rp 7-9 juta |

---

### 2.5 Alternatif Budget: Lenovo Tab P12 Pro

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Lenovo Tab P12 Pro |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| Layar | 12.6" AMOLED, 2560 x 1600, 120Hz |
| Prosesor | Qualcomm Snapdragon 870 |
| USB | USB Type-C 3.2 Gen 2 (10Gbps) |
| Baterai | 10200 mAh |

#### Catatan

- USB 3.2 Gen 2 lebih cepat dari Gen 1
- Baterai sangat besar
- Perlu cek ketersediaan di Indonesia

---

### 2.6 Tablet yang TIDAK COCOK (USB 2.0)

| Tablet | Alasan |
|--------|--------|
| Xiaomi Pad 5 | USB 2.0 |
| Poco Pad | USB 2.0 |
| Redmi Pad Pro | USB 2.0 |
| Samsung Tab S6 Lite (2024) | USB 2.0 |
| Samsung Tab S9 FE / FE+ | USB 2.0 |
| Samsung Tab A Series | USB 2.0 |

> Meskipun tablet-tablet ini memiliki port USB Type-C, versi USB-nya adalah 2.0 yang bandwidth-nya tidak cukup untuk transfer data depth camera.

---

## 3. Aksesoris Tambahan

| Item | Spesifikasi | Kisaran Harga |
|------|-------------|---------------|
| Kabel USB-C to USB-C | USB 3.1, panjang 1-2m | Rp 100-300rb |
| Tripod | Ball head, tinggi 100-180cm | Rp 300-800rb |
| Tablet holder | Clamp mount untuk tripod | Rp 50-150rb |
| Power bank | 20000mAh+, USB-C PD | Rp 300-600rb |

**Catatan Kabel:** Pastikan kabel mendukung USB 3.1 dengan bandwidth 10Gbps. Kabel charging biasa (USB 2.0) tidak akan bekerja.

---

## 4. Estimasi Total Budget

### Opsi A: Rekomendasi (Xiaomi Pad 6 + D455)

| Item | Harga (Est.) |
|------|--------------|
| Intel RealSense D455 | Rp 12.000.000 |
| Xiaomi Pad 6 (8/256GB) | Rp 5.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + holder | Rp 500.000 |
| Power bank | Rp 400.000 |
| **TOTAL** | **Rp 18.100.000** |

### Opsi B: Budget (Xiaomi Pad 6 + D435i)

| Item | Harga (Est.) |
|------|--------------|
| Intel RealSense D435i | Rp 9.000.000 |
| Xiaomi Pad 6 (8/256GB) | Rp 5.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + holder | Rp 400.000 |
| Power bank | Rp 300.000 |
| **TOTAL** | **Rp 14.900.000** |

### Opsi C: Premium (Tab S8 + D455)

| Item | Harga (Est.) |
|------|--------------|
| Intel RealSense D455 | Rp 12.000.000 |
| Samsung Tab S8 (second) | Rp 6.500.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + holder | Rp 500.000 |
| Power bank | Rp 400.000 |
| **TOTAL** | **Rp 19.600.000** |

---

## 5. Checklist Sebelum Pembelian

- [ ] Verifikasi versi USB tablet (harus USB 3.0+ / USB 3.2 Gen 1)
- [ ] Verifikasi tablet support USB OTG
- [ ] Pilih kamera depth dengan IMU (D455 atau D435**i**)
- [ ] Beli kabel USB-C to USB-C yang support USB 3.1
- [ ] Setelah terima barang, test koneksi dengan app RealSense Viewer

---

## 6. Catatan Penting

1. **Banyak tablet budget menggunakan USB 2.0** meskipun port-nya Type-C. Selalu cek spesifikasi versi USB.
2. **Xiaomi Pad 6 adalah pilihan value terbaik** - USB 3.2, Snapdragon 870, baterai besar, harga ~Rp 5 juta.
3. **Hindari iPad** - RealSense SDK tidak support iOS.
4. **Harga RealSense di Indonesia 2-3x lebih mahal** dari harga retail luar negeri karena import.
