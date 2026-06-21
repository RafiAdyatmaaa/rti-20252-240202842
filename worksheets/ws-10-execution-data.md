# WS-10: Experiment Execution & Data Collection

> **Bab 10 — Eksekusi Eksperimen & Pengumpulan Data**

---

## Ringkasan Materi

### Experiment Execution Pipeline

```
Design → Execution Plan → Controlled Execution → Data Collection → Data Logging → Dataset for Analysis
```

### Multiple Run = Non-Negotiable

Single run **tidak pernah cukup** untuk klaim ilmiah. Minimum 5-10 run per skenario dengan seed berbeda. Multiple run menghasilkan:
- Mean, std, confidence interval
- Distribusi hasil → uji statistik
- Variabilitas → error bar di grafik

### Execution Plan

Setiap eksperimen harus memiliki plan sebelum eksekusi:
- Daftar skenario
- Jumlah run per skenario
- Random seed per run (pre-determined!)
- Urutan eksekusi (randomisasi/counterbalancing)
- Pre-execution checklist

### Data Logging Komprehensif

Setiap run menghasilkan log terstruktur:
1. **Identitas** — Run ID, timestamp, skenario
2. **Konfigurasi** — Semua parameter, seed, code version
3. **Hasil** — Semua metrik, output detail
4. **Metadata** — Waktu eksekusi, resource usage, warning/error

Format: CSV/JSON/database — **bukan stdout yang di-copy-paste**.

### Engineering vs Research Execution

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Run | Sekali (deploy) | Multiple (min 5-10, seed berbeda) |
| Logging | Error log, access log | Semua parameter, metrik, metadata |
| Anomali | Bug → fix → redeploy | Investigasi → dokumentasi → analisis |
| Urutan | Tidak penting | Bisa bias — perlu randomisasi |

### Anomali = Dokumentasi, Bukan Hapus

Run gagal/anomali tidak boleh dihapus tanpa dokumentasi. Bisa jadi:
- **Bug** → fix & re-run (dokumentasikan!)
- **Batas kemampuan metode** → DNF = temuan
- **Data yang bias** jika hanya simpan run "berhasil"

### Jebakan Kognitif

1. "Satu angka cukup" → tanpa distribusi, tidak bisa diuji
2. "Seed tidak penting" → bahkan algoritma deterministik bisa dipengaruhi library stokastik
3. "Run gagal langsung hapus" → kehilangan temuan potensial
4. "Semua run harus hari ini" → thermal throttling, fatigue

---

## Template A.10 — Execution Plan & Data Log

```
EXECUTION PLAN

| Run # | Skenario | Seed | Parameter | Status | Waktu | Output File |
|-------|----------|------|-----------|--------|-------|-------------|
| 1     | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_01.csv |
| 2     | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_02.csv |
| 3     | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_03.csv |
| 4   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M  | Planned  | 10 Min | baseline_run_04.csv  |
| 5   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_05.csv |
| 6   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_06.csv |
| 7   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_07.csv |
| 8   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_08.csv  |
| 9   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_09.csv |
| 10   | Baseline (PCQ Statis) | N/A (Fixed Queue) | Load=20-50 user, rate=2M | Planned | 10 Min | baseline_run_10.csv |
| 11   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_01.csv  |
| 12   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_02.csv |
| 13   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_03.csv |
| 14   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_04.csv |
| 15   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_05.csv |
| 16   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_06.csv |
| 17   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_07.csv |
| 18   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_08.csv            |
| 19   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_09.csv            |
| 20   | Intervensi (Dyn-PCQ) | Scheduler = 5 detik | Load=20-50 user, dynamic | Planned | 10 Min | dynamic_run_10.csv            |

Jumlah runs per skenario : 10
Total runs               : 20

DATA LOG (per run):
  Sesi Run 1 s.d. 10: Contoh Log Skenario Baseline
  Run ID    : RUN-BASE-004
  Timestamp : 2026-05-18T23:15:00Z
  Skenario  : Baseline (PCQ Statis Konvensional)
  Input     : Injeksi beban bertahap hingga 50 perangkat nirkabel aktif via aplikasi Btest (Profil mixed download).
  Output    : File mentah baseline_run_04.csv mencatat rata-rata Global Throughput sebesar 48.1 Mbps dengan tingkat fluktuasi standar deviasi tinggi (±4.2 Mbps) dan Latency rata-rata klien membengkak hingga 142 ms akibat gejala bufferbloat.
  Anomali   : Tidak ditemukan adanya crash sistem, namun beban CPU router terpantau sangat rendah (di bawah 15%) karena konfigurasi bersifat kaku dan tidak melakukan perhitungan komputasi dinamis.
  Catatan   : Beberapa perangkat klien mengalami monopoli alokasi jalur data (bandwidth starvation) ketika ada 3 user melakukan unduhan file besar secara simultan.
  Sesi Run 1 s.d. 10: Contoh Log Skenario Baseline
  Run ID    : RUN-DYN-015
  Timestamp : 2026-05-18T23:55:00Z
  Skenario  : Intervensi (Algoritma Dynamic-Limit PCQ)
  Input     : Injeksi beban bertahap hingga 50 perangkat nirkabel aktif via aplikasi Btest (Profil mixed download - Identik dengan baseline).
  Output    : File mentah dynamic_run_05.csv mencatat rata-rata Global Throughput optimal sebesar 49.3 Mbps dengan sebaran distribusi data yang sangat rapat dan stabil (standar deviasi menembus angka ideal ±0.8 Mbps). Latency rata-rata klien terkendali dengan aman pada angka 42 ms.
  Anomali   : Terjadi lonjakan beban penggunaan CPU (CPU Spike) hingga mencapai 78% pada router MikroTik RB750Gr3 setiap siklus eksekusi kalkulasi algoritma (tiap interval 5 detik)
  Catatan   : Skrip RouterOS berhasil mendeteksi perubahan stasiun kerja aktif secara real-time dan membagi parameter pcq-rate secara adil, mencegah terjadinya monopoli trafik pada seluruh IP klien terdaftar. Kondisi suhu laptop Asus TUF Gaming A16 pengolah data terpantau stabil pada 65°C sepanjang pencatatan log.
  

```

---

## Latihan 1 — Execution Plan

Susun execution plan untuk eksperimen Anda. Tentukan skenario, jumlah run, dan seed sebelum eksekusi.

| Run # | Skenario | Seed | Parameter Kunci | Status |
|-------|----------|------|----------------|--------|
| *1* | *Contoh: BERT-base, DS-1* | *42* | *lr=2e-5, epoch=10* | *Planned* |
| *2* | *BERT-base, DS-1* | *123* | *lr=2e-5, epoch=10* | *Planned* |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

**Total skenario:** ____
**Run per skenario:** ____
**Total run keseluruhan:** ____

---

## Latihan 2 — Data Log Terstruktur

Desain format data log untuk eksperimen Anda. Tentukan field apa saja yang akan dicatat.

**Identitas:**
| Field | Contoh |
|-------|--------|
| Run ID | RUN-BASE-002 / RUN-DYN-007 |
| Timestamp | 2026-05-18T23:10:00Z |
| Scenario Name | Baseline_Static_PCQ |

**Konfigurasi:**
| Field | Contoh |
|-------|--------|
| Execution Interval | 5s (Scheduler Loop) |
| RouterOS Code Version | v7.12 Stable Long-term |
| Hardware Testbed | Router MikroTik RB750Gr3 |
| Target Active Stations | 50 User (Simulation load peak) |

**Hasil:**
| Metrik | Tipe Data | Range Valid |
|--------|----------|-------------|
| Global Throughput | float | 0.00 – 50.00 (Mbps) |
| Average Latency (ICMP Ping) | integer | 1 – 999 (ms) |
| Standard Deviation Throughput | float | 0.00 – 10.00 (Indikator Stabilitas) |
| CPU Load Router | integer | 0 – 100 (%) |

**Format output:** [x] CSV / [ ] JSON / [ ] Database / [ ] Lainnya: ____

---

## Latihan 3 — Anomaly Protocol

Rencanakan bagaimana menangani anomali. Untuk setiap jenis, tentukan langkah yang diambil.

| Jenis Anomali | Contoh | Tindakan |
|---------------|--------|----------|
| Run gagal (crash) | Router RB750Gr3 mengalami hang atau kernel panic (CPU Load 100%) akibat script loop memory leak. | Dokumentasikan status penggunaan memori terakhir, lakukan hard-reboot router, perbaiki alokasi penutupan variabel lokal pada script RouterOS, lalu lakukan re-run. |
| Hasil ekstrem | Nilai Latency melonjak hingga > 1000ms secara mendadak pada satu klien tertentu. | Selidiki tabel /interface wireless registration-table untuk memeriksa nilai CCQ (Client Connection Quality). Jika fluktuasi terjadi karena masalah fisik (jarak/halangan), dokumentasikan log tersebut, tandai sebagai noise fisik, dan pertahankan dalam analisis. |
| Waktu eksekusi anomali | Scheduler melompati interval eksekusi (seharusnya tiap 5 detik menjadi 15 detik). | Periksa system resource log. Batasi aktivitas pembacaan Winbox GUI secara real-time yang membebani prosesor core tunggal router. Alihkan logging ke memori internal disk. |
| Inkonsistensi dengan run lain | Nilai rata-rata throughput global turun drastis di bawah 30 Mbps pada Run ke-4 skenario intervensi. | Periksa jalur pipa hulu ISP (upstream bandwidth verification). Jalankan uji Btest standalone ke gateway ISP untuk memastikan tidak ada penurunan suplai dari operator luar. Jika ada, batalkan pengujian run tersebut dan jadwalkan ulang. |

**Prinsip:** Detect (Deteksi penurunan performa/crash) → Investigate (Identifikasi lapisan OSI/sumber gangguan) → Document (Catat pesan error/warning ke log) → Decide (Putuskan untuk perbaikan skrip atau pembersihan ulang lingkungan uji).

---

## Refleksi

> Pernahkah Anda melaporkan hasil riset/tugas dari single run? Apa risikonya? Bagaimana multiple run mengubah kepercayaan terhadap hasil?

**Pengalaman sebelumnya:**
> Ya, pada tugas-tugas konfigurasi jaringan praktikum berskala kecil sebelumnya, pengujian sering kali hanya dilakukan melalui satu kali pengamatan (single run). Segera setelah koneksi dinyatakan terhubung (Connected) dan uji ping memperlihatkan balasan paket, pengujian dianggap berhasil dan langsung didokumentasikan ke dalam laporan. Risikonya adalah mengabaikan faktor ketidakpastian (uncertainty) jaringan publik, fluktuasi interferensi spektrum nirkabel, dan masalah penumpukan beban antrean jangka panjang (bufferbloat) yang baru muncul ketika sistem dipaksa beroperasi di bawah beban puncak dalam kurun waktu yang lama.
**Yang akan dilakukan berbeda:**
> Pada riset ini, implementasi multiple run (10 kali pengulangan berdurasi masing-masing 10 menit untuk tiap skenario) bersifat mutlak dan tidak bisa ditawar. Dengan melakukan pengumpulan data berlapis ini, saya tidak lagi menampilkan angka performa jaringan tunggal yang rawan bias, melainkan menyajikan nilai rata-rata (mean) dan standar deviasi fluktuasi distribusi data. Langkah ini akan mengubah tingkat kepercayaan riset di mata penguji, karena pembuktian efisiensi algoritma Dynamic-Limit PCQ didasarkan pada data statistik yang konsisten, berulang (repeatable), serta bebas dari anomali kebetulan sesaat di laboratorium.
