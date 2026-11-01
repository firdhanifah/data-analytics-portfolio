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

Mengidentifikasi nilai yang hilang (missing values) berdasarkan konteks masing-masing kolom.

Memeriksa dan menangani data duplikat berdasarkan karakteristik serta aturan bisnis.

Menyeragamkan format dan tipe data, termasuk tanggal, waktu, kategori, dan nilai numerik.

Memeriksa konsistensi nilai dan mengidentifikasi data yang tidak sesuai dengan aturan bisnis.

Memvalidasi hubungan antar tabel menggunakan primary key dan foreign key.

Memeriksa kesesuaian nominal transaksi, detail item pesanan, dan pembayaran.

Mempertahankan nilai yang valid meskipun terindikasi sebagai nilai ekstrem (outlier), selama tidak terdapat bukti bahwa nilai tersebut merupakan kesalahan data.

Melakukan agregasi data item pesanan ke tingkat transaksi.

Menggabungkan informasi transaksi dengan data pelanggan, restoran, pembayaran, pengantaran, dan ulasan.

Dataset akhir disusun pada tingkat pesanan (order-level), dengan satu baris merepresentasikan satu pesanan. Data item pesanan diagregasi untuk menghasilkan metrik seperti jumlah jenis item, total kuantitas, dan subtotal item.

Beberapa kolom turunan juga disiapkan untuk mendukung analisis, seperti:

`item_count`: jumlah jenis item dalam satu pesanan.
`total_quantity`: total kuantitas seluruh item dalam satu pesanan.
`items_subtotal`: total nilai seluruh item dalam satu pesanan.
`has_review`: indikator ketersediaan rating pelanggan.
`rating_label`: kategori rating pelanggan.
`review_text_status`: status ketersediaan teks ulasan.
`actual_delivery_minutes`: durasi aktual pengantaran berdasarkan waktu pengambilan hingga waktu pesanan diterima.
`delay_minutes`: selisih antara durasi aktual pengantaran dan estimasi waktu yang dijanjikan.
`Delivery Performance`: klasifikasi performa pengantaran berdasarkan perbandingan durasi aktual dengan estimasi waktu.

Contoh struktur dataset akhir:
| Kolom | Tipe / Contoh Data | Deskripsi |
| :--- | :--- | :--- |
| **order_id** | `O00001` | ID unik untuk setiap transaksi/pesanan |
| **customer_id** | `C0080` | ID unik pelanggan yang melakukan pemesanan |
| **restaurant_id** | `R0018` | ID unik restoran tempat pesanan dibuat |
| **order_timestamp** | `2026-01-19 03:32:00` | Waktu/tanggal persis saat pesanan dilakukan |
| **order_status** | `completed` | Status akhir pesanan (misal: selesai, dibatalkan, dll) |
| **promo_code** | `NaN` / `NEWUSER` | Kode promo atau diskon yang digunakan |
| **subtotal_amount** | `54.000` | Total harga makanan sebelum diskon/ongkir |
| **total_amount** | `62.000` | Total akhir yang dibayar pelanggan |
| **rating** | `2.0` / `NaN` | Skor penilaian dari pelanggan (skala 1-5) |
| **item_count** | `2` | Jumlah jenis menu/item berbeda dalam satu pesanan |
| **total_quantity** | `3` | Total keseluruhan kuantitas item yang dibeli |
| **items_subtotal** | `54.000` | Subtotal khusus untuk item-item yang dipesan |
| **has_review** | `True` / `False` | Status apakah pesanan ini memiliki ulasan |
| **rating_label** | `2 - Dissatisfied` | Kategori keterangan teks dari nilai rating |
| **review_text_status**| `Text Available` | Status ketersediaan teks ulasan dari pelanggan |

---

# Metodologi Analisis

Analisis dilakukan melalui empat kelompok kinerja bisnis yang kemudian diintegrasikan untuk menghasilkan rekomendasi.

## A. Indikator Kinerja Pesanan

| Indikator | Perhitungan |
|---|---|
| Total Orders | Jumlah pesanan unik |
| Completion Rate | Pesanan selesai / Total pesanan |
| Cancellation Rate | Pesanan batal / Total pesanan |
| Completed Order Value | Total nilai transaksi pesanan selesai |
| Average Order Value (AOV) | Nilai transaksi pesanan selesai / Jumlah pesanan selesai |

## B. Indikator Kinerja Pengiriman

| Indikator | Perhitungan |
|---|---|
| On-Time Delivery Rate | Pengiriman tepat waktu / Pengiriman dengan data waktu valid |
| Late Delivery Rate | Pengiriman terlambat / Pengiriman dengan data waktu valid |
| Average Delivery Duration | Rata-rata durasi aktual pengiriman |
| Average Delay | Rata-rata durasi keterlambatan pada pesanan terlambat |

## C. Indikator Pengalaman Pelanggan

| Indikator | Perhitungan |
|---|---|
| Average Rating | Rata-rata rating yang tersedia |
| Review Rate | Pesanan dengan ulasan / Total pesanan |

## D. Indikator Kinerja Pembayaran

| Indikator | Perhitungan |
|---|---|
| Payment Success Rate | Pembayaran berhasil / Total transaksi pembayaran |
| Failed Payment Rate | Pembayaran gagal / Total transaksi pembayaran |
| Refund Rate | Pembayaran refund / Total transaksi pembayaran |

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
