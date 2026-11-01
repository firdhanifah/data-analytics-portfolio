# SwiftBite Food Delivery Analysis

Analisis data pada platform pemesanan dan pengantaran makanan SwiftBite untuk mengevaluasi performa transaksi, restoran, pelanggan, pembayaran, pengiriman, dan kepuasan pelanggan melalui proses data preparation, exploratory data analysis (EDA), serta dashboard interaktif.

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

SwiftBite merupakan platform pemesanan dan pengantaran makanan yang menghubungkan pelanggan dengan restoran melalui layanan pemesanan online. Kinerja bisnis food delivery tidak hanya ditentukan oleh jumlah pesanan dan pendapatan, tetapi juga oleh efisiensi operasional, keberhasilan pembayaran, ketepatan waktu pengantaran, serta kepuasan pelanggan.

Dalam industri food delivery yang kompetitif, pemahaman terhadap keseluruhan proses transaksi menjadi penting untuk mengevaluasi performa bisnis dan mengidentifikasi peluang perbaikan. Data pesanan yang terintegrasi dengan informasi pelanggan, restoran, item pesanan, pembayaran, pengantaran, dan ulasan memungkinkan analisis dilakukan secara menyeluruh dari sisi bisnis maupun operasional.

Melalui proyek ini, data SwiftBite dianalisis untuk memahami pola transaksi, mengevaluasi kualitas layanan, serta mengidentifikasi faktor-faktor yang berpotensi memengaruhi pengalaman pelanggan dan kinerja bisnis.

### Business Problem

Kinerja platform food delivery dipengaruhi oleh berbagai aspek yang saling berkaitan, mulai dari aktivitas pemesanan hingga pesanan diterima oleh pelanggan. Namun, tingginya jumlah transaksi tidak selalu mencerminkan operasional yang efisien atau pengalaman pelanggan yang memuaskan.

Beberapa permasalahan bisnis yang perlu dievaluasi meliputi:

- **Kinerja transaksi:** belum diketahui bagaimana tren pesanan dan nilai transaksi berkembang serta restoran atau segmen pelanggan mana yang memberikan kontribusi terbesar.
- **Efisiensi operasional:** keterlambatan pengantaran dapat memengaruhi kualitas layanan dan pengalaman pelanggan.
- **Keandalan pembayaran:** percobaan pembayaran, transaksi gagal, dan pengembalian dana perlu dievaluasi untuk memahami efektivitas proses pembayaran.
- **Kepuasan pelanggan:** rating dan ulasan dapat memberikan gambaran mengenai pengalaman pelanggan serta aspek layanan yang perlu ditingkatkan.
- **Kualitas pengambilan keputusan:** tanpa analisis yang terintegrasi, penentuan prioritas perbaikan berisiko hanya berfokus pada satu aspek bisnis tanpa mempertimbangkan keterkaitannya dengan aspek lain.

Oleh karena itu, diperlukan analisis data yang mengintegrasikan aspek transaksi, pelanggan, restoran, pembayaran, pengantaran, dan ulasan untuk mendukung pengambilan keputusan yang lebih terarah.

### Project Objectives

Proyek ini bertujuan untuk mengevaluasi kinerja bisnis dan operasional SwiftBite melalui analisis data transaksi dan layanan pengantaran makanan.

Tujuan khusus proyek meliputi:

1. Menganalisis tren jumlah pesanan dan nilai transaksi untuk memahami kinerja bisnis.
2. Mengidentifikasi kontribusi restoran, jenis masakan, zona, dan segmen pelanggan terhadap aktivitas transaksi.
3. Mengevaluasi status pesanan, keberhasilan pembayaran, serta pengembalian dana.
4. Mengukur ketepatan waktu pengantaran dan mengidentifikasi pola keterlambatan berdasarkan karakteristik operasional.
5. Menganalisis rating dan ulasan pelanggan untuk mengevaluasi pengalaman pelanggan.
6. Mengembangkan visualisasi dan dashboard interaktif untuk menyajikan hasil analisis secara informatif.
7. Menyusun rekomendasi berbasis data untuk mendukung peningkatan efisiensi operasional dan kualitas layanan.

### Key Business Questions

**A. Kinerja Bisnis dan Transaksi**

- Bagaimana tren jumlah pesanan dan nilai transaksi selama periode pengamatan?
- Restoran, jenis masakan, zona, dan segmen pelanggan mana yang memberikan kontribusi terbesar terhadap transaksi?
- Bagaimana distribusi status pesanan, dan berapa proporsi pesanan yang berhasil diselesaikan?
- Bagaimana nilai rata-rata transaksi berbeda berdasarkan karakteristik pesanan atau restoran?

**B. Pembayaran dan Kinerja Finansial**

- Seberapa tinggi tingkat keberhasilan pembayaran pada transaksi SwiftBite?
- Metode pembayaran apa yang paling banyak digunakan pelanggan?
- Berapa nilai pembayaran bersih setelah memperhitungkan pengembalian dana?
- Bagaimana diskon dan biaya pengantaran berkontribusi terhadap komposisi nilai transaksi?

**C. Efisiensi Pengantaran**

- Berapa proporsi pesanan yang terlambat dibandingkan dengan pesanan yang dikirim tepat waktu?
- Bagaimana selisih waktu pengantaran aktual dengan waktu yang dijanjikan?
- Zona, jarak pengantaran, atau kurir mana yang menunjukkan pola keterlambatan lebih tinggi?
- Apakah keterlambatan pengantaran berkaitan dengan perbedaan rating pelanggan?

**D. Pengalaman dan Kepuasan Pelanggan**

- Bagaimana distribusi rating yang diberikan pelanggan?
- Restoran atau kelompok pesanan mana yang memiliki rata-rata rating relatif rendah?
- Apa keluhan yang muncul dalam ulasan pelanggan?
- Aspek layanan apa yang perlu diprioritaskan untuk meningkatkan pengalaman pelanggan?

**E. Arah Strategis**

- Aspek bisnis dan operasional mana yang perlu mendapatkan prioritas perbaikan?
- Bagaimana hasil analisis dapat digunakan untuk meningkatkan kualitas pengantaran dan kepuasan pelanggan?
- Rekomendasi apa yang dapat dipertimbangkan untuk meningkatkan kinerja bisnis secara berkelanjutan?

### Expected Business Value

Hasil analisis diharapkan memberikan gambaran yang lebih menyeluruh mengenai kinerja transaksi, efisiensi operasional, proses pembayaran, dan pengalaman pelanggan SwiftBite.

Nilai bisnis yang diharapkan meliputi:

- **Pengambilan keputusan berbasis data:** menyediakan informasi terstruktur mengenai performa bisnis sebagai dasar penentuan prioritas.
- **Peningkatan efisiensi operasional:** membantu mengidentifikasi pola keterlambatan dan area pengantaran yang memerlukan evaluasi.
- **Optimalisasi kinerja transaksi:** memberikan gambaran mengenai kontribusi restoran, zona, dan segmen pelanggan.
- **Perbaikan proses pembayaran:** membantu mengevaluasi keberhasilan transaksi dan dampak pengembalian dana.
- **Peningkatan pengalaman pelanggan:** mengidentifikasi pola rating dan ulasan yang dapat menjadi masukan untuk perbaikan layanan.
- **Komunikasi hasil analisis yang lebih efektif:** menyajikan temuan melalui dashboard interaktif agar informasi lebih mudah dipahami oleh pemangku kepentingan.

Secara keseluruhan, proyek ini diharapkan dapat mengubah data operasional SwiftBite menjadi informasi yang relevan untuk mendukung evaluasi kinerja dan penyusunan strategi perbaikan bisnis.


---

## Data Sources
Proyek ini menggunakan beberapa dataset yang merepresentasikan aspek berbeda dari proses bisnis SwiftBite. Dataset tersedia dalam format file yang beragam sehingga setiap file perlu dibaca menggunakan metode yang sesuai.
| No. | Dataset | Format | Deskripsi |
|---|---|---|---|
| 1 | Orders | CSV | Data transaksi pesanan |
| 2 | Order items | CSV | Detail item dalam pesanan |
| 3 | Customers | JSON | Data pelanggan |
| 4 | Payments | TXT | Data pembayaran |
| 5 | Restaurants | XLSX | Data restoran |
| 6 | Delivery | CSV | Data Pengiriman |
| 7 | Reviews | CSV | Data Ulasan Pelangan |

## Data Cleaning & Preparation
Proses pembersihan dan persiapan data dilakukan untuk memastikan kualitas, konsistensi, dan kesiapan data sebelum dianalisis.

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

## Methodology

## Exploratory Data Analysis & Insights

### A. Business Performance Overview

### B. Sales & Restaurant Analysis

### C. Customer Analysis

### D. Delivery Performance

### E. Payment & Customer Satisfaction

---

## Dashboard Development

## Key Insights & Strategic Recommendations

## Limitations & Notes

## Technical Resources
