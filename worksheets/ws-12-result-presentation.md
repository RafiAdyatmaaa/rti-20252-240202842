# WS-12: Result Presentation & Visualization

> **Bab 12 — Penyajian Hasil & Visualisasi**

---

## Ringkasan Materi

### Data → Insight Model

```
Validated Data → Structured Presentation → Visualization → Pattern Recognition → Insight
```

Penyajian **mendahului** analisis. Tabel dan grafik membantu peneliti "melihat" data sebelum menghitung. Langsung ke uji statistik tanpa visualisasi berisiko kesimpulan yang secara teknis benar tapi kontekstual salah (Anscombe's Quartet, 1973).

### Tabel = Presisi, Grafik = Pola

Keduanya **saling melengkapi**:
- Tabel: angka presisi, self-contained (dipahami tanpa teks), sortable
- Grafik: pola visual, tren, perbandingan cepat

### Jenis Grafik Berdasarkan Tujuan

| Tujuan | Jenis Grafik |
|--------|-------------|
| Perbandingan antar-skenario | Bar chart (grouped/stacked) |
| Distribusi per-skenario | Box plot / violin plot |
| Tren temporal | Line chart |
| Korelasi dua variabel | Scatter plot |
| Proporsi (total = 100%) | Pie chart (hati-hati!) |

### Contoh Tabel Hasil yang Baik

| Model | Accuracy (%) | F1-Score (%) | Training Time (min) |
|-------|-------------|-------------|---------------------|
| BERT | 88.4 ± 1.2 | 87.1 ± 1.4 | 45.2 ± 3.1 |
| LSTM | 86.1 ± 1.8 | 84.5 ± 2.0 | 12.8 ± 1.2 |
| SVM | 82.3 ± 0.9 | 80.7 ± 1.1 | 0.3 ± 0.1 |

*N=10 per model. Mean ± std. Diurutkan berdasarkan Accuracy.*

### Visualization Bias — Yang Harus Dihindari

| Bias | Deskripsi | Dampak |
|------|----------|--------|
| Truncated axis | Y tidak dari 0 | Memperbesar perbedaan kecil |
| Inconsistent scale | Dua grafik skala beda | Perbandingan menyesatkan |
| Cherry-picked data | Hanya tampilkan yang "menang" | Selektif, tidak jujur |
| 3D effects | Efek 3D tanpa dimensi data ke-3 | Distorsi tanpa informasi |
| Missing error bar | Tidak ada variabilitas | Menyembunyikan ketidakpastian |

### Engineering vs Research Presentation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan grafik | Dashboard monitoring | Mendukung argumen ilmiah |
| Informasi wajib | KPI, threshold | Mean, std, CI, N, p-value |
| Bias handling | Less critical | Wajib dihindari (peer-review) |

---

## Template A.12 — Result Presentation Plan

```
RESULT PRESENTATION PLAN

Research Question : Apakah penerapan algoritma Dynamic-Limit pada metode PCQ mampu 
                    menghasilkan nilai Throughput yang lebih stabil secara signifikan 
                    daripada metode PCQ statis saat jumlah pengguna meningkat 
                    dari 20 menjadi 50 user?
Metrik Utama      : Global Throughput (Mbps) dan Latency (ms)

Tabel Hasil:
| Skenario | Metrik 1 (mean ± std) | Metrik 2 (mean ± std) | n |
|----------|----------------------|----------------------|---|
| Baseline (PCQ Statis) | 45.3 ± 4.1 Mbps | 148.5 ± 24.2 ms | 10 |
| Intervensi (Dyn-PCQ) | 49.2 ± 0.6 Mbps | 41.2 ± 3.8 ms | 10 |

Visualisasi yang Direncanakan:
| # | Jenis Grafik | Pesan Utama | Metrik |
|---|-------------|-------------|--------|
| 1 | Line Chart | Tren stabilitas data saat beban user naik bertahap | Throughput vs Time |
| 2 | Box Plot | Perbandingan sebaran dan rentang fluktuasi latency | Latency Distribution |

Bias Check:
  [x] Y-axis mulai dari 0 (atau dijustifikasi)
  [x] Error bar/CI ditampilkan
  [x] Semua data disertakan (tidak cherry-picked)
  [x] Tidak menggunakan 3D tanpa alasan
```

---

## Latihan 1 — Tabel Hasil

Buat tabel hasil eksperimen Anda (boleh dengan data simulasi jika belum punya data riil).

| Skenario | Global Throughput (mean ± std Mbps) | Latency (mean ± std ms) | Router CPU Load (mean ± std %) | n |
|----------|----------------------|----------------------|----------------------|---|
| Intervensi (Dyn-PCQ) | 49.2 ± 0.6 Mbps | 41.2 ± 3.8 ms | 76.4 ± 3.1 % | 10 |
| Baseline (PCQ Statis) | 45.3 ± 4.1 Mbps | 148.5 ± 24.2 ms | 12.3 ± 1.2 % | 10 |

**Checklist tabel:**
- [x] Self-contained (Judul: Tabel Perbandingan Performa QoS pada Kondisi Beban Puncak 50 User, Satuan Mbps/ms/% ada, N=10 tercantum)
- [x] Mean ± std (Ditampilkan lengkap untuk melihat stabilitas deviasi data)
- [x] Diurutkan berdasarkan metrik utama (Intervensi ditaruh di atas karena metrik Throughput-nya paling tinggi dan stabil)
- [x] Format konsisten di semua baris

---

## Latihan 2 — Rencana Visualisasi

Rencanakan 2-3 grafik untuk menyajikan data dari Latihan 1. Setiap grafik = satu pesan.

| # | Jenis Grafik | Pesan | Data yang Digunakan |
|---|-------------|-------|---------------------|
| 1 | Line Chart (Temporal) | Memperlihatkan penurunan throughput yang tajam pada baseline dibanding kestabilan throughput intervensi saat durasi uji berjalan dan beban user diinjeksi bertahap. | Nilai Global Throughput (Mbps) per 5 detik sepanjang total durasi 10 menit uji. |
| 2 | Box & Whisker Plot | Menunjukkan rentang sebaran latency klien. Baseline akan memiliki kotak yang panjang (fluktuatif/bufferbloat), sementara intervensi memiliki kotak yang rapat dan rendah (stabil). | Seluruh sebaran data poin Latency (ms) dari Run 1 hingga Run 20. |
| 3 | Grouped Bar Chart + Error Bar | Memperlihatkan kontras rata-rata QoS global secara langsung antar-skenario beserta variabilitas ketidakpastian datanya. | Nilai Mean ± Standard Deviation dari metrik Throughput dan Latency. |

---

## Latihan 3 — Bias Detection

Evaluasi visualisasi berikut untuk bias (skenario dari contoh):

**Skenario:** Metode A = 91.2%, Metode B = 90.8%. Bar chart dengan Y-axis mulai dari 90%.

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah Y-axis menyesatkan? | Itu benar. Tiang grafik Metode A akan terlihat berlipat-lipat lebih tinggi daripada Metode B jika pemotongan sumbu dari 44 Mbps dilakukan. Ini menciptakan ilusi bahwa Metode A memiliki kinerja yang lebih baik, meskipun sebenarnya selisih asli secara objektif adalah 3.9 Mbps. |
| Apakah error bar ditampilkan? | Tidak. Grafik yang bias sering menyembunyikan error bar untuk menutupi nilai fluktuasi besar pada skenario baseline (PCQ Statis). |
| Apakah semua kondisi ditampilkan? | Ya, semua skenario ditampilkan. Namun, karena modifikasi skala sumbu, tampilannya berubah. |
| Apa solusinya? | Mengembalikan sumbu Y-axis agar wajib dimulai dari angka 0 Mbps, serta menyematkan error bar di atas setiap tiang bar chart untuk menunjukkan variabilitas data secara jujur dan transparan kepada peer-reviewer. |

**Evaluasi grafik Anda sendiri dari Latihan 2:**
- [x] Semua bias check lulus
- [x] Ada yang perlu diperbaiki: Tidak ada. Seluruh sumbu Y pada Line Chart dan Bar Chart dikunci dari nilai 0, dan grafik Box Plot digunakan secara murni untuk memvisualisasikan data pencilan tanpa rekayasa skala.

---

## Refleksi

> Mengapa tabel dan grafik keduanya diperlukan — tidak cukup salah satu saja? Pernahkah Anda membuat grafik yang (tanpa sengaja) menyesatkan?

> Dalam penulisan laporan ilmiah, tabel dan grafik saling melengkapi. Untuk perhitungan uji statistik T-Test, tabel harus menunjukkan presisi angka yang absolut, detail, dan lengkap. Ini termasuk nilai pasti dari rata-rata (mean) dan standar deviasi hingga beberapa angka di belakang koma yang penting. Namun, hanya melihat kumpulan angka dalam tabel membuat sulit bagi orang untuk memahami pola atau tren perilaku data. Dalam hal ini, grafik berfungsi untuk memberikan gambaran instan tentang bagaimana karakteristik bufferbloat mulai terbentuk seiring bertambahnya jumlah pengguna dan waktu. Pembaca akan kehilangan data yang tepat atau pemahaman pola makro riset jika hanya ada salah satu.
> Saat mengerjakan tugas praktikum jaringan sebelumnya, saya pernah membuat grafik yang menyesatkan secara tidak sengaja. Saya menggunakan grafik garis bawaan Microsoft Excel tanpa menyamakan skala sumbu Y antara grafik pengujian hari pertama dan kedua. Karena fungsi skala otomatis memperbesar skala grafik, grafik dengan fluktuasi latency yang kecil terlihat sama dengan grafik dengan fluktuasi besar. Saya belajar dari modul WS-12 ini bahwa untuk menjaga integritas dan kejujuran ilmiah suatu penelitian, standardisasi skala, penggunaan sumbu angka 0 dan penyertaan error bar adalah hal-hal yang sangat penting.
