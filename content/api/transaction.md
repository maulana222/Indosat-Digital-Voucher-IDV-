# Transaksi

`POST /api/transaction`

Membuat transaksi topup/voucher. Proses sering **async**: response awal bisa `PENDING`, hasil akhir dikirim ke [Callback](callback.md).

## Request

```http
POST https://idv-api.pixlycode.app/api/transaction
Content-Type: application/json
```

```json
{
  "username": "client_username",
  "ref_id": "unique_ref_123",
  "sign": "md5(username + api_key + ref_id)",
  "buyer_sku_code": "freefire_60_idv",
  "customer_no": "3412409703",
  "max_price": 10000,
  "allow_dot": false,
  "cb_url": "https://client.example.com/callback"
}
```

| Field | Wajib | Keterangan |
|-------|-------|------------|
| `username` | Ya | Username API |
| `ref_id` | Ya | ID unik dari sisi Anda (idempotent) |
| `sign` | Ya | `md5(username + api_key + ref_id)` |
| `buyer_sku_code` | Ya | Kode produk / SKU |
| `customer_no` | Ya | Tujuan (nomor / game id) |
| `max_price` | Tidak* | Batas harga; gunakan harga dari price-list |
| `allow_dot` | Tidak | Default mengikuti aturan produk |
| `cb_url` | Tidak | Callback; jika kosong bisa memakai default di akun |

\* Disarankan selalu mengirim `max_price` sesuai harga aktif di price-list.

## Response

HTTP `200`. Body sama bentuknya dengan payload callback:

```json
{
  "data": {
    "ref_id": "unique_ref_123",
    "customer_no": "3412409703",
    "buyer_sku_code": "freefire_60_idv",
    "message": "Transaksi Pending",
    "status": "PENDING",
    "rc": "03",
    "sn": "",
    "price": 1000,
    "balance": 95000
  }
}
```

| Kondisi | `status` | `rc` (umum) |
|---------|----------|-------------|
| Diterima, diproses | `PENDING` | `03` |
| Sukses | `SUCCESS` | `00` |
| Gagal | `FAILED` | lihat [Status & RC](status-codes.md) |

## Contoh curl

```bash
curl -X POST "https://idv-api.pixlycode.app/api/transaction" \
  -H "Content-Type: application/json" \
  -d "{
    \"username\": \"client_username\",
    \"ref_id\": \"unique_ref_123\",
    \"sign\": \"SIGN_HEX\",
    \"buyer_sku_code\": \"freefire_60_idv\",
    \"customer_no\": \"3412409703\",
    \"max_price\": 10000,
    \"cb_url\": \"https://client.example.com/callback\"
  }"
```

## Catatan

- Ulangi `ref_id` yang sama → perilaku idempotent (status yang sudah ada dikembalikan / diproses sesuai aturan gateway).
- SN sukses ada di field `sn` (response final / callback).
