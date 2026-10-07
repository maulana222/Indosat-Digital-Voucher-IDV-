# Status & response code (RC)

Field penting di response transaksi / callback: `status`, `rc`, `message`, `sn`.

## Status

| `status` | Arti |
|----------|------|
| `PENDING` | Diterima / masih diproses |
| `SUCCESS` | Berhasil (cek `sn`) |
| `FAILED` | Gagal |
| `CANCELLED` | Dibatalkan |

## RC yang sering muncul

| `rc` | Arti umum |
|------|-----------|
| `00` | Sukses |
| `03` | Pending / diproses |
| `02` | Gagal umum / dibatalkan |
| `16` | SKU tidak ditemukan / non-aktif |
| Lainnya | Ikuti `message` pada payload |

!!! note
    Mapping RC lengkap dapat ditambah sesuai paket error penyedia. Selalu andalkan kombinasi `status` + `message`, jangan hanya `rc`.

## Idempotensi

- `ref_id` harus unik per transaksi baru.
- Mengirim ulang `ref_id` yang sama tidak boleh membuat charge ganda; server mengembalikan / mengikuti transaksi yang sudah ada.
