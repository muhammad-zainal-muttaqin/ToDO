# 3. REKOMENDASI SPESIFIKASI ALAT

---

## A. Kamera Depth

### A.1 Spesifikasi Minimal

| Syarat | Alasan |
|--------|--------|
| IMU (Inertial Measurement Unit) | Deteksi orientasi kamera |
| Outdoor-ready | Pengambilan di perkebunan siang hari |
| Jangkauan 3-6 meter | Jarak pengambilan dari batang pohon |
| USB 3.0+ | Bandwidth tinggi untuk transfer data depth |

### A.2 Rekomendasi Utama: Intel RealSense D455

| Spesifikasi | Detail |
|-------------|--------|
| Part Number | 82635DSD455 |
| Teknologi | Active Stereo |
| Jangkauan | 0.6m - 6m (optimal) |
| Akurasi | < 2% error pada 4 meter |
| Resolusi Depth | 1280 x 720 @ 90 fps |
| FOV | 86° x 57° |
| RGB | 1280 x 800 @ 30 fps, Global Shutter |
| IMU | Bosch BMI055 (6DoF) |
| Koneksi | USB 3.1 Gen 1 Type-C |
| Dimensi | 124mm x 26mm x 29mm |

**Harga di Indonesia:** Rp 10-16 juta (Tokopedia/Shopee)

### A.3 Alternatif: Intel RealSense D435i

| Spesifikasi | Detail |
|-------------|--------|
| Part Number | 82635D435I |
| Jangkauan | 0.3m - 3m (optimal) |
| RGB | Rolling Shutter |
| IMU | Bosch BMI055 (6DoF) |

**Harga di Indonesia:** Rp 7-12 juta

### A.4 Perbandingan

| Fitur | D455 | D435i |
|-------|------|-------|
| Jangkauan optimal | 6m | 3m |
| Akurasi jarak jauh | Lebih baik | Kurang |
| RGB Shutter | Global | Rolling |
| Harga | Lebih mahal | Lebih murah |

**Rekomendasi:** D455 untuk jarak 2-3m dari pohon.

---

## B. Tablet / Android Device

### B.1 Syarat Kompatibilitas RealSense

| Syarat | Alasan |
|--------|--------|
| USB 3.0+ (USB 3.2 Gen 1) | Transfer data depth |
| USB OTG Support | Koneksi kamera external |
| Qualcomm Snapdragon | Kompatibilitas SDK |
| Baterai > 7000mAh | Supply daya ke kamera |

### B.2 Tabel Kompatibilitas

| Brand | Model | USB | Kompatibel |
|-------|-------|-----|------------|
| Xiaomi | Pad 6 | USB 3.2 | ✓ Ya |
| Xiaomi | Pad 5 | USB 2.0 | ✗ Tidak |
| Poco | Poco Pad | USB 2.0 | ✗ Tidak |
| Redmi | Pad Pro | USB 2.0 | ✗ Tidak |
| Samsung | Tab S8/S9 | USB 3.2 | ✓ Ya |
| Samsung | Tab S6 Lite | USB 2.0 | ✗ Tidak |
| Samsung | Tab S9 FE | USB 2.0 | ✗ Tidak |
| Lenovo | Tab P12 Pro | USB 3.2 | ✓ Ya |

### B.3 Rekomendasi: Xiaomi Pad 6

| Spesifikasi | Detail |
|-------------|--------|
| Layar | 11" IPS, 2880x1800, 144Hz |
| Prosesor | Snapdragon 870 |
| RAM/Storage | 8GB/256GB |
| USB | USB 3.2 Gen 1 + OTG |
| Baterai | 8840 mAh |
| OS | Android 13 |

**Harga di Indonesia:** Rp 4.5-5.5 juta

### B.4 Alternatif

| Tablet | USB | Harga |
|--------|-----|-------|
| Samsung Tab S8 | USB 3.2 | Rp 7-9 juta |
| Lenovo Tab P12 Pro | USB 3.2 Gen 2 | Cek ketersediaan |

---

## C. Smartphone untuk Data Reguler (Batch 1)

### C.1 Spesifikasi Minimal

| Parameter | Nilai |
|-----------|-------|
| Kamera | ≥12 MP |
| Orientasi | Portrait |
| Rasio | 4:3 |
| Fitur AI/Beauty | Bisa dimatikan |

### C.2 Kandidat

Smartphone apa saja yang memenuhi kriteria di atas bisa digunakan. Contoh: Samsung A55, Xiaomi Redmi series, dll.

---

## D. Aksesoris

| Item | Spesifikasi | Harga Est. |
|------|-------------|------------|
| Kabel USB-C | USB 3.1, 1-2m | Rp 100-300rb |
| Tripod | Ball head, 100-180cm | Rp 300-800rb |
| Tablet holder | Clamp mount | Rp 50-150rb |
| Power bank | 20000mAh+, USB-C PD | Rp 300-600rb |

---

## E. Estimasi Budget

### Opsi A: Rekomendasi

| Item | Harga |
|------|-------|
| RealSense D455 | Rp 12.000.000 |
| Xiaomi Pad 6 | Rp 5.000.000 |
| Kabel + Tripod + Aksesoris | Rp 1.100.000 |
| **TOTAL** | **Rp 18.100.000** |

### Opsi B: Budget

| Item | Harga |
|------|-------|
| RealSense D435i | Rp 9.000.000 |
| Xiaomi Pad 6 | Rp 5.000.000 |
| Kabel + Tripod + Aksesoris | Rp 900.000 |
| **TOTAL** | **Rp 14.900.000** |

---

## F. Catatan Penting

1. **Banyak tablet menggunakan USB 2.0** meskipun port-nya Type-C. Selalu verifikasi.
2. **Xiaomi Pad 6 adalah value terbaik** untuk kompatibilitas RealSense.
3. **Hindari iPad** - RealSense SDK tidak support iOS.
4. **Pastikan beli kabel USB 3.1** bukan kabel charging biasa.
