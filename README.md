# SwiftBite Business Analytics Project
### Analisis Kinerja Operasional dan Pengalaman Pelanggan

**Tools:** Python (Pandas, Matplotlib, Seaborn), Power BI

---

# Daftar Isi

1. [Pemahaman Bisnis](#pemahaman-bisnis)
2. [Sumber Data](#sumber-data)
3. [Pembersihan dan Persiapan Data](#pembersihan-dan-persiapan-data)
4. [Metodologi Analisis](#metodologi-analisis)
5. [Analisis Data Eksploratif dan Temuan](#analisis-data-eksploratif-dan-temuan)
   - [A. Kinerja Pesanan](#a-kinerja-pesanan)
   - [B. Kinerja Pengiriman](#b-kinerja-pengiriman)
   - [C. Pengalaman Pelanggan](#c-pengalaman-pelanggan)
   - [D. Kinerja Pembayaran](#d-kinerja-pembayaran)
   - [E. Analisis Kinerja Terpadu](#e-analisis-kinerja-terpadu)
6. [Pengembangan Dashboard](#pengembangan-dashboard)
7. [Temuan Utama dan Rekomendasi Strategis](#temuan-utama-dan-rekomendasi-strategis)
8. [Keterbatasan dan Catatan](#keterbatasan-dan-catatan)
9. [Sumber Daya Teknis](#sumber-daya-teknis)

---

# Pemahaman Bisnis

## Latar Belakang Bisnis

SwiftBite merupakan platform layanan pesan antar makanan yang menghubungkan pelanggan dengan restoran melalui layanan pemesanan dan pengiriman secara daring.

Kinerja bisnis SwiftBite dipengaruhi oleh beberapa aktivitas yang saling berkaitan, mulai dari transaksi pemesanan, operasional pengiriman, proses pembayaran, hingga pengalaman pelanggan.

Proyek ini dilakukan untuk menganalisis data bisnis SwiftBite secara terpadu guna memperoleh gambaran kinerja operasional serta mengidentifikasi aspek layanan yang berpotensi untuk ditingkatkan.

## Permasalahan Bisnis

SwiftBite belum memiliki gambaran terpadu mengenai kinerja transaksi, operasional pengiriman, dan pengalaman pelanggan. Kondisi ini dapat menyulitkan perusahaan dalam mengidentifikasi permasalahan layanan serta menentukan aspek yang perlu diprioritaskan untuk perbaikan.

Bagaimana kinerja transaksi, operasional pengiriman, pembayaran, dan pengalaman pelanggan SwiftBite selama periode Januari–Maret 2026, serta aspek layanan apa yang perlu diprioritaskan untuk meningkatkan keandalan layanan dan pengalaman pelanggan?

## Tujuan Proyek

Proyek ini bertujuan untuk mengevaluasi kinerja bisnis SwiftBite secara terpadu melalui analisis transaksi, operasional pengiriman, pembayaran, dan pengalaman pelanggan.

Secara khusus, proyek ini bertujuan untuk:
- Mengevaluasi kinerja pesanan dan nilai transaksi.
- Mengidentifikasi pola keterlambatan dan kinerja pengiriman.
- Menganalisis hubungan antara kinerja pengiriman dan penilaian pelanggan.
- Mengevaluasi keberhasilan dan kendala dalam proses pembayaran.
- Menentukan aspek operasional yang perlu diprioritaskan berdasarkan hasil analisis.

## Pertanyaan Bisnis Utama

### A. Kinerja Pesanan
- Bagaimana perkembangan jumlah pesanan dan nilai transaksi selama periode analisis?
- Seberapa tinggi tingkat penyelesaian dan pembatalan pesanan?
- Apakah terdapat perbedaan kinerja berdasarkan restoran dan segmen pelanggan?

### B. Kinerja Pengiriman
- Seberapa baik SwiftBite memenuhi estimasi waktu pengiriman?
- Apakah jarak pengiriman berkaitan dengan durasi dan keterlambatan?
- Restoran atau zona mana yang menunjukkan tingkat keterlambatan lebih tinggi?

### C. Pengalaman Pelanggan
- Bagaimana tingkat kepuasan pelanggan berdasarkan rating dan ulasan?
- Apakah keterlambatan pengiriman berkaitan dengan penurunan rating?
- Segmen pelanggan mana yang memberikan penilaian berbeda?

### D. Kinerja Pembayaran
- Bagaimana tingkat keberhasilan, kegagalan, dan pengembalian pembayaran?
- Apakah terdapat perbedaan hasil pembayaran berdasarkan metode pembayaran?
- Bagaimana pola percobaan pembayaran pada transaksi yang gagal?

### E. Strategi Bisnis
- Aspek layanan apa yang paling membutuhkan perhatian?
- Bagaimana perusahaan dapat memprioritaskan perbaikan berdasarkan hasil analisis?

---

# Sumber Data

Proyek ini menggunakan beberapa dataset yang merepresentasikan berbagai aspek proses bisnis SwiftBite. Setiap dataset memiliki struktur dan format yang berbeda sehingga memerlukan metode pembacaan dan pengolahan yang sesuai.

| No. | Dataset | Format | Deskripsi |
| :--- | :--- | :---: | :--- |
| 1 | Orders | CSV | Data transaksi dan informasi pesanan pelanggan |
| 2 | Order Items | CSV | Detail item, kuantitas, dan nilai setiap item dalam pesanan |
| 3 | Customers | JSON | Informasi pelanggan dan segmentasi pelanggan |
| 4 | Payments | TXT | Informasi transaksi dan status pembayaran |
| 5 | Restaurants | XLSX | Informasi restoran, kategori kuliner, dan karakteristik restoran |
| 6 | Delivery | CSV | Informasi proses pengantaran dan performa pengiriman |
| 7 | Reviews | CSV | Rating dan ulasan pelanggan terhadap layanan |

---

# Pembersihan dan Persiapan Data

Proses data cleaning dan data preparation dilakukan untuk memastikan kualitas, konsistensi, serta kesiapan data sebelum digunakan dalam analisis.

Tahapan yang dilakukan meliputi:
- Mengidentifikasi nilai yang hilang berdasarkan konteks masing-masing kolom.
- Memeriksa dan menangani data duplikat.
- Menyeragamkan format dan tipe data.
- Memvalidasi hubungan antar tabel menggunakan primary key dan foreign key.
- Memeriksa kesesuaian data berdasarkan aturan bisnis.
- Melakukan agregasi data item pesanan ke tingkat transaksi.

Dataset akhir disusun pada tingkat pesanan (*order-level*), dengan satu baris merepresentasikan satu pesanan. Data item pesanan diagregasi untuk menghasilkan metrik seperti jumlah jenis item, total kuantitas, dan subtotal item.


| order_id | customer_id | restaurant_id | order_timestamp | order_status | promo_code | subtotal_amount | total_amount | rating | item_count | total_quantity | items_subtotal | has_review | rating_label | review_text_status |
|---|---|---|---|---|---|---:|---:|---:|---:|---:|---:|---|---|---|
| O00001 | C0080 | R0018 | 2026-01-19 03:32:00 | completed | NaN | 54.000 | 62.000 | 2.0 | 2 | 3 | 54.000 | True | 2 - Dissatisfied | Text Available |
| O00002 | C0087 | R0023 | 2026-01-10 18:34:00 | completed | NEWUSER | 130.000 | 133.000 | 2.0 | 2 | 5 | 130.000 | True | 2 - Dissatisfied | Text Available |
| O00003 | C0094 | R0003 | 2026-01-12 07:39:00 | completed | HEMAT15 | 238.000 | 233.000 | 1.0 | 4 | 11 | 238.000 | True | 1 - Very Dissatisfied | No Text |
| O00004 | C0101 | R0008 | 2026-02-03 17:42:00 | completed | NaN | 78.000 | 76.000 | NaN | 2 | 5 | 78.000 | False | No Rating | No Review |
| O00005 | C0108 | R0013 | 2026-01-17 09:22:00 | completed | NaN | 123.000 | 120.000 | NaN | 3 | 4 | 123.000 | False | No Rating | No Review |
---

# Metodologi Analisis

Analisis kinerja pengiriman dilakukan melalui pemeriksaan karakteristik pengiriman dan pembentukan variabel turunan (*feature engineering*) untuk mengukur ketepatan waktu pengiriman.

### A. Analisis Karakteristik Pengiriman

Tahap ini dilakukan untuk memahami kondisi dasar operasional pengiriman melalui beberapa aspek:

1. **Distribusi Status Pengiriman**  
   Mengidentifikasi jumlah dan persentase pesanan berdasarkan status pengiriman.

2. **Distribusi Jarak Pengiriman**  
   Mengukur karakteristik jarak pengiriman melalui statistik deskriptif, seperti rata-rata, nilai minimum, maksimum, dan kuartil.

3. **Estimasi Waktu Pengiriman**  
   Mengidentifikasi karakteristik estimasi waktu pengiriman yang ditetapkan untuk setiap pesanan.

4. **Karakteristik Pengiriman Berdasarkan Status**  
   Membandingkan jumlah pesanan, rata-rata jarak, dan estimasi waktu pengiriman berdasarkan status pengiriman.

5. **Kategorisasi Jarak Pengiriman**  
   Mengelompokkan jarak pengiriman ke dalam empat kategori:
   - 0–2 km
   - 2–5 km
   - 5–10 km
   - Lebih dari 10 km

   Selanjutnya, dilakukan perbandingan jumlah pesanan dan rata-rata estimasi waktu pengiriman pada setiap kategori jarak.

### B. Pembentukan Variabel Kinerja Pengiriman

Tahap ini dilakukan dengan membentuk variabel baru berdasarkan informasi waktu pengiriman yang tersedia.

**1. Durasi Aktual Pengiriman (`actual_delivery_minutes`)**

Menghitung durasi aktual pengiriman berdasarkan selisih waktu pesanan diterima kurir (*picked up*) hingga pesanan diterima pelanggan (*delivered*).

Rumus:

`Durasi Aktual = Waktu Diterima - Waktu Pengambilan`

**2. Durasi Keterlambatan (`delay_minutes`)**

Menghitung selisih antara durasi aktual pengiriman dan estimasi waktu pengiriman yang telah ditetapkan.

Rumus:

`Durasi Keterlambatan = Durasi Aktual - Estimasi Waktu`

Interpretasi:
- Nilai positif: pengiriman melebihi estimasi waktu.
- Nilai nol: pengiriman sesuai estimasi waktu.
- Nilai negatif: pengiriman lebih cepat dari estimasi waktu.

**3. Indikator Keterlambatan (`is_late`)**

Membentuk variabel Boolean untuk mengidentifikasi ketepatan waktu pengiriman berdasarkan durasi keterlambatan.

Kriteria:
- `True`: pengiriman terlambat (`delay_minutes > 0`).
- `False`: pengiriman tidak terlambat (`delay_minutes <= 0`).

Pesanan yang tidak memiliki informasi durasi aktual dikecualikan dari evaluasi ketepatan waktu.

### C. Evaluasi Ketepatan Waktu Pengiriman

Setelah variabel turunan terbentuk, dilakukan analisis lanjutan melalui:

1. **Statistik Durasi Aktual:** Mengidentifikasi karakteristik waktu aktual yang dibutuhkan untuk menyelesaikan pengiriman.
2. **Statistik Keterlambatan:** Mengukur distribusi selisih antara durasi aktual dan estimasi pengiriman.
3. **Distribusi Ketepatan Waktu:** Mengidentifikasi jumlah pengiriman tepat waktu dan terlambat.
4. **Perbandingan Waktu Aktual dan Estimasi:** Membandingkan durasi aktual dengan estimasi waktu pada setiap pesanan.
---

# Analisis Data Eksploratif dan Temuan

**Tools:** Python (Pandas, Matplotlib, Seaborn)

## A. Kinerja Pesanan

### 1. Tren Jumlah Pesanan
[Visualisasi tren pesanan bulanan]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

> **Insight A1 — [Judul Insight]**
>
> [Interpretasi bisnis dari hasil analisis.]

### 2. Penyelesaian dan Pembatalan Pesanan
[Visualisasi status pesanan]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

> **Insight A2 — [Judul Insight]**
>
> [Interpretasi bisnis dari hasil analisis.]

### 3. Kinerja Berdasarkan Restoran dan Segmen Pelanggan
[Visualisasi perbandingan nilai transaksi, jumlah pesanan, dan tingkat pembatalan]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

---

## B. Kinerja Pengiriman

### 1. Distribusi Durasi Pengiriman
[Visualisasi distribusi durasi pengiriman]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

> **Insight B1 — [Judul Insight]**
>
> [Interpretasi bisnis dari hasil analisis.]

### 2. Analisis Keterlambatan Pengiriman
[Visualisasi ketepatan waktu dan distribusi keterlambatan]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

### 3. Kinerja Pengiriman Berdasarkan Jarak, Restoran, dan Zona
[Visualisasi perbandingan kinerja pengiriman]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

> **Insight B2 — [Judul Insight]**
>
> [Interpretasi bisnis dari hasil analisis.]

---

## C. Pengalaman Pelanggan

### 1. Distribusi Rating Pelanggan
[Visualisasi distribusi rating]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

### 2. Hubungan Keterlambatan dan Rating
[Visualisasi perbandingan rating pengiriman tepat waktu dan terlambat]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

> **Insight C1 — [Judul Insight]**
>
> [Interpretasi bisnis dari hasil analisis.]

### 3. Analisis Ulasan Berdasarkan Segmen Pelanggan
[Visualisasi rating dan tingkat ulasan berdasarkan segmen]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

---

## D. Kinerja Pembayaran

### 1. Distribusi Status Pembayaran
[Visualisasi status pembayaran]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

### 2. Keberhasilan Pembayaran Berdasarkan Metode
[Visualisasi perbandingan metode pembayaran]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

### 3. Analisis Percobaan dan Selisih Pembayaran
[Visualisasi jumlah percobaan dan selisih nominal pembayaran]

**Temuan:**
- [Temuan berdasarkan hasil analisis]

> **Insight D1 — [Judul Insight]**
>
> [Interpretasi bisnis dari hasil analisis.]

---

## E. Analisis Kinerja Terpadu

Bagian ini menggabungkan temuan dari analisis pesanan, pengiriman, pengalaman pelanggan, dan pembayaran untuk memperoleh gambaran menyeluruh mengenai kinerja SwiftBite.

### 1. Hubungan Kinerja Pengiriman dan Pengalaman Pelanggan
[Analisis keterkaitan keterlambatan pengiriman dengan rating pelanggan.]

### 2. Hubungan Status Pesanan dan Pembayaran
[Analisis kesesuaian status pesanan dengan status pembayaran.]

### 3. Identifikasi Area Prioritas
[Perbandingan aspek bisnis berdasarkan frekuensi permasalahan dan dampaknya terhadap layanan.]

> **Insight E1 — [Kesimpulan Utama Analisis Terpadu]**
>
> [Jelaskan pola utama yang ditemukan dan alasan aspek tertentu perlu diprioritaskan.]

---

# Pengembangan Dashboard

**Tools:** Power BI

Dashboard interaktif dikembangkan untuk menyajikan indikator kinerja utama dan temuan analisis kepada pemangku kepentingan bisnis.

## Halaman 1 — Ringkasan Kinerja
[Masukkan tangkapan layar dashboard]

Menampilkan gambaran umum kinerja SwiftBite melalui indikator jumlah pesanan, tingkat penyelesaian, nilai transaksi, keterlambatan pengiriman, dan rating pelanggan.

## Halaman 2 — Kinerja Pesanan dan Pengiriman
[Masukkan tangkapan layar dashboard]

Menampilkan tren pesanan, pembatalan, nilai transaksi, serta perbandingan kinerja pengiriman berdasarkan restoran dan zona.

## Halaman 3 — Pengalaman Pelanggan dan Pembayaran
[Masukkan tangkapan layar dashboard]

Menampilkan distribusi rating, perbandingan pengalaman pelanggan, serta kinerja pembayaran berdasarkan status dan metode pembayaran.

---

# Temuan Utama dan Rekomendasi Strategis

## Temuan Utama

> **[Kesimpulan utama proyek]**
>
> [Rangkum kondisi bisnis SwiftBite berdasarkan hasil analisis. Hubungkan temuan dari pesanan, pengiriman, pengalaman pelanggan, dan pembayaran.]

## Rekomendasi Berdasarkan Prioritas

### Prioritas Tinggi
1. **[Rekomendasi 1]**
   - Temuan pendukung: [Hasil analisis]
   - Tindakan yang disarankan: [Usulan tindakan]

### Prioritas Menengah
2. **[Rekomendasi 2]**
   - Temuan pendukung: [Hasil analisis]
   - Tindakan yang disarankan: [Usulan tindakan]

### Jangka Panjang
3. **[Rekomendasi 3]**
   - Temuan pendukung: [Hasil analisis]
   - Tindakan yang disarankan: [Usulan tindakan]

---

# Keterbatasan dan Catatan

| Keterbatasan | Dampak terhadap Analisis |
|---|---|
| Periode data hanya tiga bulan | Belum cukup untuk menyimpulkan pola musiman atau tren jangka panjang |
| Jumlah data terbatas | Hasil analisis perlu dibatasi pada data yang tersedia |
| Terdapat nilai kosong pada data pengiriman dan ulasan | Beberapa indikator hanya dapat dihitung dari data yang valid |
| Tidak tersedia data biaya operasional dan komisi | Profitabilitas dan efisiensi biaya tidak dapat dihitung |
| Hubungan antarvariabel bersifat observasional | Tidak dapat langsung digunakan untuk menyimpulkan hubungan sebab-akibat |

---

# Sumber Daya Teknis

| Perangkat | Kegunaan |
|---|---|
| Python | Pengolahan dan analisis data |
| Pandas | Pembersihan dan manipulasi data |
| Matplotlib | Visualisasi data |
| Seaborn | Visualisasi statistik |
| Power BI | Pembuatan dashboard interaktif |
| Jupyter Notebook / Google Colab | Lingkungan pengembangan |

**Notebook:** [Tautan Google Colab / GitHub]

**Dashboard:** [Tautan atau tangkapan layar Power BI]

---

**Proyek oleh:** [Nama Kamu]

*Proyek ini disusun untuk tujuan pembelajaran dan portofolio.*
