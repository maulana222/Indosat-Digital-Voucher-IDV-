# IDV Open API Docs (public)

Dokumentasi **Open API** untuk integrator — dipublikasikan di:

**https://maulana222.github.io/Indosat-Digital-Voucher-IDV-/**

Repo ini **sengaja tidak** berisi panduan Docker, arsitektur internal, atau setup proyek. Dokumentasi produk/internal tetap di repo privat (`IDV/docs`).

## Lokal

```bash
pip install -r requirements-docs.txt
mkdocs serve
mkdocs gh-deploy --force   # publish ke branch gh-pages
```

## Desain UI (pakai ulang di project lain)

| Item | Nama / lokasi |
|------|----------------|
| Framework tema | **[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)** (`mkdocs-material`) |
| Skin kustom | **Pixly Teal** (nama internal) — bukan palette bawaan Material |
| File yang di-copy | `mkdocs.yml` (blok `theme` + `extra_css` + `markdown_extensions`), `overrides/`, `content/stylesheets/extra.css`, `requirements-docs.txt` |

Warna utama: teal `#0f3d3e`, aksen amber `#c45c26`, font **DM Sans** + **JetBrains Mono**.
