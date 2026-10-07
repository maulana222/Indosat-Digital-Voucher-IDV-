# OtoMax (HTTP GET)

Prefix:

- `/api/v2/otomax`
- Alias: `/reseller/api/v2/otomax`

Untuk reseller yang memakai software **OtoMax**: tidak perlu integrasi JSON Digiflazz-style. Arahkan **IP Center** OtoMax ke gateway ini.

Credential: `memberID`, `pin`, `password` (dari penyedia akun). IP server OtoMax harus di-whitelist.

Base: `https://idv-api.pixlycode.app`

---

### Transaksi — GET

```
GET {base}/api/v2/otomax/trx?product={product}&dest={dest}&refID={refID}&memberID={memberID}&pin={pin}&password={password}
```

Open denom (qty = **nominal**, bukan jumlah pembelian):

```
GET {base}/api/v2/otomax/trx?product={product}&dest={dest}&refID={refID}&qty=50000&memberID={memberID}&pin={pin}&password={password}
```

| Param | Wajib | Keterangan |
|-------|-------|------------|
| `product` | Ya | Kode produk (sama `buyer_sku_code` / Code) |
| `dest` | Ya | Nomor tujuan |
| `refID` | Ya | ID transaksi dari OtoMax (unik, idempotent) |
| `memberID` | Ya | Credential dari admin |
| `pin` | Ya* | Atau ganti dengan `sign` |
| `password` | Ya* | Atau ganti dengan `sign` |
| `sign` | Opsional | `base64url(SHA1("OtomaX\|memberID\|product\|dest\|refID\|pin\|password"))` |
| `qty` | Opsional | Hanya open denom: isi **nominal** (mis. 50000) |

\* Jika `sign` diisi, `pin`/`password` boleh dikosongkan.

#### Contoh balasan teks (Content-Type: text/plain)

**Pending (sedang diproses):**
```text
R#OMX001 TL5.0811222333 sedang diproses. Saldo: 90000
```

**Sukses:**
```text
R#OMX001 TL5.0811222333 SUKSES. SN: SN123ABC. Harga: 5500. Saldo: 84500
```

**Gagal:**
```text
R#OMX001 TL5.0811222333 GAGAL. Produk tidak ditemukan. Saldo: 90000
```

Setting parsing reply di OtoMax:

| Hasil | Pola |
|-------|------|
| Sukses | `R#{refID} {product}.{dest} SUKSES. SN: {sn}. Harga: {harga}. Saldo: {saldo}` |
| Diproses | `R#{refID} {product}.{dest} sedang diproses. Saldo: {saldo}` |
| Gagal | `R#{refID} {product}.{dest} GAGAL. {pesan}. Saldo: {saldo}` |

`refID` yang sama tidak diproses dua kali — kirim ulang mengembalikan status terkini.

---

### Cek saldo — GET

```
GET {base}/api/v2/otomax/balance?memberID={memberID}&pin={pin}&password={password}
```

**Sukses:**
```text
Saldo: 90000
```

**Gagal auth:**
```text
GAGAL. pin/password/sign tidak valid
```

---

### Report hasil akhir (callback GET)

Jika member `connection_type=otomax` dan transaksi punya `cb_url`, gateway mengirim **GET** (bukan POST JSON Digiflazz):

```
GET {cb_url}?rc=00&code=TL5&msg=SUKSES&msisdn=0811222333&sn=SN123ABC&request_id=OMX001&price=5500&trxid=16413&saldo=84500
```

| Query | Keterangan |
|-------|------------|
| `rc` | `00` sukses, lainnya gagal/pending |
| `code` | Kode produk |
| `msg` | `SUKSES` / `DIPROSES` / `GAGAL` |
| `msisdn` | Nomor tujuan |
| `sn` | Serial number |
| `request_id` | = `refID` |
| `price` | Harga |
| `trxid` | ID internal transaksi gateway |
| `saldo` | Saldo terakhir |

---

### IP Center (contoh)

Transaksi:
```text
https://idv-api.pixlycode.app/api/v2/otomax/trx?product=[product]&dest=[tujuan]&refID=[trxid]&memberID={memberID}&pin={pin}&password={password}
```

Saldo:
```text
https://idv-api.pixlycode.app/api/v2/otomax/balance?memberID={memberID}&pin={pin}&password={password}
```

Daftarkan IP server OtoMax di **Account Settings → IP Whitelist**.
