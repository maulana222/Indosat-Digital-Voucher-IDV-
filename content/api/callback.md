## Callback

Server akan melakukan callback `POST` ke `cb_url` client (jika dikirim pada request transaksi).

### Spesifikasi

- Method: `POST`
- Content-Type: `application/json`

### Contoh callback transaksi gagal

```json
{
  "data": {
    "rc": "02",
    "message": "Transaksi Gagal",
    "ref_id": "unique_ref_123",
    "customer_no": "3412409703",
    "customer_name": "3412409703",
    "buyer_sku_code": "freefire_60_idv",
    "status": "FAILED",
    "sn": "",
    "price": 0,
    "buyer_last_saldo": 0,
    "raw_msg": "Transaksi Gagal",
    "error_reason": "system_error"
  }
}
```

Detail variasi error reason bisa ditambahkan sesuai kebutuhan client.

