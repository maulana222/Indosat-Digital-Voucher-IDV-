# OtoMax (HTTP GET)

Integrasi untuk software **OtoMax**: request **GET**, balasan **teks polos** (`text/plain`), bukan JSON Digiflazz.

Base URL:

```text
https://idv-api.pixlycode.app
```

## Prefix & alias

| Path | Keterangan |
|------|------------|
| `/api/v2/otomax` | Path utama |
| `/reseller/api/v2/otomax` | **Alias** — URL lain ke **handler yang sama** |

**Maksud alias:** beberapa template OtoMax / IP Center memakai prefix lama bergaya `/reseller/api/...`. Agar tidak perlu ubah software, gateway mendaftarkan kedua path ke endpoint yang sama.

Contoh ekuivalen:

```text
https://idv-api.pixlycode.app/api/v2/otomax/trx?...
https://idv-api.pixlycode.app/reseller/api/v2/otomax/trx?...
```

Cukup isi **satu** di IP Center (disarankan path utama `/api/v2/otomax/...`).

Credential: `memberID`, `pin`, `password` (dari penyedia akun). IP server OtoMax harus di-whitelist.

---

## Transaksi — GET

```
GET {base}/api/v2/otomax/trx?product={product}&dest={dest}&refID={refID}&memberID={memberID}&pin={pin}&password={password}
```

Open denom (`qty` = **nominal**, bukan jumlah pembelian):

```
GET {base}/api/v2/otomax/trx?product={product}&dest={dest}&refID={refID}&qty=50000&memberID={memberID}&pin={pin}&password={password}
```

| Param | Wajib | Keterangan |
|-------|-------|------------|
| `product` | Ya | Kode produk (= `buyer_sku_code`) |
| `dest` | Ya | Nomor / ID tujuan |
| `refID` | Ya | ID unik dari OtoMax (idempotent) |
| `memberID` | Ya | Credential |
| `pin` | Ya* | Atau pakai `sign` |
| `password` | Ya* | Atau pakai `sign` |
| `sign` | Opsional | `base64url(SHA1("OtomaX\|memberID\|product\|dest\|refID\|pin\|password"))` |
| `qty` | Opsional | Open denom: nominal (mis. `50000`) |

\* Jika `sign` diisi, `pin`/`password` boleh dikosongkan.

---

## Pola parsing reply (trx)

Setting di OtoMax:

| Hasil | Pola |
|-------|------|
| Sukses | `R#{refID} {product}.{dest} SUKSES. SN: {sn}. Harga: {harga}. Saldo: {saldo}` |
| Diproses | `R#{refID} {product}.{dest} sedang diproses. Saldo: {saldo}` |
| Gagal | `R#{refID} {product}.{dest} GAGAL. {pesan}. Saldo: {saldo}` |

### Contoh — sukses / pending

```text
R#OMX001 TL5.0811222333 SUKSES. SN: SN123ABC. Harga: 5500. Saldo: 84500
R#OMX001 TL5.0811222333 sedang diproses. Saldo: 90000
```

### Contoh — semua pesan gagal (`{pesan}`)

**Validasi / auth OtoMax**

```text
R#OMX001 TL5.0811222333 GAGAL. product, dest, dan refID wajib. Saldo: 0
R#OMX001 TL5.0811222333 GAGAL. memberID wajib. Saldo: 0
R#OMX001 TL5.0811222333 GAGAL. memberID tidak valid atau nonaktif. Saldo: 0
R#OMX001 TL5.0811222333 GAGAL. Server type member bukan otomax. Saldo: 0
R#OMX001 TL5.0811222333 GAGAL. IP tidak diizinkan. Saldo: 0
R#OMX001 TL5.0811222333 GAGAL. pin/password/sign tidak valid. Saldo: 0
```

**Bisnis / katalog / saldo / tujuan** (dari mapping gateway)

```text
R#OMX001 TL5.0811222333 GAGAL. Nomor Tujuan Salah. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. SKU tidak ditemukan atau Non-Aktif. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Produk sedang Gangguan (Non Aktif). Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Saldo tidak cukup. Saldo: 1000
R#OMX001 FF.12345 GAGAL. Game ID tidak ditemukan. Saldo: 90000
R#OMX001 FF.12345 GAGAL. ID Game tidak valid. Saldo: 90000
R#OMX001 FF.12345 GAGAL. ID tidak ditemukan dalam sistem. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Signature tidak valid. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. User tidak valid atau tidak aktif. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Denom tidak valid. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Reference ID sudah digunakan. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Transaksi Dibatalkan. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. OTP Error. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. IP Anda tidak kami kenali. Saldo: 90000
R#OMX001 TL5.0811222333 GAGAL. Transaksi Gagal. Saldo: 90000
```

### Tabel RC ↔ pesan (callback / mapping internal)

| RC | Pesan (`{pesan}`) |
|----|-------------------|
| `54` | Nomor Tujuan Salah |
| `16` / `43` | SKU tidak ditemukan atau Non-Aktif |
| `10` | Produk sedang Gangguan (Non Aktif) |
| `17` | Saldo tidak cukup |
| `55` | Game ID tidak ditemukan |
| `02` | ID Game tidak valid / ID tidak ditemukan / Signature tidak valid / User tidak valid / Denom tidak valid / Reference ID sudah digunakan / OTP Error / Transaksi Gagal |
| `32` | Transaksi Dibatalkan |
| `18` | IP Anda tidak kami kenali |
| `00` | Sukses (bukan GAGAL) |
| `03` | Pending → teks *sedang diproses* |

`refID` yang sama tidak diproses dua kali — kirim ulang mengembalikan status terkini.

---

## Cek saldo — GET

```
GET {base}/api/v2/otomax/balance?memberID={memberID}&pin={pin}&password={password}
```

**Sukses**

```text
Saldo: 90000
```

**Gagal**

```text
GAGAL. memberID wajib
GAGAL. memberID tidak valid atau nonaktif
GAGAL. Server type member bukan otomax
GAGAL. IP tidak diizinkan
GAGAL. pin/password/sign tidak valid
GAGAL. Gagal cek saldo
```

---

## Report hasil akhir (callback GET)

Jika transaksi punya callback URL dan akun mode OtoMax, gateway mengirim **GET** (bukan POST JSON):

```
GET {cb_url}?rc=00&code=TL5&msg=SUKSES&msisdn=0811222333&sn=SN123ABC&request_id=OMX001&price=5500&trxid=16413&saldo=84500
```

| Query | Keterangan |
|-------|------------|
| `rc` | `00` sukses; lainnya lihat tabel RC |
| `code` | Kode produk |
| `msg` | `SUKSES` / `DIPROSES` / `GAGAL` |
| `msisdn` | Nomor tujuan (`dest`) |
| `sn` | Serial number |
| `request_id` | = `refID` |
| `price` | Harga |
| `trxid` | ID internal gateway |
| `saldo` | Saldo terakhir |

---

## IP Center (contoh)

Transaksi:

```text
https://idv-api.pixlycode.app/api/v2/otomax/trx?product=[product]&dest=[tujuan]&refID=[trxid]&memberID={memberID}&pin={pin}&password={password}
```

Saldo:

```text
https://idv-api.pixlycode.app/api/v2/otomax/balance?memberID={memberID}&pin={pin}&password={password}
```

(Opsional alias reseller — sama fungsinya:)

```text
https://idv-api.pixlycode.app/reseller/api/v2/otomax/trx?...
```

Daftarkan IP server OtoMax ke **IP whitelist** akun API Anda (minta ke penyedia gateway).
