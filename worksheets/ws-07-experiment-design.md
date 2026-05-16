# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

```
EXPERIMENT DESIGN

Research Question : Apakah penerapan algoritma Dynamic-Limit pada metode Per Connection Queue (PCQ) mampu menghasilkan nilai Throughput yang lebih stabil dibandingkan PCQ statis saat jumlah pengguna aktif meningkat dari 20 menjadi 50 user pada perangkat Mikrotik?
Hypothesis        : H0: Saat jumlah pengguna meningkat, nilai throughput PCQ dengan batas dinamis tidak berubah secara signifikan dari PCQ statis.
                    H1: Dalam kondisi padat, metode PCQ dengan Batas Dinamis menghasilkan throughput rata-rata yang lebih besar secara signifikan (sekurang-kurangnya 20% lebih baik) dibandingkan dengan PCQ statis.
Tipe Eksperimen   : Comparison

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Skenario baseline menggunakan konfigurasi standar | PCQ Statis | Router RB750Gr3, BW 50Mbps, 20 & 50 User, Traffic: Video & Download |
| Treatment | Skenario dengan implementasi algoritma usulan | Dynamic-Limit PCQ | Router RB750Gr3, BW 50Mbps, 20 & 50 User, Traffic: Video & Download |

Fairness Checklist:
  [ ] Dataset identik untuk semua kondisi (Tipe file dan durasi akses disamakan).
  [ ] Preprocessing setara (Konfigurasi Firewall dan Mangle tidak diubah).
  [ ] Tuning effort setara (Baseline dikonfigurasi secara optimal sesuai standar admin).
  [ ] Environment identik (Jam pengujian dan frekuensi WiFi yang sama).
  [ ] Metrik evaluasi sama (Sama-sama menggunakan Throughput dan Latency).

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    | Background update otomatis pada perangkat user yang mengganggu data | Mematikan fitur auto-update di setiap perangkat klien sebelum running test |
| External    | Hasil mungkin berbeda jika dijalankan di router merek lain | Menjelaskan keterbatasan riset bahwa solusi ini spesifik untuk RouterOS (Mikrotik) |
| Construct   | Metrik Throughput tunggal tidak menggambarkan kepuasan user | Menambahkan metrik Latency dan Jitter untuk mengukur stabilitas koneksi secara utuh |
| Conclusion  | Variasi data tinggi karena fluktuasi ISP dari pusat | Melakukan pengulangan pengujian (Running Test) sebanyak 10 kali untuk tiap skenario |

Statistical Plan:
  Uji statistik   : Independent Samples T-Test (atau Wilcoxon jika data tidak normal).
  Justifikasi      : Untuk membandingkan rata-rata performa dua metode yang berbeda secara signifikan.
  Alpha            : 0.05 (Tingkat kepercayaan 95%).
  Effect size min  : 0.5 (Medium effect).
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Apakah penerapan algoritma Dynamic-Limit pada metode PCQ mampu menghasilkan nilai Throughput yang lebih stabil dibandingkan PCQ statis saat jumlah pengguna aktif meningkat dari 20 menjadi 50 user pada perangkat Mikrotik?
**Tipe eksperimen:** [ ] Comparison (Membandingkan metode lama vs metode baru)

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Menggunakan settingan standard PCQ yang biasa dipakai di cafe/sekolah. | PCQ Statis | Router RB750Gr3, Bandwidth 50Mbps, Suhu ruang 25°C, Beban 20 & 50 user |
| Treatment | Menggunakan algoritma optimasi otomatis yang di rancang. | Dynamic-Limit PCQ | Router RB750Gr3, Bandwidth 50Mbps, Suhu ruang 25°C, Beban 20 & 50 user |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅ | Dua-duanya diuji pakai beban trafik yang sama (simulasi streaming HD & download file). |
| Preprocessing setara | ✅ | Semua settingan firewall dan routing di router disamain persis sebelum ganti metode antrian. |
| Tuning effort setara | ✅ | Parameter PCQ di metode statis udah dioptimalkan dulu biar nggak cupu-cupu banget pas dibandingin. |
| Environment identik | ✅ | Pengujian dilakukan di jam yang sama buat hindari gangguan frekuensi dari WiFi tetangga. |
| Metrik evaluasi sama | ✅ | Sama-sama ngukur Throughput (Mbps) dan Latency (ms) pakai software yang sama. |

**Ada yang tidak fair?** Tidak
> Semuanya udah dikunci (CV), yang beda cuma satu: algoritma limitasinya doang (IV). Jadi kalau ada beda hasil, itu fix karena algoritmanya.

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | Ada intervensi trafik dari aplikasi latar belakang (update Windows/HP). | Matiin fitur auto-update di semua perangkat klien sebelum pengujian dimulai. |
| External | Hasilnya mungkin cuma jalan di Mikrotik, nggak di merek lain (TP-Link/Cisco). | Jujur di laporan kalau riset ini spesifik buat RouterOS, tapi logikanya bisa diadaptasi. |
| Construct | Metrik Throughput rata-rata nggak nunjukin "penderitaan" user yang kena lag parah. | Tambahin metrik Latency (Ping) biar ketauan kalau koneksinya emang beneran stabil, bukan cuma kenceng |
| Conclusion | Pengulangan tes cuma dikit, takutnya hasilnya cuma kebetulan pas lagi lancar. | Lakuin running tes minimal 10-20 kali per kondisi buat ambil rata-rata statistik yang valid. |

**Ancaman mana yang paling sulit dimitigasi?** External Validity.
**Mengapa?**
> Karena spek hardware tiap router beda-beda. Apa yang lancar di Mikrotik seri tinggi belum tentu bisa jalan mulus di seri yang memorinya kecil banget.

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Dikasih perlawanan nggak baselinenya? Jangan-jangan pembandingnya nggak di-tuning (pakai default doang) dibanding metode punya penulis.
2. Environment-nya adil? Dataset yang digunakan hanyalah dataset "titipan" yang sangat cocok dengan algoritmanya sendiri?
3. Hasilnya kecil atau signifikan secara statistik? Jika ada perbedaan hanya 1-2% tanpa uji statistik, itu mungkin hanya faktor hoki saat pengujian.
