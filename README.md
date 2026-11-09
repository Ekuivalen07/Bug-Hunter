Laporan Tugas Bug Hunter - Kelas TRPL
Tanggal Pelaksanaan: 9 – 11 September 2026

Materi: Debugging dan Troubleshooting Project Laravel

Studi Kasus: Aplikasi "Rental Gear"

Dokumen ini berisi laporan teknis tahapan identifikasi dan penanganan kesalahan (bug hunting & troubleshooting) pada proyek aplikasi Laravel. Berikut adalah rincian masalah yang ditemukan beserta langkah-langkah solusinya.

1. Instalasi Dependensi (Missing Vendor)
Masalah: Proyek Laravel tidak dapat dijalankan karena direktori vendor/ tidak tersedia. Framework Laravel tidak dapat berfungsi tanpa modul dan pustaka dependensi inti.

Solusi: Jalankan Composer untuk mengunduh seluruh dependensi yang tertera pada composer.json / composer.lock:

Bash
composer install
(Gunakan composer update hanya jika diperlukan pembaruan versi paket).

2. Konfigurasi Environment & Pembuatan APP_KEY
Masalah: Terjadi crash saat aplikasi dimuat karena file konfigurasi lingkungan (.env) belum ada dan variabel APP_KEY belum disetel. Kunci enkripsi ini wajib ada untuk pengamanan session, cookie, dan enkripsi data sensitif di Laravel.

Solusi:

Salin template environment bawaan:

Bash
cp .env.example .env
Buat kunci kriptografi aplikasi yang baru:

Bash
php artisan key:generate
3. Akses Database Ditolak (Access Denied)
Masalah: Muncul pengecualian SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhost'. Hal ini disebabkan konfigurasi koneksi database di file .env belum aktif (masih terkomentari) atau data autentikasi (user/password) tidak cocok dengan MySQL lokal.

Solusi:

Buka file .env, hilangkan tanda komentar (#), dan sesuaikan parameter kredensial MySQL lokal:

Cuplikan kode
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
Jalankan migrasi tabel database dan nyalakan development server:

Bash
php artisan migrate
php artisan serve
4. Kesalahan Penamaan Rute (Route Not Defined)
Masalah: Tampil pesan Route [borrowings.index] not defined di browser saat memuat halaman utama.

Solusi: Berdasarkan Exception trace, kesalahan terjadi pada file layout resources/views/layouts/app.blade.php. Terdapat ketidakcocokan nama rute: kode memanggil route('borrowings.index') (bentuk jamak), sementara rute yang terdaftar pada sistem adalah borrowing.index (bentuk tunggal). Menghapus huruf akhiran 's' pada pemanggilan helper route() menyelesaikan masalah ini.

5. Daftar Perlengkapan Kosong (Empty Equipment List)
Masalah: Halaman katalog berhasil dimuat, namun tabel/daftar perlengkapan (seperti backpack, tenda, dsb.) tidak menampilkan data apa pun.

Solusi: Data bawaan awal aplikasi tersimpan pada direktori database/seeders/. Jalankan seeder database untuk mengisi kembali data awal/dummy ke dalam tabel:

Bash
php artisan db:seed
(Atau jalankan php artisan migrate:fresh --seed untuk me-reset struktur sekaligus mengisi ulang data).

6. File Gambar Aset Tidak Ditemukan (Broken Image)
Masalah: Gambar untuk produk "Power Bank" tidak tampil (broken image) pada antarmuka web.

Solusi: Setelah memeriksa direktori public/images/, ditemukan ketidaksinkronan antara nama fisik file gambar dan nama file yang dicatat pada seeder database (database/seeders/). Nama file diubah (rename) dari power-bank.jpg menjadi power-bank-missing.jpg agar sesuai dengan referensi data di sistem.

7. Kesalahan Sintaks (ParseError) pada Blade Template
Masalah: Saat tombol "Pinjam Alat" diklik, halaman mengalami Internal Server Error dengan pesan ParseError: syntax error, unexpected token "{" pada file resources/views/borrowings/create.blade.php:16.

Solusi: Melacak nomor baris sesuai petunjuk stack trace. Ditemukan kelebihan kurung kurawal ganda ({{) yang tidak ditutup dengan benar pada baris ke-16. Menghapus karakter kurung kurawal yang redundan tersebut membuat form create dapat dirender secara normal.

8. Penyesuaian Identitas Aplikasi & Pembersihan Cache
Masalah: Nama aplikasi pada antarmuka dan header masih menggunakan nilai default ("Laravel").

Solusi:

Buka file .env, lalu perbarui nilai APP_NAME dengan memberi tanda kutip ganda karena memuat spasi:

Cuplikan kode
APP_NAME="Rental Gear"
Jalankan pembersihan cache konfigurasi agar perubahan segera terbaca:

Bash
php artisan config:clear
Kesimpulan
Proses bug hunting ini menegaskan pentingnya pemanfaatan pesan error (Exception Stack Trace) di browser maupun log Laravel untuk menentukan letak file dan baris yang bermasalah secara presisi. Selain itu, pemahaman menyeluruh terhadap arsitektur dasar Laravel—meliputi konfigurasi .env, manajemen pustaka via Composer, sistem templating Blade, manajemen rute, hingga mekanisme seeding database—merupakan kunci utama dalam penyelesaian masalah (troubleshooting) secara efektif.
