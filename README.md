# MkDocs Documentation

Dokumentasi project ini dibuat dengan **MkDocs Material**.

## Lihat di UI (online)

**GitHub Pages:** [https://maulana222.github.io/Indosat-Digital-Voucher-IDV-/](https://maulana222.github.io/Indosat-Digital-Voucher-IDV-/)

## Struktur penting

- `docs/mkdocs.yml` — konfigurasi MkDocs + tema
- `docs/content/` — halaman Markdown
- `docs/overrides/` — template (announce bar)
- `docs/content/stylesheets/extra.css` — desain kustom
- `site/` — output build (di root project)

## Cara menjalankan lokal

Dari root project (`IDV/`):

```bash
pip install -r requirements-docs.txt
mkdocs serve -f docs/mkdocs.yml
```

Buka biasanya `http://127.0.0.1:8000`.

## Build statis

```bash
mkdocs build -f docs/mkdocs.yml
```

Deploy ke GitHub Pages: publish isi folder `site/` ke branch/pages yang dipakai repo [Indosat-Digital-Voucher-IDV-](https://github.com/maulana222/Indosat-Digital-Voucher-IDV-).
