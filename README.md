# MEL KOSMETIK — Laravel Inventory Management System
> **Tugas Praktikum Pertemuan 5: Laravel Fundamental**  
> *Mengubah Inventory Management System PHP Native (P4) menjadi Laravel Application (P5)*

---

## 1. Deskripsi Project

**MEL KOSMETIK** adalah aplikasi sistem manajemen inventaris (*Inventory Management System*) berbasis web untuk toko kosmetik yang dibangun menggunakan framework **Laravel 11**, **PHP 8.3**, **MySQL / MariaDB**, serta antarmuka modern dengan **Blade Template** dan **Bootstrap 5**.

Aplikasi ini merupakan migrasi langsung dan penyempurnaan dari project sistem inventaris PHP Native (Pertemuan 4) menjadi arsitektur modern Laravel MVC (Model-View-Controller) pada Pertemuan 5. Seluruh konsep bisnis toko kosmetik, relasi database, validasi, pencarian, filter, mutasi stok, hingga riwayat transaksi tetap dipertahankan dan disempurnakan.

### Identitas Visual UI/UX
- **Tema Desain:** Toko Kosmetik Modern, Elegan & Profesional
- **Palet Warna:** *Dusty Pink* (`#a84c6c`), *Soft Blush* (`#fdf0f4`), *Cream* (`#faf5f7`), *White* (`#ffffff`), dengan aksen *Rose Gold* (`#c48b9f`).
- **Komponen:** Responsive Sidebar, Sticky Topbar, Metric Stat Cards, Modern Data Tables, Modal Confirmation, dan Toast/Alert Notification.

---

## 2. Fitur Utama Aplikasi

### A. Dashboard Inventaris
- **Metrik Utama:** Total produk kosmetik, total kategori, total supplier mitra, dan total unit stok fisik.
- **Peringatan Dini Stok:** Deteksi otomatis produk dengan stok menipis (di bawah batas minimum) dan produk yang stoknya habis (0 unit).
- **Ringkasan Mutasi Transaksi:** Akumulasi total unit stok masuk (*in*), stok keluar (*out*), dan total transaksi mutasi.
- **Tabel Cepat:** Menampilkan daftar produk kosmetik terbaru dan log mutasi transaksi terakhir.

### B. Manajemen Produk Kosmetik (Resource CRUD)
- Menampilkan seluruh produk dengan informasi SKU unik, nama produk, kategori, supplier, harga satuan, dan kuantitas stok fisik.
- Tambah produk baru dengan validasi ketat.
- Edit dan perbarui informasi produk kosmetik.
- Detail lengkap spesifikasi produk dan riwayat mutasi khusus produk tersebut.
- Hapus produk aman dengan modal konfirmasi.

### C. Search, Filter Kategori, Sorting & Pagination
- **Pencarian (Search):** Berdasarkan nama produk, kode SKU, atau brand menggunakan Eloquent Query Builder `when()`.
- **Filter Kategori:** Memfilter katalog berdasarkan kategori produk terpilih.
- **Sorting Fleksibel:** Pengurutan berdasarkan nama produk (A-Z / Z-A), harga (termurah/termahal), dan stok (tersedikit/terbanyak).
- **Pagination:** Pembagian halaman rapi (10 data per halaman) dengan mempertahankan query pencarian & filter via `withQueryString()`.

### D. Manajemen Kategori
- Kelola kategori produk (*Skincare, Makeup, Body Care, Hair Care, Parfum*).
- Menghitung otomatis jumlah produk per kategori (*Eloquent withCount*).
- Halaman detail kategori untuk melihat seluruh produk kosmetik dalam kategori tersebut.
- Proteksi integritas data: Kategori yang masih memiliki produk tidak dapat dihapus sembarangan.

### E. Manajemen Supplier / Pemasok
- Kelola mitra distributor dan pemasok kosmetik (nama perusahaan, telepon/WhatsApp, email, alamat).
- Menghitung jumlah produk yang dipasok oleh supplier terkait.
- Halaman detail supplier untuk melihat seluruh katalog produk pasokan mereka.
- Proteksi relasi foreign key dari penghapusan tidak disengaja.

### F. Transaksi Mutasi Stok (Stok Masuk & Stok Keluar)
- **Stok Masuk (Restock / In):** Pencatatan pasokan masuk dari supplier yang otomatis menambah kuantitas stok produk terkait di database.
- **Stok Keluar (Penjualan / Out):** Pencatatan barang keluar untuk penjualan atau retur yang otomatis mengurangi kuantitas stok produk.
- **Business Rule Proteksi Stok:** Sistem memvalidasi agar kuantitas stok keluar tidak melebihi stok yang tersedia (*anti-negative stock*).
- **Database Transaction:** Menggunakan `DB::transaction()` dan `lockForUpdate()` untuk menjamin konsistensi ACID pencatatan mutasi dan perubahan saldo stok secara simultan.

### G. Riwayat Mutasi Stok (Stock Movement History)
- Menampilkan log kronologis: tanggal mutasi, nama produk kosmetik, kategori, jenis transaksi (*In / Out*), kuantitas mutasi, dan catatan/nomor referensi faktur.
- Filter riwayat berdasarkan produk dan jenis transaksi.

---

## 3. Struktur Direktori Project

Project telah mengikuti konvensi arsitektur Laravel:

```text
Mel-Kosmetik-Laravel/
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       ├── Controller.php
│   │       ├── DashboardController.php
│   │       ├── ProductController.php
│   │       ├── CategoryController.php
│   │       ├── SupplierController.php
│   │       └── StockMovementController.php
│   ├── Models/
│   │   ├── Category.php
│   │   ├── Product.php
│   │   ├── Supplier.php
│   │   └── StockMovement.php
│   └── Providers/
│       └── AppServiceProvider.php
├── database/
│   ├── factories/
│   │   ├── CategoryFactory.php
│   │   ├── ProductFactory.php
│   │   ├── SupplierFactory.php
│   │   └── StockMovementFactory.php
│   ├── migrations/
│   │   ├── 2026_01_01_000001_create_categories_table.php
│   │   ├── 2026_01_01_000002_create_suppliers_table.php
│   │   ├── 2026_01_01_000003_create_products_table.php
│   │   └── 2026_01_01_000004_create_stock_movements_table.php
│   └── seeders/
│       └── DatabaseSeeder.php
├── resources/
│   └── views/
│       ├── layouts/
│       │   ├── app.blade.php
│       │   └── navigation.blade.php
│       ├── components/
│       │   ├── alert.blade.php
│       │   ├── card.blade.php
│       │   └── modal.blade.php
│       ├── dashboard/
│       │   └── index.blade.php
│       ├── products/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── show.blade.php
│       ├── categories/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── show.blade.php
│       ├── suppliers/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── show.blade.php
│       └── stock-movements/
│           ├── index.blade.php
│           ├── create.blade.php
│           ├── in.blade.php
│           └── out.blade.php
├── routes/
│   └── web.php
├── tests/
│   └── Feature/
│       └── MelKosmetikTest.php
├── .env
├── composer.json
└── artisan
```

---

## 4. Persyaratan Sistem & Lingkungan

- **PHP:** Versi 8.2 atau 8.3 (dengan ekstensi `pdo_mysql`, `mbstring`, `openssl`, `tokenizer`, `xml`, `ctype`, `json`, `curl`).
- **Composer:** Versi 2.x
- **Web Server & Database:** Apache & MySQL / MariaDB (XAMPP).
- **Browser:** Google Chrome / Mozilla Firefox / Microsoft Edge terbaru.

---

## 5. Panduan Instalasi dan Menjalankan Aplikasi

### Langkah 1: Clone atau Buka Folder Project
Pastikan folder project berada pada direktori web server (misalnya `C:\xampp\htdocs\Mel-Kosmetik-Laravel`).

### Langkah 2: Instalasi Dependensi Composer (Jika Belum Terinstal)
```bash
composer install
```

### Langkah 3: Konfigurasi File Lingkungan (`.env`)
Salin file template `.env.example` menjadi `.env` jika belum ada:
```bash
copy .env.example .env
```

Pastikan konfigurasi koneksi database di file `.env` mengarah ke database `mel_kosmetik`:
```env
APP_NAME="Mel Kosmetik"
APP_ENV=local
APP_KEY=base64:...
APP_DEBUG=true
APP_URL=http://127.0.0.1:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=mel_kosmetik
DB_USERNAME=root
DB_PASSWORD=
```

### Langkah 4: Generate Application Key
```bash
php artisan key:generate
```

### Langkah 5: Buat Database MySQL
Buka MySQL CLI atau phpMyAdmin (`http://localhost/phpmyadmin`), lalu buat database baru:
```sql
CREATE DATABASE mel_kosmetik CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Langkah 6: Jalankan Migration dan Seeder Data Awal
Jalankan perintah berikut untuk mengeksekusi migrasi tabel dan mengisi data seeder realistis:
```bash
php artisan migrate:fresh --seed
```
*Perintah ini akan membuat 4 tabel utama dan mengisikan 5 kategori kosmetik, 5 supplier, 24 produk kosmetik realistis, dan 13 catatan mutasi stok awal.*

### Langkah 7: Menjalankan Server Laravel
Jalankan server pengembangan Laravel:
```bash
php artisan serve
```
Aplikasi dapat langsung diakses melalui browser pada URL:  
👉 **`http://127.0.0.1:8000`**

---

## 6. Penjelasan Arsitektur MVC (Model-View-Controller)

Sesuai materi **Pertemuan 5 Laravel Fundamental**, aplikasi ini menerapkan arsitektur MVC secara disiplin:

```text
                     +---------------------------------------+
                     |          Browser / Client             |
                     +---------------------------------------+
                                         |  1. HTTP Request
                                         v
                     +---------------------------------------+
                     |            routes/web.php             |
                     +---------------------------------------+
                                         |  2. Dispatch Route
                                         v
                     +---------------------------------------+
                     |              Controller               |
                     |  (ProductController, Dashboard, dll)  |
                     +---------------------------------------+
                            |                         ^
            3. Query Model  |                         | 4. Eloquent Collection
                            v                         |
                     +--------------+         +----------------+
                     |    Model     |<------->|    Database    |
                     |  (Eloquent)  |         | (mel_kosmetik) |
                     +--------------+         +----------------+
                            |
                            | 5. Pass Data ke View
                            v
                     +---------------------------------------+
                     |             Blade View                |
                     |  (@extends, @section, components)     |
                     +---------------------------------------+
                                         |  6. HTTP Response (HTML)
                                         v
                     +---------------------------------------+
                     |       Tampilan Halaman Pengguna       |
                     +---------------------------------------+
```

1. **Request:** Pengguna berinteraksi melalui browser (mengirim URL atau form input).
2. **Route (`routes/web.php`):** Menerima request HTTP, mencocokkan URI dan HTTP method (`GET`, `POST`, `PUT`, `DELETE`), lalu meneruskannya ke controller yang berwenang.
3. **Controller:** Berperan sebagai otak pengendali. Menerima input `Request`, menjalankan validasi data form, memanggil Eloquent Model untuk berinteraksi dengan database, dan menentukan respon (view atau redirect).
4. **Model (`app/Models`):** Mengelola representasi tabel database, mendefinisikan relasi antar-tabel (`hasMany`, `belongsTo`), mengontrol `$fillable` untuk keamanan *mass assignment*, dan mengelola *casting* tipe data.
5. **Database (`MySQL`):** Tempat penyimpanan data relasional terstruktur yang dikelola melalui Laravel Migration.
6. **Response / View (`resources/views`):** Merender tampilan HTML menggunakan Blade Template engine yang memanfaatkan escaping data `{{ }}`, layout reusable `@extends`, kondisi `@if`, perulangan `@forelse`, dan Blade Component (`<x-alert>`, `<x-card>`, `<x-modal>`).

---

## 7. Demonstrasi Satu Alur Lengkap (End-to-End Workflow)

Sesuai ketentuan Tugas Pertemuan 5, berikut adalah demonstrasi alur nyata:  
**Membuka form → Validasi → Simpan → Redirect → Tampilkan data.**

### Studi Kasus: Menambahkan Produk Kosmetik Baru

1. **Membuka Form (`GET /products/create`):**
   - User mengklik tombol **"Tambah Produk Baru"**.
   - Route `Route::get('products/create', ...)` mengarahkan ke `ProductController@create`.
   - Controller mengambil daftar kategori dan supplier dari model (`Category::all()`, `Supplier::all()`) dan mengirimkannya ke view `resources/views/products/create.blade.php`.
   - Browser merender formulir input lengkap dengan token proteksi `@csrf`.

2. **Validasi Form (`POST /products`):**
   - User mengisi form dan menekan tombol **"Simpan Produk"**. Browser mengirim request `POST /products`.
   - `ProductController@store` menjalankan validasi menggunakan `$request->validate()`:
     - `sku`: Wajib diisi, maksimal 50 karakter, dan unik pada tabel `products`.
     - `name`: Wajib diisi, maksimal 150 karakter.
     - `category_id`: Wajib dipilih dan ID harus ada di tabel `categories`.
     - `price`: Wajib angka numerik dan minimal Rp 0.
     - `stock`: Wajib bilangan bulat dan tidak boleh negatif.
     - `minimum_stock`: Wajib bilangan bulat dan minimal 0.
   - **Jika Validasi Gagal:** Request otomatis dialihkan kembali (*redirect back*) ke form dengan input lama via `old()` dan pesan error spesifik via directive `@error('field')`.
   - **Jika Validasi Berhasil:** Controller mengambil data tervalidasi `$validated`.

3. **Simpan ke Database:**
   - Controller memanggil model Eloquent:
     ```php
     Product::create($validated);
     ```
   - Eloquent membuat query `INSERT INTO products ...` secara aman menggunakan *prepared statements* (terhindar dari SQL Injection).

4. **Redirect dengan Flash Message:**
   - Controller mengembalikan respon redirect ke named route `products.index` disertai pesan flash session:
     ```php
     return redirect()->route('products.index')
         ->with('success', 'Produk kosmetik "..." berhasil ditambahkan.');
     ```

5. **Tampilkan Data:**
   - Browser menerima redirect dan memanggil `GET /products` (`ProductController@index`).
   - Controller memuat data produk dengan eager loading `with(['category', 'supplier'])`.
   - View `resources/views/products/index.blade.php` merender tabel produk terbaru yang memuat produk yang baru disimpan, beserta notifikasi sukses menggunakan komponen `<x-alert type="success">`.

---

## 8. Daftar Screenshot untuk Laporan Praktikum

Lakukan tangkapan layar (*screenshot*) pada 8 bagian berikut sesuai instruksi tugas:

| No | Halaman / Bagian | URL / Perintah | Deskripsi yang Ditampilkan |
|---|---|---|---|
| 1 | **Dashboard** | `http://127.0.0.1:8000` | Kartu ringkasan total produk, kategori, supplier, stok fisik, tabel stok menipis, stok habis, dan log mutasi terbaru. |
| 2 | **Daftar Produk (Search & Filter)** | `http://127.0.0.1:8000/products?search=Wardah` | Tabel katalog produk dengan filter aktif, badge status stok, dan tombol pagination. |
| 3 | **Form Tambah Produk** | `http://127.0.0.1:8000/products/create` | Formulir rapi dengan field SKU, nama kosmetik, kategori, supplier, harga, stok, batas minimum, dan deskripsi. |
| 4 | **Hasil Validasi Form** | `http://127.0.0.1:8000/products` (Submit kosong) | Pesan error validasi merah di bawah input form ketika field wajib dikosongkan. |
| 5 | **Halaman Kategori** | `http://127.0.0.1:8000/categories` | Tabel kategori (*Skincare, Makeup, Body Care, dll*) beserta badge jumlah produk terkait dan tombol CRUD. |
| 6 | **Halaman Supplier** | `http://127.0.0.1:8000/suppliers` | Tabel daftar supplier, kontak telepon, email, alamat kantor, dan total produk yang dipasok. |
| 7 | **Form Transaksi Stok Masuk & Keluar** | `http://127.0.0.1:8000/stock-movements/in` & `/out` | Form mutasi stok interaktif dengan kalkulasi otomatis estimasi sisa stok dan proteksi stok minus. |
| 8 | **Terminal `php artisan route:list`** | Terminal / Command Prompt | Output terminal yang menampilkan 37 route terdaftar lengkap dengan HTTP method, URI, dan nama controller. |

---

## 9. Pengujian Aplikasi (Automated Testing)

Project ini dilengkapi dengan *automated feature test* (`tests/Feature/MelKosmetikTest.php`) yang mencakup 11 test case dan 66 assertion.

Jalankan perintah pengujian:
```bash
php artisan test
```

Hasil pengujian:
```text
PASS  Tests\Feature\ExampleTest
✓ the application returns a successful response

PASS  Tests\Feature\MelKosmetikTest
✓ dashboard page loads successfully
✓ product listing and filtering
✓ category and product eloquent relationships
✓ product crud lifecycle
✓ stock in transaction increases stock
✓ stock out transaction decreases stock
✓ stock out fails when exceeding available stock
✓ category crud and show
✓ supplier crud and show

Tests:    11 passed (66 assertions)
Duration: 0.97s
```

---

## 10. Daftar Perintah Artisan yang Sering Digunakan

```bash
# Menampilkan seluruh daftar route
php artisan route:list

# Menjalankan migrasi ulang dan seeder
php artisan migrate:fresh --seed

# Menjalankan server aplikasi
php artisan serve

# Menjalankan unit & feature testing
php artisan test

# Membersihkan cache konfigurasi & view
php artisan optimize:clear
```

---
*Dikembangkan dengan penuh dedikasi untuk mata kuliah Pemrograman Web / Laravel Fundamental — MEL KOSMETIK Inventory System.*
