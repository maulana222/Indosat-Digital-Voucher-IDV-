## API Overview

Base URL (default):

- `http://localhost:6969`

### Autentikasi

- **Dashboard/private API**: kirim header `Authorization: Bearer <jwt>`
- **Public transaksi**: `POST /api/transaction` memakai **signature** (lihat halaman Transaction)

### Daftar endpoint utama

- **Auth**: `/api/auth/*`
- **Transaction**: `/api/transaction/*`
- **IDV**: `/api/idv/*`
- **Voucher**: `/api/voucher/*`
- **Account**: `/api/account/*`
- **Members**: `/api/members/*`
- **Web Push**: `/api/web-push/*`
- **Home Request**: `/api/home-request/*`

