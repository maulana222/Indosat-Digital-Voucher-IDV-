## IDV API (Private)

Prefix: `/api/idv`

Endpoint utama:

- `POST /api/idv/initialize` (inisialisasi session/cookies)
- `POST /api/idv/login`
- `GET /api/idv/session`
- `POST /api/idv/logout`
- `GET /api/idv/refresh/status`
- `GET /api/idv/home`
- `GET /api/idv/sessions`
- `DELETE /api/idv/sessions/expired`

Produk:

- `GET /api/idv/products`
- `GET /api/idv/products/:productId`
- `POST /api/idv/products`
- `POST /api/idv/products/sync`
- `PUT /api/idv/products/denom/:itemCode`
- `PUT /api/idv/products/denom/:itemCode/code`
- `POST /api/idv/products/bulk-update`

