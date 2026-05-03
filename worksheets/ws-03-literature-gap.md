# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database**: IEEE Xplore, ACM DL, Scopus, Google Scholar
2. **Boolean query** yang terdokumentasi eksplisit
3. **Snowballing**: backward (telusuri referensi) + forward (cari yang mengutip)
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      : Optimasi QoS Jaringan WiFi High-Density Menggunakan Dynamic Bandwidth Management.
Database   : Google Scholar Indonesia / Garuda Ristekdikti.
Query      : "Optimasi QoS WiFi" AND "Kepadatan Pengguna" AND "Manajemen Bandwidth".
Tahun      : 2021 – 2024.
Hasil awal : 45 paper → Screening → 5 paper final

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
| Saputra dkk | 2022 | HTB (Hierarchical Token Bucket) | Log Trafik Kampus | Bandwidth terbagi rata | Throughput turun saat beban puncak |
| Kurniawan & Fitri | 2023 | PCQ (Per Connection Queue) | Hotspot Cafe | Fairness terjaga | Tidak mendeteksi jenis trafik otomatis |
| Ramadhan et al. | 2021 | Analisis Parameter QoS | WiFi Area Publik | Identifikasi penyebab delay | Hanya observasi, tidak ada solusi otomatis |  
| Wijaya & Utomo | 2024 | Load Balancing (Nth) | Lab Komputer | Trafik tersebar ke 2 ISP | Sulit diimplementasikan di Single-AP |
| Hidayat dkk. | 2023 | Algoritma Round Robin | Jaringan Sekolah | Packet loss turun 12% | Respon lambat pada lonjakan user ekstrem |
Pola yang ditemukan:
  Metode dominan     : Implementasi fitur antrian (Queueing) Mikrotik seperti HTB dan PCQ.
  Dataset umum       : Data log trafik dari lingkungan pendidikan dan area publik lokal.
  Limitasi berulang  : Penentuan limit bandwidth masih bersifat statis, kurang adaptif terhadap fluktuasi jumlah pengguna secara real-time.

GAP IDENTIFICATION

Gap 1: [Jenis: performance / method / data / context]
  Deskripsi    : Belum adanya mekanisme manajemen bandwidth yang dapat menyesuaikan limit secara otomatis berdasarkan jumlah pengguna aktif secara real-time.
  Bukti        : Mayoritas studi (Saputra, 2022; Kurniawan, 2023) menggunakan nilai limit statis yang diinput manual.
  Signifikansi : Otomasi akan mencegah degradasi performa total saat terjadi lonjakan pengguna yang tidak terduga.

Gap 2: [Jenis: ____]
  Deskripsi    : Terbatasnya optimasi pada perangkat WiFi low-end yang memiliki keterbatasan memori namun melayani pengguna padat.
  Bukti        : Studi Wijaya (2024) membutuhkan infrastruktur ISP ganda yang jarang tersedia di lokasi publik kecil/UMKM.
  Signifikansi : Memberikan solusi yang aplikatif dan murah bagi infrastruktur IT di Indonesia tanpa perlu penggantian perangkat hardware.

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|----------|-----------|---------------|--------|
| Static HTB | Dasar pembagian bandwidth | Standar konfigurasi umum di Indonesia | Saputra dkk., 2022 |
| PCQ Method | Optimasi pembagian adil | Teknik manajemen user banyak yang paling populer | Kurniawan & Fitri, 2023 |

```

---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan Google Scholar atau database lain.

**Topik riset:** Optimasi Quality of Service (QoS) pada Jaringan WiFi Kepadatan Tinggi.
**Query pencarian:** Optimasi QoS WiFi AND Kepadatan Pengguna AND Manajemen Bandwidth
**Database:** Google Scholar (ID), Garuda (Garba Rujukan Digital).

| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | Saputra dkk. | 2022 | Hierarchical Token Bucket (HTB) | Jaringan Kampus | Bandwidth terbagi rata sesuai priority | Throughput menurun saat beban puncak |
| 2 | Kurniawan & Fitri | 2023 | PCQ (Per Connection Queue) | Hotspot Cafe | Fairness akses terjaga bagi setiap user | Belum otomatis mendeteksi jenis trafik (video vs teks) |
| 3 | Ramadhan et al | 2021 | Analisis Parameter QoS (ITU-T G.1010) | WiFi Area Publik | Identifikasi penyebab delay tinggi | Hanya observasi, belum memberikan solusi otomasi |
| 4 | Wijaya & Utomo | 2024 | Load Balancing Method (Nth) | Lab Komputer | Beban trafik tersebar ke 2 ISP | Implementasi sulit pada perangkat Single-AP |
| 5 | Hidayat dkk. | 2023 | Algoritma Round Robin | Jaringan Sekolah | Penurunan packet loss sebesar 12% | Respon lambat terhadap perubahan jumlah user ekstrem |

**Pola yang terlihat — Metode dominan:** Penggunaan fitur queue tree (HTB/PCQ) pada Mikrotik sebagai solusi manajemen trafik di lapangan.
**Limitasi yang berulang:** Penentuan limit bandwidth masih sering dilakukan secara statis (angka tetap), sehingga kurang adaptif terhadap fluktuasi jumlah pengguna yang dinamis.

---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | Ya | Metode HTB statis gagal menjaga latency di bawah 150ms saat jumlah pengguna aktif melebihi kapasitas desain awal. |
| Method Gap | Ya | Belum banyak riset lokal yang mengintegrasikan algoritma adaptif yang bisa mengubah limit secara otomatis berdasarkan jumlah user yang terdeteksi secara real-time |
| Data Gap | Tidak | Data log trafik dari lingkungan kampus dan sekolah di Indonesia sudah cukup banyak terdokumentasi |
| Context Gap | Ya | Kurangnya pengujian pada perangkat WiFi murah (low-cost) yang banyak digunakan di UMKM atau sekolah di Indonesia. |

**Gap utama yang dipilih:** Method Gap & Context Gap.
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Kebanyakan solusi di jurnal internasional menggunakan AI tingkat tinggi yang butuh server mahal. Di Indonesia, kita butuh metode yang pintar tapi cukup ringan untuk dijalankan di router yang ada (misalnya Mikrotik atau OpenWrt) agar bisa langsung diterapkan di sekolah atau cafe tanpa beli alat baru.

---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | HTB (Hierarchical Token Bucket) | Metode standar yang digunakan untuk membagi bandwidth secara hierarkis. | Merupakan common practice administrator jaringan di Indonesia. | Bukan | Saputra dkk., 2022 |
| 2 | PCQ (Per Connection Queue) | Queue)	Metode terbaru untuk membagi bandwidth secara adil otomatis. | Standar optimasi terkini untuk manajemen pengguna banyak. | Ya (State-of-the-art lokal) | Kurniawan & Fitri, 2023 |

**Apakah pemilihan baseline ini bisa dianggap straw man?** Tidak
> Justifikasi: Karena saya membandingkan metode usulan saya dengan metode yang memang saat ini menjadi standar tertinggi di implementasi jaringan lokal di Indonesia.

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> Klaim "belum ada" seringkali muncul karena malas mencari.
> Gap yang valid muncul setelah kita membaca 5-10 jurnal dan menemukan bahwa: "Oke, semua orang pakai HTB, tapi HTB mereka punya kelemahan di bagian X".
