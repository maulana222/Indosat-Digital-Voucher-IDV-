## Security

Ringkasan hal yang perlu diperhatikan:

- **Jangan commit `.env`** dan jangan log data sensitif (cookie, password, token).
- **Signature transaksi masih MD5** (untuk kompatibilitas). Untuk production disarankan migrasi ke **HMAC-SHA256**.
- Gunakan `CORS_ORIGIN` yang spesifik untuk production.
- Pastikan endpoint private selalu memakai JWT (`Authorization: Bearer ...`).

