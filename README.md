# Bug Hunter: Menjalankan aplikasi Laravel

Target: tampilkan empat perlengkapan, buka formulir pinjam, catat satu peminjaman, dan pastikan gambarnya tampil. Kamu tidak perlu menulis fitur baru. Perbaiki satu masalah pada satu waktu dan jelaskan penyebabnya.

## Aturan

- Kerjakan tanpa AI. Dokumentasi Laravel, pesan error, dan bantuan terminal boleh digunakan.
- Jangan jalankan `composer run setup`: perintah itu menyelesaikan beberapa tahap sekaligus.
- Catat tiap tahap: gejala, dugaan penyebab, tindakan, dan bukti berhasil.

## Alur kerja

1. Clone repo dari URL yang diberikan guru, lalu masuk ke folder project.
2. Periksa PHP 8.3+, Composer, Node.js, dan npm.
3. Coba `php artisan serve` dan buka URL yang ditampilkan. Amati error pertamanya.
4. Pasang kebutuhan project satu per satu. Periksa `composer.json`, `.env.example`, `database/migrations`, dan `database/seeders` saat relevan.
5. Setelah halaman awal tampil, pastikan daftar alat berisi empat item.
6. Klik **Pinjam Alat**, isi form, kirim, lalu periksa riwayat peminjaman.
7. Periksa gambar setiap alat, bukan hanya teksnya.

## Kartu hint

Buka hanya saat mencapai gejalanya. Hint sengaja tidak memberikan perintah akhir.

<details><summary>H1 — Artisan tidak menemukan vendor/autoload.php</summary>

Folder `vendor` tidak ikut Git. Cari pengelola paket PHP pada `composer.json`.

</details>

<details><summary>H2 — Konfigurasi lokal atau kunci aplikasi belum tersedia</summary>

Bandingkan `.env` dan `.env.example`. Laravel membutuhkan `APP_KEY` untuk enkripsi; cari perintah Artisan untuk membuatnya.

</details>

<details><summary>H3 — Koneksi database gagal</summary>

Lihat `DB_CONNECTION`. Database jenis apa yang sedang dicoba Laravel? Project ini dapat memakai SQLite tanpa server database terpisah.

</details>

<details><summary>H4 — Tabel database belum ada</summary>

File database dan tabel adalah dua hal berbeda. Cari migrasi serta perintah Artisan untuk menjalankannya.

</details>

<details><summary>H5 — Route bernama tidak ditemukan</summary>

Bandingkan nama route di `routes/web.php` dengan pemanggilan `route(...)` di view. Perhatikan satu huruf yang berbeda.

</details>

<details><summary>H6 — Halaman tampil tetapi alat kosong</summary>

Migrasi membuat struktur tabel. Cari sumber data awal di `database/seeders`.

</details>

<details><summary>H7 — Form pinjam menghasilkan error view</summary>

Periksa baris yang disebut pada error di `resources/views/borrowings/create.blade.php`. Hitung pembuka dan penutup ekspresi Blade pada judul halaman.

</details>

<details><summary>H8 — Salah satu gambar tidak tampil</summary>

Bandingkan path gambar pada seeder dengan file di `public/images`. Data yang sudah masuk database perlu diperbarui setelah sumbernya diperbaiki.

</details>

<details><summary>H9 — Perubahan .env tidak terbaca (jika terjadi)</summary>

Laravel dapat menyimpan konfigurasi di cache. Cari perintah Artisan untuk membersihkannya.

</details>

## Kriteria selesai

Halaman utama tampil tanpa error, berisi empat alat dan gambarnya. Form dapat dibuka dan transaksi yang dikirim muncul di riwayat. Serahkan catatan diagnosis untuk setiap masalah.
