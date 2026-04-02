## IDV Gateway

IDV Gateway adalah server API yang menjembatani sistem Anda dengan layanan IDV (Indosat Digital Voucher) dan menyediakan dashboard untuk monitoring, manajemen user, serta operasi transaksi.

### Komponen utama

- **Backend API**: melayani request transaksi, callback, autentikasi dashboard, manajemen produk, dan kontrol session IDV.
- **Dashboard**: UI admin untuk login, melihat transaksi, statistik, pengaturan akun/2FA, serta konfigurasi koneksi (cookie).

### Konsep penting

- **Dua jenis akses API**:
  - **Public transaksi**: `POST /api/transaction` untuk integrasi client (dengan signature).
  - **Private dashboard**: endpoint lain umumnya memakai **JWT** (`Authorization: Bearer ...`).
- **Session IDV**: backend menjaga session/cookies ke IDV agar request spending/produk bisa berjalan tanpa login manual terus-menerus.

