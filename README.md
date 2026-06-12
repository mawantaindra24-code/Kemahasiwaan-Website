# Kemahasiwaan-Website

# Panduan Lengkap Deploy Proyek ke cPanel (Paling Mudah, Tanpa Setup Node.js di cPanel!)

Panduan ini berisi penjelasan langkah demi langkah untuk melakukan build aplikasi **STIE Eka Prasetya** di komputer lokal Anda, lalu mengunggahnya langsung ke hosting cPanel sebagai **Website Statis**. 

Karena semua fitur website (Berita, Agenda, Pengumuman, Galeri, Unduhan, Lowongan Kerja, Kontak, dsb.) serta **Login Admin & Kelola Data** telah sepenuhnya dimigrasikan untuk berkomunikasi **langsung** secara aman dan real-time dengan **Google Firebase Firestore & Auth**, Anda **SAMA SEKALI TIDAK PERLU** mengkonfigurasi atau Setup Node.js App di cPanel!

Aplikasi Anda sekarang berjalan **100% Serverless & Client-Side**, yang berarti:
*   🚀 **Performa Instan**: Website memuat jauh lebih cepat karena berjalan langsung di browser pengunjung tanpa beban server hosting cPanel.
*   🔒 **Super Aman**: Tidak ada celah keamanan backend di server hosting Anda. Keamanan autentikasi dan database dijaga penuh oleh enkripsi sistem Google Firebase.
*   💸 **Bebas Limit Hosting**: Tidak memakan resource RAM/CPU hosting Anda, sehingga hosting cPanel Anda tidak akan pernah mengalami "Resource Limit Reached."

---

## 🚀 Langkah-Langkah Deploy ke cPanel (Lewat Komputer Lokal)

Berikut 3 langkah sangat mudah untuk membuat website Anda online:

### Langkah 1: Kembangkan & Build di Komputer Lokal Anda
1.  Unduh source code proyek ini ke komputer lokal Anda (misal berupa file ZIP dari Settings AI Studio).
2.  Buka terminal/command prompt di folder proyek tersebut pada komputer Anda, lalu jalankan perintah berikut untuk mengunduh semua library:
    ```bash
    npm install
    ```
3.  Jalankan perintah pengompilasan (build):
    ```bash
    npm run build
    ```
4.  Setelah proses build selesai, Anda akan melihat sebuah folder baru bernama **`dist/`** muncul di komputer Anda. 
    *Folder `dist/` ini berisi file `index.html` dan kumpulan file aset CSS & JavaScript yang telah dioptimalkan secara maksimal.*

---

### Langkah 2: Mengunggah Folder `dist` ke cPanel
1.  Masuk ke dashboard **cPanel** website Anda.
2.  Cari dan buka menu **File Manager**.
3.  Masuk ke direktori **`public_html`** (ini adalah folder utama di mana file website Anda ditampilkan ke publik).
    *Jika sebelumnya ada file lama, Anda bisa mengamankannya terlebih dahulu ke folder cadangan.*
4.  Pada komputer lokal Anda, masuklah ke dalam folder **`dist/`** yang dihasilkan di Langkah 1.
5.  Pilih semua file dan folder di **dalam** folder `dist` tersebut (misal: folder `assets/`, file `index.html`, dsb.), lalu **kompres semuanya ke format ZIP** (namai file ZIP tersebut misalnya `web.zip`).
    *(⚠️ PENTING: Jangan kompres folder `dist`nya, melainkan isi file-file di DALAM folder `dist` tersebut!)*
6.  Di File Manager cPanel, klik tombol **Upload** di bagian atas, lalu unggah file `web.zip` tersebut ke dalam folder **`public_html`**.
7.  Setelah proses upload selesai (bar berwarna hijau), kembali ke File Manager, klik kanan pada file `web.zip` yang baru diunggah, lalu pilih **Extract**.

---

### Langkah 3: Mengatasi Routing SPA (Single Page Application) di cPanel
Karena aplikasi ini menggunakan React Router (Single Page Application), kita perlu menambahkan konfigurasi kecil agar saat halaman di-refresh (misalnya pengunjung masuk ke `/news` atau `/gallery` secara langsung), server hosting Apache cPanel tidak menampilkan error "404 Not Found".

1.  Di dalam folder **`public_html`** pada File Manager cPanel, cari file bernama **`.htaccess`**.
    *(Jika file tersebut belum ada atau tidak terlihat, klik tombol **Settings** di kanan atas File Manager, centang **"Show Hidden Files (dotfiles)"**, lalu klik **Save**).*
2.  Jika file `.htaccess` masih belum ada, buat file baru di dalam `public_html`dengan nama **`.htaccess`** (diawali dengan titik).
3.  Klik kanan pada file `.htaccess` tersebut, pilih **Edit**, dan masukkan kode berikut ini:
    ```apache
    <IfModule mod_rewrite.c>
      RewriteEngine On
      RewriteBase /
      RewriteRule ^index\.html$ - [L]
      RewriteCond %{REQUEST_FILENAME} !-f
      RewriteCond %{REQUEST_FILENAME} !-d
      RewriteRule . /index.html [L]
    </IfModule>
    ```
4.  Klik **Save Changes** (Simpan Perubahan) di kanan atas.

---

### 🎉 Selesai! Website Anda Sudah Online!
Buka alamat domain website Anda (misal: `https://kemahasiswaan.ekaprasetya.ac.id`), maka website portals mahasiswa Anda sekarang sudah aktif, langsung terhubung ke database cloud Firebase Firestore!

---

## 🔑 Cara Login ke Halaman Admin Portal

Dengan dimigrasikannya autentikasi ke Firebase, mengelola portal menjadi sangat canggih dan aman:

1.  Buka halaman login website Anda, contoh: `https://domain-anda.com/login`
2.  Anda dapat menggunakan metode masuk yang sangat praktis:
    *   **Metode 1 (Rekomendasi - Google Sign-In)**: Klik tombol **"Google Account"** di bagian bawah form login, lalu hubungkan dengan email administrator Anda (misalnya `itekaprasetya@gmail.com`). Akun Google Anda akan langsung dikenali sebagai Administrator portal secara instan dan sangat aman tanpa kata sandi!
    *   **Metode 2 (Email & Password)**: Daftarkan email admin di tab Firebase Authentication pada Firebase Console Anda, lalu lakukan login menggunakan email dan password tersebut secara langsung lewat form di halaman login.

---

## 🛠️ Tanya & Jawab (FAQ)

### 1. Bagaimana cara mengunggah atau mengganti gambar/file unduhan?
Toko data gambar galeri, spanduk, agenda, dan file PDF/dokumen akan langsung tersimpan secara aman dalam layanan **Firebase Storage** dan **Firestore**, sehingga Anda dapat mengelola website Anda secara real-time dari mana pun tanpa perlu bersentuhan dengan server cPanel lagi.

### 2. Apakah saya membutuhkan modul Node.js Server lagi di hosting?
Sama sekali tidak! Semua fungsi server dinamis sekarang disediakan gratis dan andal oleh cloud Google Firebase. Hosting cPanel Anda saat ini hanya berfungsi menyajikan file statis (HTML, CSS, JS) secara secepat kilat dengan penggunaan resource 0% CPU karena browser klien berinteraksi langsung dengan Firebase SDK.

---
*Dokumen ini dibuat khusus untuk mempermudah pemeliharaan portal kemahasiswaan STIE Eka Prasetya agar selalu stabil dan bebas kendala server.*
