# SwiftBite Business Analytics Project - Bootcamp Data Analyst Plan Indonesia
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
   - [E. Strategi Bisnis](#e-strategi-bisnis)
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


| order_id | customer_id | restaurant_id | order_timestamp | ... | total_amount | rating | ... | has_review | rating_label |
|---|---|---|---|---|---:|---:|---|---|---|
| O00001 | C0080 | R0018 | 2026-01-19 03:32:00 | ... | 62.000 | 2.0 | ... | True | 2 - Dissatisfied |
| O00002 | C0087 | R0023 | 2026-01-10 18:34:00 | ... | 133.000 | 2.0 | ... | True | 2 - Dissatisfied |
| O00003 | C0094 | R0003 | 2026-01-12 07:39:00 | ... | 233.000 | 1.0 | ... | True | 1 - Very Dissatisfied |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

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

Bagaimana perkembangan jumlah pesanan dan nilai transaksi selama periode analisis?

| Bulan | Jumlah | Total Transaksi |
| :--- | :--- | :--- |
| Januari | 140 | Rp21.766.000 |
| Februari | 105 | Rp16.268.000 |
| Maret | 105 | Rp15.757.000 |

<img width="789" height="390" alt="image" src="https://github.com/user-attachments/assets/6ddb92b3-acb4-4f43-bd30-db598261950a" />

<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/7c6d59c3-56a8-454c-999e-a8ee3f1edf49" />

**Temuan:**
- Januari mencatat jumlah pesanan dan total transaksi tertinggi.
- Jumlah pesanan turun sekitar 25% pada Februari dibandingkan Januari.
- Pada Maret, jumlah pesanan tetap, tetapi total transaksi kembali menurun.

<img width="889" height="390" alt="image" src="https://github.com/user-attachments/assets/35a5120d-409f-4a7f-be02-b2fc6e5d880f" />

**Temuan:**
- Senin menjadi hari dengan jumlah pesanan tertinggi (57).
- Sabtu berada di posisi kedua (56).
- Kamis mencatat jumlah pesanan terendah (45).

<img width="989" height="390" alt="image" src="https://github.com/user-attachments/assets/1313e9f7-c277-44c0-b4bd-cd03b8f9b66f" />

**Temuan:**
- Pukul 20.00 mencatat jumlah pesanan tertinggi, yaitu 24 pesanan.
- Pukul 09.00 berada di urutan kedua dengan 20 pesanan.
- Pukul 19.00 mencatat jumlah pesanan terendah, yaitu 7 pesanan.


### 2. Penyelesaian dan Pembatalan Pesanan
Seberapa tinggi tingkat penyelesaian dan pembatalan pesanan?
  
<img width="690" height="390" alt="image" src="https://github.com/user-attachments/assets/f1e6460f-6eac-4dc2-9bd1-ae253b709b64" />
<img width="690" height="390" alt="image" src="https://github.com/user-attachments/assets/11610778-524f-44c2-a4ec-7b068e05d92f" />

**Temuan:**
Berdasarkan data
- 90,86% pesanan berhasil diselesaikan, sedangkan 9,14% dibatalkan.
- Rata-rata nilai transaksi pesanan completed sebesar Rp155.443, lebih tinggi dibandingkan cancelled sebesar Rp136.250


### 3. Kinerja Berdasarkan Restoran dan Segmen Pelanggan
Apakah terdapat perbedaan kinerja berdasarkan restoran dan segmen pelanggan?

|restaurant_id | jumlah_pesanan | total_transaksi | rata_rata_transaksi |
| :--- | :--- | :--- | :--- |
| R0003 |64 | 10276000	| 160562.50 |
| R0018 |64	| 10212000	| 159562.50 |
| R0013 |63	| 9870000	| 156666.67 |
| R0008 |63	| 9571000	| 151920.63 |
| R0023 |64	| 9502000	| 148468.75 |

**Temuan**
- R0003 mencatat total transaksi tertinggi sebesar Rp10.276.000, dengan 64 pesanan.
- R0018 berada di posisi kedua sebesar Rp10.212.000.
- R0023 memiliki total transaksi terendah sebesar Rp9.502.000.
- Jumlah pesanan antar restoran relatif merata, yaitu 63–64 pesanan.

| customer_segment | jumlah_pesanan |	total_transaksi |	rata_rata_transaksi |
| :--- | :--- | :--- | :--- |
| Loyal | 106	| 16960000 |	160000.00 |
| New |	105 |	16783000 |	159838.10 |
| Regular |	107 |	15688000 |	146616.82 |

**Temuan**
- Segmen Loyal menghasilkan total transaksi tertinggi sebesar Rp16.960.000.
- Segmen Regular memiliki jumlah pesanan terbanyak (107), tetapi rata-rata transaksinya paling rendah, yaitu Rp146.617.
- Segmen New menghasilkan rata-rata transaksi Rp159.838, hampir sama dengan segmen Loyal sebesar Rp160.000.

---

## B. Kinerja Pengiriman

### 1. Distribusi Durasi Pengiriman
Seberapa baik SwiftBite memenuhi estimasi waktu pengiriman?

| Delivery Performance | Jumlah Pesanan | Persentase |
|----------------------|---------------:|-----------:|
| On Time              | 280            | 88.05%     |
| Late                 | 38             | 11.95%     |
| **Total**            | **318**        | **100%**   |

| Statistic | Actual Delivery (Minutes) | Promised Delivery (Minutes) | Delay (Minutes) |
|-----------|--------------------------:|----------------------------:|----------------:|
| Count     | 318.00                    | 318.00                      | 318.00          |
| Mean      | 29.23                     | 45.51                       | -16.28          |
| Std       | 15.13                     | 8.71                        | 12.12           |
| Min       | 5.00                      | 30.00                       | -37.00          |
| 25%       | 17.25                     | 38.00                       | -25.00          |
| 50%       | 26.50                     | 45.00                       | -19.00          |
| 75%       | 39.75                     | 53.00                       | -10.00          |
| Max       | 81.00                     | 60.00                       | 32.00           |

**Temuan:**
- Sebanyak 280 pesanan (88,05%) berhasil dikirim sesuai estimasi waktu.
- Sebanyak 38 pesanan (11,95%) mengalami keterlambatan.
- Rata-rata durasi aktual pengiriman adalah 29,23 menit, lebih cepat dibandingkan estimasi 45,51 menit.
- Median keterlambatan sebesar -19 menit, artinya separuh pesanan selesai setidaknya 19 menit lebih cepat dari estimasi.
- Keterlambatan terlama mencapai 32 menit.

### 2. Analisis Keterlambatan Pengiriman
Apakah jarak pengiriman berkaitan dengan durasi dan keterlambatan?

<img width="600" height="440" alt="image" src="https://github.com/user-attachments/assets/f4a5cda1-82a4-479e-82c4-1f4470923204" />


**Temuan:**
Nilai Korelasi (Pearson r):**
*   **0.70 s/d 1.00** atau **-0.70 s/d -1.00**: Hubungan sangat kuat.
*   **0.40 s/d 0.69** atau **-0.40 s/d -0.69**: Hubungan moderat/sedang.
*   **0.10 s/d 0.39** atau **-0.10 s/d -0.39**: Hubungan lemah.
*   **Mendekati 0.00**: Tidak ada hubungan linear.


### 3. Kinerja Pengiriman Berdasarkan Jarak, Restoran, dan Zona
 Restoran atau zona mana yang menunjukkan tingkat keterlambatan lebih tinggi?

| Restaurant | Total Orders | Late Orders | Late Percentage |
|------------|-------------:|------------:|----------------:|
| Partner 08 | 63           | 9           | 14.29%          |
| Partner 23 | 64           | 9           | 14.06%          |
| Partner 13 | 63           | 8           | 12.70%          |
| Partner 18 | 64           | 7           | 10.94%          |
| Partner 03 | 64           | 5           | 7.81%           |

| Customer Zone | Total Orders | Late Orders | Average Delivery Duration (Minutes) | Late Percentage |
|---------------|-------------:|------------:|-------------------------------------:|----------------:|
| East          | 40           | 8           | 29.50                                | 20.00%          |
| North         | 79           | 12          | 30.47                                | 15.19%          |
| Central       | 27           | 3           | 28.33                                | 11.11%          |
| South         | 53           | 5           | 29.19                                | 9.43%           |
| West          | 119          | 10          | 28.53                                | 8.40%           |


**Temuan:**
- East memiliki tingkat keterlambatan tertinggi sebesar 20%, meskipun jumlah pesanannya bukan yang terbanyak.
- North mencatat jumlah pesanan terlambat terbanyak, yaitu 12 pesanan, dengan persentase 15,19%.
- West memiliki jumlah pesanan terbanyak (119), tetapi persentase keterlambatannya paling rendah, yaitu 8,40%.


---

## C. Pengalaman Pelanggan

### 1. Distribusi Rating Pelanggan
Bagaimana tingkat kepuasan pelanggan berdasarkan rating dan ulasan?

<img width="790" height="440" alt="image" src="https://github.com/user-attachments/assets/5f1f7c47-87f8-443e-a9ba-7aeaa570dac1" />

| Rating | Number of Reviews |
|--------|-------------------:|
| 1 - Very Dissatisfied | 13 |
| 2 - Dissatisfied      | 24 |
| 3 - Neutral           | 41 |
| 4 - Satisfied         | 72 |
| 5 - Very Satisfied    | 54 |
| **Total**             | **204** |

**Temuan:**
Rata-rata Rating: 3.64 / 5.00 (Menunjukkan tingkat kepuasan umum yang berada di kategori cukup baik/moderat).
Median Rating: 4 (Sebagian besar pelanggan yang memberikan ulasan merasa Puas).
Tingkat Partisipasi Ulasan: Baru 58.29% dari keseluruhan pesanan yang memberikan ulasan (204 dari 350 pesanan), sedangkan 41.71% pesanan (146 pesanan) tidak menyertakan rating/ulasan (No Rating).

### 2. Hubungan Keterlambatan dan Rating
Apakah keterlambatan pengiriman berkaitan dengan penurunan rating?

| Delivery Performance | Number of Orders | Average Rating | Median Rating |
|----------------------|-----------------:|---------------:|--------------:|
| Late                 | 38               | 2.80           | 3.0           |
| On Time              | 280              | 3.75           | 4.0           |

**Temuan:**
Pesanan yang mengalami keterlambatan cenderung memperoleh rating lebih rendah. Hal ini menunjukkan adanya keterkaitan antara ketepatan waktu pengiriman dan kepuasan pelanggan. Namun, hasil ini belum membuktikan bahwa keterlambatan merupakan satu-satunya penyebab penurunan rating.


### 3. Analisis Ulasan Berdasarkan Segmen Pelanggan
Segmen pelanggan mana yang memberikan penilaian berbeda?

| Customer Segment | Number of Ratings | Average Rating | Median Rating |
|------------------|------------------:|---------------:|--------------:|
| Loyal            | 69                | 3.83           | 4.0           |
| New              | 67                | 3.46           | 4.0           |
| Regular          | 68                | 3.62           | 4.0           |

**Temuan:**
- Segmen Loyal memberikan penilaian tertinggi dengan rata-rata 3,83.
- Segmen Regular memiliki rata-rata rating 3,62.
- Segmen New memberikan penilaian terendah dengan rata-rata 3,46.
- Meskipun rata-rata rating berbeda, ketiga segmen memiliki median yang sama, yaitu 4.

---

## D. Kinerja Pembayaran

### 1. Distribusi Status Pembayaran
Bagaimana tingkat keberhasilan, kegagalan, dan pengembalian pembayaran?

| Payment Status | Number of Transactions | Percentage |
|----------------|-----------------------:|-----------:|
| Successful      | 318                    | 90.86%     |
| Failed          | 21                     | 6.00%      |
| Refunded        | 11                     | 3.14%      |
| **Total**       | **350**                | **100%**   |

**Temuan:**
- Sebanyak 318 transaksi (90,86%) berhasil dibayarkan.
- Sebanyak 21 transaksi (6%) mengalami kegagalan pembayaran.
- Sebanyak 11 transaksi (3,14%) mengalami pengembalian dana (refunded).

### 2. Keberhasilan Pembayaran Berdasarkan Metode
Apakah terdapat perbedaan hasil pembayaran berdasarkan metode pembayaran?

| Payment Method | Failed | Refunded | Successful | Total Transactions | Success Rate |
|----------------|-------:|---------:|----------:|-------------------:|-------------:|
| Bank Transfer  | 5      | 3        | 80        | 88                 | 90.91%       |
| Cash           | 5      | 3        | 79        | 87                 | 90.80%       |
| Credit Card    | 6      | 2        | 79        | 87                 | 90.80%       |
| E-Wallet       | 5      | 3        | 80        | 88                 | 90.91%       |

**Temuan:**
- Bank transfer dan e-wallet memiliki tingkat keberhasilan tertinggi, yaitu 90,91%.
- Cash dan credit card memiliki tingkat keberhasilan sebesar 90,80%.
- Credit card mencatat kegagalan pembayaran terbanyak, yaitu 6 transaksi.
- Perbedaan tingkat keberhasilan antarmetode sangat kecil, hanya 0,11 poin persentase.

### 3. Analisis Percobaan dan Selisih Pembayaran
Bagaimana pola percobaan pembayaran pada transaksi yang gagal?

| Statistic | Attempt Number |
|-----------|---------------:|
| Count     | 21             |
| Mean      | 1.00           |
| Median    | 1.00           |
| Min       | 1.00           |
| Max       | 1.00           |

**Temuan:**
- Terdapat 21 transaksi yang mengalami kegagalan pembayaran.
- Seluruh transaksi gagal tercatat hanya memiliki satu kali percobaan pembayaran.
- Tidak ditemukan variasi jumlah percobaan pada transaksi gagal.

---

## E. Strategi Bisnis

Bagian ini menggabungkan temuan dari analisis pesanan, pengiriman, pengalaman pelanggan, dan pembayaran untuk memperoleh gambaran menyeluruh mengenai kinerja SwiftBite.

### 1. Aspek layanan apa yang paling membutuhkan perhatian?
| Aspek Layanan | Temuan | Hal yang Perlu Diperhatikan |
|---------------|--------|-----------------------------|
| Pesanan | 90,86% pesanan berhasil diselesaikan, sedangkan 9,14% dibatalkan. | Tingkat pembatalan pesanan |
| Pengiriman | 11,95% pesanan mengalami keterlambatan. | Ketepatan waktu pengiriman |
| Restoran | Partner 08 memiliki tingkat keterlambatan 14,29%, diikuti Partner 23 sebesar 14,06%. | Kinerja pengiriman dari masing-masing restoran |
| Zona Pelanggan | Zona East memiliki tingkat keterlambatan tertinggi sebesar 20%, diikuti North sebesar 15,19%. | Kinerja pengiriman di zona East dan North |
| Kepuasan Pelanggan | Pesanan yang terlambat memiliki rata-rata rating 2,80, sedangkan pesanan tepat waktu memiliki rata-rata rating 3,75. | Hubungan antara keterlambatan pengiriman dan kepuasan pelanggan |
| Pembayaran | 6% transaksi gagal dan 3,14% transaksi dikembalikan (*refunded*). | Kegagalan transaksi dan pengembalian pembayaran |

**Temuan**
Prioritas utama perbaikan layanan SwiftBite adalah meningkatkan ketepatan waktu pengiriman, terutama pada restoran dan zona dengan tingkat keterlambatan tertinggi.

### 2. Bagaimana perusahaan dapat memprioritaskan perbaikan berdasarkan hasil analisis?

Berdasarkan hasil analisis A–D, SwiftBite dapat memprioritaskan perbaikan sebagai berikut:
| Prioritas | Aspek yang Diperbaiki | Dasar Hasil Analisis | Tindakan yang Disarankan |
|----------|------------------------|----------------------|--------------------------|
| 1 | Ketepatan Waktu Pengiriman | 11,95% pesanan terlambat dan rata-rata rating pesanan terlambat hanya 2,80. | Evaluasi proses pengiriman, terutama di zona East dan North. |
| 2 | Kinerja Restoran | Partner 08 dan Partner 23 memiliki persentase keterlambatan tertinggi. | Evaluasi koordinasi dan kesiapan pesanan dari kedua restoran. |
| 3 | Kepuasan Pelanggan | Rata-rata rating keseluruhan 3,64 dan rating pesanan terlambat lebih rendah. | Tingkatkan konsistensi kualitas layanan dan penanganan keluhan pelanggan. |
| 4 | Pembayaran | 6% transaksi gagal dan 3,14% transaksi dikembalikan (*refunded*). | Evaluasi penyebab kegagalan dan pengembalian pembayaran. |
| 5 | Pembatalan Pesanan | 9,14% pesanan dibatalkan. | Identifikasi penyebab pembatalan untuk mengurangi transaksi yang tidak selesai. |

---

# Pengembangan Dashboard

**Tools:** Power BI

## SwiftBite Dashboard Interaktif

[View Full Dashboard Preview](SwiftBite/Dashboard/Dashboard1.pdf)


<img width="100%" src="https://github.com/user-attachments/assets/99e84875-214c-49be-97fa-a40a18bb22ac" />



**1. Order Performance**

Tujuan: Mengevaluasi aktivitas pemesanan, pendapatan, dan kontribusi restoran.

- Tren pesanan dan revenue menurun. Pada Januari terdapat 140 pesanan dengan revenue Rp21,77 juta. Jumlah pesanan turun menjadi 105 pada Februari dan tetap 105 pada Maret. Revenue juga turun menjadi Rp16,27 juta dan Rp15,76 juta.

> **Masalah**: Terjadi penurunan aktivitas pemesanan setelah Januari.
> 
> **Insight**: Penurunan jumlah pesanan diikuti penurunan revenue. Hal ini mengindikasikan adanya penurunan aktivitas bisnis, bukan sekadar perubahan nilai transaksi.
> 
> **Rekomendasi**: Evaluasi faktor yang memengaruhi penurunan pesanan, seperti aktivitas promosi, permintaan pelanggan, dan ketersediaan restoran.
>

- Completion rate mencapai 90,86%. Sebanyak 318 dari 350 pesanan berhasil diselesaikan, sedangkan 32 pesanan dibatalkan.

> **Masalah**: Masih terdapat 9,14% pesanan yang tidak berhasil diselesaikan.
> 
> **Insight**: Sebagian besar pesanan berhasil diproses, tetapi pembatalan tetap menyebabkan hilangnya potensi transaksi.
> 
> **Rekomendasi**: Identifikasi penyebab pembatalan untuk mengurangi jumlah pesanan yang gagal diselesaikan.
>

- Kontribusi revenue restoran relatif merata. Partner 03 menghasilkan revenue tertinggi sebesar Rp10,276 juta, sedangkan Partner 08 menghasilkan Rp9,571 juta.
  
> **Masalah**: Belum terlihat satu restoran yang mendominasi kontribusi pendapatan.
> 
> **Insight**: Pendapatan SwiftBite tersebar relatif merata pada lima restoran.
> 
> **Rekomendasi**: Pertahankan kerja sama dengan restoran berkinerja baik dan evaluasi peluang peningkatan penjualan pada restoran dengan kontribusi lebih rendah.
>


**2. Delivery & Customer Experience**

Tujuan: Mengevaluasi ketepatan pengiriman dan kaitannya dengan pengalaman pelanggan.

- Tingkat keterlambatan berbeda antarwilayah. East memiliki late rate tertinggi sebesar 20%, disusul North sebesar 15,19%. West memiliki late rate terendah sebesar 8,40%.
  
> **Masalah**: Tingginya proporsi keterlambatan di East berpotensi mengurangi ketepatan layanan pengiriman.
> 
> **Insight**: East dan North perlu mendapat perhatian lebih karena memiliki proporsi keterlambatan yang lebih tinggi.
>
> **Rekomendasi**: Evaluasi proses pengantaran di kedua wilayah, termasuk alokasi kurir dan kondisi operasional setempat.

  
- Estimasi waktu pengiriman cenderung searah dengan waktu aktual. Scatter plot menunjukkan bahwa pesanan dengan estimasi waktu lebih lama cenderung memiliki durasi aktual lebih lama.


> **Masalah**: Masih terdapat variasi antara estimasi dan durasi aktual.
>
> **Insight**: Estimasi waktu dapat menjadi acuan durasi pengiriman, tetapi belum cukup untuk menyimpulkan tingkat akurasinya.
>
> **Rekomendasi**: Pantau selisih waktu aktual dan estimasi untuk mengevaluasi ketepatan prediksi waktu pengiriman.
>

- Pesanan terlambat memiliki rating lebih rendah. Rata-rata rating pesanan Late adalah 2,80, sedangkan On-Time mencapai 3,75.

> **Masalah**: Pesanan terlambat berkaitan dengan penilaian pelanggan yang lebih rendah.
>
> **Insight**: Ketepatan waktu pengiriman merupakan salah satu aspek yang berkaitan dengan pengalaman pelanggan.
>
> **Rekomendasi**: Prioritaskan perbaikan proses pengiriman untuk mengurangi keterlambatan dan menjaga kepuasan pelanggan.

- Rating pelanggan rata-rata 3,64 dari 5.

> **Masalah**: Masih terdapat ruang untuk meningkatkan pengalaman pelanggan.
> 
> **Insight**: Rating dapat digunakan sebagai indikator evaluasi layanan, terutama jika dikaitkan dengan performa pengiriman.
>
> **Rekomendasi**: Evaluasi ulasan pelanggan dan identifikasi faktor yang paling sering berkaitan dengan rating rendah.

**3. Payment Performance**

Tujuan: Mengevaluasi keberhasilan transaksi dan nominal pembayaran berdasarkan metode yang digunakan.

- Mayoritas pembayaran berhasil diproses. Sebanyak 318 transaksi successful (90,86%), 21 failed (6%), dan 11 refunded (3,14%).

> **Masalah**: Masih terdapat transaksi gagal dan pengembalian dana.
> 
> **Insight**: Proses pembayaran secara umum berjalan baik, tetapi 32 transaksi memerlukan perhatian lebih lanjut.
> 
> **Rekomendasi**: Evaluasi penyebab kegagalan transaksi dan refund agar proses pembayaran lebih lancar.
> 

- Jumlah pembayaran berhasil relatif merata antar-metode. Bank Transfer dan E-Wallet masing-masing mencatat 80 transaksi successful, sedangkan Cash dan Credit Card masing-masing 79 transaksi.

> **Masalah**: Credit Card memiliki transaksi gagal terbanyak, yaitu 6 transaksi.
> 
> **Insight**: Perbedaan jumlah transaksi gagal antar-metode relatif kecil, sehingga belum cukup untuk menyimpulkan adanya masalah khusus pada Credit Card.
> 
> **Rekomendasi**: Pantau tingkat keberhasilan setiap metode pembayaran dan evaluasi penyebab transaksi gagal.
>

- Rata-rata nominal pembayaran berbeda antar-metode. Setelah filter successful dan completed diterapkan, visual Average Payment by Payment Method dapat digunakan untuk membandingkan rata-rata nominal transaksi.
  
> **Masalah**: Perbedaan rata-rata nominal belum tentu menunjukkan perbedaan kontribusi total revenue.
> 
> **Insight**: Metode dengan rata-rata nominal lebih tinggi belum tentu menghasilkan total pendapatan terbesar.
> 
> **Rekomendasi**: Bandingkan rata-rata nominal dengan jumlah transaksi dan total payment amount untuk memahami kontribusi setiap metode secara lebih menyeluruh.

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

**Notebook:** [Google Colab Notebook](SwiftBite/Notebooks/CAPSTONE_BOOTCAMP.ipynb)
---

**Proyek oleh:** Firdha Hanifah Suparlan

*Proyek ini disusun untuk tujuan pembelajaran dan portofolio.*
