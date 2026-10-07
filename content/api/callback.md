## Callback

Setelah status transaksi berubah, gateway mengirim notifikasi ke URL callback client.

### Sumber URL

1. Field `cb_url` pada request transaksi (prioritas), atau
2. `users.default_cb_url` dari **Account Settings** jika `cb_url` kosong.

### Format menurut tipe koneksi

| `connection_type` | Method | Format |
|-------------------|--------|--------|
| `api` (default) | `POST` | JSON Digiflazz `{ "data": { ... } }` |
| `otomax` | `GET` | Querystring OtoMax (`rc`, `code`, `msg`, `msisdn`, `sn`, …) |

Pemilik ditentukan dari `transactions.user_id` / `username`.

---

### Digiflazz — POST JSON

- Method: `POST`
- Content-Type: `application/json`

```json
{
  "data": {
    "ref_id": "unique_ref_123",
    "customer_no": "3412409703",
    "buyer_sku_code": "freefire_60_idv",
    "message": "Transaksi Berhasil",
    "status": "SUCCESS",
    "rc": "00",
    "sn": "...",
    "price": 1000,
    "balance": 95000
  }
}
```

Contoh gagal: `status` = `FAILED`, `rc` sesuai error, `sn` kosong.

Field `status`: `PENDING` | `SUCCESS` | `FAILED` | `CANCELLED`.

---

### OtoMax — GET querystring

Contoh:

```text
GET {callback_url}?rc=00&code=TL5&msg=SUKSES&msisdn=0811222333&sn=SN123&request_id=OMX001&price=5500&trxid=16413&saldo=84500
```

Detail field: [OtoMax](otomax.md).
