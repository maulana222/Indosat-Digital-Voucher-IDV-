## Quickstart

Baca versi UI online: [Dokumentasi IDV Gateway](https://maulana222.github.io/Indosat-Digital-Voucher-IDV-/)

### Prasyarat

- Node.js (untuk backend dan dashboard)
- MySQL (untuk database)
- Setelah clone: jalankan migrasi DB (`cd backend && npm run migrate`) termasuk **028–029**

### Menjalankan backend

1. Masuk ke folder `backend/`
2. Buat file `.env` (lihat halaman **Konfigurasi**)
3. Install dependency dan jalankan:

```bash
npm install
npm start
```

Production API: `https://idv-api.pixlycode.app`  
Lokal default port: `6969`

### Menjalankan dashboard

1. Masuk ke folder `dashboard.idv/`
2. Install dependency dan jalankan:

```bash
npm install
npm run dev
```

### Menjalankan dokumentasi (MkDocs)

File konfigurasi: `docs/mkdocs.yml`. Isi halaman Markdown ada di `docs/content/` (syarat MkDocs: `docs_dir` harus subfolder, bukan folder yang sama dengan config).

Jalankan dari **root project** (`IDV/`):

```bash
pip install -r requirements-docs.txt
mkdocs serve -f docs/mkdocs.yml
```

Atau dari folder `docs/`:

```bash
cd docs
mkdocs serve
```

Build statis (output di `site/` di root project):

```bash
mkdocs build -f docs/mkdocs.yml
```

