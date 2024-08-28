## **MATERI : SQL Injection UNION ATTACK**
Ketika aplikasi rentan terhadap SQL Injection, dan hasil dari query adalah respond dari aplikasi. Kamu bisa menggunakan ***UNION*** untuk mendapatkan data dari bagian lain ( _Other Table_ ) database. Ini biasa disebut dengan SQL injection UNION ATTACK. <br>
***UNION*** memungkinkan kamu untuk meng-execute 1 atau lebih _SELECT_ Query dan menambah hasil Query original. Contohnya :

>     SELECT a, b FROM table1 UNION SELECT c, d FROM table2
 SQL Query ini memberi 1 hasil dengan 2 kolom.<br> Berisi Kolom _a_ & _b_ pada _table1_ dan kolom _c_ & _d_ pada _table2_ 

agar ***UNION*** Query bekerja, 2 kunci utama harus terpenuhi: <br> <br>
_1. Queri Individu harus memberikan jumlah kolom yang sama_ <br>
_2. Tipe data pada tiap kolom harus kompatible diantara queri individu_

Untuk melakukan SQLi UNION Attack, Pastikan kamu memenuhi 2 keperluan ini. Normalnya ini bertujuan untuk mengetahui: <br> <br>
_1. Berapa kolom yang muncul dari query original_ <br>
_2. Manakah kolom yang muncul dari query original yang cocok dengan tipe data untuk menahan hasil dari query yang di inject_

## **MATERI : Determining the number of columns required**
Ketika kamu melakukan SQLi UNION Attack, ada 2 metode efektif untuk menentukan berapa banyak kolom yang muncul dari query original.
Salah satu metodenya melibatkan seri dari klausa _ORDER BY_ dan menambah kolom index yang spesifik hingga error terjadi. Contohnya, jika inject point adalah quoted string didalam klausa _WHERE_ dari query original, kamu harus menyertakan :
>
>
     ' ORDER BY 1--
     ' ORDER BY 2--
     ' ORDER BY 3--
     dst.
## **LAB : SQL injection UNION attack, retrieving multiple values in a single column**

1. Gunakan BURP SUITE untuk Intercept dan modifikasi request yang menyusun kategori produk filter
2. Memastikan angka dari kolom yang di kembalikan dari query dan memastikan kolom mana yang berisi data
	Verifikasi bila query mengembalikan 2 kolom, dan hanya 1 yang berisi text.
	menggunakan  payload seperti berikut di parameter " category "
>     ' UNION SELECT NULL,'abc'--

3. Menggunakan Payload berikut untuk mendapatkan isi content dari users table:
>     ' UNION SELECT NULL username || '~' || password FROM users--

4. Verifkasi respond apliaksi yang berisi username & password

## **Materi : Menganalisa database pada serangan SQL Injection**
Untuk melakukan exploit SQL injection, terkadang menemukan infromasi tentang database tersebut. Seperti: <br>
***~Tipe dan Versi software database*** <br>
***~Isi dari Table & Column ( Kolom & Baris ) Database*** <br>

## **Materi : Menanyakan Tipe dan Versi database**
Kamu bisa berpotensi mengidentifikasi tipe dan versi database secara bersamaan dengan cara meng-inject ( provfiider-specific query ) untuk melihat apakah ada yang berhasil. <br>
Berikut adalah beberapa Query untuk menentukan versi database untuk beberapa tipe database populer: <br>

>      
    Database type		Query
    Microsoft, MySQL	SELECT @@version
    Oracle	SELECT * 	FROM v$version
    PostgreSQL		SELECT version()

Sebagai contoh, kamu bisa menggunakan "UNION" attack dengan input berikut:
>     ' UNION SELECT @@version--

Ini adalah kemungkinan outputnya, pada tahap ini kamu bisa mengkonfirmasi jika database Microsoft SQL Server dan
melihat versi yang digunakan:
>

    Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64)
    Mar 18 2018 09:11:49
    Copyright (c) Microsoft Corporation
    Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)



