# WS-05: Variabel & Metrik

> **Bab 5 — Metric, Measurement & Data**

---

## Ringkasan Materi

### Measurement Alignment Model

Setiap pengukuran yang valid harus bisa ditelusuri melalui rantai ini tanpa lompatan logis:

```
Problem → Concept → Variable → Metric → Data → Result
```

### Operationalization = Keputusan Desain

Menerjemahkan konsep abstrak menjadi variabel terukur bukan proses mekanis. "Code quality" yang diukur via SonarQube code smells membawa asumsi implisit. Setiap operasionalisasi harus didokumentasikan dan dijustifikasi.

### Empat Tipe Data (NOIR)

| Tipe | Ciri | Contoh | Operasi Valid |
|------|------|--------|---------------|
| **Nominal** | Kategori, tanpa urutan | Jenis algoritma (RF, SVM, CNN) | Modus, chi-square |
| **Ordinal** | Urutan, interval tidak sama | Skala Likert (1-5) | Median, Spearman |
| **Interval** | Jarak bermakna, tanpa nol absolut | Suhu Celsius | Mean, Pearson, t-test |
| **Ratio** | Jarak bermakna + nol absolut | Waktu eksekusi (ms) | Semua operasi |

Tipe data menentukan uji statistik yang valid. Kebanyakan metrik performa TI = ratio; persepsi pengguna = ordinal.

### Kriteria Pemilihan Metrik

- **Representative** — Mewakili konsep yang diteliti
- **Sensitive** — Cukup peka menangkap perbedaan bermakna (hindari ceiling effect)
- **Feasible** — Bisa dikumpulkan dalam batasan waktu dan biaya

### Pre-registration

Metrik harus ditentukan **sebelum** eksperimen. Memilih metrik setelah melihat data = **p-hacking**. Metrik tambahan yang ditemukan kemudian dilaporkan sebagai *exploratory*, bukan *confirmatory*.

### Primary vs Secondary Metric

- **Primary Metric** — Langsung terikat ke hipotesis, menentukan kesimpulan
- **Secondary Metric** — Pendukung, dilaporkan di samping primary; statusnya suplementer

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Pemilihan metrik | Berdasarkan kebiasaan/tool yang ada | Berdasarkan construct validity |
| Anomali | Dihapus untuk laporan bersih | Diinvestigasi — bisa jadi temuan |
| Kapan dipilih | Setelah sistem jadi (monitoring) | Sebelum eksperimen (by design) |

### Istilah Penting

- **Operationalization** — Transformasi konsep abstrak menjadi variabel terukur
- **Construct Validity** — Sejauh mana pengukuran benar-benar mengukur konsep yang dimaksud
- **Measurement Scale** — Klasifikasi data (NOIR) yang menentukan analisis valid
- **Multi-metric Evaluation** — Menggunakan beberapa metrik untuk menangkap konsep kompleks

---

## Template A.5 — Definisi Variabel, Metrik & Justifikasi

```
VARIABLE & METRIC DEFINITION

Research Question: Apakah penerapan algoritma Dynamic-Limit pada metode PCQ mampu menghasilkan Throughput yang lebih stabil dibandingkan PCQ statis saat jumlah pengguna meningkat dari 20 ke 50 user pada router Mikrotik?

| Variabel | Tipe | Konsep | Metrik | Skala | Satuan | Cara Mengukur | Justifikasi |
|----------|------|--------|--------|-------|--------|---------------|-------------|
| Metode Limitasi | IV   | Teknik pembagian bandwidth | Kategori: Statis vs Dynamic-Limit | Nominal | - | Mengubah konfigurasi pada menu Queue Mikrotik. | Untuk membandingkan cara lama dan cara baru yang ditawarkan |
| Kualitas Koneksi | DV   | Kecepatan data aktual | Average Throughput | Ratio | Mbps | Menggunakan alat ukur trafik (seperti Iperf atau Btools). | Metrik utama untuk membuktikan apakah internet benar-benar jadi cepat atau tidak |
| Responsivitas | DV   | Kelancaran akses | Latency (Ping) | Ratio | ms | Mengirim paket data uji ke server lokal. | Mengukur tingkat "lag" yang sering dikeluhkan saat WiFi ramai. |
| Kepadatan User | CV | Beban pengguna | Jumlah perangkat aktif | Ratio | User | Membatasi jumlah user yang terhubung di WiFi uji. | Memastikan perbandingan dilakukan pada kondisi beban yang adil dan sama. |
Alignment Check:
  RQ → Concept → Variable → Metric → Data → Result
  [ ] Setiap langkah terdokumentasi
  [ ] Tidak ada "lompatan logis"
  [ ] Metrik mengukur apa yang dimaksud (construct validity)
```

---

## Latihan 1 — Operationalization Chain

Gunakan RQ dari WS-04. Definisikan variabel dan metriknya.

**RQ:** __________________________________________________

| Variabel | Tipe | Konsep Abstrak | Metrik Konkret | Skala (NOIR) | Satuan |
|----------|------|---------------|----------------|-------------|--------|
| Metode Limitasi | IV | Pendekatan manajemen bandwith | kategori: PCQ Statis vs Dynamic-Limit PCQ | Nominal | — |
| Kualitas Jaringan | DV | Kecepatan transfer data nyata | Average Throughput | Ratio | Mbps |
| Beban Pengguna | CV | Intensitas kepadatan jaringan | Jumlah perangkat terhubung | Ratio | User |

**Apakah ada lompatan logis dalam rantai?** Tidak
> Jika ya, di mana? ____________________________________

---

## Latihan 2 — Evaluasi Metrik

Evaluasi metrik DV yang dipilih di Latihan 1 menggunakan 3 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Representative | 5 | Throughput mewakili kapasitas data aktual yang diterima pengguna, sesuai dengan keluhan lemot pada masalah awal. |
| Sensitive | 4 | Metrik ini sangat peka terhadap perubahan limit pada queue router dan kepadatan trafik. |
| Feasible | 5 | Data sangat mudah dikumpulkan melalui fitur Traffic Monitor bawaan Mikrotik atau tools seperti Btools dan Iperf. |

**Apakah perlu secondary metric?** Ya
> Jika ya, apa dan mengapa? Latency (ms) dan Jitter (ms). Karena internet terasa lambat bukan hanya karena kecepatan turun, tapi juga karena respons yang lama saat banyak pengguna.

**Contoh kasus ceiling effect untuk metrik ini:**
> Kondisi di mana total bandwidth dari ISP memang sudah habis terpakai (limit fisik), sehingga metode optimasi apa pun tidak akan menunjukkan kenaikan Throughput lagi.

---

## Latihan 3 — Data Quality Check

Bayangkan data yang akan dikumpulkan dari eksperimen. Evaluasi 4 dimensi kualitas data.

| Dimensi | Pertanyaan | Jawaban | Strategi Mitigasi |
|---------|-----------|---------|------------------|
| Completeness | *Apakah semua data point terkumpul?* | Ya, selama durasi pengujian (misal 10 menit per sesi). | Gunakan auto-logging untuk mencatat data per detik tanpa jeda. |
| Consistency | *Apakah ada kontradiksi internal?* | Mungkin ada lonjakan data tiba-tiba karena background update | Lakukan pengujian di lingkungan terkontrol dengan jenis trafik yang seragam. |
| Validity | *Apakah benar-benar mengukur yang dimaksud?* | Ya, Throughput mengukur kecepatan real. | Kalibrasi alat ukur dan pastikan tidak ada aplikasi lain yang berjalan di latar belakang perangkat uji |
| Representativeness | *Apakah sampel mewakili populasi target?* | Ya, simulasi 20-50 user mewakili kondisi cafe/kantor kecil. | Gunakan berbagai jenis perangkat (ponsel dan laptop) dalam pengujian. |

---

## Refleksi

> Mengapa memilih metrik setelah melihat data dianggap p-hacking? Apa bedanya dengan eksplorasi data yang sah?

**Jawaban:**
> Karena peneliti cenderung memilih metrik yang hanya menunjukkan hasil yang "bagus" atau signifikan secara statistik untuk mendukung hipotesisnya, meskipun metrik tersebut mungkin tidak relevan sejak awal, memilih metrik setelah melihat data dianggap sebagai p-hacking. Ini merusak integritas penelitian karena hasilnya menjadi bias.
> Ekplorasi data yang sah berbeda dari eksplorasi data yang sah dalam hal tujuan dan pelaporannya. Meskipun penelitian dilakukan untuk menemukan pola baru atau anomali yang tidak terduga, kesimpulan utama penelitian tetap harus didasarkan pada metrik awal.
