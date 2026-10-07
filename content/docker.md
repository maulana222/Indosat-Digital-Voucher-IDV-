# Docker

Panduan menjalankan **IDV Gateway** (MySQL + Backend API + Dashboard) dengan Docker.

Dokumen ini sengaja langkah-demi-langkah agar Anda bisa menjalankan perintah sendiri sambil belajar. File konfigurasi sudah disiapkan di root project — **jangan dijalankan otomatis oleh agent**; Anda yang eksekusi.

---

## Apa yang akan berjalan?

| Service | Container | Port di host (default) | Fungsi |
|---------|-----------|------------------------|--------|
| `db` | `idv-db` | `3307` → 3306 | MySQL 8 |
| `backend` | `idv-backend` | `6969` → 6969 | API Gateway |
| `dashboard` | `idv-dashboard` | `8080` → 80 | UI (nginx + React build) |

Jaringan internal Docker memakai hostname service (`db`, `backend`). Browser di PC Anda tetap mengakses lewat `localhost`.

```
Browser ──► localhost:8080  (dashboard)
       └──► localhost:6969  (backend API)
Backend ──► db:3306         (MySQL di jaringan Docker)
```

---

## Prasyarat

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/Mac) atau Docker Engine + Compose (Linux).
2. Pastikan Docker sudah jalan: buka terminal, ketik:

```bash
docker --version
docker compose version
```

3. Buka terminal di **root project** (`IDV/`), bukan di dalam `backend/` saja.

---

## File yang dibuat untuk Docker

```
IDV/
├── docker-compose.yml          # Orkestrasi 3 service
├── .env.docker.example         # Contoh env — copy jadi .env.docker
├── docker/mysql/init/          # SQL init (opsional, hanya volume baru)
├── backend/
│   ├── Dockerfile
│   └── .dockerignore
└── dashboard.idv/
    ├── Dockerfile
    ├── nginx.conf
    └── .dockerignore
```

---

## Langkah 1 — Siapkan environment

```bash
# Windows (PowerShell / CMD)
copy .env.docker.example .env.docker

# Linux / macOS
cp .env.docker.example .env.docker
```

Edit `.env.docker`:

- Ganti `DB_PASSWORD`, `SECRET_KEY`, `JWT_SECRET`
- Sesuaikan `CORS_ORIGIN=http://localhost:8080`
- `VITE_API_BASE_URL=http://localhost:6969` (browser memanggil API di host)

> `.env.docker` jangan di-commit (berisi secret).

---

## Langkah 2 — Build & jalankan

```bash
docker compose --env-file .env.docker up -d --build
```

Arti singkat:

| Bagian | Makna |
|--------|--------|
| `up` | Buat & start container |
| `-d` | Detached (jalan di background) |
| `--build` | Build image dari Dockerfile |
| `--env-file` | Baca variabel dari `.env.docker` |

Cek status:

```bash
docker compose ps
docker compose logs -f
```

Log satu service saja:

```bash
docker compose logs -f backend
docker compose logs -f db
```

---

## Langkah 3 — Database & migration

Volume MySQL kosong hanya diisi otomatis jika Anda menaruh file `.sql` di `docker/mysql/init/` **sebelum** pertama kali `up` (volume baru).

Cara yang disarankan untuk belajar:

### A. Cek MySQL dari host

```bash
# Port host default 3307 (lihat DB_PORT_HOST)
mysql -h 127.0.0.1 -P 3307 -u idv -p idv_gateway
```

Atau masuk ke container:

```bash
docker compose exec db mysql -u idv -p idv_gateway
```

### B. Import schema / migration

Contoh jalankan migration `idv_item_code` (sesuaikan path file):

```bash
# Dari host (Windows — sesuaikan path mysql client)
mysql -h 127.0.0.1 -P 3307 -u idv -p idv_gateway < backend\database\migrations\022_add_idv_item_code_to_product_denoms.sql
```

Atau pipe dari dalam container:

```bash
docker compose exec -T db mysql -u idv -p idv_gateway < backend/database/schema.sql
```

Urutan umum:

1. Schema dasar / dump yang sudah ada
2. Migration berurutan (`004_...`, `016_...`, `022_...`, dst.)
3. Seed jika perlu

---

## Langkah 4 — Uji di browser

| URL | Harapan |
|-----|---------|
| [http://localhost:8080](http://localhost:8080) | Dashboard |
| [http://localhost:6969/health](http://localhost:6969/health) | `{"status":"ok",...}` |

Login dashboard memakai user di database (sama seperti development lokal).

---

## Perintah harian yang sering dipakai

```bash
# Stop (container tetap ada)
docker compose stop

# Start lagi
docker compose start

# Stop + hapus container (volume DB TETAP aman)
docker compose down

# Stop + hapus volume DB juga (DATA HILANG — hati-hati)
docker compose down -v

# Rebuild satu service saja
docker compose --env-file .env.docker up -d --build backend

# Masuk shell container backend
docker compose exec backend sh
```

---

## Konsep penting (belajar)

### Image vs Container

- **Image** = blueprint (hasil `Dockerfile` + `build`)
- **Container** = proses yang berjalan dari image

### Volume

Data MySQL disimpan di volume `idv_mysql_data`.  
`docker compose down` **tidak** menghapus volume.  
`docker compose down -v` **menghapus** volume → database kosong lagi.

### Jaringan & hostname

Di dalam Compose, backend memakai `DB_HOST=db` (nama service), **bukan** `localhost`.  
`localhost` di dalam container = container itu sendiri.

### Vite `VITE_*`

Variabel `VITE_API_BASE_URL` di-bake saat **build** image dashboard.  
Ubah nilai → harus **rebuild** dashboard:

```bash
docker compose --env-file .env.docker up -d --build dashboard
```

### Bind host backend

Backend sekarang mendukung `HOST`:

- Lokal tanpa Docker: default `127.0.0.1`
- Di Docker: `HOST=0.0.0.0` (wajib agar port mapping bekerja)

---

## Troubleshooting

| Gejala | Cek |
|--------|-----|
| Backend `Connection refused` ke DB | `docker compose logs db` — tunggu healthcheck MySQL sehat |
| Dashboard kosong / API gagal CORS | `CORS_ORIGIN` harus cocok dengan URL dashboard (`http://localhost:8080`) |
| Dashboard tetap hit URL API lama | Rebuild dashboard setelah ubah `VITE_API_BASE_URL` |
| Port sudah dipakai | Ubah `BACKEND_PORT_HOST` / `DASHBOARD_PORT_HOST` / `DB_PORT_HOST` di `.env.docker` |
| `npm ci` gagal saat build | Pastikan `package-lock.json` ada di `backend/` dan `dashboard.idv/` |

Lihat log error:

```bash
docker compose logs --tail=100 backend
```

---

## Apa yang tidak ikut di Compose ini?

- **`idv.server`** (mock IDV lokal) — tidak di-compose; jalankan terpisah jika perlu testing tanpa IDV production.
- **MkDocs** — dokumentasi tetap `pip` + `mkdocs serve` di host (lihat `docs/README.md`).

---

## Checklist pertama kali

1. [ ] Docker Desktop / Engine terpasang
2. [ ] Copy `.env.docker.example` → `.env.docker` dan isi secret
3. [ ] `docker compose --env-file .env.docker up -d --build`
4. [ ] Import schema / migration ke MySQL
5. [ ] Buka `http://localhost:8080` dan `http://localhost:6969/health`
6. [ ] Upload cookie IDV di Settings (sama seperti sebelumnya)
