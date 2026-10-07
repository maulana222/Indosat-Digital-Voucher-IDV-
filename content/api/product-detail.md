## Product Detail API (Open API)

Prefix: `/api/product-detail`

Endpoint publik untuk client mengambil **detail 1 SKU** (termasuk daftar denom penyusun).

### Ambil detail produk

`POST /api/product-detail`

Body:

```json
{
  "username": "client_username",
  "sign": "md5(username + api_key + \"productdetail\")",
  "code": "freefire_60_idv"
}
```

| Parameter | Deskripsi | Tipe | Wajib |
|-----------|-----------|------|-------|
| `username` | Username dari menu atur koneksi API | String | Ya |
| `sign` | Signature `md5(username + apiKey + "productdetail")` | String | Ya |
| `code` | Kode SKU (`products.code`) | String | Ya |

Catatan signature:

- Payload: `username + api_key + productdetail` (literal, tanpa spasi)
- Hash: MD5 (hex, case-insensitive)
- IP client harus ada di whitelist API key

### Response sukses

HTTP `200`:

```json
{
  "data": {
    "code": "freefire_60_idv",
    "name": "Free Fire 60 Diamond",
    "desc": "",
    "price": 10000,
    "status": true,
    "brand": "Free Fire",
    "category": "free_fire",
    "type": "topup",
    "denoms": [
      {
        "name": "Free Fire 60",
        "code": "ff_60",
        "qty": 1,
        "amount": 10000,
        "status": true
      }
    ]
  }
}
```

| Field | Sumber |
|-------|--------|
| `code` / `name` / `desc` / `price` / `status` | `products` |
| `brand` | `categories.product_name` |
| `category` | `categories.product_id` |
| `type` | `categories.product_type` |
| `denoms[].name` | `product_denoms.name` |
| `denoms[].code` | `product_denoms.item_code` |
| `denoms[].qty` | `product_details.quantity` |
| `denoms[].amount` | `product_details.unit_price` |
| `denoms[].status` | `product_details.is_active` |

Catatan: `cost_price` / data internal supplier **tidak** di-expose ke client.

### Response gagal (SKU tidak ada / auth)

HTTP `200`:

```json
{
  "data": {
    "rc": "16",
    "message": "SKU tidak ditemukan atau Non-Aktif"
  }
}
```

### Response validasi body

HTTP `400` — format sama seperti price-list.

### Contoh curl

```bash
curl -X POST https://idv-api.pixlycode.app/api/product-detail \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"client_username\",\"sign\":\"<md5>\",\"code\":\"freefire_60_idv\"}"
```
