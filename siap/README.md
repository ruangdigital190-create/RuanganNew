# RUANGDIGITAL190 — Website Baru dari Nol (v2)

Website pelanggan + dashboard admin ditulis **baru**, tanpa perlu file kode proyek sebelumnya. Bisa diterbitkan gratis dengan GitHub Pages dan menggunakan **satu project Supabase baru** sebagai database/Auth/Storage/Realtime. Tidak perlu Node.js, PowerShell, Docker, atau Supabase Edge Function untuk sistem inti.

> Penting: website ini adalah source code, **bukan database cloud yang sudah terpasang**. Agar login, perubahan katalog online, upload gambar, pesanan, dan tracking perangkat berjalan, ikuti pemasangan Supabase dan GitHub di bawah. Jangan menganggap semua fitur sudah aktif sebelum pengujian pada project milik Anda.

## 1. Apa saja isinya?

```text
RUANGDIGITAL190-BARU-LENGKAP/
├── index.html                  halaman toko pelanggan
├── admin.html                  dashboard admin (username + password)
├── reset.html                  halaman penggantian password via email
├── css/style.css               tampilan HP / tablet / PC
├── js/config.js                hanya URL dan public key Supabase
├── js/shared.js                koneksi, utilitas, sesi perangkat
├── js/store.js                 katalog, checkout, akun, poster, tiket
├── js/admin.js                 CRUD admin dan statistik
├── js/reset.js                 password reset
├── data/catalog-fallback.json  12 produk / 76 varian cadangan
├── assets/ruangdigital-logo.jpg
├── assets/mlbb-price-list.png
├── assets/mlbb-price-list.webp
├── assets/freefire-price-list.jpeg
├── assets/freefire-price-list.webp
├── sql/01-schema.sql           struktur tabel, RLS, Storage, Realtime
├── sql/02-seed-products.sql    12 produk dan 76 varian
└── sql/03-activate-admin.sql   aktifkan username sholeh123 sebagai admin
```

## 2. Pembuatan PROJECT SUPABASE BARU (lewat browser)

1. Buka <https://supabase.com/dashboard> dan masuk ke akun Supabase Anda.
2. Klik **New project** pada organization yang dipilih.
3. Pilih nama project misalnya **ruangdigital190-baru**.
4. Buat database password yang kuat dan simpan secara pribadi (jangan ditaruh di GitHub).
5. Pilih region yang dekat lalu klik **Create new project**.
6. Setelah project siap, di sidebar pilih **SQL Editor** → **New query**.
7. Buka file lokal `sql/01-schema.sql` dengan Notepad, salin **SEMUA** kode ke SQL Editor, klik **Run**. Harus sukses tanpa error.
8. Buka **New query** lagi, salin seluruh `sql/02-seed-products.sql`, klik **Run**.
9. Cek baris hasil: **products = 12**, **variants = 76**.
10. **Jangan jalankan `03-activate-admin.sql` dahulu**. Admin harus didaftarkan dulu setelah website online.

**Jika error pada SQL:** berhenti. Jangan jalankan file kedua atau hapus database sembarangan. Salin teks error (jangan screenshot kredensial) untuk diperiksa.

## 3. Salin URL dan PUBLIC KEY Supabase

1. Pada Supabase, buka **Project Settings** (ikon roda gigi) → bagian **API Keys** atau **Data API** sesuai tata letak dashboard Anda.
2. Cari **Project URL**, bentuknya `https://xxxxxxxx.supabase.co`.
3. Cari **Publishable key** (`sb_publishable_...`) atau **anon/public key** (`eyJ...`). Boleh dipakai di frontend bersama RLS.
4. Buka file `js/config.js` dan ubah tepat dua nilai:

```js
supabaseUrl: "https://REF_PROJECT_ANDA.supabase.co",
supabaseAnonKey: "PUBLIC_KEY_SUPABASE_ANDA",
```

5. Bila nomor WhatsApp toko berubah, ubah `whatsappNumber` juga.
6. **Jangan memasukkan `service_role`, `sb_secret_...`, database password, token pribadi, atau password admin ke HTML, JS, atau GitHub.**

## 4. Buat REPOSITORY GITHUB BARU

1. Buka <https://github.com> dan login.
2. Klik tanda **+** kanan atas → **New repository**.
3. Isi **Repository name** misalnya `RuangDigital190-Baru`.
4. Pilih **Public** (GitHub Free mendukung GitHub Pages pada repository publik).
5. Centang **Add a README file** supaya branch `main` langsung dibuat.
6. Klik **Create repository**.
7. Di halaman repository, klik **Add file** → **Upload files**.
8. Ekstrak ZIP paket ini pada komputer. **Masuk ke folder hasil ekstraksi**, lalu tarik **seluruh ISI** ke area upload GitHub (bukan folder pembungkus). Struktur `css/`, `js/`, `sql/`, `assets/`, dan `data/` wajib dipertahankan.
9. Scroll ke bawah → klik **Commit changes**.
10. Pastikan file `index.html`, `admin.html`, `reset.html`, serta folder `css`, `js`, `sql`, `assets`, dan `data` terlihat pada **root repository**, bukan masuk ke satu subfolder paket.
11. Jika file `js/config.js` masih berisi placeholder, klik `js` → `config.js` → ikon **pensil/Edit** → tempel URL+public key baru → **Commit changes**.

## 5. Terbitkan GITHUB PAGES

1. Pada repo baru, klik **Settings**.
2. Di sidebar kiri cari **Pages** (kadang di bawah **Code and automation**).
3. Di **Build and deployment**, pilih **Source → Deploy from a branch**.
4. **Branch → main** dan folder **/(root)**, lalu klik **Save**.
5. Setelah GitHub memproses deployment, tautan biasanya berbentuk:
   `https://NAMA-AKUN.github.io/RuangDigital190-Baru/`
6. Klik **Visit site** yang muncul di halaman Pages.
7. Jika 404, pastikan `index.html` benar-benar berada di root branch `main`, tunggu status deployment GitHub dan refresh.

## 6. Atur AUTH URL agar email verifikasi menuju situs baru

1. Supabase → **Authentication** → **URL Configuration**.
2. Set **Site URL** menjadi URL Pages baru, misalnya `https://NAMA-AKUN.github.io/RuangDigital190-Baru/`.
3. Tambahkan **Redirect URLs** untuk:
   - `https://NAMA-AKUN.github.io/RuangDigital190-Baru/`
   - `https://NAMA-AKUN.github.io/RuangDigital190-Baru/reset.html`
4. Pastikan provider **Email** aktif pada **Authentication → Providers**.
5. Untuk website produksi, gunakan SMTP Anda sendiri agar konfirmasi akun dan reset password dapat dikirim dengan andal.

## 7. DAFTAR ADMIN `sholeh123`

1. Buka **website pelanggan** yang sudah online, bukan admin.html.
2. Klik tombol **Akun** kanan atas → **Buat Akun**.
3. Isi username: **`sholeh123`**.
4. Isi nama pemilik, **email aktif milik Anda**, dan **password baru**. Bila Anda tetap ingin memulai dengan `sholeh123` sebagai password, masukkan secara manual saat pendaftaran. Password **tidak ada di source**. Demi keamanan, segera ganti password tersebut dengan yang unik dan kuat.
5. Klik **Daftar Akun Baru**, lalu verifikasi email jika ada permintaan.
6. Buka Supabase → **Authentication** → **Users**, cari email pemilik untuk memastikan pendaftaran berhasil.
7. Supabase → **SQL Editor** → **New query** → buka `sql/03-activate-admin.sql`, salin semua → klik **Run**.
8. Harus muncul **satu baris**: `username = sholeh123`, `role = admin`. Jika 0 baris, periksa langkah pendaftaran dulu.
9. Buka `https://NAMA-AKUN.github.io/RuangDigital190-Baru/admin.html`.
10. Pada **Username Admin**, ketik `sholeh123`; pada **Password**, ketik password yang Anda daftarkan; klik **Masuk ke Dashboard**.
11. Anda dapat mengelola produk dan paket, mengunggah gambar, memperbarui poster, melihat pembeli dan perangkat/browser login.

**Mengapa tanpa Edge Function?** Admin login memanggil RPC `resolve_admin_login` dalam database, kemudian login password resmi via Supabase Auth. Lalu database memeriksa role `admin`. Tidak ada endpoint `username-login` yang perlu di-deploy. RPC khusus admin tersebut mengembalikan email login akun admin kepada pemanggil yang mengetahui username; gunakan email khusus admin jika privasi email penting.

## 8. Menguji katalog LIVE

1. Masuk ke dashboard `admin.html`.
2. Pilih **Katalog** → **+ Tambah Produk**.
3. Isi nama, brand, jenis, kategori, gambar (boleh pilih dari file HP), deskripsi, catatan, dan status **Aktif** → **Simpan Produk**.
4. Pilih **Varian & Harga** → **+ Tambah Varian**, pilih produk tadi, isi nama paket, harga, stok → **Simpan Varian**.
5. Di perangkat/tab lain, buka website publik pada halaman katalog. Produk baru harus tampil (Realtime bila aktif; fallback refresh otomatis tiap 60 detik, dan halaman refresh manual juga mengambil versi terbaru).
6. Coba ubah harga/stok, lalu periksa di website publik. Jangan lupa bahwa harga di checkout online dihitung ulang oleh SQL dari database.
7. Pilih **Price List** pada dashboard admin, upload poster baru MLBB atau Free Fire. Gambar tampil di website publik.
8. Jika gambar gagal: periksa apakah SQL 01 berhasil dan bucket `rd190-images` ada pada menu **Storage**.

## 9. Tentang pembelian dan QRIS

- Guest dapat membuat ringkasan pesanan ke WhatsApp tanpa membuat akun.
- Pelanggan yang login dapat membuat pesanan online; server mencatat `orders` dan `order_items` beserta harga aktual.
- Admin melihat pesanan dan dapat memperbarui pemenuhan pesanan serta status pembayaran **setelah verifikasi manual**.
- **Pembayaran QRIS otomatis tidak tersedia** pada paket ini. Jangan mengklaim pembayaran otomatis atau mengirim barang hanya berdasarkan input pelanggan. Integrasi payment gateway dengan webhook tervalidasi diperlukan jika ingin QRIS otomatis kelak.
- Tidak ada data kartu atau password pelanggan yang disimpan di HTML.

## 10. Keamanan dan batasan

- **Jangan gunakan password admin sederhana sebagai password permanen.** Password yang sudah disebutkan di percakapan harus dianggap berisiko dan diganti sebelum layanan publik.
- `RLS` membatasi perubahan produk/admin. Halaman admin yang bisa dibuka sebagai URL **bukan** berarti orang umum bisa masuk dashboard, karena akses data tetap diperiksa Supabase.
- Pada `device_sessions`, **online** berarti heartbeat dalam tiga menit terakhir. Ini jumlah sesi browser, bukan jumlah perangkat fisik yang pasti.
- Hanya gambar produk/poster yang bisa diunggah admin. Bucket berstatus publik agar poster bisa dilihat pembeli.
- Produk awal dari data katalog terdahulu dipakai sebagai **contoh seed**. Pastikan harga, stok, lisensi, serta ketentuan penggunaan sudah tepat dan sah sebelum menerima pembayaran.
- Pengiriman produk digital dan konfirmasi pembayaran masih manual melalui admin.
- **Tidak ada jaminan nol bug** untuk kombinasi semua browser dan pengaturan cloud. Tes di project Supabase Anda dengan akun pelanggan dan admin berbeda sebelum mengumumkan website sebagai toko produksi.

## 11. Jika login gagal, diagnosis cepat

| Pesan | Penyebab yang umum | Tindakan |
|---|---|---|
| Supabase belum dikonfigurasi | `js/config.js` belum diisi atau salah | Isi URL dan public key project BARU |
| Username/password tidak sesuai | Belum mendaftar/verify atau salah password | Cek Authentication → Users, lalu coba lagi |
| Akun belum admin | SQL `03-activate-admin.sql` belum dijalankan atau 0 baris | Daftar dulu lalu jalankan SQL, harus muncul 1 baris |
| Database belum siap | `01-schema.sql` gagal/terlewat | Buka SQL Editor dan jalankan SQL baru sampai sukses |
| Produk tidak berubah | Realtime belum terkoneksi atau belum reload | Refresh halaman publik dan periksa tabel products |
| Upload ditolak | Tidak ada role admin atau storage policy/bucket belum dibuat | Periksa `Storage → rd190-images`, role admin dan SQL |
| Email tidak masuk | Belum atur SMTP / email confirmation | Periksa Supabase Authentication → Email dan Spam |
| 404 GitHub | File di subfolder atau Pages salah branch | Pindahkan `index.html` ke root, Settings → Pages main/root |

Dokumentasi: <https://supabase.com/docs/guides/auth/passwords>, <https://supabase.com/docs/guides/database/postgres/row-level-security>, <https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site>.
