# BAGIAN 4: SPESIFIKASI HARDWARE (DEPTH & TABLET)

**Status:** Untuk Batch Selanjutnya & Eksperimen

---

## 1. Kamera Depth

### Spesifikasi Minimal
- IMU (Inertial Measurement Unit) - deteksi orientasi
- Tahan cahaya matahari siang
- Jangkauan minimal 3-4 meter

### Utama: Intel RealSense D455

| Spesifikasi | Detail |
|-------------|--------|
| Harga | ~$350-400 USD |
| Jangkauan | 0.6m - 6m |
| IMU | Ada (BMI055) |
| Global Shutter | Ada |
| Koneksi | USB 3.1 Type-C |

### Alternatif: Intel RealSense D435i

| Spesifikasi | Detail |
|-------------|--------|
| Harga | ~$250-300 USD |
| Jangkauan | 0.3m - 3m |
| IMU | Ada (huruf "i") |
| Koneksi | USB 3.0 Type-C |

> **Catatan:** Pastikan ada huruf "i" (D435**i**). "i" menandakan adanya IMU.

---

## 2. Tablet

### Spesifikasi Minimal
- **USB Type-C 3.0/3.1** - USB 2.0 tidak cukup bandwidth
- **Baterai Besar (7000mAh+)** - menyuplai daya ke kamera
- **OS Android** - kompatibel RealSense SDK

### Rekomendasi

| Tablet | USB | Baterai | Harga | Catatan |
|--------|-----|---------|-------|---------|
| Tab Active 4 Pro | 3.2 | 7600mAh | Rp 10-12jt | Rugged |
| Tab S6 Lite 2024 | 3.0 | 7040mAh | Rp 5-6jt | Budget |
| Samsung A55 (HP) | 2.0 | 5000mAh | Murah | Perlu test |

> Samsung A55 kemungkinan tidak kompatibel karena USB 2.0

---

## 3. Estimasi Budget

| Item | Harga |
|------|-------|
| RealSense D455 | Rp 5.5-6.5 juta |
| Tab Active 4 Pro | Rp 10-12 juta |
| Tripod + holder | Rp 700rb - 1.2jt |
| **TOTAL** | **Rp 16-20 juta** |

**Alternatif Budget:** D435i + Tab S6 Lite = Rp 10-12 juta
