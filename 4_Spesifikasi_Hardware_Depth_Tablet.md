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
| Kompatibel Android | Digunakan dengan tablet sebagai viewfinder |

---

### 1.2 Rekomendasi Utama: Intel RealSense Depth Camera D455

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Intel RealSense Depth Camera D455 |
| **Part Number** | 82635DSD455 |
| **Harga** | $350 - $450 USD (~Rp 5.5-7 juta) |

#### Spesifikasi Teknis Lengkap

| Parameter | Nilai |
|-----------|-------|
| **Teknologi Depth** | Active Stereo |
| **Jangkauan Operasional** | 0.6m - 6m (optimal), hingga 20m (maksimum) |
| **Akurasi Depth** | < 2% error pada jarak 4 meter |
| **Resolusi Depth** | 1280 x 720 @ 90 fps |
| **FOV Depth** | 86° x 57° (±3°) |
| **Resolusi RGB** | 1280 x 800 @ 30 fps |
| **FOV RGB** | 90° x 65° (matched dengan depth FOV) |
| **Shutter RGB** | Global Shutter |
| **Baseline Stereo** | 95mm (lebih akurat untuk jarak jauh) |
| **IMU** | Bosch BMI055 (6DoF - Accelerometer + Gyroscope) |
| **Koneksi** | USB 3.1 Gen 1 Type-C |
| **Dimensi** | 124mm x 26mm x 29mm |
| **Berat** | ~72 gram |
| **Environment** | Indoor & Outdoor |
| **Power** | Via USB (~2 Watt) |

#### Keunggulan D455 untuk Proyek Ini

1. **IMU Built-in** - Dapat mendeteksi apakah kamera mendongak/menunduk
2. **Jangkauan 6m** - Cukup untuk jarak 2-3m dari pohon
3. **Global Shutter RGB** - Tidak blur saat gerakan
4. **FOV Lebar (86°)** - Cakupan area besar dalam 1 frame
5. **Outdoor Ready** - Bisa digunakan di bawah sinar matahari

#### Link Pembelian

| Toko | URL | Harga (Est.) |
|------|-----|--------------|
| B&H Photo | https://www.bhphotovideo.com/c/product/1580370-REG/intel_82635dsd455_realsense_depth_camera_d455.html | $364 |
| SparkFun | https://www.sparkfun.com/products/17750 | $349 |
| Mouser | https://www.mouser.com/ProductDetail/Intel/82635DSD455 | $411 |
| eBay | https://www.ebay.com/sch/i.html?_nkw=intel+realsense+d455 | $295-$450 |

---

### 1.3 Alternatif: Intel RealSense Depth Camera D435i

| Spesifikasi | Detail |
|-------------|--------|
| **Nama Produk** | Intel RealSense Depth Camera D435i |
| **Part Number** | 82635D435I |
| **Harga** | $250 - $350 USD (~Rp 4-5.5 juta) |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| **Teknologi Depth** | Active Stereo |
| **Jangkauan Operasional** | 0.3m - 3m (optimal), hingga 10m (maksimum) |
| **Resolusi Depth** | 1280 x 720 @ 90 fps |
| **FOV Depth** | 87° x 58° |
| **Resolusi RGB** | 1920 x 1080 @ 30 fps |
| **Shutter RGB** | Rolling Shutter |
| **Baseline Stereo** | 50mm |
| **IMU** | Bosch BMI055 (6DoF) |
| **Koneksi** | USB 3.1 Gen 1 Type-C |
| **Dimensi** | 90mm x 25mm x 25mm |
| **Min-Z (jarak minimum)** | ~10.5 cm |

#### Catatan

- Huruf "i" pada D435**i** menandakan ada IMU (tanpa "i" = tanpa IMU)
- Jangkauan lebih pendek (3m optimal vs 6m pada D455)
- RGB menggunakan Rolling Shutter (bisa blur saat gerakan cepat)
- Lebih compact dan lebih murah

---

### 1.4 Perbandingan D455 vs D435i

| Fitur | D455 | D435i | Rekomendasi |
|-------|------|-------|-------------|
| Jangkauan Optimal | 0.6m - 6m | 0.3m - 3m | D455 ✓ |
| Baseline | 95mm | 50mm | D455 ✓ |
| Akurasi (4m) | < 2% error | > 2% error | D455 ✓ |
| RGB Shutter | Global | Rolling | D455 ✓ |
| IMU | Ada | Ada | Sama |
| Ukuran | Lebih besar | Lebih compact | D435i ✓ |
| Harga | $350-450 | $250-350 | D435i ✓ |
| **Proyek Ini** | **Direkomendasikan** | Alternatif | - |

**Kesimpulan:** D455 lebih cocok untuk proyek ini karena jarak pengambilan 2-3m masih dalam range optimal dan akurasi lebih baik.

---

## 2. Tablet / Android Device

### 2.1 Syarat Kompatibilitas dengan RealSense

| Syarat | Alasan |
|--------|--------|
| **USB 3.0+ (Type-C)** | Transfer data depth butuh bandwidth tinggi. USB 2.0 TIDAK CUKUP |
| **USB OTG Support** | Untuk koneksi ke kamera external |
| **Qualcomm Snapdragon** | Direkomendasikan untuk kompatibilitas SDK |
| **Android 10+** | RealSense SDK support |
| **Baterai > 7000mAh** | Menyuplai daya ke kamera (~2W) |
| **Power Output USB** | Minimal 1A output via USB-C |

### 2.2 PENTING: Versi USB pada Tablet Samsung

| Model | Versi USB | Kompatibel RealSense |
|-------|-----------|---------------------|
| Galaxy Tab S8 / S8+ / S8 Ultra | USB 3.2 | ✓ Ya |
| Galaxy Tab S9 / S9+ / S9 Ultra | USB 3.2 | ✓ Ya |
| Galaxy Tab Active 4 Pro | USB 3.2 | ✓ Ya |
| Galaxy Tab S6 Lite (2024) | USB 2.0 | ✗ Tidak |
| Galaxy Tab S9 FE | USB 2.0 | ✗ Tidak |
| Galaxy Tab A Series | USB 2.0 | ✗ Tidak |
| Galaxy A55 (Smartphone) | USB 2.0 | ✗ Tidak |

---

### 2.3 Rekomendasi Utama: Samsung Galaxy Tab Active 4 Pro

| Spesifikasi | Detail |
|-------------|--------|
| **Model** | Samsung Galaxy Tab Active 4 Pro |
| **Part Number** | SM-T636B |
| **Harga** | Rp 10-13 juta |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| **Layar** | 10.1" TFT LCD, 1920 x 1200 |
| **Prosesor** | Qualcomm Snapdragon 778G 5G |
| **RAM** | 4GB / 6GB |
| **Storage** | 64GB / 128GB |
| **USB** | USB Type-C 3.2 Gen 1 |
| **Baterai** | 7600 mAh (Removable) |
| **OS** | Android 12 (upgradable) |
| **Sertifikasi** | IP68, MIL-STD-810H |
| **Fitur Khusus** | No Battery Mode (bisa operasi tanpa baterai via USB power) |

#### Keunggulan untuk Proyek Ini

1. **USB 3.2** - Kompatibel dengan RealSense D455/D435i
2. **Rugged** - Tahan banting, debu, air (IP68) - cocok untuk lapangan
3. **Snapdragon** - Direkomendasikan untuk RealSense SDK
4. **No Battery Mode** - Bisa beroperasi terus dengan power external
5. **Removable Battery** - Bisa swap battery di lapangan

#### Link Pembelian

| Toko | URL |
|------|-----|
| Samsung Indonesia | https://www.samsung.com/id/tablets/galaxy-tab-active/galaxy-tab-active4-pro/ |
| Tokopedia | Search: "Samsung Tab Active 4 Pro" |
| Shopee | Search: "SM-T636B" |

---

### 2.4 Alternatif Budget: Samsung Galaxy Tab S8

| Spesifikasi | Detail |
|-------------|--------|
| **Model** | Samsung Galaxy Tab S8 |
| **Part Number** | SM-X700 (WiFi) / SM-X706 (5G) |
| **Harga** | Rp 8-10 juta (second hand / promo) |

#### Spesifikasi Teknis

| Parameter | Nilai |
|-----------|-------|
| **Layar** | 11" LTPS TFT, 2560 x 1600 |
| **Prosesor** | Qualcomm Snapdragon 8 Gen 1 |
| **RAM** | 8GB |
| **Storage** | 128GB / 256GB |
| **USB** | USB Type-C 3.2 Gen 1 |
| **Baterai** | 8000 mAh |
| **OS** | Android 12 (upgradable to 14+) |

#### Catatan ⚠️

- Tidak rugged (perlu case pelindung untuk lapangan)
- Processor lebih powerful
- Layar lebih besar dan resolusi tinggi

---

### 2.5 Device yang TIDAK DISARANKAN

| Device | Alasan | USB Version |
|--------|--------|-------------|
| Samsung Galaxy Tab S6 Lite (2024) | USB 2.0 - bandwidth tidak cukup | USB 2.0 |
| Samsung Galaxy Tab S9 FE | USB 2.0 | USB 2.0 |
| Samsung Galaxy Tab A9 / A9+ | USB 2.0 | USB 2.0 |
| Samsung Galaxy A55 (HP) | USB 2.0, bukan tablet | USB 2.0 |
| iPad (semua model) | Tidak support RealSense SDK | - |

---

## 3. Aksesoris Tambahan

### 3.1 Kabel USB-C

| Item | Spesifikasi | Harga Est. |
|------|-------------|------------|
| Kabel USB-C to USB-C | USB 3.1, min 10Gbps, panjang 1-2m | Rp 100-300rb |

**Catatan:** Gunakan kabel yang support USB 3.1 dengan bandwidth 10Gbps. Kabel charging biasa (USB 2.0) tidak akan bekerja.

### 3.2 Tripod

| Item | Spesifikasi | Harga Est. |
|------|-------------|------------|
| Tripod kamera | Ball head, tinggi adjustable 100-180cm | Rp 300-800rb |
| Tablet holder | Clamp mount untuk tripod | Rp 50-150rb |
| Camera mount | 1/4" screw untuk RealSense | Rp 30-100rb |

### 3.3 Power

| Item | Spesifikasi | Harga Est. |
|------|-------------|------------|
| Power bank | 20000mAh+, USB-C PD output | Rp 300-600rb |

---

## 4. Estimasi Total Budget

### Opsi A: Premium (Rekomendasi)

| Item | Harga (Est.) |
|------|--------------|
| Intel RealSense D455 | Rp 6.000.000 |
| Samsung Tab Active 4 Pro | Rp 11.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + holder | Rp 500.000 |
| Power bank | Rp 400.000 |
| **TOTAL** | **Rp 18.100.000** |

### Opsi B: Budget

| Item | Harga (Est.) |
|------|--------------|
| Intel RealSense D435i | Rp 4.500.000 |
| Samsung Tab S8 (Second) | Rp 8.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + holder | Rp 400.000 |
| Power bank | Rp 300.000 |
| **TOTAL** | **Rp 13.400.000** |

---

## 5. Checklist Sebelum Pembelian

- [ ] Verifikasi USB version tablet (harus 3.0+)
- [ ] Verifikasi tablet support USB OTG
- [ ] Pilih kamera depth dengan IMU (D455 atau D435**i**)
- [ ] Siapkan kabel USB-C to USB-C (bukan charging cable biasa)
- [ ] Test kompatibilitas dengan app RealSense Viewer setelah terima barang
