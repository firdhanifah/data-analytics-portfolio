# NusaMart Data Profiling Report — Sesi 02

**Scope Dataset**: `orders`, `order_items`, `customers`, `payments`  
**Execution Type**: Automated Profiling Baseline (Antigravity)  
**Tujuan**: Mengidentifikasi metadata struktural, keunikan candidate key, kelengkapan data, variasi representasi kategori/tanggal/angka, serta candidate issues untuk diverifikasi manual oleh peserta.  
**Prinsip Kerja**: Detect → Measure → Verify (Tanpa data cleaning, tanpa standardisasi sepihak, tanpa relational join kompleks).

---

## 1. Executive Profiling Summary

Profiling dilakukan terhadap 4 logical datasets NusaMart yang telah di-load ke dalam memori dari berbagai format sumber raw (`.xlsx`, `.csv`, `.json`, `.txt`). Keempat dataset ini merepresentasikan entitas bisnis e-commerce utama: pesanan (*orders*), rincian produk pesanan (*order items*), master pelanggan (*customers*), dan transaksi pembayaran (*payments*).

### Rangkuman Utama Hasil Profiling:
1. **Integritas Struktur & Skema Dasar**: Keempat file berhasil dimuat secara utuh dengan skema kolom yang sesuai spesifikasi. Total record yang terbaca: Orders (320 baris), Order Items (763 baris), Customers (215 objek), Payments (337 baris).
2. **Kelengkapan Data (Completeness)**: Tingkat kelengkapan data sangat tinggi. Dari seluruh kolom pada keempat dataset, hanya field `customer_segment` pada dataset `customers` yang memiliki missing/null values sebanyak 4 record (1.86%). Seluruh field lainnya memiliki kelengkapan 100% (0 missing/blank).
3. **Keunikan & Candidate Keys**:
   - `orders` (`order_id`): 100% unik (320 distinct dari 320 baris, 0 duplikat).
   - `customers` (`customer_id`): 100% unik (215 distinct dari 215 baris, 0 duplikat).
   - `order_items` (`order_item_id`): Ditemukan **3 candidate key berulang** (`ITM00130`, `ITM00215`, `ITM00464`), masing-masing muncul 2 kali (total 6 baris, 3 baris ekstra duplikat). Ketiga pasang baris ini teridentifikasi sebagai *exact full-row duplicates*.
   - `payments` (`payment_id`): Ditemukan **2 candidate key berulang** (`PAY0095`, `PAY0206`), masing-masing muncul 2 kali (total 4 baris, 2 baris ekstra duplikat). Kedua pasang baris ini juga teridentifikasi sebagai *exact full-row duplicates*.
4. **Konsistensi Label Kategori**:
   - `sales_channel` pada `orders`: Ditemukan 6 variasi penulisan non-standar untuk kanal *Marketplace* (variasi spasi, huruf kecil, huruf kapital).
   - `customer_segment` pada `customers`: Ditemukan 6 variasi label non-standar (`regular`, `new customer`, `REG`, `vip`) di luar kategori kanonikal (`Regular`, `New`, `VIP`).
5. **Representasi Format Tanggal**:
   - Seluruh tanggal (100%) pada keempat dataset berhasil diparsing tanpa kegagalan (0 *unresolved dates*).
   - Namun, ditemukan variasi format teks: pada `orders` terdapat 4 baris teks tanggal (`DD/MM/YYYY`, `YYYY/MM/DD`, `DD-MM-YYYY`), dan pada `payments` terdapat 3 baris format non-standar (`DD-MM-YYYY`, `YYYY/MM/DD`, `DD/MM/YYYY`).
6. **Representasi Angka & Validitas Numerik**:
   - Ditemukan pemisah ribuan koma (`,`) pada representasi string angka: 2 baris pada `unit_price`, 1 baris pada `discount_amount` (keduanya pada `order_items`), serta 3 baris pada `payment_amount` (pada `payments`). Hal ini menyebabkan tipe data terinferensi sebagai string dan gagal dicasting ke numerik murni tanpa penanganan awal.
   - Ditemukan 1 nilai negatif pada `payment_amount` (`-344000` pada `PAY0182`), yang memerlukan verifikasi manual terkait konteks bisnis (refund/reversal vs kesalahan input).
   - Ditemukan 2 kuantitas produk yang tidak biasa (*unusually high*) pada `order_items`: kuantitas 24 (`ITM00454`) dan 30 (`ITM00676`), berbanding rata-rata kuantitas 1.48.
7. **Kronologi Transaksi**:
   - Ditemukan 1 indikasi isu kronologis logis pada `payments`: record `PAY0335` tercatat bertanggal `2025-12-27` untuk pesanan `ORD0320` yang tanggal pesanannya adalah `2026-01-06` (pembayaran mendahului pembuatan order 10 hari).

---

## 2. Orders Profiling

### 2.1 Dataset Overview
- **Source File**: `data/raw/nusamart/nusamart_orders.xlsx` (Sheet: `orders_raw`)
- **Physical Format**: Microsoft Excel OpenXML Spreadsheet (`.xlsx`)
- **Logical Record Structure**: 1 row = 1 order
- **Total Records / Rows**: 320
- **Total Columns / Fields**: 8

| Field Name | Inferred Data Type | Deskripsi Domain |
| :--- | :--- | :--- |
| `order_id` | String / Alphanumeric Code | Identifier unik pesanan (`ORDxxxx`) |
| `customer_id` | String / Alphanumeric Code | Identifier referensi pelanggan (`CUSxxx`) |
| `order_date` | Date / DateTime | Tanggal transaksi pesanan dilakukan |
| `sales_channel` | String / Categorical | Saluran penjualan tempat pesanan dibuat |
| `order_status` | String / Categorical | Status operasional/pemrosesan pesanan |
| `shipping_city` | String / Categorical | Kota tujuan pengiriman |
| `shipping_fee` | Numeric (Integer, IDR) | Biaya ongkos kirim pesanan |
| `order_total` | Numeric (Integer, IDR) | Total nilai transaksi pesanan (IDR) |

#### Contoh Record (Sample 3 Baris Pertama):
| order_id | customer_id | order_date | sales_channel | order_status | shipping_city | shipping_fee | order_total |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ORD0001 | CUS024 | 2026-03-25 00:00:00 | Mobile App | Completed | Jakarta | 20000 | 1450000 |
| ORD0002 | CUS163 | 2026-02-19 00:00:00 | Mobile App | Processing | Yogyakarta | 40000 | 641000 |
| ORD0003 | CUS202 | 2026-03-12 00:00:00 | Mobile App | Completed | Malang | 25000 | 482000 |

---

### 2.2 Missing / Blank Check
Pemeriksaan dilakukan terhadap keberadaan `None`, `NaN`, serta string kosong/whitespace (`""`).

| Field Name | Missing / Blank Count | Persentase (%) | Status Kelengkapan |
| :--- | :---: | :---: | :--- |
| `order_id` | 0 | 0.00% | Lengkap (100%) |
| `customer_id` | 0 | 0.00% | Lengkap (100%) |
| `order_date` | 0 | 0.00% | Lengkap (100%) |
| `sales_channel` | 0 | 0.00% | Lengkap (100%) |
| `order_status` | 0 | 0.00% | Lengkap (100%) |
| `shipping_city` | 0 | 0.00% | Lengkap (100%) |
| `shipping_fee` | 0 | 0.00% | Lengkap (100%) |
| `order_total` | 0 | 0.00% | Lengkap (100%) |

- **Field dengan missing terbesar**: Tidak ada. Seluruh 8 kolom memiliki kelengkapan 100%.

---

### 2.3 Candidate Key / Uniqueness Check
- **Candidate Key yang Diuji**: `order_id`
- **Grain Statement**: 1 record (row) = 1 pesanan (*order*)
- **Null Count**: 0 (0.00%)
- **Distinct Count**: 320
- **Duplicate Count**: 0
- **Exact Full-Row Duplicates**: 0 baris
- **Status Uniqueness**: `True` (100% unik pada raw source)
- **Candidate Key Finding**: `order_id` memenuhi kriteria primary candidate key secara sempurna tanpa ada collision atau duplikasi pada raw level.

---

### 2.4 Category / Consistency Check

#### A. Field `sales_channel` (Total: 320 records, 9 distinct tokens)
Kategori kanonikal yang diharapkan adalah `Mobile App`, `Web`, dan `Marketplace`.

| Nilai Label Kategori | Record Count | Persentase (%) | Catatan Observasi Konsistensi |
| :--- | :---: | :---: | :--- |
| `Mobile App` | 128 | 40.00% | Nilai kanonikal standar |
| `Web` | 112 | 35.00% | Nilai kanonikal standar |
| `Marketplace` | 74 | 23.13% | Nilai kanonikal standar |
| `Marketplace ` | 1 | 0.31% | **Variasi**: trailing whitespace di akhir string |
| `marketplace` | 1 | 0.31% | **Variasi**: lowercase (huruf kecil semua) |
| `market place` | 1 | 0.31% | **Variasi**: pemisahan dua kata dengan spasi (lowercase) |
| `Market Place` | 1 | 0.31% | **Variasi**: pemisahan dua kata dengan spasi (title case) |
| `MARKET PLACE` | 1 | 0.31% | **Variasi**: uppercase dengan pemisahan spasi |
| `MARKETPLACE` | 1 | 0.31% | **Variasi**: uppercase tanpa spasi |

- **Sample Record Key untuk Variasi `sales_channel`**:
  - `ORD0041`: `'Marketplace '`
  - `ORD0053`: `'marketplace'`
  - `ORD0062`: `'market place'`
  - `ORD0070`: `'Market Place'`
  - `ORD0265`: `'MARKET PLACE'`
  - `ORD0311`: `'MARKETPLACE'`

#### B. Field `order_status` (Total: 320 records, 3 distinct tokens)
| Nilai Status | Record Count | Persentase (%) | Status Observasi |
| :--- | :---: | :---: | :--- |
| `Completed` | 260 | 81.25% | Standar konsisten |
| `Processing` | 35 | 10.94% | Standar konsisten |
| `Cancelled` | 25 | 7.81% | Standar konsisten |

#### C. Field `shipping_city` (Total: 320 records, 10 distinct tokens)
Distribusi 10 kota tujuan: Semarang (46), Yogyakarta (45), Depok (39), Bogor (38), Tangerang (36), Bekasi (29), Malang (27), Bandung (23), Surabaya (21), Jakarta (16). Tidak ditemukan variasi penulisan kota non-standar.

---

### 2.5 Date / Time Check
- **Field yang Diperiksa**: `order_date`
- **Parseability**: 320/320 record (100%) dapat diparsing menjadi tanggal valid.
- **Unresolved / Unparseable Dates**: 0 record.
- **Rentang Tanggal (Min & Max)**:
  - **Minimum Date**: `2026-01-01 00:00:00` (e.g., `ORD0008`)
  - **Maximum Date**: `2026-04-02 00:00:00` (e.g., `ORD0318`)
- **Variasi Representasi Format Fisik**:
  - Format Datetime Object (Timestamp): 316 baris (98.75%)
  - Format String Teks: 4 baris (1.25%), dengan rincian:
    - `14/02/2026` (`DD/MM/YYYY`) pada record `ORD0027`
    - `2026/01/25` (`YYYY/MM/DD`) pada record `ORD0164`
    - `04/02/2026` (`DD/MM/YYYY`) pada record `ORD0197`
    - `15-03-2026` (`DD-MM-YYYY`) pada record `ORD0263`
- **Distingsi Format vs Kronologi**:
  - *Format Issue*: Teridentifikasi 4 baris memiliki representasi string yang berbeda dari mayoritas timestamp Excel.
  - *Logical Chronology Issue*: Tidak ditemukan anomali kronologis internal pada tabel orders; seluruh tanggal berada dalam horizon transaksi kuartal pertama tahun 2026 (Jan–Apr 2026).

---

### 2.6 Numeric Check
Pemeriksaan dilakukan terhadap field `shipping_fee` dan `order_total`.

| Numeric Metric | `shipping_fee` | `order_total` |
| :--- | :---: | :---: |
| Invalid / Non-numeric Count | 0 | 0 |
| Negative Values Count (< 0) | 0 | 0 |
| Zero Values Count (= 0) | 26 | 0 |
| Nilai Minimum (Min) | 0 | 38,000 |
| Nilai Rata-rata (Mean) | 21,656.25 | 821,350.00 |
| Nilai Maksimum (Max) | 40,000 | 5,001,000 |
| Sample Nilai Tertinggi | 40,000 (`ORD0002`) | 5,001,000 (`ORD0045`) |
| Sample Nilai Terendah | 0 (`ORD0007`) | 38,000 (`ORD0142`) |

- **Observasi Nilai Ekstrem/Nol**:
  - `shipping_fee = 0`: Ditemukan pada 26 pesanan (8.13%). Hal ini lazim dalam domain e-commerce sebagai fasilitas program promo *free shipping* (ongkir gratis).
  - `order_total`: Nilai tertinggi Rp5.001.000 dan terendah Rp38.000. Seluruh nilai bernilai positif.

---

### 2.7 Business Logic Candidate Check
- **Rentang Transaksi**: Tidak ada nilai negatif pada total order maupun ongkir.
- **Kondisi Status Pesanan**: Terdapat 25 pesanan berstatus `Cancelled`. Dalam tahapan berikutnya, perlu diperiksa apakah pesanan yang dibatalkan memiliki implikasi terhadap pencatatan pembayaran terkait.

---

## 3. Order Items Profiling

### 3.1 Dataset Overview
- **Source File**: `data/raw/nusamart/nusamart_order_items.csv`
- **Physical Format**: Comma-Separated Values (`.csv`, encoding UTF-8)
- **Logical Record Structure**: 1 row = 1 item dalam pesanan
- **Total Records / Rows**: 763
- **Total Columns / Fields**: 8

| Field Name | Inferred Data Type | Deskripsi Domain |
| :--- | :--- | :--- |
| `order_item_id` | String / Alphanumeric Code | Identifier unik baris item (`ITMxxxxx`) |
| `order_id` | String / Alphanumeric Code | Identifier referensi pesanan (`ORDxxxx`) |
| `product_id` | String / Alphanumeric Code | Identifier referensi produk (`PRDxxx`) |
| `product_category` | String / Categorical | Kategori produk yang dipesan |
| `quantity` | Numeric / Text Integer | Jumlah kuantitas produk yang dibeli |
| `unit_price` | Numeric / Text Currency | Harga satuan produk per unit (IDR) |
| `discount_amount` | Numeric / Text Currency | Besaran diskon yang diterapkan pada baris item (IDR) |
| `line_amount` | Numeric (Integer, IDR) | Nilai total transaksi baris item (IDR) |

#### Contoh Record (Sample 3 Baris Pertama):
| order_item_id | order_id | product_id | product_category | quantity | unit_price | discount_amount | line_amount |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ITM00001 | ORD0001 | PRD024 | Electronics | 1 | 1343000 | 0 | 1343000 |
| ITM00002 | ORD0001 | PRD007 | Grocery | 1 | 65000 | 5000 | 60000 |
| ITM00003 | ORD0001 | PRD043 | Grocery | 1 | 27000 | 0 | 27000 |

---

### 3.2 Missing / Blank Check
| Field Name | Missing / Blank Count | Persentase (%) | Status Kelengkapan |
| :--- | :---: | :---: | :--- |
| `order_item_id` | 0 | 0.00% | Lengkap (100%) |
| `order_id` | 0 | 0.00% | Lengkap (100%) |
| `product_id` | 0 | 0.00% | Lengkap (100%) |
| `product_category` | 0 | 0.00% | Lengkap (100%) |
| `quantity` | 0 | 0.00% | Lengkap (100%) |
| `unit_price` | 0 | 0.00% | Lengkap (100%) |
| `discount_amount` | 0 | 0.00% | Lengkap (100%) |
| `line_amount` | 0 | 0.00% | Lengkap (100%) |

- **Field dengan missing terbesar**: Tidak ada (0 missing di semua kolom).

---

### 3.3 Candidate Key / Uniqueness Check
- **Candidate Key yang Diuji**: `order_item_id`
- **Grain Statement**: 1 record (row) = 1 item dalam 1 pesanan
- **Null Count**: 0 (0.00%)
- **Distinct Count**: 760
- **Duplicate Count (Repeated Distinct Keys)**: 3 key berulang
- **Total Baris di Bawah Key Berulang**: 6 baris (3 key × 2 baris)
- **Baris Ekstra Duplikat**: 3 baris ekstra (0.39% dari total 763 baris)
- **Status Uniqueness**: `False`

#### Rincian Key yang Berulang:
1. `ITM00130` (muncul 2 kali pada pesanan `ORD0058`):
   - Baris 1: `order_id=ORD0058`, `product_id=PRD011`, `category=Sports`, `qty=1`, `unit_price=379000`, `discount=11000`, `line_amount=368000`
   - Baris 2: `order_id=ORD0058`, `product_id=PRD011`, `category=Sports`, `qty=1`, `unit_price=379000`, `discount=11000`, `line_amount=368000`
2. `ITM00215` (muncul 2 kali pada pesanan `ORD0090`):
   - Baris 1: `order_id=ORD0090`, `product_id=PRD013`, `category=Grocery`, `qty=1`, `unit_price=51000`, `discount=6000`, `line_amount=45000`
   - Baris 2: `order_id=ORD0090`, `product_id=PRD013`, `category=Grocery`, `qty=1`, `unit_price=51000`, `discount=6000`, `line_amount=45000`
3. `ITM00464` (muncul 2 kali pada pesanan `ORD0199`):
   - Baris 1: `order_id=ORD0199`, `product_id=PRD008`, `category=Beauty`, `qty=2`, `unit_price=172000`, `discount=34000`, `line_amount=310000`
   - Baris 2: `order_id=ORD0199`, `product_id=PRD008`, `category=Beauty`, `qty=2`, `unit_price=172000`, `discount=34000`, `line_amount=310000`

- **Observasi Full-Row Exact Duplicates**: Seluruh atribut dari ketiga pasang baris di atas bernilai identik dari ujung ke ujung (*exact duplicate rows*). 
- **Candidate Key Finding**: Candidate key `order_item_id` mengalami pelanggaran keunikan sebanyak 3 key (6 baris), yang disebabkan oleh keberadaan baris duplikat identik di file sumber.

---

### 3.4 Category / Consistency Check

#### Field `product_category` (Total: 763 records, 6 distinct categories)
| Kategori Produk | Record Count | Persentase (%) | Catatan Konsistensi |
| :--- | :---: | :---: | :--- |
| `Grocery` | 188 | 24.64% | Standar konsisten |
| `Fashion` | 126 | 16.51% | Standar konsisten |
| `Beauty` | 121 | 15.86% | Standar konsisten |
| `Home & Living` | 117 | 15.33% | Standar konsisten |
| `Sports` | 109 | 14.29% | Standar konsisten |
| `Electronics` | 102 | 13.37% | Standar konsisten |

- **Observasi Kategori**: Penulisan label kategori produk 100% konsisten pada 6 kelompok produk standar.

---

### 3.5 Date / Time Check
Dataset `order_items` tidak memuat atribut tanggal/waktu transaksi.

---

### 3.6 Numeric Check

| Numeric Metric | `quantity` | `unit_price` | `discount_amount` | `line_amount` |
| :--- | :---: | :---: | :---: | :---: |
| Invalid / Non-numeric Format | 0 | 2 baris | 1 baris | 0 |
| Negative Values Count (< 0) | 0 | 0 | 0 | 0 |
| Zero Values Count (= 0) | 0 | 0 | 465 baris | 0 |
| Nilai Minimum (Min) | 1 | 19,000 | 0 | 18,000 |
| Nilai Rata-rata (Mean) | 1.48 | 469,067.11 | 25,622.05 | 620,297.51 |
| Nilai Maksimum (Max) | 30 | 1,348,000 | 424,000 | 3,800,000 |
| Sample Nilai Tertinggi | 30 (`ITM00676`) | 1,348,000 (`ITM00705`) | 424,000 (`ITM00539`) | 3,800,000 (`ITM00539`) |

#### Observasi Khusus Representasi & Nilai Ekstrem Numerik:
1. **Pemisah Ribuan Karakter Koma (`,`) pada String Angka**:
   - `unit_price` memiliki 2 record dengan tanda koma:
     - `ITM00481`: `'486,000'`
     - `ITM00611`: `'50,000'`
   - `discount_amount` memiliki 1 record dengan tanda koma:
     - `ITM00191`: `'15,000'`
   - *Dampak*: Menghalangi inferensi tipe numerik murni oleh parser standar jika dibaca tanpa sanitasi karakter koma.
2. **Whitespace Padding pada Field `quantity`**:
   - `ITM00181` (`ORD0077`): string `' 1 '` (terdapat spasi pembuka dan penutup).
   - `ITM00314` (`ORD0131`): string `' 2 '` (terdapat spasi pembuka dan penutup).
3. **Kuantitas Pembelian Ekstrem (*Unusually High Values*)**:
   - Distribusi kuantitas mayoritas berada pada rentang 1 hingga 4 (kuantitas 1: 561 baris; 2: 145 baris; 3: 49 baris; 4: 6 baris).
   - Terdapat 2 record dengan kuantitas jauh di atas rata-rata:
     - `ITM00454` (`ORD0194`): kuantitas = `24` (Produk `PRD025` - Grocery, line amount Rp517.000).
     - `ITM00676` (`ORD0288`): kuantitas = `30` (Produk `PRD025` - Grocery, line amount Rp553.000).
   - *Catatan Observasi*: Kuantitas 24 dan 30 terjadi pada produk sembako (*Grocery*), yang secara bisnis sangat mungkin merupakan pembelian grosir/karton (*bulk buying*), sehingga tidak otomatis merupakan data-entry error.

---

### 3.7 Business Logic Candidate Check
- **Formula Baris Item**: `line_amount = (quantity * unit_price) - discount_amount`.
- **Hasil Pemeriksaan**:
  - Jika dievaluasi secara teks mentah, terdapat 3 baris yang gagal dihitung akibat format koma (`ITM00191`, `ITM00481`, `ITM00611`).
  - Setelah koma diabaikan untuk verifikasi logika perhitungan:
    - `ITM00191`: `(3 * 103,000) - 15,000 = 309,000 - 15,000 = 294,000` (Cocok dengan `line_amount=294000`).
    - `ITM00481`: `(1 * 486,000) - 0 = 486,000` (Cocok dengan `line_amount=486000`).
    - `ITM00611`: `(1 * 50,000) - 2,000 = 48,000` (Cocok dengan `line_amount=48000`).
  - **Kesimpulan Bisnis**: Logika aritmatika nilai baris item pada 763 record 100% konsisten. Isu yang timbul murni merupakan representasi penulisan teks (*formatting representation issue*), bukan inkonsistensi rumus matematis.

---

## 4. Customers Profiling

### 4.1 Dataset Overview
- **Source File**: `data/raw/nusamart/nusamart_customers.json`
- **Physical Format**: JavaScript Object Notation (`.json`, JSON Array of Objects)
- **Logical Record Structure**: 1 object = 1 customer
- **Total Records / Objects**: 215
- **Total Columns / Fields**: 8

| Field Name | Inferred Data Type | Deskripsi Domain |
| :--- | :--- | :--- |
| `customer_id` | String / Alphanumeric Code | Identifier unik pelanggan (`CUSxxx`) |
| `customer_name` | String / Personal Name | Nama lengkap pelanggan (PII) |
| `city` | String / Categorical | Kota domisili pelanggan |
| `customer_segment` | String / Categorical | Segmentasi loyalitas/aktivitas pelanggan |
| `join_date` | Date (ISO-8601 String) | Tanggal registrasi/bergabung pelanggan |
| `email` | String / Email Address | Alamat surel kontak pelanggan (PII) |
| `phone` | String / Phone Number | Nomor telepon seluler pelanggan (PII) |
| `customer_status` | String / Categorical | Status keanggotaan pelanggan (`Active`/`Inactive`) |

#### Contoh Record (Sample 3 Objek Pertama):
| customer_id | customer_name | city | customer_segment | join_date | email | phone | customer_status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| CUS001 | Wahyu Utami | Semarang | Regular | 2025-05-28 | wahyu.utami.001@example.com | 081210007919 | Active |
| CUS002 | Nina Nugraha | Semarang | Regular | 2025-04-18 | nina.nugraha.002@example.com | 081210015838 | Active |
| CUS003 | Omar Lestari | Depok | New | 2025-09-01 | omar.lestari.003@example.com | 081210023757 | Active |

---

### 4.2 Missing / Blank Check
| Field Name | Missing / Blank Count | Persentase (%) | Status Kelengkapan |
| :--- | :---: | :---: | :--- |
| `customer_id` | 0 | 0.00% | Lengkap (100%) |
| `customer_name` | 0 | 0.00% | Lengkap (100%) |
| `city` | 0 | 0.00% | Lengkap (100%) |
| `customer_segment` | **4** | **1.86%** | **Ditemukan Missing** |
| `join_date` | 0 | 0.00% | Lengkap (100%) |
| `email` | 0 | 0.00% | Lengkap (100%) |
| `phone` | 0 | 0.00% | Lengkap (100%) |
| `customer_status` | 0 | 0.00% | Lengkap (100%) |

- **Field dengan Missing Terbesar**: `customer_segment` dengan 4 record hilang (1.86%).
- **Sample Record Key dengan Missing Segment**:
  - `CUS071` (Dewi Nugraha, Bandung, join: 2024-08-15)
  - `CUS108` (Omar Santoso, Semarang, join: 2024-09-06)
  - `CUS118` (Jihan Ramadhan, Malang, join: 2025-12-16)
  - `CUS174` (Nina Lestari, Bandung, join: 2025-05-13)

---

### 4.3 Candidate Key / Uniqueness Check
- **Candidate Key yang Diuji**: `customer_id`
- **Grain Statement**: 1 record (object) = 1 pelanggan (*customer*)
- **Null Count**: 0 (0.00%)
- **Distinct Count**: 215
- **Duplicate Count**: 0
- **Exact Full-Row Duplicates**: 0
- **Status Uniqueness**: `True` (100% unik pada raw source)
- **Candidate Key Finding**: `customer_id` memenuhi syarat keunikan entitas pelanggan secara sempurna.

---

### 4.4 Category / Consistency Check

#### A. Field `customer_segment` (Total: 215 records, 8 distinct category tokens)
Segmentasi standar yang diakui dalam model bisnis: `Regular`, `New`, dan `VIP`.

| Kategori Segmentasi | Record Count | Persentase (%) | Status Observasi Konsistensi |
| :--- | :---: | :---: | :--- |
| `Regular` | 111 | 51.63% | Nilai kanonikal standar |
| `New` | 52 | 24.19% | Nilai kanonikal standar |
| `VIP` | 42 | 19.53% | Nilai kanonikal standar |
| `<MISSING>` / `NaN` | 4 | 1.86% | **Isu Kelengkapan** (Missing Values) |
| `regular` | 3 | 1.40% | **Inkonsistensi Casing**: lowercase dari `Regular` |
| `new customer` | 1 | 0.47% | **Inkonsistensi Sinonim**: varian frasa dari `New` |
| `REG` | 1 | 0.47% | **Inkonsistensi Singkatan**: varian akronim dari `Regular` |
| `vip` | 1 | 0.47% | **Inkonsistensi Casing**: lowercase dari `VIP` |

- **Sample Record Key untuk Inkonsistensi Kategori Segment**:
  - `CUS014`: `'new customer'`
  - `CUS015`: `'regular'`
  - `CUS062`: `'regular'`
  - `CUS132`: `'regular'`
  - `CUS165`: `'REG'`
  - `CUS168`: `'vip'`

#### B. Field `customer_status` (Total: 215 records, 2 distinct tokens)
| Status Pelanggan | Record Count | Persentase (%) | Catatan Konsistensi |
| :--- | :---: | :---: | :--- |
| `Active` | 198 | 92.09% | Standar konsisten |
| `Inactive` | 17 | 7.91% | Standar konsisten |

#### C. Field `city` (Total: 215 records, 10 distinct tokens)
Distribusi 10 kota: Semarang (32), Yogyakarta (29), Bogor (24), Surabaya (22), Depok (21), Bekasi (20), Malang (19), Bandung (17), Tangerang (17), Jakarta (14). Seluruh nama kota konsisten.

---

### 4.5 Date / Time Check
- **Field yang Diperiksa**: `join_date`
- **Parseability**: 215/215 record (100%) valid dan dapat diparsing.
- **Unresolved Dates**: 0 record.
- **Rentang Tanggal (Min & Max)**:
  - **Minimum Date**: `2023-01-15` (e.g., `CUS035`)
  - **Maximum Date**: `2025-12-30` (e.g., `CUS205`)
- **Format Representation**: Seluruh 215 baris (100%) konsisten menggunakan format ISO `YYYY-MM-DD`.
- **Chronology Check**: Seluruh pelanggan terdaftar antara Januari 2023 hingga Desember 2025. Periode pendaftaran ini mendahului transaksi pesanan yang terjadi di kuartal pertama 2026.

---

### 4.6 Numeric & PII Check
- Dataset ini tidak memiliki field numerik moneter.
- **Pemeriksaan Atribut Kontak (PII)**:
  - `phone`: 215 record seluruhnya terdiri atas deret angka valid berawalan `08...` (11–12 digit).
  - `email`: 215 record memiliki struktur sintaks email standar (`@example.com`).
  - *Catatan Sesuai Guardrails*: Keberadaan data identitas pelanggan (nama, email, no HP) yang valid adalah bagian alami dari master data pelanggan dan **tidak dianggap sebagai data quality error**.

---

### 4.7 Business Logic Candidate Check
- Terdapat 17 pelanggan berstatus `Inactive`.
- Terdapat 4 pelanggan tanpa segmentasi yang jelas (`customer_segment` hilang). Perlu diverifikasi apakah status pelanggan baru atau pendaftaran belum tuntas yang menyebabkan atribut ini kosong.

---

## 5. Payments Profiling

### 5.1 Dataset Overview
- **Source File**: `data/raw/nusamart/nusamart_payments.txt`
- **Physical Format**: Semi-structured Key-Value delimited text (`.txt`, pemisah `|`)
- **Logical Record Structure**: 1 line = 1 transaksi pembayaran
- **Total Records / Lines**: 337
- **Total Columns / Fields**: 6

| Field Name | Inferred Data Type | Deskripsi Domain |
| :--- | :--- | :--- |
| `payment_id` | String / Alphanumeric Code | Identifier unik transaksi pembayaran (`PAYxxxx`) |
| `order_id` | String / Alphanumeric Code | Identifier referensi pesanan (`ORDxxxx`) |
| `payment_date` | Date (String Text) | Tanggal transaksi pembayaran dicatat |
| `payment_method` | String / Categorical | Metode pembayaran yang digunakan |
| `payment_amount` | Numeric / Text Currency | Jumlah nominal pembayaran (IDR) |
| `payment_status` | String / Categorical | Status hasil pembayaran (`Paid`/`Pending`/`Failed`) |

#### Contoh Record (Sample 3 Baris Pertama):
| payment_id | order_id | payment_date | payment_method | payment_amount | payment_status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| PAY0001 | ORD0001 | 2026-03-27 | QRIS | 564000 | Paid |
| PAY0002 | ORD0001 | 2026-03-25 | QRIS | 886000 | Paid |
| PAY0003 | ORD0002 | 2026-02-21 | Bank Transfer | 233000 | Pending |

---

### 5.2 Missing / Blank Check
| Field Name | Missing / Blank Count | Persentase (%) | Status Kelengkapan |
| :--- | :---: | :---: | :--- |
| `payment_id` | 0 | 0.00% | Lengkap (100%) |
| `order_id` | 0 | 0.00% | Lengkap (100%) |
| `payment_date` | 0 | 0.00% | Lengkap (100%) |
| `payment_method` | 0 | 0.00% | Lengkap (100%) |
| `payment_amount` | 0 | 0.00% | Lengkap (100%) |
| `payment_status` | 0 | 0.00% | Lengkap (100%) |

- **Field dengan missing terbesar**: Tidak ada (0 missing di semua kolom).

---

### 5.3 Candidate Key / Uniqueness Check
- **Candidate Key yang Diuji**: `payment_id`
- **Grain Statement**: 1 record (line) = 1 transaksi pembayaran
- **Null Count**: 0 (0.00%)
- **Distinct Count**: 335
- **Duplicate Count (Repeated Distinct Keys)**: 2 key berulang
- **Total Baris di Bawah Key Berulang**: 4 baris (2 key × 2 baris)
- **Baris Ekstra Duplikat**: 2 baris ekstra (0.59% dari total 337 baris)
- **Status Uniqueness**: `False`

#### Rincian Key yang Berulang:
1. `PAY0095` (muncul 2 kali pada pesanan `ORD0093`):
   - Baris 1: `order_id=ORD0093`, `date=2026-02-02`, `method=QRIS`, `amount=74000`, `status=Paid`
   - Baris 2: `order_id=ORD0093`, `date=2026-02-02`, `method=QRIS`, `amount=74000`, `status=Paid`
2. `PAY0206` (muncul 2 kali pada pesanan `ORD0204`):
   - Baris 1: `order_id=ORD0204`, `date=2026-02-20`, `method=Credit Card`, `amount=299000`, `status=Paid`
   - Baris 2: `order_id=ORD0204`, `date=2026-02-20`, `method=Credit Card`, `amount=299000`, `status=Paid`

- **Observasi Full-Row Exact Duplicates**: Seluruh atribut dari kedua pasang baris di atas bernilai sama persis (*exact duplicate rows*).
- **Candidate Key Finding**: Candidate key `payment_id` mengalami pelanggaran keunikan sebanyak 2 key (4 baris), yang disebabkan oleh baris duplikat identik pada data sumber raw TXT.

---

### 5.4 Category / Consistency Check

#### A. Field `payment_method` (Total: 337 records, 4 distinct categories)
| Metode Pembayaran | Record Count | Persentase (%) | Catatan Konsistensi |
| :--- | :---: | :---: | :--- |
| `Credit Card` | 99 | 29.38% | Standar konsisten |
| `E-Wallet` | 83 | 24.63% | Standar konsisten |
| `Bank Transfer` | 78 | 23.15% | Standar konsisten |
| `QRIS` | 77 | 22.85% | Standar konsisten |

#### B. Field `payment_status` (Total: 337 records, 3 distinct categories)
| Status Pembayaran | Record Count | Persentase (%) | Catatan Konsistensi |
| :--- | :---: | :---: | :--- |
| `Paid` | 307 | 91.10% | Standar konsisten |
| `Pending` | 20 | 5.93% | Standar konsisten |
| `Failed` | 10 | 2.97% | Standar konsisten |

- **Observasi Kategori**: Seluruh nilai kategori pada kedua field memiliki representasi penulisan yang 100% konsisten.

---

### 5.5 Date / Time Check
- **Field yang Diperiksa**: `payment_date`
- **Parseability**: 337/337 record (100%) berhasil diparsing.
- **Unresolved / Unparseable Dates**: 0 record.
- **Rentang Tanggal (Min & Max)**:
  - **Minimum Date**: `2025-12-27` (`PAY0335`)
  - **Maximum Date**: `2026-04-02` (`PAY0331`, `PAY0332`)
- **Variasi Representasi Format Tanggal**:
  - `YYYY-MM-DD`: 334 baris (99.11%)
  - Non-standar: 3 baris (0.89%), dengan rincian:
    - `PAY0113` (`ORD0113`): `'27-02-2026'` (format `DD-MM-YYYY`)
    - `PAY0118` (`ORD0117`): `'2026/03/31'` (format `YYYY/MM/DD`)
    - `PAY0210` (`ORD0208`): `'29/01/2026'` (format `DD/MM/YYYY`)
- **Isu Kronologis Logis (*Chronology Candidate Issue*)**:
  - Record `PAY0335` (terkait pesanan `ORD0320`) memiliki `payment_date = 2025-12-27`.
  - Pada dataset `orders`, pesanan `ORD0320` memiliki `order_date = 2026-01-06`.
  - *Distingsi*: Ini bukan isu format parsing, melainkan **logical chronology candidate issue** di mana tanggal pembayaran tercatat 10 hari sebelum pesanan dibuat.

---

### 5.6 Numeric Check
Pemeriksaan dilakukan terhadap field `payment_amount`.

| Numeric Metric | `payment_amount` |
| :--- | :---: |
| Invalid / Non-numeric Representation Count | 3 baris |
| Negative Values Count (< 0) | 1 baris |
| Zero Values Count (= 0) | 0 baris |
| Nilai Minimum (Min) | -344,000 |
| Nilai Rata-rata (Mean, setelah sanitasi) | 712,407.19 |
| Nilai Maksimum (Max) | 4,176,000 |
| Sample Nilai Tertinggi | 4,176,000 (`PAY0247`) |
| Sample Nilai Terendah | -344,000 (`PAY0182`) |

#### Observasi Khusus:
1. **Pemisah Ribuan Karakter Koma (`,`) pada String Angka**:
   - `PAY0005` (`ORD0004`): `'468,000'`
   - `PAY0042` (`ORD0039`): `'2,481,000'`
   - `PAY0333` (`ORD0319`): `'212,000'`
   - *Dampak*: Nilai ini gagal dikonversi ke numerik murni tanpa pra-pembersihan tanda koma.
2. **Nilai Nominal Negatif**:
   - `PAY0182` (`ORD0183`): nominal pembayaran `-344000` dengan status `Paid` dan metode `E-Wallet`.
   - *Konteks Relasional Singkat*: Pada dataset `orders`, `ORD0183` memiliki total order Rp344.000. Kemungkinan nilai negatif ini merefleksikan pencatatan refund/pengembalian dana, pembalikan mutasi, atau kesalahan tanda (*sign error*).

---

### 5.7 Business Logic Candidate Check
- **Relasi Pembayaran Ganda pada 1 Pesanan**:
  - Ditemukan beberapa pesanan yang memiliki lebih dari satu baris pembayaran, misalnya `ORD0001` yang memiliki dua pembayaran sukses (`PAY0001` sebesar Rp564.000 dan `PAY0002` sebesar Rp886.000; total = Rp1.450.000, persis sama dengan total order `ORD0001` sebesar Rp1.450.000). Hal ini mengindikasikan adanya mekanisme pembayaran bertahap (*installment*) atau split payment dalam alur bisnis.
- **Transaksi Berstatus `Failed`**: Terdapat 10 transaksi dengan status pembayaran `Failed`.

---

## 6. Cross-Dataset Profiling Summary

> **Batasan Sesuai Panduan**: Bagian ini hanya merangkum perbandingan struktural (row count, grain candidate, candidate key, jumlah candidate findings, dan area perhatian). Bagian ini **tidak melakukan relational join** dan **tidak menghasilkan KPI bisnis lintas tabel**.

| Dataset | Format Sumber | Record Count | Candidate Grain | Candidate Key | Status Keunikan Key | Missing Fields | Jml Candidate Findings | Area yang Perlu Perhatian Lebih Lanjut |
| :--- | :---: | :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| **Orders** | XLSX | 320 | 1 row = 1 order | `order_id` | Unik (100%) | 0 field | 2 | Variasi label `sales_channel`, format teks pada `order_date` |
| **Order Items** | CSV | 763 | 1 row = 1 item dalam order | `order_item_id` | Tidak Unik (3 duplikat) | 0 field | 4 | Repeated keys & exact duplicates, tanda koma pada harga/diskon, spasi pada quantity, outlier kuantitas |
| **Customers** | JSON | 215 | 1 object = 1 customer | `customer_id` | Unik (100%) | 1 field (`customer_segment`) | 2 | Missing values pada `customer_segment`, inkonsistensi label segmen |
| **Payments** | TXT | 337 | 1 line = 1 transaksi pembayaran | `payment_id` | Tidak Unik (2 duplikat) | 0 field | 5 | Repeated keys & exact duplicates, tanda koma pada nominal, nominal negatif, variasi format tanggal, tanggal mendahului order |

---

## 7. Candidate Findings for Manual Verification

Berikut adalah daftar temuan (*candidate findings*) hasil profiling otomatis yang disiapkan sebagai bukti (*evidence*) untuk diverifikasi secara manual oleh peserta. Setiap temuan disertai nomor record/key yang dapat ditelusuri.

---

### Finding 1: Pelanggaran Keunikan Candidate Key pada Order Items (Repeated Keys)
- **Dataset**: `order_items`
- **Field / Area**: `order_item_id` (Candidate Key)
- **Finding Description**: Candidate key `order_item_id` memiliki 3 nilai kunci yang muncul lebih dari satu kali (repeated keys). Hal ini melanggar asumsi keunikan baris pada grain 1 baris = 1 order item.
- **Count Affected**: 3 distinct keys (total 6 records terdampak)
- **Percentage Affected**: 0.79% baris (atau 0.39% jika dihitung baris redundan ekstra)
- **Sample Record Key**: `ITM00130` (ORD0058), `ITM00215` (ORD0090), `ITM00464` (ORD0199)
- **Evidence / Angka Pendukung**:
  - `total_records = 763`
  - `distinct_keys = 760`
  - `repeated_keys_count = 3`
  - Frekuensi kemunculan: `{'ITM00130': 2, 'ITM00215': 2, 'ITM00464': 2}`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 2: Keberadaan Baris Duplikat Identik pada Order Items (Exact Duplicates)
- **Dataset**: `order_items`
- **Field / Area**: Full Row (Seluruh Kolom)
- **Finding Description**: Terdapat 3 baris ekstra yang merupakan duplikasi identik sempurna (*exact full-row duplicates*) dari baris yang sudah ada sebelumnya.
- **Count Affected**: 3 baris ekstra (total 6 baris terlibat pasangan kembar)
- **Percentage Affected**: 0.39% dari total baris
- **Sample Record Key**: `ITM00130` (ORD0058), `ITM00215` (ORD0090), `ITM00464` (ORD0199)
- **Evidence / Angka Pendukung**:
  - `exact_duplicate_extras = 3`
  - Rincian record kembar:
    - Baris kembar 1: `ITM00130 | ORD0058 | PRD011 | Sports | 1 | 379000 | 11000 | 368000`
    - Baris kembar 2: `ITM00215 | ORD0090 | PRD013 | Grocery | 1 | 51000 | 6000 | 45000`
    - Baris kembar 3: `ITM00464 | ORD0199 | PRD008 | Beauty | 2 | 172000 | 34000 | 310000`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 3: Missing Values pada Field Customer Segment
- **Dataset**: `customers`
- **Field / Area**: `customer_segment`
- **Finding Description**: Ditemukan nilai null / missing pada field `customer_segment`. Pelanggan yang terdampak tidak memiliki label segmentasi loyalitas pada profil akun.
- **Count Affected**: 4 records
- **Percentage Affected**: 1.86% dari total 215 records pelanggan
- **Sample Record Key**: `CUS071`, `CUS108`, `CUS118`, `CUS174`
- **Evidence / Angka Pendukung**:
  - `missing_or_blank = 4`, `pct = 1.86%`
  - Record terdampak:
    - `CUS071` (Dewi Nugraha, Bandung, join: 2024-08-15)
    - `CUS108` (Omar Santoso, Semarang, join: 2024-09-06)
    - `CUS118` (Jihan Ramadhan, Malang, join: 2025-12-16)
    - `CUS174` (Nina Lestari, Bandung, join: 2025-05-13)
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 4: Inkonsistensi Penulisan Kategori pada Customer Segment
- **Dataset**: `customers`
- **Field / Area**: `customer_segment`
- **Finding Description**: Terdapat variasi representasi label kategori yang tidak konsisten dengan standar kanonikal (`Regular`, `New`, `VIP`), meliputi perbedaan huruf kecil (*lowercase*), variasi sinonim frasa, dan singkatan.
- **Count Affected**: 6 records
- **Percentage Affected**: 2.79% dari total records
- **Sample Record Key**: `CUS014`, `CUS015`, `CUS062`, `CUS132`, `CUS165`, `CUS168`
- **Evidence / Angka Pendukung**:
  - `'regular'` (3 record): `CUS015`, `CUS062`, `CUS132`
  - `'new customer'` (1 record): `CUS014`
  - `'REG'` (1 record): `CUS165`
  - `'vip'` (1 record): `CUS168`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 5: Representasi Angka Non-Standar dengan Pemisah Koma pada Payments
- **Dataset**: `payments`
- **Field / Area**: `payment_amount`
- **Finding Description**: Field `payment_amount` memuat nilai dengan format pemisah ribuan berupa koma (`,`), yang mengakibatkan nilai tersebut berstatus *invalid nonblank numeric representation* jika dikonversi secara langsung.
- **Count Affected**: 3 records
- **Percentage Affected**: 0.89% dari total 337 baris pembayaran
- **Sample Record Key**: `PAY0005`, `PAY0042`, `PAY0333`
- **Evidence / Angka Pendukung**:
  - `invalid_nonblank = 3`
  - Nilai aktual pada sumber:
    - `PAY0005` (`ORD0004`): `'468,000'`
    - `PAY0042` (`ORD0039`): `'2,481,000'`
    - `PAY0333` (`ORD0319`): `'212,000'`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 6: Nilai Pembayaran Negatif pada Transaksi Payments
- **Dataset**: `payments`
- **Field / Area**: `payment_amount`
- **Finding Description**: Ditemukan nilai nominal pembayaran negatif (`-344000`) dengan status `Paid` dan metode `E-Wallet`. Nilai negatif ini memerlukan konfirmasi apakah merupakan pencatatan refund/retur atau kesalahan input tanda minus.
- **Count Affected**: 1 record
- **Percentage Affected**: 0.30% dari total 337 baris pembayaran
- **Sample Record Key**: `PAY0182`
- **Evidence / Angka Pendukung**:
  - `payment_id = PAY0182`, `order_id = ORD0183`, `payment_date = 2026-02-14`, `amount = -344000`, `status = Paid`
  - Pada tabel `orders`, pesanan `ORD0183` memiliki nilai `order_total = 344000`.
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 7: Pelanggaran Keunikan Candidate Key pada Payments (Repeated Keys & Exact Duplicates)
- **Dataset**: `payments`
- **Field / Area**: `payment_id` (Candidate Key)
- **Finding Description**: Candidate key `payment_id` memiliki 2 nilai kunci yang muncul lebih dari satu kali, yang keduanya merupakan baris duplikat identik (*exact full-row duplicates*).
- **Count Affected**: 2 distinct keys (total 4 baris terdampak, 2 baris ekstra)
- **Percentage Affected**: 0.59% dari total 337 baris pembayaran
- **Sample Record Key**: `PAY0095`, `PAY0206`
- **Evidence / Angka Pendukung**:
  - `PAY0095` (2 baris identik): `order_id=ORD0093`, `date=2026-02-02`, `method=QRIS`, `amount=74000`, `status=Paid`
  - `PAY0206` (2 baris identik): `order_id=ORD0204`, `date=2026-02-20`, `method=Credit Card`, `amount=299000`, `status=Paid`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 8: Variasi Format Tanggal Representasi Teks pada Payments
- **Dataset**: `payments`
- **Field / Area**: `payment_date`
- **Finding Description**: Ditemukan 3 baris dengan format tanggal teks yang tidak konsisten dengan standar mayoritas (`YYYY-MM-DD`). Seluruh nilai berhasil diparsing, sehingga ini diklasifikasikan sebagai *format representation issue*, bukan *unresolved parse failure*.
- **Count Affected**: 3 records
- **Percentage Affected**: 0.89% dari total 337 baris pembayaran
- **Sample Record Key**: `PAY0113`, `PAY0118`, `PAY0210`
- **Evidence / Angka Pendukung**:
  - `PAY0113` (`ORD0113`): `'27-02-2026'` (format `DD-MM-YYYY`)
  - `PAY0118` (`ORD0117`): `'2026/03/31'` (format `YYYY/MM/DD`)
  - `PAY0210` (`ORD0208`): `'29/01/2026'` (format `DD/MM/YYYY`)
  - Total `unresolved_dates = 0` (100% parseable)
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 9: Inkonsistensi Kronologis Logis Tanggal Pembayaran Mendahului Pesanan
- **Dataset**: `payments` (berbanding `orders`)
- **Field / Area**: `payment_date`
- **Finding Description**: Transaksi pembayaran `PAY0335` tercatat bertanggal `2025-12-27`, sedangkan pesanan terkait `ORD0320` tercatat bertanggal `2026-01-06`. Pembayaran tercatat 10 hari sebelum pesanan dibuat.
- **Count Affected**: 1 record
- **Percentage Affected**: 0.30% dari total transaksi pembayaran
- **Sample Record Key**: `PAY0335` (terkait `ORD0320`)
- **Evidence / Angka Pendukung**:
  - `PAY0335.payment_date = 2025-12-27`
  - `ORD0320.order_date = 2026-01-06`
  - Selisih: -10 hari (kronologi mendahului pemesanan)
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 10: Variasi Penulisan Label Kategori Sales Channel pada Orders
- **Dataset**: `orders`
- **Field / Area**: `sales_channel`
- **Finding Description**: Terdapat 6 record pesanan yang menggunakan penulisan saluran penjualan *Marketplace* secara tidak konsisten (perbedaan spasi pembuka/penutup, pemisahan dua kata, serta penggunaan huruf besar/kecil).
- **Count Affected**: 6 records
- **Percentage Affected**: 1.88% dari total 320 pesanan
- **Sample Record Key**: `ORD0041`, `ORD0053`, `ORD0062`, `ORD0070`, `ORD0265`, `ORD0311`
- **Evidence / Angka Pendukung**:
  - `ORD0041`: `'Marketplace '` (trailing space)
  - `ORD0053`: `'marketplace'` (lowercase)
  - `ORD0062`: `'market place'` (spasi lowercase)
  - `ORD0070`: `'Market Place'` (spasi title case)
  - `ORD0265`: `'MARKET PLACE'` (spasi uppercase)
  - `ORD0311`: `'MARKETPLACE'` (uppercase tanpa spasi)
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 11: Variasi Representasi Format Tanggal pada Orders
- **Dataset**: `orders`
- **Field / Area**: `order_date`
- **Finding Description**: Dari 320 baris pesanan, 316 baris tersimpan sebagai objek timestamp datetime Excel, sedangkan 4 baris tersimpan sebagai string teks dengan pola tanggal bervariasi.
- **Count Affected**: 4 records
- **Percentage Affected**: 1.25% dari total pesanan
- **Sample Record Key**: `ORD0027`, `ORD0164`, `ORD0197`, `ORD0263`
- **Evidence / Angka Pendukung**:
  - `ORD0027`: `'14/02/2026'` (`DD/MM/YYYY`)
  - `ORD0164`: `'2026/01/25'` (`YYYY/MM/DD`)
  - `ORD0197`: `'04/02/2026'` (`DD/MM/YYYY`)
  - `ORD0263`: `'15-03-2026'` (`DD-MM-YYYY`)
  - Parseability: 100% parseable, `unresolved_dates = 0`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 12: Representasi Karakter Koma pada Unit Price & Discount Amount (Order Items)
- **Dataset**: `order_items`
- **Field / Area**: `unit_price`, `discount_amount`
- **Finding Description**: Terdapat nilai moneter yang disimpan menggunakan karakter koma sebagai pemisah ribuan, yang menghalangi konversi langsung ke tipe numerik.
- **Count Affected**: 3 records (2 baris pada `unit_price`, 1 baris pada `discount_amount`)
- **Percentage Affected**: 0.39% dari total 763 baris order items
- **Sample Record Key**: `ITM00191`, `ITM00481`, `ITM00611`
- **Evidence / Angka Pendukung**:
  - `ITM00481`: `unit_price = '486,000'`
  - `ITM00611`: `unit_price = '50,000'`
  - `ITM00191`: `discount_amount = '15,000'`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 13: Karakter Spasi (Whitespace Padding) pada Kuantitas Order Items
- **Dataset**: `order_items`
- **Field / Area**: `quantity`
- **Finding Description**: Ditemukan karakter spasi tambahan pada teks angka kuantitas yang dapat memengaruhi validasi string.
- **Count Affected**: 2 records
- **Percentage Affected**: 0.26% dari total baris order items
- **Sample Record Key**: `ITM00181`, `ITM00314`
- **Evidence / Angka Pendukung**:
  - `ITM00181` (`ORD0077`): string `' 1 '`
  - `ITM00314` (`ORD0131`): string `' 2 '`
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 14: Kuantitas Pembelian Tinggi Tidak Biasa (Unusually High Quantity)
- **Dataset**: `order_items`
- **Field / Area**: `quantity`
- **Finding Description**: Ditemukan 2 transaksi dengan kuantitas produk sembako sebesar 24 dan 30 unit, jauh melampaui distribusi kuantitas normal (1–4 unit). Perlu diverifikasi manual apakah merupakan pesanan grosir wajar (*bulk order*) atau anomali kuantitas.
- **Count Affected**: 2 records
- **Percentage Affected**: 0.26% dari total baris order items
- **Sample Record Key**: `ITM00454`, `ITM00676`
- **Evidence / Angka Pendukung**:
  - `ITM00454` (`ORD0194`): produk `PRD025` (Grocery), kuantitas = 24
  - `ITM00676` (`ORD0288`): produk `PRD025` (Grocery), kuantitas = 30
  - Rata-rata kuantitas seluruh dataset: 1.48 unit
- **Finding Status**: `Candidate — Requires Manual Verification`

---

### Finding 15: Baseline Konfirmasi Parseability Tanggal Payments (Clean Baseline)
- **Dataset**: `payments`
- **Field / Area**: `payment_date`
- **Finding Description**: Seluruh tanggal pembayaran pada file semi-structured `.txt` dapat diparsing menjadi objek tanggal valid tanpa ada record yang gagal dikonversi (*0 unresolved dates*).
- **Count Affected**: 0 records gagal (337/337 berhasil)
- **Percentage Affected**: 100.00% validitas konversi
- **Sample Record Key**: `PAY0001`, `PAY0335`
- **Evidence / Angka Pendukung**:
  - `records = 337`
  - `unresolved_dates = 0`
  - `min_parsed = 2025-12-27`
  - `max_parsed = 2026-04-02`
- **Finding Status**: `Candidate — Requires Manual Verification`
