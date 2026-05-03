# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  : ____________________

Research Question:
  Tipe         : [ ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : ____________________
  Variabel IV  : ____________________
  Variabel DV  : ____________________
  Metrik       : ____________________
  Dataset      : ____________________
  Baseline     : ____________________

Quality Check RQ:
  [ ] Variabel spesifik
  [ ] Metrik jelas
  [ ] Baseline ada
  [ ] Konteks disebutkan
  [ ] Memerlukan eksperimen (bukan hanya survei literatur)

Contribution Statement:
  Apa yang baru diketahui : ____________________
  Jenis kontribusi        : [ ] Improvement  [ ] Comparison  [ ] Novel approach
  Gap yang diisi          : ____________________

Hypothesis Pair:
  H₀ : ____________________
  H₁ : ____________________
  Threshold              : ____________________
  Justifikasi threshold  : ____________________
```

---

## Latihan 1 — Dari Gap ke RQ

Gunakan gap yang ditemukan di WS-03. Transformasikan menjadi Research Question.

**Gap dari WS-03:** Kurangnya mekanisme pembagian bandwidth yang adaptif (otomatis) terhadap jumlah pengguna aktif secara real-time pada perangkat jaringan menengah ke bawah.

**RQ versi pertama (tulis bebas):**
> "Apakah penerapan algoritma Dynamic-Limit pada metode Per Connection Queue (PCQ) mampu menghasilkan nilai Throughput yang lebih stabil dibandingkan PCQ statis saat jumlah pengguna aktif meningkat dari 20 menjadi 50 user pada perangkat Mikrotik?"

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | Ya | Algoritma Dynamic-Limit pada PCQ |
| Metrik terukur | Ya | Throughput (Mbps) |
| Baseline | Ya | PCQ Statis (konvensional) |
| Dataset/konteks | Ya | Jaringan Mikrotik dengan beban 20-50 user |

**Tipe RQ:** Improvement

**RQ versi revisi (setelah evaluasi):**
> ___________________________________________________

---

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | Tidak ada perbedaan signifikan pada nilai Throughput antara PCQ dengan Dynamic-Limit dibandingkan PCQ statis saat kepadatan pengguna meningkat. |
| H₁ | Metode PCQ dengan Dynamic-Limit menghasilkan Throughput rata-rata yang lebih tinggi secara signifikan (minimal 20% lebih baik) dibandingkan PCQ statis pada kondisi padat. |
| Metrik | Average Throughput (Mbps) dan Jitter (ms). |
| Threshold | P-Value < 0.05 (Uji T atau Wilcoxon). |
| Justifikasi threshold | Batas standar signifikansi statistik untuk menolak hipotesis nol dalam riset rekayasa jaringan. |

**Apakah hipotesis ini falsifiable?** [ ] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? ___________________

---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | Perbandingan efektivitas Dynamic-Limit PCQ vs PCQ Statis terhadap stabilitas Throughput. |
| Variable (IV) | Pengaturan limitasi bandwidth (Statis vs Dinamis/Adaptif). |
| Variable (DV) | Kualitas jaringan (Throughput dan Latency). |
| Metric | Megabits per second (Mbps) dan milidetik (ms). |
| Data source | Pengujian langsung (Live Test) pada Access Point menggunakan perangkat lunak traffic generator. |
| Analysis method | Analisis deskriptif (tabel/grafik) dan uji beda statistik (T-Test) untuk melihat konsistensi performa. |

**Apakah rantai lengkap?** Ya
> Jika tidak, tahap mana yang perlu direvisi? ______________

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?

**Judul:** Analisis QoS pada Jaringan WiFi Menggunakan Metode PCQ di Cafe X.
**RQ yang diekstrak:** Bagaimana pengaruh penerapan metode PCQ terhadap kecepatan internet pengguna di Cafe X?
**Komponen yang hilang:** RQ tersebut tidak menyebutkan metrik spesifik (apa yang diukur?), tidak ada baseline (dibandingkan dengan apa?), dan tidak menyebutkan target beban pengguna yang jelas. RQ ini cenderung seperti pertanyaan survei biasa, bukan riset eksperimental yang mendalam.
