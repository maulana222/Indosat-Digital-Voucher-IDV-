# Autentikasi Open API

Endpoint publik **tidak** memakai JWT dashboard. Autentikasi = `username` + `api_key` lewat **signature**, plus **IP whitelist**.

## Signature MD5

Semua hash: **MD5 hex** (perbandingan case-insensitive di server).

| Endpoint | Rumus `sign` |
|----------|----------------|
| Transaksi `POST /api/transaction` | `md5(username + api_key + ref_id)` |
| Price list `POST /api/price-list` | `md5(username + api_key + "pricelist")` |
| Product detail `POST /api/product-detail` | `md5(username + api_key + "productdetail")` |

String literal `pricelist` / `productdetail` tanpa spasi tambahan.

### Contoh (Node.js)

```js
import crypto from "crypto";

const username = "client_username";
const apiKey = "your-api-key";
const refId = "unique_ref_123";

const sign = crypto
  .createHash("md5")
  .update(username + apiKey + refId)
  .digest("hex");
```

### Contoh (PHP)

```php
$sign = md5($username . $apiKey . $refId);
```

## IP whitelist

Request harus berasal dari IP yang sudah didaftarkan untuk API key Anda. IP publik server outbound (NAT) yang dipakai — bukan IP lokal.

## Keamanan credential

- Jangan commit `api_key` ke repo publik.
- Jangan kirim `api_key` di body request (hanya dipakai untuk menghitung `sign`).
- Putar key jika bocor; hubungi penyedia gateway.
