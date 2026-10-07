# IDV Open API

Dokumentasi **integrasi client** ke gateway transaksi. Cocok untuk sistem Digiflazz-style, OtoMax, atau middleware Anda sendiri.

Base URL production:

```text
https://idv-api.pixlycode.app
```

[Memulai](getting-started.md){ .md-button .md-button--primary }
[Transaksi](api/transaction.md){ .md-button }

---

### Endpoint publik

| API | Method | Path |
|-----|--------|------|
| Transaksi | `POST` | `/api/transaction` |
| Price list | `POST` | `/api/price-list` |
| Product detail | `POST` | `/api/product-detail` |
| OtoMax trx | `GET` | `/api/v2/otomax/trx` |
| OtoMax saldo | `GET` | `/api/v2/otomax/balance` |
| Callback | — | URL Anda (kami yang memanggil) |

### Yang tidak dibahas di sini

Dokumen ini **bukan** panduan instalasi server, Docker, dashboard admin, atau arsitektur internal. Hal tersebut bersifat privat untuk pemilik produk.

---

## Alur singkat

1. Dapatkan `username` + `api_key` (+ whitelist IP) dari penyedia gateway.
2. Panggil Price List / Product Detail untuk SKU & harga.
3. Kirim Transaksi dengan `sign` MD5.
4. Terima response sync (`PENDING` / `FAILED` / …) dan **callback** ke `cb_url` saat status final.
