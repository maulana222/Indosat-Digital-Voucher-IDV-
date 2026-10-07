# IDV Gateway

Server API yang menjembatani sistem Anda dengan **Indosat Digital Voucher (IDV)**, plus dashboard multi-admin untuk monitoring dan konfigurasi.

[Quickstart](getting-started.md){ .md-button .md-button--primary }
[Dokumentasi online](https://maulana222.github.io/Indosat-Digital-Voucher-IDV-/){ .md-button }

---

### Backend API

Transaksi Digiflazz-style, OtoMax GET, callback, katalog produk, session IDV (cookie / H2H).

### Dashboard

Login JWT, transaksi, statistik, Account Settings, Koneksi Cookie/H2H per admin.

### Isolasi per admin

Harga, cookie, kredensial H2H, dan upstream transaksi terikat `user_id` pemilik — tidak saling pakai. Baca [Isolasi per admin](architecture/per-admin.md).

---

## Konsep penting

- **Public transaksi**: `POST /api/transaction` (signature MD5) atau OtoMax `GET /api/v2/otomax/*`
- **Dashboard / private**: JWT `Authorization: Bearer ...`
- **Upstream ke IDV**: tiap admin memilih **Cookie** atau **H2H Nuon** di halaman Koneksi
- **Tipe client**: `api` (POST Digiflazz) atau `otomax` (GET) di Account Settings

## Baca selanjutnya

| Halaman | Isi |
|---------|-----|
| [Fitur](features.md) | Ringkasan kemampuan |
| [Isolasi per admin](architecture/per-admin.md) | Keamanan multi-tenant |
| [Upstream H2H](architecture/upstream-h2h.md) | Adapter cookie vs Nuon |
| [OtoMax](api/otomax.md) | Integrasi HTTP GET |
| [Security](security.md) | Checklist production |

Dokumentasi UI (GitHub Pages): [maulana222.github.io/Indosat-Digital-Voucher-IDV-](https://maulana222.github.io/Indosat-Digital-Voucher-IDV-/)
