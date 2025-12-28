# BAGIAN 1: DATA COLLECTION (BATCH 1 - RGB)

**Tujuan:** Standarisasi input data agar model AI bisa dikembangkan dengan benar.

---

## 1. SOP Pengambilan Gambar

### 1.1 Urutan Pengambilan (Sequence)

| Aspek | Ketentuan |
|-------|-----------|
| **Aturan** | Wajib berurutan dan **konsisten** |
| **Arah** | Utara → Timur → Selatan → Barat |
| **Rotasi** | **Searah Jarum Jam (Clockwise)** untuk keseragaman seluruh surveyor |

**Alasan:** Agar saat *stitching* atau *counting*, algoritma tahu gambar ke-2 adalah sisi kanan dari gambar ke-1.

### 1.2 Posisi & Jarak

| Parameter | Nilai | Catatan |
|-----------|-------|---------|
| **Jarak** | 2 - 3 meter dari batang | Jarak optimal untuk cakupan frame |
| **Tinggi Kamera** | 150 cm (setinggi dada) | Gunakan tripod untuk konsistensi, atau patokan leher surveyor |
| **Angle** | Lurus ke depan (*Frontal/Eye-level*) | Tidak boleh terlalu mendongak, kecuali pohon tinggi (harus dicatat) |

### 1.3 Komposisi Frame (WAJIB)

**Cakupan yang HARUS terlihat dalam foto:**
1. ✅ **Batang Pohon** - Sebagai referensi ukuran
2. ✅ **Pangkal Pelepah/Tajuk** - Tempat buah berada

> **⚠️ Alasan Kritis:** Tanpa batang dalam frame, AI tidak punya pembanding untuk membedakan "Buah Kecil difoto dekat" vs "Buah Besar difoto jauh".

---

## 2. Setting Kamera

| Parameter | Pengaturan | Keterangan |
|-----------|------------|------------|
| **Device** | Samsung A55 (atau setara) | Sebagai kandidat utama |
| **Orientasi** | Portrait (Tegak) | - |
| **Rasio** | 4:3 (Native Sensor) | ❌ Jangan 16:9 (memotong atas-bawah) |
| **Fitur AI/Beauty** | **OFF** | ⚠️ WAJIB dimatikan agar warna buah tidak dimanipulasi |
| **Flash** | OFF | - |
| **Resolusi** | Highest Available | Untuk keperluan zoom saat pelabelan manual |

---

## 3. Quality Control (Sortir Batch 1)

### 3.1 Prosedur Sortir

```
Target Awal: ~1000 gambar
        ↓
    [SORTIR MANUAL]
        ↓
Kriteria Reject:
- ❌ Gambar blur
- ❌ Backlight parah  
- ❌ Batang tidak terlihat
        ↓
Dataset Final: ~900 gambar "Sempurna"
```

### 3.2 Jadwal Pengambilan

Pengambilan gambar terdiri dalam beberapa batch:
- Bulan Januari: 1 kali
- Bulan April: 1 kali
- Hasil gambar dari setiap batch akan disortir lagi
- Yang diambil hanya yang bagus dan sesuai standard

---

## 4. Kondisi Cuaca

> **⚠️ PENTING:** Pencahayaan menjadi *concern* utama. Supaya data konsisten bagus, **WAJIB cek cuaca terlebih dahulu** sebelum pengambilan.

---

## Checklist Surveyor

- [ ] Cek cuaca sebelum berangkat
- [ ] Bawa tripod (jika memungkinkan)
- [ ] Setting kamera sesuai SOP di atas
- [ ] Pastikan urutan pengambilan Clockwise (U→T→S→B)
- [ ] Pastikan batang pohon masuk dalam frame
- [ ] Periksa hasil foto langsung setelah pengambilan (blur check)
