# Status & response code (RC)

Berlaku untuk **API JSON** (`POST /api/transaction` + callback Digiflazz) dan mapping yang sama dipakai teks OtoMax `GAGAL. {pesan}`.

## Status (JSON)

| `status` | Arti |
|----------|------|
| `PENDING` | Diterima / masih diproses |
| `SUCCESS` | Berhasil (cek `sn`) |
| `FAILED` | Gagal |
| `CANCELLED` | Dibatalkan |

## RC & pesan lengkap

| `rc` | `message` |
|------|-----------|
| `00` | (sukses — bukan error) |
| `03` | Transaksi Pending / sedang diproses |
| `54` | Nomor Tujuan Salah |
| `16` | SKU tidak ditemukan atau Non-Aktif |
| `43` | SKU tidak ditemukan atau Non-Aktif |
| `10` | Produk sedang Gangguan (Non Aktif) |
| `17` | Saldo tidak cukup |
| `55` | Game ID tidak ditemukan |
| `02` | ID Game tidak valid |
| `02` | ID tidak ditemukan dalam sistem |
| `02` | Signature tidak valid |
| `02` | User tidak valid atau tidak aktif |
| `02` | Denom tidak valid |
| `02` | Reference ID sudah digunakan |
| `02` | OTP Error |
| `02` | Transaksi Gagal |
| `32` | Transaksi Dibatalkan |
| `18` | IP Anda tidak kami kenali |

!!! tip "OtoMax"
    Lihat contoh teks lengkap di halaman [OtoMax](otomax.md) — pola `R#… GAGAL. {pesan}. Saldo: …`.

## Idempotensi

- `ref_id` / `refID` harus unik per transaksi baru.
- Kirim ulang ID yang sama mengikuti transaksi yang sudah ada (tidak charge ganda).
