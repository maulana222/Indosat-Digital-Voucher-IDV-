# Callback

Gateway memanggil URL Anda ketika status transaksi berubah (terutama ke `SUCCESS` / `FAILED`).

## URL callback

1. Field `cb_url` pada request transaksi, atau  
2. Default callback yang dikonfigurasi di akun Anda (jika `cb_url` kosong).

## Mode API (Digiflazz-style) — POST JSON

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

Contoh gagal: `status` = `FAILED`, `sn` biasanya kosong.

Nilai `status`: `PENDING` | `SUCCESS` | `FAILED` | `CANCELLED`.

!!! tip "Praktik terbaik"
    Endpoint callback harus idempotent (bisa menerima ulang notifikasi yang sama) dan merespons HTTP 2xx cepat.

## Mode OtoMax — GET querystring

Jika akun memakai tipe OtoMax, callback dikirim sebagai **GET** dengan querystring. Contoh:

```text
GET {callback_url}?rc=00&code=TL5&msg=SUKSES&msisdn=0811222333&sn=SN123&request_id=OMX001&price=5500&trxid=16413&saldo=84500
```

Detail: [OtoMax](otomax.md).
