# MkDocs Documentation

Dokumentasi project ini dibuat dengan **MkDocs**.

## Struktur penting

- `docs/mkdocs.yml`: konfigurasi MkDocs
- `docs/content/`: semua halaman Markdown (`*.md`) yang ditampilkan di website dokumentasi
- `site/`: output hasil build (dibuat oleh MkDocs)

## Cara menjalankan

### Dari folder root project (`IDV/`)

```bash
pip install -r requirements-docs.txt
mkdocs serve -f docs/mkdocs.yml
```

### Dari folder `docs/`

```bash
cd docs
mkdocs serve
```

## Build statis

```bash
mkdocs build -f docs/mkdocs.yml
```

