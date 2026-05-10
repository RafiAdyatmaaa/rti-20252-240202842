# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

```
SYSTEM-EXPERIMENT MAPPING

Research Question: ____________________

Variable → Component Mapping:
| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|----------|------|-----------------|---------------------------|
|          | IV   |                 |                           |
|          | DV   |                 |                           |
|          | CV   |                 |                           |

4 Prinsip Desain:
  [ ] Traceability — Setiap komponen bisa ditelusuri ke variabel
  [ ] Variable Isolation — IV bisa diubah tanpa mengubah CV
  [ ] Measurement Integration — Pengukuran DV built-in
  [ ] Reproducibility — Setup bisa direkonstruksi

Experimental Setup:
  Input data     : ____________________
  Parameter      : ____________________
  Output format  : ____________________
```

---

## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** Apakah saat jumlah pengguna Mikrotik yang aktif meningkat dari dua puluh menjadi lima puluh, algoritma batas dinamis dapat menghasilkan nilai throughput yang lebih stabil daripada metode PCQ statis?

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
| metode eliminasi | IV | Script / Konfigurasi Queue Tree | Mengganti profil dari PCQ Statis ke Dynamic-Limit lewat terminal Mikrotik. |
| Kualitas Jaringan | DV | Traffic Monitor & Logging | Mencatat data Throughput (Mbps) secara otomatis ke file .csv. |
| Beban Pengguna | CV | DHCP Server / Access Point | Membatasi jumlah user yang bisa terhubung (max 20 atau 50) lewat settingan AP |

**Apakah semua variabel bisa di-map?** Ya
> Jika tidak, komponen apa yang perlu ditambahkan? Semua variabel riset sudah ditempatkan di dalam konfigurasi router, sehingga kami memiliki kontrol penuh selama pengujian.

---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability | ✅ | Saya sudah memberi label kepada semua script Mikrotik, seperti "queue-static" dan "queue-dynamic", agar jelas mana yang sedang diuji. |
| Modularity | ✅ | Algoritma Dynamic-Limit dibuat dalam modul script yang berbeda, jadi jika Anda ingin mengubah metode, Anda tidak perlu mengubah konfigurasi internet dasar. |
| Controllability | ✅ | Dengan menggunakan variabel dalam script, Anda dapat mengubah parameter seperti batas maksimal dan jumlah user tanpa perlu menginstal ulang router. |
| Measurability | ✅ | Agar data tidak hilang, router diatur untuk mengirimkan log performa ke komputer penguji setiap detik. |

**Prinsip mana yang paling sulit dipenuhi?** Controllability.
**Strategi untuk mengatasinya:**
> Seringkali, pengguna asli masuk-keluar secara acak. Saya menggunakan simulasi perangkat (dummy client) untuk memastikan bahwa jumlah pengguna tetap stabil di antara dua puluh atau lima puluh, sesuai dengan rencana pengujian.

---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study.

| Kondisi | Komponen A (Dynamic PCQ) | Komponen B (User Detection) | Komponen C (Priority Traffic)| Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full | ✅aktif | ✅aktif | ✅aktif | Koneksi paling stabil dan adil. |
| – A | ❌ (ganti statis) | ✅aktif | ✅aktif | Internet mulai melambat saat user nambah |
| – B | ✅aktif | ❌ (manual input) | ✅aktif | Sistem telat merespon saat user membludak |
| – C | ✅aktif | ✅aktif | ❌ (tanpa prioritas) | Download lancar tapi video call patah-patah |

**Komponen mana yang diprediksi paling berkontribusi?** Komponen B (User Detection)
**Mengapa?**
> Algoritma Dynamic PCQ tidak tahu kapan harus memperketat atau melonggarkan bandwidth jika tidak ada deteksi user yang akurat. Ini adalah pusat penelitian saya.

---

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Jika kita membuat sistem yang mirip dengan produk jadi (monolitik) yang belum pernah dicoba sebelumnya, risikonya adalah akan sulit untuk menemukan "bagian mana sih yang sebenarnya membuat internet menjadi cepat?" Semuanya berubah menjadi campuran. Kita akan bingung apakah ada masalah dengan setting router, jumlah user, atau algoritma yang tidak berfungsi jika terjadi masalah.
> Karena kita membutuhkan isolasi variabel, arsitektur modular sangat penting untuk penelitian. Kita harus dapat mencoba menghapus satu fitur tanpa mengganggu fitur lain. Oleh karena itu, kita dapat dengan yakin mengatakan: "Fitur A inilah yang bikin Throughput naik 20%" karena data yang kami terima benar.
