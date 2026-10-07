## Auth (Dashboard)

Semua endpoint auth berada di prefix ` /api/auth `.

### Login

`POST /api/auth/login`

Contoh:

```bash
curl -X POST "https://idv-api.pixlycode.app/api/auth/login" \
  -H "Content-Type: application/json" \
  -d "{\"email\":\"you@example.com\",\"password\":\"your-password\"}"
```

Response umumnya mengembalikan JWT. JWT digunakan untuk endpoint private lain.

### Profile

`GET /api/auth/profile` (private)

```bash
curl "https://idv-api.pixlycode.app/api/auth/profile" \
  -H "Authorization: Bearer <jwt>"
```

### 2FA (TOTP)

- `GET /api/auth/2fa/status` (private)
- `GET /api/auth/2fa/setup` (private)
- `POST /api/auth/2fa/enable` (private)

