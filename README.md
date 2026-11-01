# SwiftBite Business Analytics Project
### Improving Operational Performance and Customer Experience
<font size="2" color="#656d76">Tools: Python (Pandas, Matplotlib/Seaborn) + Power BI</font>
---
Analisis data pada platform pemesanan dan pengantaran makanan SwiftBite untuk mengevaluasi kinerja bisnis secara menyeluruh, dengan fokus pada pertumbuhan transaksi, kualitas operasional, dan kepuasan pelanggan. Proyek ini mencakup proses data cleaning, data preparation, exploratory data analysis (EDA), analisis kinerja bisnis, serta pengembangan dashboard interaktif untuk mendukung pengambilan keputusan berbasis data.

---

## Table of Contents

1. [Business Understanding](#business-understanding)
2. [Data Sources](#data-sources)
3. [Data Cleaning & Preparation](#data-cleaning--preparation)
4. [Methodology](#methodology)
5. [Exploratory Data Analysis & Insights](#exploratory-data-analysis--insights)
6. [Dashboard Development](#dashboard-development)
7. [Key Insights & Strategic Recommendations](#key-insights--strategic-recommendations)
8. [Limitations & Notes](#limitations--notes)
9. [Technical Resources](#technical-resources)

---

## Business Understanding

### Business Background

SwiftBite merupakan platform pemesanan dan pengantaran makanan yang menghubungkan pelanggan dengan berbagai restoran melalui layanan digital.

Dalam menjalankan bisnis food delivery, pertumbuhan transaksi dan pendapatan merupakan salah satu indikator penting untuk mengukur perkembangan bisnis. Namun, peningkatan aktivitas transaksi tidak selalu mencerminkan kinerja bisnis yang baik secara keseluruhan. Pertumbuhan tersebut juga perlu diimbangi dengan kualitas operasional, keandalan proses pembayaran, serta pengalaman pelanggan yang memuaskan.

Kualitas pengantaran, keberhasilan penyelesaian pesanan, dan kepuasan pelanggan merupakan aspek yang dapat memengaruhi keberlanjutan bisnis. Apabila pertumbuhan transaksi tidak diikuti dengan kualitas layanan yang memadai, perusahaan berpotensi menghadapi permasalahan operasional dan penurunan pengalaman pelanggan.

Melalui data transaksi yang terintegrasi dengan informasi pelanggan, restoran, pembayaran, pengantaran, dan ulasan, SwiftBite memiliki sumber informasi yang dapat digunakan untuk mengevaluasi berbagai aspek kinerja bisnis.

Oleh karena itu, proyek ini dilakukan untuk menganalisis kinerja bisnis SwiftBite secara menyeluruh dan memahami apakah pertumbuhan transaksi telah berjalan seiring dengan kualitas operasional dan kepuasan pelanggan.

### Business Problem

Pertumbuhan bisnis pada platform food delivery tidak hanya ditentukan oleh peningkatan jumlah transaksi dan pendapatan, tetapi juga oleh **kemampuan perusahaan dalam mempertahankan kualitas layanan**.

SwiftBite perlu memahami **apakah perkembangan aktivitas bisnis telah diimbangi dengan performa operasional dan pengalaman pelanggan yang baik**.

Beberapa aspek yang perlu dievaluasi meliputi:

- **Business Growth:** Belum diketahui bagaimana perkembangan transaksi dan pendapatan selama periode pengamatan serta faktor-faktor yang berkontribusi terhadap kinerja bisnis.
- **Customer & Restaurant Contribution:** Belum diketahui bagaimana kontribusi segmen pelanggan, restoran, dan kategori kuliner terhadap aktivitas transaksi dan pendapatan.
- **Operational Performance:** Performa penyelesaian pesanan, ketepatan waktu pengantaran, dan keberhasilan pembayaran perlu dievaluasi untuk memahami kualitas proses operasional.
- **Customer Satisfaction:** Rating dan ulasan pelanggan perlu dianalisis untuk mengetahui gambaran pengalaman pelanggan terhadap layanan SwiftBite.
- **Business Performance Alignment:** Belum diketahui apakah pertumbuhan transaksi telah berjalan seiring dengan kualitas operasional dan kepuasan pelanggan.

Berdasarkan permasalahan tersebut, diperlukan analisis data yang mengintegrasikan aspek pertumbuhan bisnis, operasional, dan kepuasan pelanggan agar perusahaan memperoleh gambaran kinerja yang lebih menyeluruh.

### Project Objectives

Proyek ini bertujuan untuk **mengevaluasi kinerja bisnis SwiftBite** dengan menganalisis pertumbuhan transaksi, kualitas operasional, dan kepuasan pelanggan.

Tujuan khusus proyek meliputi:

1. Menganalisis perkembangan jumlah transaksi dan pendapatan SwiftBite selama periode pengamatan.
2. Mengidentifikasi kontribusi pelanggan, restoran, dan kategori kuliner terhadap kinerja bisnis.
3. Mengevaluasi tingkat penyelesaian pesanan dan keberhasilan pembayaran.
4. Mengukur ketepatan waktu pengantaran serta mengidentifikasi pola keterlambatan.
5. Menganalisis rating dan ulasan pelanggan untuk memahami tingkat kepuasan terhadap layanan.
6. Mengevaluasi keterkaitan antara pertumbuhan transaksi, kualitas operasional, dan kepuasan pelanggan.
7. Mengembangkan dashboard interaktif untuk menyajikan hasil analisis secara informatif.
8. Menyusun rekomendasi berbasis data untuk mendukung peningkatan kinerja bisnis SwiftBite.

### Key Business Questions

**A. Business Growth & Transaction Performance**

- Bagaimana **perkembangan jumlah transaksi dan pendapatan** selama periode pengamatan?
- Bagaimana tren nilai rata-rata transaksi (*Average Order Value/AOV*)?
- Segmen pelanggan mana yang memberikan kontribusi terbesar terhadap transaksi dan pendapatan?
- Restoran dan kategori kuliner mana yang memiliki kontribusi terbesar terhadap kinerja bisnis?
- Bagaimana distribusi transaksi berdasarkan zona dan karakteristik restoran?

**B. Operational Performance**

- Berapa proporsi pesanan yang berhasil diselesaikan dibandingkan dengan keseluruhan pesanan?
- Bagaimana tingkat ketepatan waktu pengantaran SwiftBite?
- Bagaimana distribusi durasi aktual dan estimasi waktu pengantaran?
- Apakah jarak pengantaran berkaitan dengan durasi pengiriman?
- Bagaimana tingkat keberhasilan pembayaran dan distribusi status transaksi pembayaran?

**C. Customer Satisfaction**

- Bagaimana distribusi rating yang diberikan pelanggan?
- Bagaimana tingkat kepuasan pelanggan berdasarkan rating?
- **Apakah terdapat perbedaan rating antara pesanan yang dikirim tepat waktu dan terlambat?**
- Restoran atau kategori kuliner mana yang memperoleh rating relatif tinggi dan rendah?
- Apa saja pola umpan balik yang dapat diidentifikasi dari ulasan pelanggan?

**D. Overall Business Performance Evaluation**

- Apakah perkembangan jumlah transaksi dan pendapatan diikuti oleh kualitas operasional yang baik?
- Apakah peningkatan aktivitas transaksi berkaitan dengan perubahan tingkat penyelesaian pesanan?
- Apakah kualitas pengantaran berkaitan dengan kepuasan pelanggan?
- Aspek bisnis dan operasional mana yang perlu menjadi prioritas evaluasi?
- Rekomendasi apa yang dapat diberikan untuk mendukung peningkatan kinerja bisnis secara berkelanjutan?

### Expected Business Value

Hasil analisis diharapkan dapat memberikan gambaran menyeluruh mengenai kondisi dan kinerja bisnis SwiftBite berdasarkan data transaksi dan layanan.

Nilai bisnis yang diharapkan meliputi:

- **Business Performance Evaluation:** Memberikan gambaran mengenai perkembangan transaksi, pendapatan, serta kontribusi pelanggan dan restoran terhadap bisnis.
- **Operational Improvement:** Membantu mengidentifikasi permasalahan operasional, khususnya terkait penyelesaian pesanan dan ketepatan waktu pengantaran.
- **Payment Reliability:** Memberikan informasi mengenai keberhasilan pembayaran dan kendala dalam proses transaksi.
- **Customer Experience Improvement:** Mengidentifikasi pola kepuasan pelanggan serta aspek layanan yang berpotensi membutuhkan perbaikan.
- **Data-Driven Decision Making:** Menyediakan insight berbasis data sebagai dasar evaluasi dan penentuan prioritas bisnis.
- **Business Sustainability:** Mendukung upaya peningkatan kinerja bisnis dengan mempertimbangkan keseimbangan antara pertumbuhan transaksi, kualitas operasional, dan kepuasan pelanggan.

Secara keseluruhan, proyek ini diharapkan dapat membantu SwiftBite memahami apakah pertumbuhan bisnis telah berjalan seiring dengan kualitas layanan serta mengidentifikasi area yang perlu ditingkatkan untuk mendukung keberlanjutan bisnis.

---

## Data Sources

Proyek ini menggunakan beberapa dataset yang merepresentasikan berbagai aspek proses bisnis SwiftBite. Setiap dataset memiliki struktur dan format yang berbeda sehingga memerlukan metode pembacaan dan pengolahan yang sesuai.

| No. | Dataset | Format | Deskripsi |
|---|---|---|---|
| 1 | Orders | CSV | Data transaksi dan informasi pesanan pelanggan |
| 2 | Order Items | CSV | Detail item, kuantitas, dan nilai setiap item dalam pesanan |
| 3 | Customers | JSON | Informasi pelanggan dan segmentasi pelanggan |
| 4 | Payments | TXT | Informasi transaksi dan status pembayaran |
| 5 | Restaurants | XLSX | Informasi restoran, kategori kuliner, dan karakteristik restoran |
| 6 | Delivery | CSV | Informasi proses pengantaran dan performa pengiriman |
| 7 | Reviews | CSV | Rating dan ulasan pelanggan terhadap layanan |

---

## Data Cleaning & Preparation

Proses data cleaning dan data preparation dilakukan untuk memastikan kualitas, konsistensi, serta kesiapan data sebelum digunakan dalam analisis.

Tahapan yang dilakukan meliputi:

- Mengidentifikasi nilai yang hilang (*missing values*) berdasarkan konteks masing-masing kolom.
- Memeriksa dan menangani data duplikat berdasarkan karakteristik serta aturan bisnis.
- Menyeragamkan format dan tipe data, termasuk tanggal, waktu, kategori, dan nilai numerik.
- Memeriksa konsistensi nilai dan mengidentifikasi data yang tidak sesuai dengan aturan bisnis.
- Memvalidasi hubungan antar tabel menggunakan *primary key* dan *foreign key*.
- Memeriksa kesesuaian nominal transaksi, detail item pesanan, dan pembayaran.
- Mempertahankan nilai yang valid meskipun terindikasi sebagai nilai ekstrem (*outlier*), selama tidak terdapat bukti bahwa nilai tersebut merupakan kesalahan data.
- Melakukan agregasi data item pesanan ke tingkat transaksi.
- Menggabungkan informasi transaksi dengan data pelanggan, restoran, pembayaran, pengantaran, dan ulasan.

Dataset akhir disusun pada tingkat pesanan (*order-level*), dengan satu baris merepresentasikan satu pesanan. Data item pesanan diagregasi untuk menghasilkan metrik seperti jumlah jenis item, total kuantitas, dan subtotal item.

Beberapa kolom turunan juga disiapkan untuk mendukung analisis, seperti:

- `item_count`: jumlah jenis item dalam satu pesanan.
- `total_quantity`: total kuantitas seluruh item dalam satu pesanan.
- `items_subtotal`: total nilai seluruh item dalam satu pesanan.
- `has_review`: indikator ketersediaan rating pelanggan.
- `rating_label`: kategori rating pelanggan.
- `review_text_status`: status ketersediaan teks ulasan.
- `actual_delivery_minutes`: durasi aktual pengantaran berdasarkan waktu pengambilan hingga waktu pesanan diterima.
- `delay_minutes`: selisih antara durasi aktual pengantaran dan estimasi waktu yang dijanjikan.
- `Delivery Performance`: klasifikasi performa pengantaran berdasarkan perbandingan durasi aktual dengan estimasi waktu.

Contoh struktur dataset akhir:

| order_id | customer_id | restaurant_id | order_timestamp | order_status | promo_code | subtotal_amount | total_amount | rating | item_count | total_quantity | items_subtotal | has_review | rating_label | review_text_status |
|---|---|---|---|---|---|---:|---:|---:|---:|---:|---:|---|---|---|
| O00001 | C0080 | R0018 | 2026-01-19 03:32:00 | completed | NaN | 54.000 | 62.000 | 2.0 | 2 | 3 | 54.000 | True | 2 - Dissatisfied | Text Available |
| O00002 | C0087 | R0023 | 2026-01-10 18:34:00 | completed | NEWUSER | 130.000 | 133.000 | 2.0 | 2 | 5 | 130.000 | True | 2 - Dissatisfied | Text Available |
| O00003 | C0094 | R0003 | 2026-01-12 07:39:00 | completed | HEMAT15 | 238.000 | 233.000 | 1.0 | 4 | 11 | 238.000 | True | 1 - Very Dissatisfied | No Text |
| O00004 | C0101 | R0008 | 2026-02-03 17:42:00 | completed | NaN | 78.000 | 76.000 | NaN | 2 | 5 | 78.000 | False | No Rating | No Review |
| O00005 | C0108 | R0013 | 2026-01-17 09:22:00 | completed | NaN | 123.000 | 120.000 | NaN | 3 | 4 | 123.000 | False | No Rating | No Review |

---

## Methodology


---

## Exploratory Data Analysis & Insights
<img width="1179" height="484" alt="download" src="https://github.com/user-attachments/assets/9ad53474-b1d3-46bb-b457-8c7817aacad1" />

<img width="1180" height="484" alt="image" src="https://github.com/user-attachments/assets/ac95cec3-657b-42e6-93be-a1742e2fe842" />

<img width="884" height="484" alt="image" src="https://github.com/user-attachments/assets/d85ed66c-7a68-4c4b-8a86-8bf7c1430ec4" />

<img width="984" height="484" alt="image" src="https://github.com/user-attachments/assets/b3ee1cd6-4480-4bcf-9354-0d50c0f3cc24" />

<img width="984" height="484" alt="image" src="https://github.com/user-attachments/assets/4f52fcdf-9b70-4256-91e1-439b79c70c4f" />

<img width="984" height="484" alt="image" src="https://github.com/user-attachments/assets/6140fcc4-0943-4e7b-bc52-1d37d2d1028f" />


---

## Dashboard Development



---

## Key Insights & Strategic Recommendations

Bagian ini menyajikan temuan utama berdasarkan hasil Exploratory Data Analysis dan dashboard kinerja bisnis SwiftBite.

Insight difokuskan pada tiga aspek utama:

**1. Business Growth**

Mengidentifikasi perkembangan transaksi dan pendapatan, sumber kontribusi utama, serta pola aktivitas pelanggan dan restoran.

**2. Operational Performance**

Mengidentifikasi kondisi penyelesaian pesanan, ketepatan waktu pengiriman, dan keberhasilan pembayaran untuk mengetahui area operasional yang membutuhkan evaluasi.

**3. Customer Satisfaction**

Mengidentifikasi tingkat kepuasan pelanggan, pola rating, serta perbedaan pengalaman pelanggan berdasarkan karakteristik layanan.

Berdasarkan temuan tersebut, rekomendasi strategis disusun untuk membantu SwiftBite menentukan prioritas perbaikan dan meningkatkan kinerja bisnis secara menyeluruh.

*Insight dan rekomendasi akan dilengkapi setelah seluruh hasil analisis dan validasi data selesai dilakukan.*

---

## Limitations & Notes

Beberapa keterbatasan dan catatan dalam analisis ini meliputi:

- Dataset merepresentasikan aktivitas bisnis SwiftBite selama periode Januari hingga April 2026 sehingga hasil analisis terbatas pada periode pengamatan tersebut.
- Data transaksi dan pelanggan hanya menggambarkan aktivitas yang tercatat pada platform SwiftBite, bukan keseluruhan kondisi pasar food delivery.
- Data rating dan ulasan tidak tersedia untuk seluruh pesanan sehingga analisis kepuasan pelanggan hanya menggunakan data yang memiliki informasi penilaian.
- Analisis ketepatan waktu pengiriman hanya dilakukan pada pesanan yang memiliki informasi waktu aktual dan estimasi pengiriman yang memadai.
- Pengukuran durasi aktual pengiriman menggunakan waktu sejak pesanan diambil oleh kurir hingga diterima pelanggan.
- Perbandingan durasi aktual dan estimasi pengiriman mengasumsikan kedua variabel menggunakan dasar pengukuran waktu yang sebanding.
- Analisis hubungan antarvariabel digunakan untuk mengidentifikasi pola dan asosiasi, bukan untuk membuktikan hubungan sebab-akibat.
- Rekomendasi bisnis disusun berdasarkan karakteristik dan temuan dari dataset yang tersedia. Informasi eksternal seperti ukuran pasar, jumlah kompetitor, dan kondisi demografi wilayah tidak termasuk dalam cakupan analisis.

---

## Technical Resources

Tools dan teknologi yang digunakan dalam proyek ini meliputi:

| Tools | Purpose |
|---|---|
| Python | Data cleaning, data preparation, dan exploratory data analysis |
| Pandas | Manipulasi dan pengolahan data |
| NumPy | Operasi numerik dan perhitungan statistik |
| DuckDB / SQL | Query, transformasi, dan validasi data |
| Matplotlib / Seaborn | Visualisasi data untuk exploratory data analysis |
| Microsoft Power BI | Pengembangan dashboard interaktif |
| DAX | Pembuatan measure dan perhitungan metrik bisnis |
| Jupyter Notebook | Lingkungan pengolahan dan analisis data |

---

**Project Focus:** Business Performance Analysis | Data Analytics | Business Intelligence

**Main Business Question:**

*Is SwiftBite's transaction growth aligned with its operational performance and customer satisfaction?*
