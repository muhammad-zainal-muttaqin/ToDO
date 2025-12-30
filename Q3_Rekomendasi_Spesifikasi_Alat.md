# 3. REKOMENDASI SPESIFIKASI ALAT

## 3.1 Ikhtisar

Dokumen ini menyediakan rekomendasi spesifikasi hardware untuk pengumpulan data depth, tablet sebagai viewfinder, smartphone alternatif, GPS, dan tongkat teleskopik. Spesifikasi disusun berdasarkan kebutuhan teknis proyek dan ketersediaan di pasar Indonesia.

**Estimasi Budget Total:** Rp 21.000.000 per set (termasuk powerbank dan aksesoris)

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

## 3.4 Smartphone (Opsi Non-Tablet)

Untuk penggunaan tanpa tablet, smartphone dengan spesifikasi berikut dapat digunakan:

### 3.4.1 Spesifikasi Minimal

| Parameter | Spesifikasi |
|-----------|-------------|
| Kamera | 12 MP minimum |
| USB | Type-C 3.0+ (untuk RealSense) |
| USB OTG | Didukung |
| Baterai | 5000mAh+ |
| Prosesor | Qualcomm Snapdragon (rekomendasi) |

### 3.4.2 Rekomendasi Smartphone

| Model | USB | Baterai | Harga |
|-------|-----|---------|-------|
| Samsung Galaxy S23 | USB 3.2 | 3900mAh | Rp 10-12 juta |
| Google Pixel 7 | USB 3.2 | 4355mAh | Rp 8-10 juta |
| Samsung Galaxy A54 | USB 2.0 | 5000mAh | Rp 5-6 juta |

Catatan: Samsung A54/A55 menggunakan USB 2.0, sehingga tidak kompatibel dengan RealSense. Untuk foto RGB biasa tanpa depth, smartphone apa pun dengan kamera 12MP+ dapat digunakan.

---

## 3.5 GPS

### 3.5.1 Kegunaan

| Fungsi | Deskripsi |
|--------|-----------|
| Geotagging | Mencatat lokasi setiap pohon |
| Mapping | Pemetaan kebun |
| Navigasi | Pencarian pohon yang sudah disurvei |

### 3.5.2 Opsi GPS

| Opsi | Deskripsi | Akurasi | Harga |
|------|-----------|---------|-------|
| GPS Smartphone | Fitur bawaan smartphone | 3-5 meter | Gratis |
| GPS External (Bluetooth) | Garmin GLO 2, Bad Elf | 2-3 meter | Rp 2-4 juta |
| GPS Handheld | Garmin GPSMAP 66 | 1-3 meter | Rp 5-8 juta |

Untuk kebutuhan proyek ini, GPS bawaan smartphone umumnya sudah memadai.

---

## 3.6 Tongkat Teleskopik (Pole)

### 3.6.1 Kegunaan

Untuk pengambilan gambar pohon tinggi di mana posisi eye-level tidak dapat menjangkau tajuk dan tandan.

### 3.6.2 Spesifikasi

| Parameter | Spesifikasi |
|-----------|-------------|
| Panjang | 3-5 meter (teleskopik) |
| Material | Carbon fiber atau aluminum |
| Berat | Ringan (< 1 kg) |
| Mount | Universal 1/4" atau custom |
| Fitur | Kunci setiap segmen |

### 3.6.3 Opsi Tongkat

| Opsi | Deskripsi | Estimasi Harga |
|------|-----------|----------------|
| Commercial | Telescopic Inspection Camera Pole | $200-500 USD |
| DIY | Rakit dari tongkat pancing teleskopik + mount | Rp 300.000 - 1.000.000 |
| Painting Pole | Extension pole untuk cat + adapter | Rp 200.000 - 500.000 |

**Referensi Commercial Pole:**
- [Telescopic Inspection Camera Pole (Tip Top Composites)](https://www.tiptopcomposites.com/product/telescopic-inspection-camera-pole/)

Catatan: Untuk opsi lebih murah, dapat merakit sendiri dari komponen lokal.

### 3.6.4 Pertimbangan DIY

```
STRUKTUR TONGKAT DIY

    [SMARTPHONE/TABLET]
           |
    +------+------+
    |  MOUNT/     |
    |  HOLDER     |
    +------+------+
           |
    +------+------+
    | SEGMEN 1    |  <- carbon fiber / aluminum
    | (paling atas)|
    +------+------+
           |
    +------+------+
    | SEGMEN 2    |
    |             |
    +------+------+
           |
    +------+------+
    | SEGMEN 3    |
    | (paling     |
    |  bawah)     |
    +------+------+
           |
    [GRIP/HANDLE]

Komponen yang dibutuhkan:
1. Tongkat teleskopik (fishing rod / painting pole)
2. Mount smartphone/tablet (universal clamp)
3. Adapter 1/4" (jika diperlukan)
4. Perekat/baut penguat
```

---

## 3.7 Aksesoris

| Item | Spesifikasi | Estimasi Harga |
|------|-------------|----------------|
| Kabel USB-C to USB-C | USB 3.1, 1-2 meter | Rp 100.000 - 300.000 |
| Tripod | Ball head, tinggi 100-180cm | Rp 300.000 - 800.000 |
| Tablet holder | Clamp mount untuk tripod | Rp 50.000 - 150.000 |
| Camera mount | 1/4 inch screw untuk RealSense | Rp 30.000 - 100.000 |
| Power bank | 20000mAh+, USB-C PD output | Rp 300.000 - 600.000 |
| Carrying case | Untuk RealSense + tablet | Rp 100.000 - 300.000 |

Catatan: Gunakan kabel USB 3.1 yang support bandwidth 10Gbps, bukan kabel charging standar.

---

## 3.8 Estimasi Budget

**Target budget per set:** Rp 21.000.000

### Opsi A: Konfigurasi Utama (dengan Tablet)

| Item | Estimasi Harga |
|------|----------------|
| Intel RealSense D455 | Rp 12.000.000 |
| Xiaomi Pad 6 (8/256GB) | Rp 5.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + mount | Rp 500.000 |
| Power bank (20000mAh) | Rp 500.000 |
| Tongkat DIY | Rp 500.000 |
| Carrying case | Rp 200.000 |
| **Total** | **Rp 18.900.000** |

### Opsi B: Konfigurasi Budget

| Item | Estimasi Harga |
|------|----------------|
| Intel RealSense D435i | Rp 9.000.000 |
| Xiaomi Pad 6 (8/256GB) | Rp 5.000.000 |
| Kabel USB-C 3.1 | Rp 200.000 |
| Tripod + mount | Rp 400.000 |
| Power bank (20000mAh) | Rp 400.000 |
| Tongkat DIY | Rp 400.000 |
| Carrying case | Rp 150.000 |
| **Total** | **Rp 15.550.000** |

### Opsi C: Konfigurasi Premium

| Item | Estimasi Harga |
|------|----------------|
| Intel RealSense D455 | Rp 12.000.000 |
| Samsung Tab S8 | Rp 8.000.000 |
| Kabel USB-C 3.1 | Rp 300.000 |
| Tripod + mount | Rp 700.000 |
| Power bank (30000mAh) | Rp 700.000 |
| Commercial Pole | Rp 1.500.000 |
| Carrying case | Rp 300.000 |
| **Total** | **Rp 23.500.000** |

---

## 3.9 Catatan Teknis

1. Tablet dengan port USB Type-C tidak selalu menggunakan USB 3.0. Verifikasi spesifikasi perangkat sebelum pembelian.

2. RealSense D455 dan D435i memerlukan USB 3.0 untuk operasi penuh. USB 2.0 tidak menyediakan bandwidth yang cukup.

3. iPad tidak didukung oleh RealSense SDK.

4. Harga kamera depth di Indonesia lebih tinggi dari harga retail internasional karena biaya impor.

5. Untuk pengujian kompatibilitas, gunakan aplikasi RealSense Viewer setelah perangkat diterima.

6. Budget Rp 21 juta sudah mencakup toleransi untuk variasi harga dan aksesoris tambahan.

---

## 3.10 Sumber Pembelian

| Item | Platform |
|------|----------|
| Intel RealSense | Tokopedia, Shopee (cari: "RealSense D455" atau "RealSense D435i") |
| Xiaomi Pad 6 | Mi Store, Tokopedia, Shopee, Lazada, Blibli |
| Samsung Tab S8 | Samsung Store, Tokopedia, Shopee |
| GPS External | Tokopedia, Shopee (cari: "GPS Bluetooth" atau "Garmin GLO") |
| Tongkat teleskopik | Tokopedia, Shopee (cari: "telescopic pole" atau "tongkat pancing carbon") |
| Aksesoris | Tokopedia, Shopee |
