## Transaction API

Prefix: `/api/transaction`

### Membuat transaksi (public)

`POST /api/transaction`

Body minimal:

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

Catatan signature:

- Payload: `username + api_key + ref_id`
- Hash: MD5 (hex)

### Ambil list transaksi (private)

`GET /api/transaction?limit=10&offset=0`

```bash
curl "http://localhost:6969/api/transaction?limit=10&offset=0" \
  -H "Authorization: Bearer <jwt>"
```

### Statistik (private)

`GET /api/transaction/statistics`

### Logs (private)

- `GET /api/transaction/:id/logs`
- `GET /api/transaction/ref/:refId/logs`
- `GET /api/transaction/detail/:detailId/logs`

### Cancel / Retry (private)

- `PUT /api/transaction/:id/cancel`
- `POST /api/transaction/batch-cancel`
- `POST /api/transaction/:id/retry`

