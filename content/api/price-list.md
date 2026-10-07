## Price List API (Open API)

Prefix: `/api/price-list`

Endpoint publik untuk client mengambil daftar produk jualan (SKU) dari gateway.

### Ambil daftar produk

`POST /api/price-list`

Body:

```json
{
  "username": "client_username",
  "sign": "md5(username + api_key + \"pricelist\")",
  "code": "opsional_filter_sku"
}
```

| Parameter | Deskripsi | Tipe | Wajib |
|-----------|-----------|------|-------|
| `username` | Username dari menu atur koneksi API | String | Ya |
| `sign` | Signature `md5(username + apiKey + "pricelist")` | String | Ya |
| `code` | Filter satu SKU (`products.code`) | String | Tidak |

Catatan signature:

- Payload: `username + api_key + pricelist` (literal string `pricelist`, tanpa spasi)
- Hash: MD5 (hex, case-insensitive)
- IP client harus ada di whitelist API key (sama seperti transaksi)

### Response sukses

HTTP `200`:

```json
{
  "data": [
    {
      "code": "freefire_60_idv",
      "name": "Free Fire 60 Diamond",
      "desc": "",
      "price": 10000,
      "status": true,
      "brand": "Free Fire",
      "category": "free_fire",
      "type": "topup"
    }
  ]
}
```

Mapping field (dari database):

| Field response | Sumber |
|----------------|--------|
| `code` | `products.code` |
| `name` | `products.name` |
| `desc` | `products.description` |
| `price` | `products.price` |
| `status` | `products.is_active` |
| `brand` | `categories.product_name` |
| `category` | `categories.product_id` |
| `type` | `categories.product_type` (`topup` / `voucher`) |

### Response gagal auth / sign / IP

HTTP `200` (format mirip error Digiflazz):

```json
{
  "data": {
    "rc": "02",
    "message": "Signature tidak valid"
  }
}
```

### Response validasi body

HTTP `400`:

```json
{
  "status": "error",
  "message": "Validation failed",
  "errors": ["username is required and must be a non-empty string"]
}
```

### Contoh curl

```bash
curl -X POST https://idv-api.pixlycode.app/api/price-list \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"client_username\",\"sign\":\"<md5>\"}"
```
