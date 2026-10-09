# Peta folder `C:\scalping`

**Diperbarui 13 Agustus 2026.** Ini satu-satunya file yang perlu dibuka pertama.

> **Kalau kamu sesi Claude yang baru:** baca
> `BACA_DULU_SEBELUM_NGAPA-NGAPAIN.md` lalu
> `04_Documentation\PELAJARAN_DAN_KESALAHAN_NEWS_M1.md` sampai habis
> sebelum menyentuh file apa pun. Dua file itu isinya berbulan-bulan kena batunya.

---

## Kenapa cuma tiga file di akar

| file | kenapa tidak dipindah |
|---|---|
| `00_INDEX.md` | ini petanya — tugasnya memang ditemukan pertama |
| `AGENTS.md` | konvensi alat: dibaca otomatis dari akar repo |
| `BACA_DULU_SEBELUM_NGAPA-NGAPAIN.md` | pintu masuk sesi baru, dirujuk namanya di banyak tempat |

Selebihnya sudah masuk folder. Nol file yatim.

---

## SATU folder untuk semua yang live — `LIVE\`

**Dirapikan 13 Agustus 2026.** Kalau mau pasang ke akun nyata, **cuma buka
`LIVE\`**. Mulai dari `LIVE\BACA_DULU.md`.

```
LIVE\NEWS\   bot news  - Straddle_News_M1 v2.42 + setfile + laporan
LIVE\TRIO\   bot lama  - Straddle_Trio_LIVE_OLD v3.24 + setfile + laporan
```

| | bot | status |
|---|---|---|
| lama | `Straddle_Trio_LIVE_OLD` v3.24 | **SEDANG JALAN di duit asli** · sapuan terakhir 31 Jul 2026 |
| baru | `Straddle_News_M1` **v2.42** | PPI sudah divalidasi MT5, siap pasang |

**Enam folder lama sudah DIHAPUS** sesudah isinya dipindah dan diperiksa satu
per satu (nol file hilang): `LIVE_NEWS\`, `16_NEWS_M1\`, `14_News_Jadwal\`,
`17_TRIO_LIVE\`, `09_LIVE\`, `15_LIVE_OLD\`. Semua ada di `LIVE\` sekarang.

**EA news ada di DUA tempat, isinya identik:** `01_EAs\Straddle_News_M1.mq5`
(tempat kerja) dan `LIVE\NEWS\` (paket pasang). Ubah satu, salin ke satunya.

---

## Setelan yang berlaku — bot news

| event | file | setelan |
|---|---|---|
| **PPI** | `LIVE\NEWS\setfile\PPI_FLORA.set` | H240 / B0 / SL150 / TP350 / trail 200 / ronde 3 |
| **NFP + CPI** | `LIVE\NEWS\setfile\NFP_CPI_NEMESIS.set` | H240 / B0 / SL120 / TP6000 / trail MATI / ronde 1 |

Semua sudah divalidasi MT5. Angkanya ada di `LIVE\NEWS\laporan\`.

**Jadwal news hardcode dan ada ujungnya:** NFP habis **4 Des 2026**,
CPI **10 Des 2026**, PPI **15 Des 2026**. Lewat itu EA diam total
(fail-closed), bukan rusak.

---

## Isi tiap folder

| folder | isi |
|---|---|
| `01_EAs\` | semua source `.mq5`. **EA news ada di sini**, bukan di akar |
| `02_Tools\` | alat bantu. **`cek_ps1.py` dan `cek_mq5.py` WAJIB dijalankan sebelum menyerahkan file** |
| `03_Reports_HTML\` | laporan HTML umum · `arsip_2026\` isi laporan lama |
| `04_Documentation\` | semua dokumentasi & pelajaran · `arsip_riset_2026\` isi catatan riset lama |
| `05_Backtest_Sims\` | simulator Python |
| `06_Sweep_Results\` | hasil sapuan CSV |
| `07_MT5_Reports_Excel\` | export XML/Excel dari MT5 |
| `08_MCP_MT5\` | jembatan MCP ke MT5 |
| `10_Hasil_Vonis\` | vonis riset per ronde |
| `11_Rencana_Usulan\` | rencana yang belum dieksekusi |
| `12_Forensik_Audit\` | audit & forensik lama |
| `13_Setfile_Arsip\` | `.set` arsip, termasuk `LIVE_SAKLAR_TRIO.set` |
| `AUTOTEST\` | seluruh script otomasi tester `.ps1` + `.bat` |
| `LIVE\` | **SATU-SATUNYA folder buat pasang live** — NEWS + TRIO |
| `DATA\`, `DATA TICK\`, `LOG\` | data mentah dan log |
| `Client\`, `GPT\`, `Gemini\`, `MCP\`, `SS\`, `OPTIMIZATION\`, `FORENSIK\` | folder lama, jarang disentuh |

---

## Dokumen yang harus dibaca, urut kepentingan

1. `BACA_DULU_SEBELUM_NGAPA-NGAPAIN.md` — konteks orangnya, aturan bahasa, batas kejujuran
2. `04_Documentation\PELAJARAN_DAN_KESALAHAN_NEWS_M1.md` — **13 kesalahan + 13 palang wajib**
3. `04_Documentation\MASTER_STRADDLE.md` — konsep strategi straddle
4. `04_Documentation\LESSONS_SIM_DEV.md` — pelajaran bikin simulator
5. `04_Documentation\MQL5_CODING_NOTES.md` — jebakan MQL5

---

## Laporan bot news — `LIVE\NEWS\laporan\`

| file | isi |
|---|---|
| `RAPOR_PPI_10_DEWA_MT5.html` | **PPI** — 10 dewa, semua angka MT5 |
| `VONIS_FINAL_NEMESIS_NFP_CPI.html` | **NFP + CPI** — kesimpulan akhir |
| `RISET_CPI_12_DEWA.html` | 12 dewa di CPI, diurut rasio |
| `RAPOR_LENGKAP_11_DEWA.html` | 11 dewa di NFP |
| `FORENSIK_CPI_12MEI2026.html` | kenapa satu hari rugi di 10 dari 11 dewa |
| `FORENSIK_ADP_EHS.html` | kenapa ADP dan EHS ditolak |
| `FORENSIK_NFP_07AGT2026.html` | insiden broker Vantage Live 7 saat NFP live |

Laporan riset lama ada di `LIVE\NEWS\arsip_riset\laporan\` (13 file).

---

## Aturan yang tidak boleh dilanggar

1. **Nol angka simulator di laporan.** Simulator hanya untuk mencari kandidat.
   Yang masuk laporan hanya angka MT5.
2. **Gerbang identitas build** — HESTIA@NFP wajib **$9.516,50**. Meleset =
   laporan tidak ditulis sama sekali.
3. **Jalankan `02_Tools\cek_ps1.py`** sebelum menyerahkan `.ps1` apa pun.
4. **Tiga ujian** sebelum sebuah setelan dipercaya: sebaran (buang 2 event
   terbesar), dataran (geser tiap tombol), pindah event (NFP → CPI).
5. **Jangan buang kandidat tanpa konfirmasi.**
