# WS-09: Implementation & Environment

> **Bab 9 — Implementasi Riset & Kontrol Lingkungan**

---

## Ringkasan Materi

### Implementasi Riset ≠ Coding Biasa

Tujuan implementasi riset bukan membuat software yang berfungsi, melainkan membangun **instrumen pengukuran yang konsisten**. Setiap modul harus di-mapping ke variabel (dari Bab 6), parameter harus config-driven, dan logging aktif dari hari pertama.

### Reproducible Implementation Model

```
Design → Implementation → Environment Setup → Execution Consistency → Reproducibility → Trustworthy Result
```

Setiap transisi memiliki syarat:
- Design → Implementation: kode sesuai mapping variabel-ke-komponen
- Implementation → Environment: versi, dependency, seed, path, OS eksplisit
- Environment → Consistency: seed terkunci, urutan deterministik
- Consistency → Reproducibility: dokumentasi lengkap
- Reproducibility → Trust: siapa pun ikuti dokumentasi → hasil sama/serupa

### Repeatability vs Reproducibility

| Level | Peneliti | Environment | Hasil |
|-------|---------|-------------|-------|
| **Repeatability** | Sama | Sama | Sama persis |
| **Reproducibility** | Berbeda | Berbeda (ikuti docs) | Sama/serupa |

Capai **repeatability** dulu, baru **reproducibility**.

### Engineering vs Research Perspective

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Sistem berfungsi untuk user | Instrumen pengukuran konsisten |
| Dependency | Update ke terbaru | Lock di versi spesifik |
| Testing | Unit, integration, E2E | Repeatability test (run ulang → sama?) |
| Dokumentasi | User guide, API docs | Environment spec, execution steps, expected output |
| Config | Default masuk akal | Setiap parameter eksplisit & adjustable |

### Jebakan Kognitif

1. Menunda environment setup → bug sulit dilacak
2. Tidak pakai version control → hasil tidak bisa direkonstruksi
3. Menolak Docker/container → "di laptop saya bisa" saat review
4. 3× hasil sama ≠ repeatable (bisa cache/state tersimpan)

### Istilah Penting

- **Environment Specification** — Deskripsi lengkap: hardware, OS, runtime, library + versi, config, seed
- **Dependency** — Komponen eksternal yang harus di-lock versinya
- **Config-driven** — Parameter dieksternalisasi ke file konfigurasi, bukan hardcode

---

## Template A.9 — Dokumentasi Setup Eksperimen

```
EXPERIMENT SETUP DOCUMENTATION

Hardware:
  CPU     : AMD Ryzen 7 7435HS
  RAM     : 16 GB DDR5
  GPU     : AMD Radeon RX 7600S / 7700S
  Storage : 512 GB

Software:
  OS        : MikroTik RouterOS v7.12 Stable (Long-term)
  Runtime   : RouterOS Scripting Engine v7
  Framework : Winbox Management UI / RouterOS CLI Environment

Dependencies:
| Library | Version | Sumber | Hash/Checksum |
|---------|---------|--------|---------------|
| system package | 7.12 | Official MikroTik       | Bundled system package |
| wireless package | 7.12 | Official MikroTik       | Bundled legacy wireless |
| dhcp package | 7.12 | Official MikroTik       | Bundled network stack |
| MikroTik Btest | 2.0 | Winbox Utility       | Standalone application |
| Terminal CLI | 7.12 | RouterOS Core       | Native Environment |

Konfigurasi:
  Config file     : dynamic_pcq_setup.rsc (RouterOS Script & Configuration File)
  Random seed     : Not Applicable (Deterministic network routing & static time scheduler)
  Hyperparameters : totalBandwidth=50Mbps, minRate=512Kbps, evaluationInterval=5s

Reproducibility Check:
  [x] Dependency terdokumentasi (requirements.txt / lock file)
  [x] Seed ditetapkan di semua level (Python, NumPy, framework)
  [x] Config di version control
  [x] README instruksi reproduksi lengkap
```

---

## Latihan 1 — Environment Specification

Dokumentasikan environment untuk eksperimen Anda (boleh environment saat ini atau yang direncanakan).

| Komponen | Spesifikasi |
|----------|------------|
| CPU | AMD Ryzen 7 7435HS |
| RAM | 16 GB DDR5 |
| GPU | NVIDIA RTX 4050 |
| OS | Windows 11 Home |
| Runtime | RouterOS Scripting Interpreter Engine |
| Framework | MikroTik RouterOS Queue Management Core (PCQ Sub-system) |
| Random Seed | Not Applicable (Algoritma penjadwalan eksekusi bersifat waktu-deterministik) |

**Dependencies (minimal 5):**

| Library | Version | Alasan Dibutuhkan |
|---------|---------|-------------------|
| System RouterOS Base | 7.12 | Layanan sistem operasi inti, alokasi memori, manajemen antrean, dan perutean IP data paket. |
| Wireless Package | 7.12 | Menangkap parameter status stasiun terhubung aktif (/interface wireless registration-table). |
| DHCP Stack | 7.12 | Alokasi tabel pemetaan alamat IP dinamis ke alamat MAC fisik perangkat klien. |
| Traffic Monitor Core | 7.12 | Modul trigger performa untuk memantau volume batas atas bandwidth real-time. |
| MikroTik Bandwidth Test (Btest) | 2.0 | Aplikasi eksternal traffic generator untuk menginjeksi beban paket data TCP/UDP tiruan. |

---

## Latihan 2 — Repeatability Test Plan

Rancang tes repeatability sederhana: jalankan kode yang sama 3× di environment yang sama.

| Run | Seed | Metrik Utama | Hasil Sama? |
|-----|------|-------------|-------------|
| 1 | N/A | Global Average Throughput | — |
| 2 | N/A | Global Average Throughput | [x] Ya / [ ] Tidak (Margin error fluktuasi < 1%) |
| 3 | N/A | Global Average Throughput | [x] Ya / [ ] Tidak (Margin error fluktuasi < 1%) |

**Jika hasil berbeda, kemungkinan penyebab:**
> Terjadinya fluktuasi bandwidth suplai utama dari penyedia layanan internet (ISP upstream), adanya interferensi sinyal frekuensi nirkabel (cross-talk atau co-channel interference) dari jaringan luar di sekitar laboratorium selama durasi pengujian, atau lonjakan beban kerja CPU router akibat proses latar belakang sistem OS (logging timer).

**Checklist kontrol yang sudah diterapkan:**
- [x] Random seed di-set di semua level (N/A; logika bersifat deterministic time-driven)
- [x] Tidak ada background process yang mengganggu (Layanan DNS caching, Torch, dan Graphing dinonaktifkan)
- [x] Cache dibersihkan antar-run (Statistik counter queue dan active wireless station dibersihkan total sebelum running test baru)
- [x] Config file yang sama untuk semua run (Menggunakan file script import .rsc yang sama)

---

## Latihan 3 — README Eksperimen

Tulis README minimum untuk eksperimen Anda (6 komponen wajib).

```
# Judul Eksperimen: ____________________

## 1. Environment
> (Salin spesifikasi dari Latihan 1)
- Hardware: Perangkat Utama MikroTik RB750Gr3 (1 Unit) + Dedicated Access Point (1 Unit)
- Perangkat Klien: Minimal 20 hingga maksimal 50 stasiun nirkabel aktif (PC/Smartphone Simulator)
- OS Versi: MikroTik RouterOS v7.12 Stable
- Bandwidth Input: 50 Mbps ter-limit statis pada sisi upstream gateway internet

## 2. Installation
> (Langkah instalasi, misal: "pip install -r requirements.txt")
1. Hubungkan port ether1 router uji ke upstream internet gateway (50 Mbps).
2. Hubungkan port ether2 router uji ke port LAN Access Point distribusi nirkabel.
3. Buka Winbox, buka menu Terminal, lalu impor konfigurasi awal infrastruktur:
   /import file-name=dynamic_pcq_setup.rsc

## 3. Data
> (Deskripsi data: sumber, format, ukuran)
- Sumber Data: Modul /interface wireless registration-table (Input) dan /queue tree (Output).
- Format Data: Berkas ekspor log teks terkompresi atau berkas log performa (.csv).
- Ukuran Data: Sampel data performa berkala dengan resolusi pencatatan log per 5 detik sepanjang durasi uji total 10 menit.

## 4. Execution
> (Command untuk menjalankan eksperimen)
1. Jalankan pengujian Skenario Baseline (Kondisi A):
   Aktifkan antrean PCQ Statis tanpa mengaktifkan script otomasi scheduler, jalankan traffic generator Btest.
2. Jalankan pengujian Skenario Intervensi (Kondisi B):
   Unggah skrip dynamic_limit, lalu aktifkan skrip otomasi scheduler pada terminal CLI Winbox:
   /system scheduler add interval=5s name=Run_Dynamic_Limit script="/system script run dynamic_pcq_script"

## 5. Configuration
> (File config yang digunakan + parameter kunci)
- Berkas Konfigurasi Utama: dynamic_pcq_setup.rsc
- Parameter Kunci di dalam Skrip:
  * totalBandwidth = 50000000 (Representasi kapasitas total pipa data internet: 50 Mbps)
  * minRate = 512000 (Representasi ambang batas alokasi kecepatan minimum per user: 512 Kbps)
  * Interval Scheduler = 5s (Frekuensi eksekusi perhitungan ulang alokasi bandwidth)

## 6. Expected Output
> (Contoh output yang diharapkan + format)
Berkas log .csv terstruktur yang secara konsisten mencatat:
- Timestamp | Jumlah User Aktif | Nilai pcq-rate Otomatis | Total Throughput (Mbps) | Latency (ms)
- Contoh Output Kondisi Padat Stabil: "12:00:05 | 40 user | pcq-rate=1.25Mbps | 49.2 Mbps | 45 ms"

```

---

## Refleksi

> Apakah eksperimen Anda saat ini bisa direproduksi oleh orang lain tanpa bantuan Anda? Komponen apa yang masih hilang?

**Level saat ini:** [x] Repeatability / [ ] Reproducibility / [ ] Belum keduanya
**Komponen yang belum terdokumentasi:**
> Eksperimen saat ini berada pada tingkat Repeatability yang sangat solid karena pengujian di lingkungan laboratorium terkontrol sudah dikunci ketat menggunakan konfigurasi yang config-driven. Komponen yang masih belum terdokumentasi lengkap menuju tingkat Reproducibility yang sempurna adalah standarisasi profil spesifikasi teknis dari alat pembangkit trafik pihak ketiga (traffic generator). Diperlukan panduan dokumentasi tambahan apabila peneliti lain ingin beralih menggunakan platform berbasis Linux (seperti iPerf3) untuk memastikan profil beban paket data yang disuntikkan terbukti ekuivalen dengan aplikasi Btest bawaan MikroTik.
