## Konfigurasi

### Environment backend (`backend/.env`)

Copy contoh berikut ke `backend/.env` lalu sesuaikan nilainya:

```bash
# Server Configuration
PORT=6969
NODE_ENV=development
SECRET_KEY=your-super-secret-key-change-this-in-production

# JWT Configuration
JWT_SECRET=your-jwt-secret-key-change-this-in-production
JWT_EXPIRES_IN=7d

# Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_NAME=idv_gateway
DB_CONNECTION_LIMIT=10
DB_CHARSET=utf8mb4

# CORS Configuration
CORS_ORIGIN=*

# IDV Service Configuration
IDV_BASE_URL=https://idv.co.id
IDV_EMAIL=your-idv-email@example.com
IDV_PASSWORD=your-idv-password
IDV_TRANSACTION_ENDPOINT=/api/transaction
IDV_STATUS_ENDPOINT=/api/transaction
IDV_OTP_SECRET=OZDPYCSTM55UAJ22

# Auto Refresh Configuration
IDV_AUTO_REFRESH_ENABLED=true
IDV_REFRESH_INTERVAL=120
IDV_REFRESH_ENDPOINT=/api/auth/session
IDV_REFRESH_ROTATION_ENABLED=true
IDV_REFRESH_ROTATION_INTERVAL=120
IDV_SESSION_REFRESH_ENABLED=true
IDV_SESSION_REFRESH_INTERVAL=60
IDV_SESSION_REFRESH_ENDPOINT=/api/auth/session
IDV_AUTO_LOGIN_ENABLED=true

# Home Request Configuration (dengan Push Notifications)
IDV_HOME_REQUEST_ENABLED=true
IDV_HOME_REQUEST_INTERVAL=120
IDV_HOME_REQUEST_ENDPOINT=/api/getHome
IDV_HOME_PUSH_NOTIFICATION_ENABLED=true

# Web Push Notification Configuration
WEB_PUSH_ENABLED=true
VAPID_PUBLIC_KEY=your-vapid-public-key-here
VAPID_PRIVATE_KEY=your-vapid-private-key-here
VAPID_EMAIL=noreply@idv-gateway.com

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# Logging
LOG_LEVEL=info
LOG_FILE=logs/app.log
```

### Catatan

- Jangan commit file `.env`.
- Untuk production, gunakan nilai secret yang kuat dan batasi `CORS_ORIGIN`.

