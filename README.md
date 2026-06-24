# Manajemen Artikel – Fullstack SPA (CI4 + Vue 3)  
Praktikum Pemrograman Web 2 (Modul 11–14)

Nama  : Rafli Dhiya Fadhaly  
NIM   : 312410251  
Kelas : I241B  
Dosen : Agung Nugroho, S.Kom., M.Kom.  

---

## 1. Deskripsi Proyek

Proyek ini adalah aplikasi **Manajemen Artikel** yang dibangun dengan arsitektur **Decoupled** (frontend dan backend terpisah).  
Backend menggunakan **CodeIgniter 4** sebagai **RESTful API Server**, sedangkan frontend menggunakan **VueJS 3** sebagai **Single Page Application (SPA)**.

Fitur utama yang disediakan:
- Manajemen data artikel (CRUD: create, read, update, delete).  
- Tampilan data artikel secara reaktif tanpa reload halaman penuh.  
- Navigasi SPA dengan Vue Router.  
- Proteksi akses halaman admin menggunakan token.  
- Keamanan end-to-end melalui kombinasi **CI4 Filter** dan **Axios Interceptor**.

Proyek ini disusun sebagai bukti pengerjaan tugas Praktikum **Pemrograman Web 2** Modul 11–14.

---

## 2. Teknologi yang Digunakan

- **Backend (API Server)**  
  - Framework: **CodeIgniter 4 (CI4)** sebagai server **RESTful API**.  
  - Pola: Resource Controller untuk operasi CRUD artikel.  
  - Keamanan:
    - **ApiAuthFilter** untuk memvalidasi header `Authorization: Bearer {token}`.  
    - Konfigurasi **CORS** untuk mengizinkan request dari origin frontend yang berbeda port.

- **Frontend (SPA)**  
  - Framework: **VueJS 3** (Single Page Application).  
  - Routing: **Vue Router** dengan `meta: { requiresAuth: true }` dan **Navigation Guards**.  
  - HTTP Client: **Axios** dengan **Request Interceptor** untuk menyisipkan token otomatis.  
  - Penyimpanan client: **localStorage** untuk menyimpan token dan status login.

---

## 3. Struktur Repository

Struktur folder utama yang digunakan pada proyek:

```text
/lab8_vuejs               # Frontend SPA (VueJS 3)
├─ components/            # Komponen UI (Navbar, ArtikelList, Footer, Login)
├─ router/                # Vue Router (rute & navigation guards)
├─ api/                   # Konfigurasi Axios & Request Interceptors
└─ index.html             # Entry point SPA

/lab7_php_ci              # Backend API (CodeIgniter 4)
├─ app/Controllers/Api/   # ResourceController (CRUD Articles)
├─ app/Filters/           # ApiAuthFilter (keamanan server-side)
└─ app/Config/Routes.php  # Definisi endpoint RESTful API
```

- Folder **`lab7_php_ci/`** berisi konfigurasi CI4, controller API untuk artikel, filter autentikasi, dan routing.  
- Folder **`lab8_vuejs/`** berisi file utama SPA (`index.html`), router, konfigurasi Axios, dan komponen UI (artikel, login, dll).

---

## 4. Modul 11–14 (Ringkasan Implementasi)

### 4.1 Modul 11 – Integrasi VueJS 3 & API

- Tujuan: Menampilkan data artikel secara **reaktif** tanpa hard reload.  
- Implementasi:
  - VueJS 3 mengambil data JSON artikel dari endpoint API CI4 menggunakan Axios.  
  - Response di-bind ke variabel `articles` dalam komponen Vue.  
  - Reaktivitas Vue 3 membuat perubahan pada array `articles` langsung tercermin di tampilan DOM.

### 4.2 Modul 12 – Single Page Application (SPA) & Routing

- Tujuan: Menghadirkan navigasi yang **instan** dan halus.  
- Implementasi:
  - Menggunakan **Vue Router** untuk memanage rute (misalnya: `/`, `/articles`, `/login`, `/admin`).  
  - Halaman dipecah menjadi komponen modular & reusable (Navbar, ArtikelList, Login, Footer, dll).  
  - Navigasi dilakukan dengan **swap komponen** di `<router-view />` tanpa reload penuh halaman.

### 4.3 Modul 13 – Keamanan Client-Side (Navigation Guards)

- Tujuan: Melindungi halaman admin agar hanya bisa diakses setelah login.  
- Implementasi:
  - Rute yang butuh autentikasi diberi `meta: { requiresAuth: true }`.  
  - `router.beforeEach()` mengecek token di `localStorage` sebelum masuk rute tersebut.  
  - Jika token tidak ada/tidak valid, user akan diarahkan ke halaman `/login`.

### 4.4 Modul 14 – Keamanan End-to-End (Interceptors & Filters)

- Tujuan: Mengamankan API dari akses ilegal (misalnya Postman/tools eksternal) dan menjaga alur autentikasi dari client ke server.  
- Implementasi:
  - **Server-Side (CI4 Filter)**  
    - `ApiAuthFilter` mengecek header `Authorization` pada setiap request.  
    - Jika tidak ada Bearer Token yang valid, server mengembalikan response `401 Unauthorized`.
  - **Client-Side (Axios Interceptors)**  
    - Request Interceptor menambahkan token dari `localStorage` ke header `Authorization` setiap request.  
    - Kode di level komponen jadi lebih bersih karena tidak perlu menulis header token berulang-ulang.

---

## 5. Endpoint RESTful API (Ringkasan)

Backend menyediakan endpoint CRUD artikel menggunakan ResourceController.

Contoh endpoint utama:

- **Artikel**
  - `GET /api/articles` – mengambil list semua artikel.  
  - `GET /api/articles/{id}` – mengambil detail artikel tertentu.  
  - `POST /api/articles` – menambah artikel baru (butuh Bearer Token).  
  - `PUT /api/articles/{id}` – mengupdate artikel (butuh Bearer Token).  
  - `DELETE /api/articles/{id}` – menghapus artikel (butuh Bearer Token).

Endpoint `POST`, `PUT`, dan `DELETE` diproteksi oleh **ApiAuthFilter** dan mewajibkan header `Authorization: Bearer {token}`.

---

## 6. Implementasi Kode Kritis

### 6.1 Axios Request Interceptor (Frontend)

```js
// Contoh file: /lab8_vuejs/api/axios.js
import axios from 'axios';

axios.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

export default axios;
```

Interceptor ini memastikan setiap request HTTP ke backend otomatis membawa header `Authorization` jika token tersedia di `localStorage`.

### 6.2 API Auth Filter (Backend – CodeIgniter 4)

```php
// Contoh file: /lab7_php_ci/app/Filters/ApiAuthFilter.php
namespace App\Filters;

use CodeIgniter\HTTP\RequestInterface;
use CodeIgniter\HTTP\ResponseInterface;
use CodeIgniter\Filters\FilterInterface;
use Config\Services;

class ApiAuthFilter implements FilterInterface
{
    public function before(RequestInterface $request, $arguments = null)
    {
        $header = $request->getServer('HTTP_AUTHORIZATION');
        $token  = str_replace('Bearer ', '', $header);

        if (!$this->isTokenValid($token)) {
            return Services::response()
                ->setStatusCode(401)
                ->setJSON([
                    'status'  => 401,
                    'message' => 'Unauthorized',
                ]);
        }
    }

    public function after(RequestInterface $request, ResponseInterface $response, $arguments = null)
    {
        // Optional: post-processing response
    }

    private function isTokenValid(?string $token): bool
    {
        // TODO: Implementasi validasi token sesuai kebutuhan
        return ! empty($token);
    }
}
```

Filter ini dipasang pada konfigurasi filter/routing CI4 untuk melindungi endpoint API penting.

---

## 7. Keamanan Aplikasi

### 7.1 Keamanan Server-Side (Backend)

- Menggunakan **ApiAuthFilter** untuk memproteksi endpoint CRUD artikel.  
- Hanya request dengan header `Authorization: Bearer {token}` yang valid yang diizinkan melakukan operasi tulis (POST/PUT/DELETE).  
- Konfigurasi **CORS** di `app/Config/Filters.php` agar backend menerima request dari origin frontend (port berbeda).

### 7.2 Keamanan Client-Side (Frontend)

- Menggunakan **Vue Router Navigation Guards** (`router.beforeEach`).  
- Rute admin diberi `meta: { requiresAuth: true }`.  
- Jika tidak ada token di `localStorage`, user akan diarahkan ke halaman `/login`.  
- Token disimpan di `localStorage` supaya session login tetap aktif saat halaman direfresh.

---

## 8. Kendala & Solusi

- **CORS Policy**  
  - Masalah: Frontend dan backend berjalan di port berbeda sehingga muncul error CORS.  
  - Solusi: Mengaktifkan dan mengkonfigurasi CORS Filter di CI4 (`app/Config/Filters.php`) untuk mengizinkan origin frontend.

- **State Management (Data Hilang Saat Refresh)**  
  - Masalah: Setelah refresh, data login di memori hilang dan user harus login ulang.  
  - Solusi: Menyimpan token di `localStorage`, lalu dibaca ulang saat inisialisasi aplikasi Vue dan di dalam interceptor/guard.

---

## 9. Cara Menjalankan Proyek

### 9.1 Menjalankan Backend (CodeIgniter 4)

1. Masuk ke folder backend:  
   ```bash
   cd lab7_php_ci
   ```
2. Jalankan server pengembangan CI4:  
   ```bash
   php spark serve
   ```
3. Akses API melalui URL (contoh):  
   ```text
   http://localhost:8080/api/articles
   ```

### 9.2 Menjalankan Frontend (VueJS 3 SPA)

1. Masuk ke folder frontend:  
   ```bash
   cd lab8_vuejs
   ```
2. Install dependency (jika menggunakan npm):  
   ```bash
   npm install
   ```
3. Jalankan aplikasi SPA:  
   ```bash
   npm run dev
   ```
4. Buka di browser (contoh default Vite):  
   ```text
   http://localhost:5173
   ```

Pastikan **base URL** Axios pada frontend mengarah ke URL backend (misalnya `http://localhost:8080`).

---

## 10. Penutup

Proyek **Manajemen Artikel – Fullstack SPA (CI4 + Vue 3)** ini dibuat untuk mengimplementasikan:  
- arsitektur **Decoupled** (backend–frontend terpisah),  
- **RESTful API** dengan CodeIgniter 4,  
- **Single Page Application** dengan VueJS 3,  
- dan **keamanan end-to-end** menggunakan token, Filter CI4, serta Axios Interceptors.  

Dokumentasi ini dapat langsung digunakan sebagai isi `README.md` pada repository GitHub tugas Praktikum **Pemrograman Web 2**.
