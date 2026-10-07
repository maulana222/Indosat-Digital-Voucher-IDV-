## Fitur

### Backend API

- **Transaksi**: buat transaksi, list, statistik, logs, cancel, retry — harga dari katalog `owner_user_id` admin.
- **OtoMax (HTTP GET)**: mode kompatibilitas reseller OtoMax per-member (`connection_type=otomax`) — lihat [OtoMax](api/otomax.md).
- **Callback**: Digiflazz `POST` JSON, atau OtoMax `GET` querystring; `default_cb_url` dari Account Settings jika request tidak mengirim `cb_url`.
- **Auth dashboard**: register/login, profile, 2FA (TOTP).
- **Upstream IDV**: Cookie spending **atau** H2H Nuon (`getUpstreamAdapter(userId)`) — lihat [Upstream H2H](architecture/upstream-h2h.md).
- **Account**: API key, whitelist, tipe koneksi api/otomax, callback URL, credential OtoMax.
- **Web push** & **Home request** (opsional).

### Dashboard

- Login & manajemen akun (termasuk 2FA).
- Monitoring transaksi (admin hanya milik sendiri; Super Admin semua).
- **Account Settings**: mode API / OtoMax, IP whitelist, callback default, credentials.
- **Koneksi**: Cookie IDV atau H2H Nuon — **per admin**, tidak dibagikan.

### Isolasi multi-admin

| Aspek | Scope |
|-------|--------|
| Katalog & harga | `products.owner_user_id` |
| Cookie IDV | `settings` + `user_id` |
| Mode & secret H2H | `settings` + `user_id` |
| Transaksi async | `transactions.user_id` → adapter/cookie pemilik |
| Dashboard list/detail | filter by owner |

Detail: [Isolasi per admin](architecture/per-admin.md).
