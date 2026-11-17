# Lab8Web - PHP dan Database MySQL (CRUD Data Barang)

Nama : Jezen Dwiki Erlangga
NIM  : 312510106
Kelas: I252A

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

<img width="826" height="537" alt="Screenshot 2026-11-17 231748" src="https://github.com/user-attachments/assets/fd39b507-a1d2-42c5-9066-721ad19516be" />


## 2. Mengakses phpMyAdmin

Buka browser dan akses `http://localhost/phpmyadmin/`. phpMyAdmin dipakai untuk mengelola database MySQL lewat tampilan web.

<img width="1920" height="1080" alt="Screenshot (298)" src="https://github.com/user-attachments/assets/14a98c82-c2cb-4c3a-81fb-3cfbc323e769" />


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


<img width="1342" height="463" alt="Screenshot 2026-11-17 234115" src="https://github.com/user-attachments/assets/bd2eee5c-7bde-43cc-99cc-9e5e22a2e756" />



## 4. Menambahkan Data Awal

```sql
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES
('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

Data dicek dengan `SELECT * FROM data_barang;`.

<img width="1161" height="600" alt="Screenshot 2026-11-17 231859" src="https://github.com/user-attachments/assets/63747969-448d-4242-94a4-50766efda0bf" />


## 5. Membuat Folder Project

Buat folder `lab8_php_database` di `htdocs` (contoh: `d:\xampp\htdocs\lab8_php_database`), lalu akses lewat `http://localhost/lab8_php_database/`.

<img width="1920" height="1080" alt="Screenshot (299)" src="https://github.com/user-attachments/assets/8dcd3d43-0b54-46c6-8613-f14e2fa059f8" />


## 6. File Koneksi Database (`koneksi.php`)

File ini membuat koneksi ke MySQL memakai `mysqli_connect()` dengan host `localhost`, user `root`, password kosong, dan database `latihan1`. Jika koneksi gagal, program menampilkan pesan lalu berhenti (`die()`). File ini di-include oleh file lain yang butuh akses database.

<img width="1920" height="1080" alt="Screenshot (287)" src="https://github.com/user-attachments/assets/9b607dbc-68d7-4554-9310-d992106f9b6e" />


## 7. Menampilkan Data / Read (`index.php`)

`index.php` menjalankan `SELECT * FROM data_barang`, lalu hasilnya diulang dengan `while` + `mysqli_fetch_array()` dan ditampilkan dalam tabel. Setiap baris punya tautan **Ubah** dan **Hapus**, dan di atas tabel ada tautan **Tambah Barang**.


<img width="1920" height="1080" alt="Screenshot (292)" src="https://github.com/user-attachments/assets/d121f531-1bbb-4144-8a07-711fa16b58e9" />



## 8. Menambah Data / Create (`tambah.php`)

Form berisi nama, kategori, harga jual, harga beli, stok, dan upload gambar (`enctype="multipart/form-data"`). Saat tombol **Simpan** diklik, data dikirim lewat `POST`, gambar dipindahkan ke folder `gambar/` dengan `move_uploaded_file()`, lalu data disimpan dengan query `INSERT`. Setelah itu halaman diarahkan kembali ke `index.php`.

<img width="1920" height="1080" alt="Screenshot (300)" src="https://github.com/user-attachments/assets/4f5c7439-db38-4260-8ab8-6860b1dcafaf" />
<img width="1920" height="1080" alt="Screenshot (294)" src="https://github.com/user-attachments/assets/5437983f-e32a-49d4-9867-cef77c1dffa0" />



## 9. Mengubah Data / Update (`ubah.php`)

Halaman menerima `id` lewat `GET`, mengambil data lama dari database, dan mengisikannya ke form. Saat disimpan, query `UPDATE ... WHERE id_barang` dijalankan. Gambar hanya diperbarui kalau user memilih file baru.

<img width="1920" height="1080" alt="Screenshot (295)" src="https://github.com/user-attachments/assets/0f5e2eb1-62f4-4a62-9940-16e61183aad1" />


## 10. Menghapus Data / Delete (`hapus.php`)

Halaman menerima `id` lewat `GET`, menjalankan `DELETE FROM data_barang WHERE id_barang`, lalu kembali ke `index.php`.

<img width="1920" height="1080" alt="Screenshot (296)" src="https://github.com/user-attachments/assets/3cde6e1a-5fde-4d7c-ba02-ef5400a4cfae" />
<img width="1920" height="1080" alt="Screenshot (297)" src="https://github.com/user-attachments/assets/d1ea7e89-3b74-4c28-864f-25a5b7ec392a" />



## Catatan Perbaikan dari Modul

Beberapa bagian kode modul saya perbaiki supaya program berjalan benar:

- Header tabel `index.php` dibuat urut sesuai data (Harga Beli, Harga Jual), dan kolom Aksi diisi tautan Ubah/Hapus (di modul kolom itu menampilkan id).
- Menambah tautan **Tambah Barang** di `index.php`.
- `enctype` di `ubah.php` diperbaiki dari `multipart/formdata` menjadi `multipart/form-data` (kalau salah, upload gambar tidak jalan).
- Fungsi `is_select()` di `ubah.php` sekarang membandingkan dengan nilai opsi masing-masing (di modul semuanya dibandingkan dengan 'Komputer').
- Nama file gambar disimpan tanpa awalan `gambar/` agar sesuai data awal dan tidak jadi `gambar/gambar/...`.
- Query memakai prepared statement untuk mencegah SQL Injection, dan output memakai `htmlspecialchars()`.
- Ukuran kolom `nama` disamakan menjadi `varchar(50)` sesuai tabel rancangan di modul.
