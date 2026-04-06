# WS-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (artefak dibuat sebagai instrumen pengujian hipotesis, bukan tujuan akhir).

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : Muhammad Rafi Adyatma
Tanggal          : 06 April 2026

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya: 95% akurat itu diukur berdasarkan dataset apa, dan apakah dataset tersebut mewakili kondisi dunia nyata?
   - Data yang dibutuhkan untuk verifikasi:
     Detail dataset (jumlah sampel, distribusi kelas, sumber data)
     Metode evaluasi yang digunakan (cross-validation? hold-out?)
     Confusion matrix lengkap (bukan hanya akurasi, tapi precision, recall, F1-score)
     Perbandingan dengan baseline atau metode lain pada dataset yang sama

2. Posisi paradigma:
   - Pendekatan: [x] Positivis  [ ] Interpretivis  [x] Design Science  [ ] Mixed
   - Alasan: Penelitian di bidang TI umumnya mengukur fenomena yang dapat diobservasi dan diukur secara objektif (positivis) sekaligus membangun artefak sebagai instrumen uji hipotesis (design science). Keduanya saling melengkapi.

3. Identifikasi distorsi:
   - Asumsi tersembunyi: Data yang dikumpulkan diasumsikan bebas noise dan representatif tanpa verifikasi lebih lanjut
   - Sumber bias potensial: Sampling bias (hanya mengambil data pada waktu tertentu), confirmation bias (hanya mencari bukti yang mendukung hipotesis)
   - Langkah mitigasi: Dokumentasikan seluruh proses pengumpulan dan pembersihan data, gunakan multiple data sources, lakukan sensitivity analysis

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: Data mentah (raw data) harus disimpan utuh, tidak boleh dihapus atau diubah hanya demi hasil signifikan
   - Batasan yang diakui sejak awal: Keterbatasan dataset, generalisasi yang tidak bisa dibuat, asumsi statistik yang mungkin tidak terpenuhi
```

---

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

**Paper yang dipilih:**
> Judul: Model Statistika untuk Optimalisasi Kinerja Sistem Informasi
> Penulis (Tahun): Siti Fatimah Pohan, Nur Fatimah Adella, Khairunnesa Rahmadani Siregar, Bunga Shintya, Rulia Pitriani (2024)
> Sumber e-journal: Jurnal Computer Science and Information Technology (JCoInT) - Universitas Labuhanbatu
Website: https://jurnal.ulb.ac.id/index.php/JCoInT/article/view/6107

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengumpulkan data dari "berbagai sistem informasi di beberapa organisasi besar" untuk mengukur waktu respons, throughput, tingkat kesalahan, dan kepuasan pengguna | Sampling bias — Tidak disebutkan jumlah organisasi, sektor industri, atau periode pengumpulan data. Organisasi yang bersedia berpartisipasi mungkin adalah yang sudah matang sistemnya, sehingga tidak representatif |
| Data → Processing | Data dianalisis menggunakan regresi, ANOVA, dan metode optimasi berbasis data untuk mengidentifikasi faktor-faktor yang mempengaruhi kinerja | Asumsi statistik tidak diperiksa — ANOVA dan regresi punya asumsi normalitas residual dan homoskedastisitas. Paper tidak menyebutkan pemeriksaan asumsi ini, sehingga hasil bisa tidak valid |
| Processing → Analysis | Menyimpulkan bahwa konfigurasi perangkat keras, efisiensi kode, dan volume transaksi memiliki "pengaruh signifikan" terhadap kinerja sistem | Laporan statistik tidak lengkap — Tidak ada nilai p-value, F-statistic, atau R-squared yang ditampilkan. Klaim "signifikan" tanpa bukti statistik yang transparan adalah red flag |
| Analysis → Inference | Menyimpulkan bahwa model statistika yang diusulkan dapat meningkatkan waktu respons hingga 30% dan throughput hingga 20% | Kausalitas terbalik — Tidak jelas apakah model yang menyebabkan peningkatan, atau organisasi yang sudah berkinerja baik lebih mungkin menerapkan model |
| Inference → Knowledge | Merekomendasikan bahwa "organisasi dapat secara proaktif mengidentifikasi dan mengatasi masalah kinerja" menggunakan model ini | Generalisasi berlebihan — Studi hanya pada "beberapa organisasi besar" tapi rekomendasi diberikan untuk semua organisasi tanpa mempertimbangkan konteks dan skala |

**Distorsi paling besar di tahap:**  Reality → Data (Sampling bias paling fundamental karena jika data awal sudah bias, seluruh tahap berikutnya ikut bias)

**Dua distorsi spesifik yang teridentifikasi:**
1. Sampling Bias (Reality → Data): Frasa "beberapa organisasi besar" sangat samar tanpa menyebutkan jumlah, sektor, kriteria pemilihan, atau periode data. Ini membuat hasil tidak bisa digeneralisasi ke populasi yang lebih luas termasuk UMKM atau organisasi dengan sistem yang belum matang.
2. Laporan Statistik Tidak Lengkap (Processing → Analysis): Klaim "pengaruh signifikan" dan peningkatan persentase (30%, 20%) tidak disertai standar error, confidence interval, atau p-value. Tanpa informasi ini, pembaca tidak bisa menilai apakah temuan robust atau hanya kebetulan statistik.

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | 	Menghapus outlier semata-mata untuk mencapai signifikansi adalah bentuk p-hacking (manipulasi data). Kejujuran menuntut peneliti untuk melaporkan hasil apa adanya (null result) atau menyajikan kedua analisis secara transparan. Peneliti juga harus menjelaskan mengapa outlier tersebut layak dikeluarkan (misal: kesalahan teknis pengukuran), bukan karena "mengganggu hasil" |
| Transparansi | Peneliti wajib menyatakan secara eksplisit di bagian metode: "Kami melakukan analisis dengan dan tanpa outlier." Metode deteksi outlier (IQR, Z-score, atau visual inspection) harus disebutkan sebelum melihat hasil signifikansi. Jika alasan penghapusan hanya karena ingin p-value < 0.05, itu adalah pelanggaran etika transparansi metodologi |
| Peer review | Reviewer seharusnya meminta akses ke data mentah (raw data) dan syntax analisis. Reviewer yang baik akan curiga jika laporan metode tidak menyebutkan handling of outliers secara eksplisit. Reviewer juga harus memeriksa apakah keputusan menghapus outlier dibuat post-hoc (setelah melihat hasil) atau a priori (sebelum analisis) |

**Keputusan akhir dan justifikasi:**
> Laporkan hasil asli (tidak signifikan) dan diskusikan outlier tersebut secara terpisah dalam bagian limitasi.
> Justifikasi: Menghapus outlier demi hasil signifikan adalah bentuk Questionable Research Practices (QRP) yang tidak etis dan termasuk kategori scientific misconduct ringan hingga sedang tergantung niatnya. Hasil null (tidak signifikan) adalah temuan yang valid secara ilmiah—dalam banyak kasus, hasil null justru lebih jujur dan penting untuk mencegah publication bias (kecenderungan jurnal hanya menerbitkan hasil positif/signifikan)

---

## Latihan 3 — Posisi Paradigma

**Topik riset:**  Pengaruh Gamifikasi terhadap Produktivitas Karyawan di Perusahaan Startup Teknologi
| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | 4 | 3 | 5 |
| Jenis data yang dikumpulkan | Data kuantitatif: skor produktivitas sebelum-sesudah implementasi gamifikasi, jumlah task diselesaikan, waktu penyelesaian, A/B testing (grup dengan vs tanpa gamifikasi) | Data kualitatif: wawancara mendalam tentang pengalaman karyawan, observasi partisipatif, diary study tentang motivasi dan persepsi terhadap sistem gamifikasi | Prototype sistem gamifikasi (poin, lencana, leaderboard) yang diimplementasikan di lingkungan kerja nyata. Evaluasi berdasarkan utility (apakah sistem dipakai?) dan efficacy (apakah produktivitas naik?) |
| Limitasi paradigma | 	Mengabaikan konteks sosial dan psikologis. Produktivitas diukur angka padahal bisa dipengaruhi faktor lain (kondisi keluarga, kelelahan, konflik dengan atasan) yang tidak terukur. Asumsi bahwa semua karyawan merespons gamifikasi dengan cara yang sama | Hasil sulit digeneralisasi. Wawancara 10 karyawan tidak bisa memprediksi perilaku 1000 karyawan di perusahaan lain. Sulit membuktikan kausalitas (A menyebabkan B) karena tidak ada kontrol statistik | Fokus pada "apakah solusi ini bekerja?" daripada "mengapa ini bekerja?" Risiko menghasilkan artefak yang fungsional tetapi tidak menghasilkan teori general tentang perilaku manusia. Bisa jadi solusi hanya bekerja di konteks tertentu |

**Paradigma yang dipilih:** Design Science
**Alasan:** Alasan: Topik tentang gamifikasi untuk meningkatkan produktivitas karyawan sangat cocok dengan paradigma Design Science karena esensi penelitian ini adalah membangun artefak (sistem gamifikasi berupa poin, lencana, leaderboard) untuk memecahkan masalah dunia nyata (produktivitas karyawan yang rendah). Design Science tidak sekadar mengamati atau menginterpretasi fenomena, tetapi secara aktif menciptakan solusi dan mengevaluasi efektivitasnya dalam konteks spesifik.

Skor kesesuaian 5 karena topik ini menuntut pembangunan prototype, implementasi di lapangan, dan evaluasi berbasis utility—yang merupakan inti dari Design Science. Pendekatan Positivis (skor 4) juga relevan untuk mengukur efektivitas secara kuantitatif, tetapi tanpa artefak yang dibangun, penelitian tidak akan menghasilkan kontribusi praktis. Interpretivis (skor 3) berguna untuk memahami mengapa gamifikasi bekerja atau tidak, tetapi tidak cukup sebagai paradigma utama karena tidak menghasilkan solusi yang dapat diimplementasikan.

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> Sebelum membaca materi ini, saya mungkin hanya akan bertanya secara sederhana: "95% dari data apa?" atau "Akurasi 95% itu bagus atau tidak?" tanpa menyadari bahwa angka tersebut bisa sangat menyesatkan tergantung konteks.
> Setelah memahami rantai distorsi, saya tidak akan lagi bertanya "Apakah hasilnya benar?" secara naif.

Sebaliknya, saya akan mengajukan lima pertanyaan kritis yang menyasar setiap tahap dalam Research Trust Model:
1. Tahap Reality → Data: "Data ini dikumpulkan dari mana, kapan, dan bagaimana? Apakah proses pengumpulannya bisa menyebabkan bias seleksi? Apakah datasetnya representatif terhadap populasi yang diklaim?"

2. Tahap Data → Processing: "Keputusan subjektif apa yang diambil saat membersihkan data? Apakah ada outlier yang dihapus? Apakah missing value diisi dengan cara tertentu? Apakah semua keputusan ini dilaporkan secara transparan?"

3. Tahap Processing → Analysis: "Apakah klaim 'signifikan' disertai dengan bukti statistik yang lengkap (p-value, confidence interval, effect size)? Apakah metode statistik yang digunakan sesuai dengan jenis data dan asumsinya?"

4. Tahap Analysis → Inference: "Apakah peneliti membedakan antara korelasi dan kausalitas? Apakah ada variabel perancu (confounding variables) yang tidak diukur tetapi bisa menjelaskan hasil?"

5. Tahap Inference → Knowledge: "Apakah rekomendasi peneliti melampaui apa yang benar-benar didukung oleh data? Apakah mereka mengakui keterbatasan penelitian mereka?"

Kesimpulannya: Sebuah klaim "95% akurat" tidak ada artinya tanpa mengetahui distorsi apa saja yang mungkin terjadi di kelima tahap transformasi. Saya sekarang akan membaca paper dengan sikap healthy skepticism—bukan untuk mencari-cari kesalahan, tetapi untuk memahami dengan jujur apa yang benar-benar diketahui dan apa yang masih belum diketahui dari penelitian tersebut.

---
