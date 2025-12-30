
# Catatan feedback

### Q1.

* "07:00-10:00 atau 15:00-17:00": Jam ini tidak bisa dibatasi. Sesuaikan dengan survei normal 8-4 sore (ada lunch break sebentar). Palingan nanti datasetnya diberi catatan waktu/cuaca.
* Pengambilan UTSB: Apakah benar-benar dimulai dari Utara _real_?
* Video Data: Apakah tidak jadi mengambil data video?
* Sampling: Pengukuran tandan dikurangi, cukup 3 tandan x 10 pohon saja. Ukur keliling tetap 50.


### Q2.

* Data yang kurang ideal dipisah tetapi dilabeli juga, agar bisa digunakan untuk _training_/pengujian tambahan.
*

* Training yolov8n terlalu sedikit data dengan 200 image. Alternatif lain yaitu:
  * \[ ] Prelabel deteksi tandan  dengan model yang sudah ada (cari model _pretrained_ tandan sawit di pohon di Roboflow yang MAP-nya tinggi). _Ignore_/buang kelas, jadi hanya ambil output _bounding box_.
  * \[ ] Label Kelas (Seed): Pakar memberi label kelas 100 pohon/400 images, tidak perlu menggambar kotak lagi, kecuali revisi.
  * \[ ] Train Classifier dari 100 pohon tersebut.
  * \[ ] Auto-label: Prelabel kelas untuk pohon 101 dst.
  * \[ ] Validasi/pengecekan kelas oleh pakar.

> **Request:** Saat pelabelan/validasi kelas, jika pakar kurang yakin, agar bisa di-_note_ ketidakyakinan ini. Informasi ketidakyakinan ini siapa tahu bisa dimanfaatkan sebagai opsi ketika training/analisis.

Info terakhir: 

* Yang terlibat untuk pelabelan ini dari GMK adalah 4 pakar. Jadi nanti akan dibikin 2 tim paralel dengan per tim 1 asisten & 2 pakar.
* Pakar ini fleksibel, apakah 2 orang bertugas sekaligus (bisa sambil diskusi) atau bergantian.
* Alokasi Waktu: 3-5 hari x 7 jam/hari
* Tempat: Kemungkinan di kantor mereka atau sewa _meeting space_.


### Q3.

* Tambahan spesifikasi alat, bisa GPS.
* Tambahkan 1 opsi untuk _phone_ (non-tablet).
* Tongkat (Pole): Sebetulnya rencana tongkatnya 3-5 meter, tantangannya harus ringan dan kuat. Semacam ini: [Telescopic Inspection Camera Pole (link)](https://www.tiptopcomposites.com/product/telescopic-inspection-camera-pole/), tapi cari yang lebih murah atau merakit sendiri.
* Budget: Anggaran 1 set perangkat Rp 21 juta (termasuk powerbank). Tidak apa-apa lebih atau kurang.

***
### Q4 dan Q5
* Kegiatan terkait Q4 dan Q5 masih agak lama dan akan didiskusikan lagi setelah urusan _data collecting_ dan _labeling_ sudah beres.
* Secara umum sudah bagus, nanti yang perlu ditambahkan protokol untuk _baseline_ yang lebih menyeluruh.
