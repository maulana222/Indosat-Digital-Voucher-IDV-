## Security

### Checklist production

- Jangan commit `.env`; jangan log cookie, password, atau secret H2H.
- Batasi `CORS_ORIGIN` (jangan `*` di production jika memungkinkan).
- Endpoint private selalu memakai JWT.
- Signature transaksi publik masih **MD5** (kompatibilitas Digiflazz); pertimbangkan migrasi HMAC-SHA256 ke depan.
- Jalankan migrasi termasuk **029** (`uk_user_key`) agar settings per-admin aman.
- Tiap admin wajib punya koneksi IDV sendiri — sistem **tidak** meminjam cookie/H2H admin lain.

### Isolasi data

- Katalog & harga: `owner_user_id`
- Cookie / H2H: `settings.user_id`
- Transaksi: `transactions.user_id` → upstream adapter milik owner
- Dashboard: admin ter-scope ke data sendiri

### Rate limiting

| Limiter | Contoh endpoint | Batas (default) |
|---------|-----------------|-----------------|
| Auth | login | Ketat per IP |
| Sensitive ops | `/api/account/connection`, `/callback`, regenerate key | ~10 / 5 menit / IP |
| API umum | banyak route | Window terpisah |

Jika dashboard menampilkan *Too many sensitive operations*, tunggu jendela rate-limit lalu simpan lagi — ini proteksi abuse, bukan kegagalan isolasi.
