# Manajemen Artikel – Fullstack SPA (CI4 + Vue 3)  
Praktikum Pemrograman Web 2 (Modul 11–14)

Nama  : Rafli Dhiya Fadhaly  
NIM   : 312410251  
Kelas : I241B  
Dosen : Agung Nugroho, S.Kom., M.Kom.  

---
# Laporan Praktikum Pemrograman Web 2  
### Framework CodeIgniter 4 – Praktikum 1 sampai 10

Repositori ini berisi hasil praktikum mata kuliah Pemrograman Web 2 dengan studi kasus aplikasi **Blog Sederhana** menggunakan framework CodeIgniter 4 pada project `lab11_ci/ci4`.  
Setiap praktikum dikerjakan sesuai langkah pada modul, disertai penjelasan singkat dan screenshot perubahan yang dilakukan. [file:4][file:2][file:3]

---

## Praktikum 1 – Instalasi CI4 & Konfigurasi Dasar

**Tujuan:**  
Memahami konsep dasar framework, MVC, struktur direktori CodeIgniter 4, serta konfigurasi environment dan layout dasar. [file:4]

**Langkah-langkah yang dilakukan:**

1. Mengaktifkan ekstensi PHP di XAMPP  
   - Mengedit `C:\xampp\php\php.ini` untuk mengaktifkan ekstensi yang dibutuhkan (`mysqli`, `intl`, dll).  
   - Restart Apache dan MySQL dari XAMPP Control Panel.  
   *(Screenshot: XAMPP & php.ini)* [file:4]

2. Instalasi CodeIgniter 4 secara manual  
   - Ekstrak CI4 ke `C:\xampp\htdocs\lab11_ci\ci4`.  
   - Mengakses `http://localhost/lab11_ci/ci4/public/` untuk memastikan CI4 tampil.  
   *(Screenshot: tampilan default CodeIgniter 4)* [file:4]

3. Konfigurasi environment  
   - Mengubah nama file `env` menjadi `.env`.  
   - Mengatur `CI_ENVIRONMENT = development` agar error detail muncul saat debug. [file:4]

4. Konfigurasi `.env` dan baseURL  
   - Menambahkan konfigurasi koneksi database ke `lab11_ci`.  
   - Mengatur `app.baseURL = 'http://localhost/lab11_ci/ci4/public/'`.  
   *(Screenshot: isi file `.env`)* [file:4]

5. Memahami struktur direktori CI4  
   - Mempelajari fungsi folder `app`, `public`, `system`, `writable`, dan file `spark`. [file:4]

6. Membuat routing, controller, dan view sederhana  
   - Menambahkan route untuk `home`, `about`, `contact` di `app/Config/Routes.php`.  
   - Membuat controller `Home` dan `Page` di `app/Controllers`.  
   - Membuat view sederhana di `app/Views` untuk menampilkan teks.  
   *(Screenshot: tampilan halaman Home/About/Contact)* [file:4]

7. Membuat layout dan template  
   - Menambahkan file `style.css` di folder `public`.  
   - Membuat `app/Views/template/header.php` dan `footer.php` sebagai layout utama.  
   - Mengubah view agar menggunakan `header` dan `footer` template.  
   *(Screenshot: tampilan layout blog sederhana)* [file:4]

---

## Praktikum 2 – CRUD Artikel (Model, Controller, View)

**Tujuan:**  
Memahami konsep Model dan CRUD (Create, Read, Update, Delete) pada tabel `artikel`. [file:2]

**Langkah-langkah yang dilakukan:**

1. Membuat database dan tabel `artikel`  
   - Menggunakan database `lab11_ci`.  
   - Membuat tabel `artikel` dengan field: `id`, `judul`, `isi`, `slug`, `gambar`, `status` dan timestamp.  
   *(Screenshot: struktur tabel artikel di phpMyAdmin)* [file:2]

2. Membuat `ArtikelModel`  
   - File: `app/Models/ArtikelModel.php`.  
   - Menentukan `$table = 'artikel'`, `$primaryKey = 'id'`, dan `$allowedFields` sesuai kolom tabel. [file:2]

3. Membuat controller `Artikel`  
   - File: `app/Controllers/Artikel.php`.  
   - Method `index()` untuk menampilkan daftar artikel.  
   - Method `view($slug)` untuk menampilkan detail artikel berdasarkan slug.  
   *(Screenshot: daftar artikel dan detail artikel)* [file:2]

4. Membuat view `artikel/index.php` dan `artikeldetail.php`  
   - Menampilkan judul, potongan isi, gambar, dan link ke detail.  
   - Menggunakan layout template header/footer dari praktikum 1. [file:2]

5. Membuat menu admin artikel  
   - Method `adminIndex()` untuk daftar artikel di halaman admin.  
   - View `artikel/admin_index.php` untuk menampilkan tabel artikel dengan tombol `Edit` dan `Delete`. [file:2]

6. Implementasi tambah, ubah, dan hapus artikel  
   - Method `add()`, `edit($id)`, dan `delete($id)` di controller `Artikel`.  
   - View `artikelform_add.php` dan `artikelform_edit.php` untuk form input.  
   *(Screenshot: form tambah, ubah, dan hasil penghapusan artikel)* [file:2]

---

## Praktikum 3 – Penyempurnaan Tampilan & Routing

**Tujuan:**  
Menyempurnakan struktur tampilan front-end dan admin, serta pengaturan routing yang lebih rapi. [file:1][file:2]

**Langkah-langkah yang dilakukan:**

1. Menyusun ulang layout front dan admin  
   - Menambahkan template khusus admin (`template/admin_header.php`, `template/admin_footer.php`).  
   - Memisahkan tampilan front (user) dan admin.  
   *(Screenshot: tampilan admin index artikel)* [file:2]

2. Menyesuaikan routing untuk artikel dan halaman lain  
   - Menambah route `artikel` dan `artikel/(:any)` untuk detail.  
   - Menambah group route `admin` untuk halaman admin artikel. [file:2]

3. Pengujian menu front dan admin  
   - Mengakses `Home`, `Artikel`, `About`, `Contact` di front.  
   - Mengakses `/admin/artikel` untuk halaman admin.  
   *(Screenshot: navigasi header beserta link yang berfungsi)* [file:1][file:2]

---

## Praktikum 4 – Modul Login (Auth & Filter)

**Tujuan:**  
Membuat modul login sederhana dengan auth dan filter untuk membatasi akses ke halaman admin. [file:3]

**Langkah-langkah yang dilakukan:**

1. Membuat tabel `user` di database `lab11_ci`  
   - Field: `id`, `username`, `useremail`, `userpassword`.  
   - Menambahkan index unik pada `useremail`.  
   *(Screenshot: struktur tabel user di phpMyAdmin)* [file:3]

2. Membuat `UserModel`  
   - File: `app/Models/UserModel.php`.  
   - Mengatur `$table = 'user'`, `$primaryKey = 'id'`, dan `$allowedFields` (`username`, `useremail`, `userpassword`). [file:3]

3. Membuat seeder `UserSeeder`  
   - File: `app/Database/Seeds/UserSeeder.php`.  
   - Menambahkan data user admin (`admin@example.com`) dengan password yang di-hash (`password_hash('admin123', PASSWORD_DEFAULT)`).  
   - Menjalankan seeder: `php spark db:seed UserSeeder`.  
   *(Screenshot: hasil seeder di CLI dan data user di database)* [file:3]

4. Membuat controller `User`  
   - Method `login()` untuk proses login (cek email, verifikasi password, set session).  
   - Method `logout()` untuk menghapus session dan redirect ke halaman login. [file:3]

5. Membuat view `user/login.php`  
   - Form login dengan input email dan password.  
   - Menampilkan pesan error menggunakan `flashdata` jika login gagal.  
   *(Screenshot: halaman login)* [file:3]

6. Membuat filter `Auth`  
   - File: `app/Filters/Auth.php`.  
   - Mengecek session `logged_in`. Jika belum login, redirect ke `/user/login`. [file:3]

7. Konfigurasi filter dan routes  
   - Menambahkan filter `auth` di `app/Config/Filters.php`.  
   - Menggunakan filter `auth` untuk group route `admin`.  
   *(Screenshot: percobaan akses `/admin/artikel` sebelum dan sesudah login)* [file:3]

---

## Praktikum 5 – Upload Gambar Artikel

**Tujuan:**  
Menambahkan fitur upload gambar untuk artikel dan menyimpan file di folder `public/gambar`. [file:8]

**Langkah-langkah yang dilakukan:**

1. Menyiapkan folder upload  
   - Membuat folder `public/gambar` untuk menyimpan file gambar artikel. [file:8]

2. Menambahkan field `gambar` di model dan form  
   - Memastikan `ArtikelModel` menyertakan field `gambar` di `$allowedFields`.  
   - Mengubah view form tambah/edit artikel supaya memiliki input type `file` (`<input type="file" name="gambar">`). [file:2][file:8]

3. Mengubah controller `Artikel` untuk menangani upload  
   - Pada method `add()` dan `edit()`, memproses file upload menggunakan `$this->request->getFile('gambar')`.  
   - Menyimpan nama file gambar ke database dan memindahkan file ke folder `public/gambar`.  
   *(Screenshot: form artikel dengan upload gambar dan tampilan artikel dengan gambar)* [file:8]

---

## Praktikum 6 – Relasi Artikel dan Kategori (Query Builder)

**Tujuan:**  
Menerapkan relasi **One-to-Many** antara tabel `kategori` dan `artikel`, serta menggunakan Query Builder untuk join data. [file:5]

**Langkah-langkah yang dilakukan:**

1. Membuat tabel `kategori`  
   - Field: `idkategori`, `namakategori`, `slugkategori`.  
   - Menggunakan database `lab11_ci`.  
   *(Screenshot: struktur tabel kategori)* [file:5]

2. Menambah foreign key di tabel `artikel`  
   - Menambahkan field `idkategori` pada tabel `artikel`.  
   - Menambahkan constraint `FOREIGN KEY (idkategori) REFERENCES kategori(idkategori)`.  
   *(Screenshot: relasi artikel–kategori di phpMyAdmin)* [file:5]

3. Membuat `KategoriModel`  
   - File: `app/Models/KategoriModel.php`.  
   - Menentukan `$table = 'kategori'`, `$primaryKey = 'idkategori'`, dan `$allowedFields` (`namakategori`, `slugkategori`). [file:5]

4. Memodifikasi `ArtikelModel` untuk join kategori  
   - Menambahkan method `getArtikelDenganKategori()` yang menggunakan Query Builder untuk join `artikel` dan `kategori`. [file:5]

5. Memodifikasi controller `Artikel`  
   - Method `index()` dan `adminIndex()` menggunakan data join untuk menampilkan nama kategori.  
   - Menambahkan filter pencarian judul dan filter kategori di admin. [file:5]

6. Memodifikasi view index dan admin  
   - Menampilkan nama kategori pada daftar artikel.  
   - Menambahkan dropdown kategori di form tambah/edit artikel dan di filter admin.  
   *(Screenshot: daftar artikel dengan nama kategori dan form dengan pilihan kategori)* [file:5]

---

## Praktikum 7–8 – Pagination dan Search

**Tujuan:**  
Menambah fitur pencarian dan pagination untuk daftar artikel di halaman admin dan front-end. [file:7][file:10]

**Langkah-langkah yang dilakukan:**

1. Menambahkan pencarian judul artikel  
   - Di controller `Artikel`, menambahkan parameter `q` dari request untuk keyword pencarian.  
   - Menggunakan `like()` pada Query Builder jika keyword diisi. [file:5][file:10]

2. Menambahkan filter kategori di admin  
   - Mengambil `kategoriid` dari request dan menerapkan `where('artikel.idkategori', $kategoriid)` jika dipilih. [file:5]

3. Menambahkan pagination  
   - Menggunakan `paginate(10)` di model untuk membatasi jumlah artikel per halaman.  
   - Menampilkan link pagination menggunakan `$pager->links()` di view admin. [file:10]

4. Menyesuaikan tampilan  
   - Menambahkan form pencarian dan filter kategori di `artikel/admin_index.php`.  
   - Menampilkan hasil pencarian dan pagination di halaman admin.  
   *(Screenshot: tampilan admin dengan search dan pagination)* [file:10]

---

## Praktikum 9 – AJAX Pagination dan Search

**Tujuan:**  
Mengimplementasikan pagination dan search menggunakan AJAX untuk meningkatkan user experience pada halaman admin artikel. [file:6]

**Langkah-langkah yang dilakukan:**

1. Menyiapkan endpoint AJAX di controller `Artikel`  
   - Memodifikasi `adminIndex()` untuk mengembalikan partial view atau JSON jika request adalah AJAX (`$this->request->isAJAX()`). [file:6]

2. Menambahkan script jQuery di view admin  
   - Menangani event submit form pencarian dan klik link pagination.  
   - Mengirim request AJAX ke endpoint admin artikel dan mengupdate isi tabel secara dinamis. [file:6]

3. Menguji AJAX search dan pagination  
   - Mengakses `/admin/artikel`.  
   - Melakukan pencarian artikel dan berpindah halaman tanpa reload penuh.  
   *(Screenshot: hasil AJAX search & pagination di admin)* [file:6]

---

## Praktikum 10 – Komponen & Penyempurnaan Blog

**Tujuan:**  
Menambahkan komponen seperti **artikel terkini** menggunakan Cell, serta menyempurnakan tampilan blog. [file:9][file:1]

**Langkah-langkah yang dilakukan:**

1. Membuat Cell `ArtikelTerkini`  
   - File: `app/Cells/ArtikelTerkini.php`.  
   - Meng-extend `CodeIgniter\View\Cells\Cell` dan menambahkan method `render(): string` untuk mengambil 5 artikel terbaru dan me-render view `components/artikel_terkini`. [file:1]

2. Membuat view komponen artikel terkini  
   - File: `app/Views/components/artikel_terkini.php`.  
   - Menampilkan daftar judul artikel terkini dengan link ke detail. [file:9]

3. Menyisipkan Cell di layout  
   - Menggunakan `view_cell('App\Cells\ArtikelTerkini::render')` di layout front (misalnya di sidebar atau bawah konten). [file:9]

4. Penyempurnaan tampilan dan navigasi  
   - Menyesuaikan menu utama: `Home`, `Artikel`, `About`, `Contact`.  
   - Menambahkan link ke halaman admin artikel dan halaman login.  
   *(Screenshot: tampilan blog dengan komponen artikel terkini)* [file:9][file:1]

---

## Penutup

Seluruh praktikum 1 sampai 10 telah diimplementasikan dalam satu project CodeIgniter 4 (`lab11_ci/ci4`) dengan studi kasus blog sederhana:  

- Instalasi dan konfigurasi dasar CI4.  
- CRUD artikel beserta upload gambar.  
- Modul login dengan auth filter.  
- Relasi artikel–kategori dan Query Builder.  
- Pagination, search, dan AJAX untuk admin artikel.  
- Komponen artikel terkini menggunakan Cell. [file:2][file:3][file:5][file:6][file:9]

Setiap langkah telah didokumentasikan dalam README ini dan didukung oleh screenshot perubahan sesuai ketentuan modul. README akan diperbarui kembali saat praktikum 11–14/15 dikerjakan.
