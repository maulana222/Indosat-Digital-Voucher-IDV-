## API Overview

Base URL (production):

- `https://idv-api.pixlycode.app`

Lokal (dev): `http://localhost:6969`

### Autentikasi

- **Dashboard/private API**: kirim header `Authorization: Bearer <jwt>`
- **Public transaksi**: `POST /api/transaction` memakai **signature** `md5(username + api_key + ref_id)`
- **OtoMax (HTTP GET)**: `GET /api/v2/otomax/trx` & `/balance` — credential `memberID`/`pin`/`password` (lihat [OtoMax](otomax.md))
- **Public price-list**: `POST /api/price-list` memakai **signature** `md5(username + api_key + "pricelist")`
- **Public product-detail**: `POST /api/product-detail` memakai **signature** `md5(username + api_key + "productdetail")`

Harga & katalog mengikuti **owner** API key (`owner_user_id`). Upstream IDV mengikuti koneksi admin pemilik.

### Account (dashboard)

| Method | Path | Keterangan |
|--------|------|------------|
| `GET` | `/api/account/api-key` | Key, `connection_type`, `default_cb_url`, kredensial OtoMax |
| `PUT` | `/api/account/connection` | `api` \| `otomax` (+ `regenerate_otomax`) |
| `PUT` | `/api/account/callback` | Simpan / kosongkan `default_cb_url` |
| `PUT` | `/api/account/api-key/whitelist` | IP whitelist |

### Daftar endpoint utama

- **Auth**: `/api/auth/*`
- **Transaction**: `/api/transaction/*`
- **OtoMax**: `/api/v2/otomax/*` (alias `/reseller/api/v2/otomax/*`)
- **Price List (Open API)**: `/api/price-list`
- **Product Detail (Open API)**: `/api/product-detail`
- **IDV**: `/api/idv/*` (cookie / H2H config)
- **Voucher**: `/api/voucher/*`
- **Account**: `/api/account/*`
- **Members**: `/api/members/*` (Super Admin)
- **Web Push**: `/api/web-push/*`
- **Home Request**: `/api/home-request/*`

