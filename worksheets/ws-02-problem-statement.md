# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

---

## Ringkasan Materi

### Problem Formation Model

Masalah riset melewati 5 tahap transformasi. Melompat langsung dari Reality ke Variable adalah kesalahan paling umum.

```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : Jaringan Komputer / Infrastruktur Telekomunikasi
  Konteks  : Area publik atau perkantoran dengan kepadatan pengguna tinggi (High-Density WLAN)

System Context
  Input       : Jumlah perangkat terhubung (STAs), volume trafik data, konfigurasi kanal radio
  Process     : Alokasi bandwidth, manajemen antrian paket (queueing), mekanisme CSMA/CA
  Output      : Transmisi paket data ke perangkat pengguna
  Outcome     : Stabilitas kecepatan internet dan kepuasan pengguna (User Experience)
  Constraints : Kapasitas transmisi AP terbatas, interferensi sinyal, lebar pita ISP
  Stakeholders: Administrator Jaringan, Pengguna WiFi, Penyedia Layanan Internet

Fenomena → Problem
  Fenomena yang diamati             : Penurunan performa internet secara drastis saat jumlah pengguna meningkat
  Gejala (symptom) yang terukur     : Latency melonjak (>200ms), throughput turun di bawah 1 Mbps, packet loss meningkat
  Masalah yang didiagnosis          : Ketidakmampuan algoritma alokasi bandwidth statis dalam menangani kepadatan trafik (kongesti)
  Masalah riset (researchable)      : Efektivitas penerapan metode alokasi dinamis/load balancing untuk menjaga stabilitas QoS pada jaringan padat
  Variabel yang terukur             : Throughput, Latency, Jitter, dan Fairness Index

Problem Quality Check
  [x] Clarity — Apakah satu orang membaca akan paham? Sangat Jelas. Masalah ini mudah dipahami karena merupakan fenomena umum. Fokusnya spesifik: korelasi antara jumlah pengguna (density) dengan penurunan kinerja (performance degradation).
  [x] Measurability — Apakah ada metrik kuantitatif? Sangat Terukur. Masalah ini memiliki metrik kuantitatif yang baku dalam standar IEEE 802.11, seperti Throughput (bps), Latency (ms), dan Packet Loss Rate (%).
  [x] Relevance — Apakah penting untuk domain? Sangat Relevan. Di bidang TI, efisiensi spektrum dan manajemen kepadatan adalah tantangan utama, terutama untuk implementasi Smart City, IoT, dan kampus digital.
  [x] Testability — Apakah bisa gagal? Dapat Diuji. Hipotesis penelitian ini bisa gagal. Misalnya, jika setelah dipasang algoritma baru ternyata latency tetap tinggi, berarti ada faktor lain (seperti interferensi fisik) yang lebih dominan.
  [x] Impact — Apakah ada kontribusi jika terjawab? Kontribusi Tinggi. Jika terjawab, penelitian ini bisa menjadi acuan bagi Admin Jaringan untuk mengatur konfigurasi Access Point agar lebih adil (fairness) bagi semua pengguna.

Problem Statement (1 paragraf):
  Meskipun ketersediaan akses WiFi di area publik meningkat, performa jaringan sering kali mengalami degradasi signifikan saat jumlah pengguna aktif bertambah, yang ditandai dengan lonjakan latency dan penurunan throughput yang drastis. Masalah ini berakar pada manajemen sumber daya radio dan alokasi bandwidth yang masih bersifat statis, sehingga tidak mampu beradaptasi terhadap fluktuasi beban trafik yang tinggi (kongesti). Penelitian ini bertujuan untuk menganalisis dan mengoptimalkan mekanisme alokasi sumber daya dinamis guna meningkatkan stabilitas Quality of Service (QoS), sehingga pengalaman pengguna tetap konsisten meskipun dalam kondisi kepadatan jaringan yang ekstrim.

```

---

## Latihan 1 — Dari Topik ke Masalah Riset

Pilih satu topik di bidang TI yang diminati. Transformasikan melalui 5 tahap Problem Formation Model.

**Topik awal:** Optimasi performa jaringan WiFi pada area dengan kepadatan pengguna tinggi.

| Tahap | Hasil |
|-------|-------|
| Reality | *Pengguna WiFi di area publik (kantor/kampus) sering mengeluhkan koneksi lambat saat jam sibuk.* |
| Observed Issue (Symptom) | *Penurunan throughput hingga di bawah 1 Mbps dan latency (ping) melonjak di atas 200ms saat user aktif > 50 orang.* |
| Diagnosed Problem (Root Cause) | Terjadinya packet collision yang tinggi dan manajemen alokasi bandwidth yang tidak dinamis (statis) |
| Researchable Problem | Bagaimana pengaruh penerapan algoritma Dynamic Bandwidth Allocation dalam menurunkan tingkat packet loss pada jaringan padat? |
| Measurable Variable | Throughput, Latency, Jitter, dan Packet Loss Ratio |

**Apakah terjebak solution-first thinking?** Tidak
> Jika ya, kembali ke tahap mana? ________________________

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | *Jumlah perangkat terhubung, permintaan data (HTTP/Streaming), konfigurasi SSID* |
| Process | Otentikasi pengguna, routing paket data, manajemen antrian (queueing), transmisi sinyal radio |
| Output | Akses internet ke perangkat, data yang berhasil terkirim |
| Outcome | Kecepatan akses yang stabil dan pengalaman pengguna yang lancar (User Experience) |
| Constraints | Kapasitas Backhaul ISP terbatas, interferensi sinyal frekuensi 2.4GHz/5GHz, radius jangkauan AP |
| Stakeholders | Network Administrator, Pengguna WiFi, Penyedia Layanan Internet (ISP) |

**Komponen mana yang paling relevan dengan masalah riset?** (terutama pada bagian manajemen antrian dan alokasi sumber daya).

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 5 | Sangat jelas mengacu pada masalah performa akibat kepadatan |
| Measurability | 5 | Menggunakan metrik standar QoS (Quality of Service) yang bisa dihitung |
| Relevance | 5 | Masalah nyata di era IoT dan high-density networking |
| Testability | 4 | Bisa diuji melalui simulasi (NS-3/OMNeT++) atau testbed langsung |
| Impact | 4 | Memberikan solusi bagi penyedia layanan publik untuk meningkatkan kepuasan pengguna |

**Skor total:** 23 / 25

**Problem statement versi final (1 paragraf):**
> Peningkatan jumlah pengguna pada jaringan WiFi publik seringkali menyebabkan degradasi performa yang signifikan, ditandai dengan lonjakan latency di atas 200ms dan penurunan throughput yang drastis. Masalah ini berakar pada ketidakmampuan sistem manajemen bandwidth statis dalam menangani lonjakan trafik yang fluktuatif, sehingga memicu congestion dan tabrakan paket. Riset ini bertujuan untuk menganalisis efektivitas algoritma alokasi dinamis dalam mengoptimalkan Quality of Service (QoS) guna memastikan stabilitas koneksi meskipun dalam kondisi kepadatan pengguna yang tinggi.

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah Coding (Bug/Error): Bersifat deterministik. Ada aturan yang dilanggar (sintaks atau logika), solusinya adalah "perbaikan" agar sistem berjalan sesuai spesifikasi yang sudah ada. Tujuannya adalah fungsionalitas.
> Masalah Riset: Bersifat probabilistik dan eksploratif. Tidak ada jawaban "benar/salah" yang instan. Fokusnya bukan sekadar membenarkan yang rusak, melainkan mencari pemahaman baru, membandingkan metode, atau mengoptimalkan efisiensi dari sistem yang sebenarnya sudah "jalan" tapi belum maksimal. Tujuannya adalah pengembangan ilmu atau optimasi.
