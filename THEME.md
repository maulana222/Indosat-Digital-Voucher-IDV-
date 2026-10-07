# Theme pack: Pixly Teal (Material for MkDocs)

## Nama resmi stack

- **Material for MkDocs** — tema open-source (`pip install mkdocs-material`)
- Skin project ini: **Pixly Teal** (custom CSS, bukan nama palette Material seperti `indigo` / `teal` bawaan)

## Cara pakai di project lain

1. Install: `mkdocs-material` (+ `mkdocs<2`, `pymdown-extensions`)
2. Salin:
   - `content/stylesheets/extra.css`
   - `overrides/main.html` (opsional — announce bar)
   - bagian `theme:`, `extra_css:`, `markdown_extensions:`, `plugins:` dari `mkdocs.yml`
3. Set `docs_dir` / `nav` sesuai konten project baru
4. Ganti `site_name`, `site_url`, warna di `extra.css` jika perlu brand berbeda

Tidak perlu meng-copy isi halaman API — hanya skin-nya.
