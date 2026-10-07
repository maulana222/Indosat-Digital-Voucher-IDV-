# Memulai (integrator)

## Prasyarat

- Akses API dari penyedia: `username`, `api_key`
- IP server Anda didaftarkan di **whitelist** API key
- (Opsional) URL callback HTTPS yang bisa menerima `POST` JSON atau `GET` OtoMax

## Base URL

```text
https://idv-api.pixlycode.app
```

## Pilih mode integrasi

=== "API Digiflazz-style (JSON)"

    Pakai `POST /api/transaction`, `/api/price-list`, `/api/product-detail` dengan signature MD5.  
    Lihat [Autentikasi](authentication.md) dan [Transaksi](api/transaction.md).

=== "OtoMax (HTTP GET)"

    Arahkan **IP Center** OtoMax ke `https://idv-api.pixlycode.app/api/otomax/trx`. Credential: `memberID`, `pin`, `password`.  
    Lihat [OtoMax](api/otomax.md).

## Tes cepat

```bash
curl -X POST "https://idv-api.pixlycode.app/api/price-list" \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"YOUR_USER\",\"sign\":\"YOUR_SIGN\"}"
```

`sign` = `md5(username + api_key + "pricelist")` (hex).

Jika IP belum di-whitelist, request akan ditolak.
