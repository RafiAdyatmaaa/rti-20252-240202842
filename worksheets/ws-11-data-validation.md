# WS-11: Data Validation & Integrity

> **Bab 11 — Validasi Data & Integritas**

---

## Ringkasan Materi

### Data Trust Model

```
Raw Data → Data Cleaning → Consistency Check → Validation Process → Trusted Data
```

Data mentah belum bisa dipercaya. Harus melewati pipeline validasi sebelum siap untuk analisis statistik.

### Empat Pilar Data Quality

| Pilar | Deskripsi | Contoh Pelanggaran |
|-------|----------|-------------------|
| **Accuracy** | Nilai dalam range masuk akal | Akurasi = 1.5 (di luar [0,1]) |
| **Consistency** | Format seragam di semua run | Run 1: CSV, Run 2: JSON |
| **Completeness** | Tidak ada data hilang dari plan | 97 dari 100 run tercatat |
| **Validity** | Data sesuai desain eksperimen | Parameter baseline tercampur treatment |

### Proses Validasi Progresif

1. **Format validation** — Tipe file, header, kolom
2. **Range validation** — Nilai dalam batas logis
3. **Consistency validation** — Format seragam antar-run
4. **Logic validation** — Data cocok dengan desain eksperimen

Jika gagal di langkah awal → tidak perlu lanjut.

### Anomaly Detection — 3 Jenis

| Jenis | Deskripsi | Deteksi |
|-------|----------|---------|
| **Statistical outlier** | Nilai di luar distribusi normal | IQR: < Q1-1.5×IQR atau > Q3+1.5×IQR |
| **Contextual anomaly** | Normal absolut, abnormal dalam konteks | Run 1-10: ~91%, Run 11-20: ~88% |
| **Pattern anomaly** | Pola sistematis (bukan random) | Performa menurun berurutan |

**Prinsip:** Detect → Investigate → Document → Decide — **JANGAN langsung hapus.**

### Engineering vs Research Validation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Data sesuai spesifikasi bisnis | Data layak untuk analisis statistik |
| Missing data | Impute / set default | Investigasi penyebab → dokumentasi |
| Outlier | Bug → fix | Mungkin temuan → investigasi |
| Dokumentasi | Minimal (log error) | Komprehensif (anomali + keputusan) |

### Jebakan Kognitif

1. "Logging otomatis ≠ data benar" → bisa ada bug di logger
2. "Outlier = hapus" → bisa jadi temuan penting
3. "Dataset kecil tidak perlu validasi" → justru lebih rentan
4. "Mean normal = data benar" → [94, 95, 93, **44**, 94] → mean 84% terlihat wajar

---

## Template A.11 — Data Validation Checklist

```
DATA VALIDATION CHECKLIST

Completeness:
  [x] Semua skenario tercakup
  [x] Jumlah run sesuai rencana
  [x] Tidak ada file output hilang
  Missing: 0 dari 20 data points

Format Consistency:
  [x] Semua file format sama (Seluruhnya berekstensi .csv)
  [x] Header konsisten (Timestamp, Active_Users, PCQ_Rate, Throughput, Latency, CPU_Load)
  [x] Tipe data konsisten (Active_Users & Latency = Integer, Throughput = Float)

Range & Logic:
  [x] Nilai dalam range masuk akal
  [x] Tidak ada waktu negatif
  [x] Metrik 0–100%, tidak di luar range
  Anomali ditemukan: Terjadi pencatatan latency ekstrem (>500ms) pada RUN-BASE-004 saat user memuncak.

Cross-Validation:
  [x] Run identik → hasil mendekati (Selisih throughput antar-run baseline < 5%)
  [x] Trend konsisten dengan ekspektasi teori (User naik -> Latency naik pada baseline; Latency terjaga pada intervensi)

Keputusan:
  [x] Data siap analisis
  [ ] Perlu cleaning
  [ ] Perlu re-run (skenario: ____)
```

---

## Latihan 1 — Completeness Check

Verifikasi apakah semua data yang direncanakan sudah terkumpul.

| Skenario | Run Direncanakan | Run Tercatat | Missing | Alasan |
|----------|-----------------|-------------|---------|--------|
| Baseline (PCQ Statis) | 10 | 10 | 0 | — |
| Intervensi (Dynamic PCQ) | 10 | 10 | 0 | - |
| | | | | |
| | | | | |

**Total expected:** 20 | **Total actual:** 20 | **Missing:** 0

**Keputusan untuk data missing:**
> Tidak ada data yang hilang (Zero Missing Data). Seluruh berkas log sebanyak 20 file .csv dari eksekusi automated script/scheduler dan traffic monitor MikroTik berhasil ditarik secara lengkap melalui Winbox FTP ke storage laptop Asus TUF Gaming A16 peneliti.

---

## Latihan 2 — Anomaly Investigation

Periksa data Anda untuk anomali. Gunakan metode IQR atau z-score.

**Dataset sampel (atau data Anda sendiri):**

| Run | Accuracy (%) |
|-----|-------------|
| 11 (Dyn-01) | 49.2 |
| 12 (Dyn-02) | 48.9 |
| 13 (Dyn-03) | 49.4 |
| 14 (Dyn-04) | 35.1 (Potensi Anomali) |
| 15 (Dyn-05) | 49.1 |

**Deteksi outlier:**
- Data diurutkan: 35.1, 48.9, 49.1, 49.2, 49.4
- Q1 = 48.9 | Q3 = 49.3 | IQR = Q3 - Q1 = 0.4
- Batas bawah (Q1 - 1.5×IQR) = 48.9 - 0.6 = 48.3Mbps
- Batas atas (Q3 + 1.5×IQR) = 49.3 + 0.6 = 49.9Mbps
- Outlier terdeteksi: Run 14 (Nilai 35.1 Mbps berada jauh di bawah batas bawah 48.3 Mbps).

**Investigasi (untuk setiap outlier):**

| Outlier | Nilai | Kemungkinan Penyebab | Keputusan |
|---------|-------|---------------------|-----------|
| Run 14 (Dyn-04) | 35.1 Mbps | Terjadi penurunan suplai alokasi bandwidth nasional (ISP Upstream Jitter/Drop) dari penyedia layanan internet pusat saat jam uji, bukan karena kegagalan algoritma router. | Dokumentasikan gangguan eksternal ini pada log, lakukan cleaning/filtering data pencilan eksternal ini, lalu lakukan re-run khusus untuk Run 14 pada kondisi internet stabil agar tidak merusak uji statistik T-Test. |

---

## Latihan 3 — Validation Report

Buat laporan validasi ringkas untuk dataset eksperimen Anda.

**1. Completeness:** 100% data eksperimen berhasil terkumpul (20 dari 20 run).
**2. Format:** [x] Konsisten / [ ] Ada inkonsistensi: Seluruh data berformat .csv dengan delimeter koma standar MikroTik Logger.
**3. Range check (anomali):**Batas kapasitas internet 50 Mbps valid, tidak ditemukan nilai throughput negatif atau melebihi 50 Mbps. Terdeteksi 1 statistical outlier pada aspek throughput akibat faktor ISP drop.
**4. Logic check:** [x] Parameter sesuai plan / [ ] Ada ketidaksesuaian: Rentang manipulasi user dari 20 ke 50 user terekam secara runtut sesuai skenario beban interaktif.

**Kesimpulan:** [x] Data siap analisis / [ ] Perlu tindakan: Data siap dianalisis menggunakan uji komparatif Independent Samples T-Test setelah re-run data pencilan eksternal diselesaikan.

---

## Refleksi

> Apa perbedaan antara "data yang benar" dan "data yang dipercaya"? Mengapa proses validasi formal diperlukan meskipun data dikumpulkan secara otomatis?

> Data yang benar adalah data apa adanya yang dihasilkan oleh sistem perekam otomatis. Sebagai contoh, modul internal sebenarnya "benar" mencatat throughput turun ke 35 Mbps di router. Namun, untuk sebuah kesimpulan ilmiah, data tersebut belum tentu menjadi "data yang dipercaya". Jika data baru bebas dari bias atau gangguan luar non-eksperimental, mereka dapat "dipercaya". Contohnya adalah ketika ISP hilang dari operator pusat di atas atau gangguan interferensi frekuensi luar.
> Meskipun data dikumpulkan secara otomatis, proses validasi formal tetap diperlukan. Hanya efisiensi pengumpulan yang dijamin oleh otomatisasi perekaman log script MikroTik ke dalam file CSV, bukan kebenaran konteks eksperimen. Dalam perhitungan statistik, anomali infrastruktur atau gangguan lingkungan sektoral tidak akan dimasukkan tanpa validasi formal seperti pemeriksaan batas logis (range check) dan deteksi pencilan (outlier detection). Hal ini dapat menyebabkan hasil penelitian yang salah; misalnya, mereka dapat menganggap algoritma Dynamic-Limit tidak berfungsi, padahal penurunan kinerja murni disebabkan oleh komponen pipa ISP eksternal.
