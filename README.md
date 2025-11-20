# Lab8Web - Praktikum 8: PHP dan Database MySQL  
### Aplikasi CRUD Sederhana Data Barang (PHP Native + MySQL)

<p align="center">
  <img src="https://img.shields.io/badge/PHP-8.0%2B-blue?style=for-the-badge&logo=php"/>
  <img src="https://img.shields.io/badge/MySQL-8.0-blue?style=for-the-badge&logo=mysql"/>
  <img src="https://img.shields.io/badge/XAMPP-v8.2-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Selesai-brightgreen?style=for-the-badge"/>
</p>

**Mata Kuliah**    : Pemrograman Web  
**Dosen Pengampu** : Agung Nugroho, S.Kom., M.Kom.  
**Email Dosen**    : agung@pelitabangsa.ac.id  
**Universitas**    : Universitas Pelita Bangsa  
**Repository**     : Lab8Web (Praktikum 8)

---

## Tujuan Praktikum
1. Mahasiswa memahami konsep dasar Database dan MySQL
2. Mahasiswa memahami konsep dasar operasi CRUD menggunakan PHP
3. Mahasiswa mampu membuat program CRUD sederhana dengan PHP native

## Studi Kasus
Aplikasi manajemen **Data Barang** (tabel `data_barang`) dengan fitur:
- Create (Tambah barang + upload gambar)
- Read (Tampilkan semua data)
- Update (Ubah data barang)
- Delete (Hapus barang)

## Struktur Folder Project

lab8_php_database/
├── index.php        -> Halaman utama (Read - Daftar Barang)
├── tambah.php       -> Form tambah barang baru (Create)
├── ubah.php         -> Form ubah data barang (Update)
├── hapus.php        -> Proses hapus barang (Delete)
├── koneksi.php      -> Koneksi ke database MySQL
├── style.css        -> Styling tampilan
├── gambar/          -> Folder penyimpanan gambar barang yang di-upload
└── db_latihan1.sql  -> Script SQL database + tabel + data awal


## Langkah-langkah Praktikum & Penjelasan

### 1. Persiapan Environment
- Text editor: Visual Studio Code
- Web server: XAMPP
- Buat folder `lab8_php_database` di `htdocs`

### 2. Menjalankan MySQL Server & phpMyAdmin
- Start Apache dan MySQL dari XAMPP Control Panel  
- Akses: http://localhost/phpmyadmin

### 3. Membuat Database, Tabel, Menambah Data

### A. Membuat Database
```
CREATE DATABASE latihan1;
```

### B. Membuat Tabel
```
CREATE TABLE data_barang (
 id_barang int(10) auto_increment Primary Key,
 kategori varchar(30),
 nama varchar(30),
 gambar varchar(100),
 harga_beli decimal(10,0),
 harga_jual decimal(10,0),
 stok int(4)
);
```

# C. Menambah Data
```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

### 4. Membuat file koneksi database
```
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db   = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);

if (!$conn) {
    echo "Koneksi ke server gagal.";
    die();
} else {
    echo "Koneksi berhasil!";
}
?>
```

### 5. Membuat file index untuk menampilkan data (Read)
```
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = "SELECT * FROM data_barang";
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Data Barang</title>
<link href="style.css" rel="stylesheet" type="text/css">
</head>
<body>
<div class="container">
    <h1>Data Barang</h1>
    <a href="tambah.php">+ Tambah Barang</a>
    <br><br>
    <table border="1" cellpadding="10" cellspacing="0">
        <tr>
            <th>Gambar</th>
            <th>Nama Barang</th>
            <th>Kategori</th>
            <th>Harga Jual</th>
            <th>Harga Beli</th>
            <th>Stok</th>
            <th>Aksi</th>
        </tr>
        <?php if($result && mysqli_num_rows($result) > 0): ?>
            <?php while($row = mysqli_fetch_assoc($result)): ?>
                <tr>
                    <td>
                        <?php if($row['gambar']): ?>
                            <img src="<?= $row['gambar'];?>" width="80" alt="<?= $row['nama'];?>">
                        <?php else: ?>
                            -
                        <?php endif; ?>
                    </td>
                    <td><?= $row['nama'];?></td>
                    <td><?= $row['kategori'];?></td>
                    <td><?= $row['harga_jual'];?></td>
                    <td><?= $row['harga_beli'];?></td>
                    <td><?= $row['stok'];?></td>
                    <td>
                        <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a> | 
                        <a href="hapus.php?id=<?= $row['id_barang'];?>" onclick="return confirm('Yakin ingin dihapus?')">Hapus</a>
                    </td>
                </tr>
            <?php endwhile; ?>
        <?php else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
        <?php endif; ?>
    </table>
</div>
</body>
</html>
```
