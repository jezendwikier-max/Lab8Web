# Lab8Web - PHP dan Database MySQL (CRUD Data Barang)

Nama : (isi nama)
NIM  : (isi NIM)
Kelas: (isi kelas)

Praktikum 8 Pemrograman Web: membuat aplikasi CRUD sederhana dengan PHP dan MySQL (XAMPP).

## Struktur File

```
lab8_php_database/
├── koneksi.php   # koneksi ke database
├── index.php     # Read: menampilkan data
├── tambah.php    # Create: menambah data
├── ubah.php      # Update: mengubah data
├── hapus.php     # Delete: menghapus data
├── style.css     # tampilan
├── database.sql  # script database & tabel
└── gambar/       # folder penyimpanan gambar upload
```

## 1. Persiapan: Menjalankan MySQL Server

Buka XAMPP Control Panel, lalu klik **Start** pada modul **Apache** dan **MySQL** sampai statusnya berwarna hijau.

![XAMPP Control](screenshot/01_xampp.png)

## 2. Mengakses phpMyAdmin

Buka browser dan akses `http://localhost/phpmyadmin/`. phpMyAdmin dipakai untuk mengelola database MySQL lewat tampilan web.

![phpMyAdmin](screenshot/02_phpmyadmin.png)

## 3. Membuat Database dan Tabel

Di tab **SQL**, jalankan perintah berikut untuk membuat database `latihan1` dan tabel `data_barang` (lengkapnya ada di `database.sql`).

```sql
CREATE DATABASE latihan1;

CREATE TABLE data_barang (
  id_barang  int(10) auto_increment PRIMARY KEY,
  kategori   varchar(30),
  nama       varchar(50),
  gambar     varchar(100),
  harga_beli decimal(10,0),
  harga_jual decimal(10,0),
  stok       int(4)
);
```

Penjelasan: `id_barang` adalah primary key yang naik otomatis (auto_increment), sedangkan kolom lain menyimpan kategori, nama, nama file gambar, harga beli, harga jual, dan stok barang.

![Membuat tabel](screenshot/03_buat_tabel.png)

## 4. Menambahkan Data Awal

```sql
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES
('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

Data dicek dengan `SELECT * FROM data_barang;`.

![Data barang](screenshot/04_data_barang.png)

## 5. Membuat Folder Project

Buat folder `lab8_php_database` di `htdocs` (contoh: `d:\xampp\htdocs\lab8_php_database`), lalu akses lewat `http://localhost/lab8_php_database/`.

![Direktori lab8](screenshot/05_direktori.png)

## 6. File Koneksi Database (`koneksi.php`)

File ini membuat koneksi ke MySQL memakai `mysqli_connect()` dengan host `localhost`, user `root`, password kosong, dan database `latihan1`. Jika koneksi gagal, program menampilkan pesan lalu berhenti (`die()`). File ini di-include oleh file lain yang butuh akses database.

![Tes koneksi](screenshot/06_koneksi.png)

## 7. Menampilkan Data / Read (`index.php`)

`index.php` menjalankan `SELECT * FROM data_barang`, lalu hasilnya diulang dengan `while` + `mysqli_fetch_array()` dan ditampilkan dalam tabel. Setiap baris punya tautan **Ubah** dan **Hapus**, dan di atas tabel ada tautan **Tambah Barang**.

![Tampilan index](screenshot/07_index.png)

## 8. Menambah Data / Create (`tambah.php`)

Form berisi nama, kategori, harga jual, harga beli, stok, dan upload gambar (`enctype="multipart/form-data"`). Saat tombol **Simpan** diklik, data dikirim lewat `POST`, gambar dipindahkan ke folder `gambar/` dengan `move_uploaded_file()`, lalu data disimpan dengan query `INSERT`. Setelah itu halaman diarahkan kembali ke `index.php`.

![Tambah barang](screenshot/08_tambah.png)

## 9. Mengubah Data / Update (`ubah.php`)

Halaman menerima `id` lewat `GET`, mengambil data lama dari database, dan mengisikannya ke form. Saat disimpan, query `UPDATE ... WHERE id_barang` dijalankan. Gambar hanya diperbarui kalau user memilih file baru.

![Ubah barang](screenshot/09_ubah.png)

## 10. Menghapus Data / Delete (`hapus.php`)

Halaman menerima `id` lewat `GET`, menjalankan `DELETE FROM data_barang WHERE id_barang`, lalu kembali ke `index.php`.

![Hapus barang](screenshot/10_hapus.png)

## Catatan Perbaikan dari Modul

Beberapa bagian kode modul saya perbaiki supaya program berjalan benar:

- Header tabel `index.php` dibuat urut sesuai data (Harga Beli, Harga Jual), dan kolom Aksi diisi tautan Ubah/Hapus (di modul kolom itu menampilkan id).
- Menambah tautan **Tambah Barang** di `index.php`.
- `enctype` di `ubah.php` diperbaiki dari `multipart/formdata` menjadi `multipart/form-data` (kalau salah, upload gambar tidak jalan).
- Fungsi `is_select()` di `ubah.php` sekarang membandingkan dengan nilai opsi masing-masing (di modul semuanya dibandingkan dengan 'Komputer').
- Nama file gambar disimpan tanpa awalan `gambar/` agar sesuai data awal dan tidak jadi `gambar/gambar/...`.
- Query memakai prepared statement untuk mencegah SQL Injection, dan output memakai `htmlspecialchars()`.
- Ukuran kolom `nama` disamakan menjadi `varchar(50)` sesuai tabel rancangan di modul.
