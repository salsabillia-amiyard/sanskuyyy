# FORMAT JAWABAN YANG ALIF MAU (ditetapkan 26 Agt 2026)

Kalau Alif minta kesimpulan / "intinya gimana", jawab PERSIS 3 nomor ini,
urut, singkat. Jangan nambah bagian lain, jangan ngasih cerita panjang duluan.

**1. Setelan final** — EA-nya apa aja, dan isi setelannya apa aja.
   Sebut nama berkas EA + nilai input yang penting. Bukan deskripsi, ANGKA.

**2. Angka** — PnL per bulan, jeblok per bulan, jeblok seluruh periode,
   dan jumlah lot per bulan. Tabel, bukan paragraf.

   **WAJIB nempel di tabel bulanan (ditambahin 26 Agt 2026):**
   - **Modal berapa** — tulis modal awal periode, DAN kolom modal awal
     tiap bulan. PnL dolar tanpa modal itu angka kosong.
   - **PnL persen** — sandingin sama PnL dolar. $741 di bulan ke-17 itu
     4,7%, bukan lebih bagus dari $399 di bulan ke-1 yang 19,9%.
   - **Konsep lot** — lot TETAP apa lot ngikut modal (compounding)?
     Kalau tetap, bilang tetap dan sebut lot entry-nya per mesin,
     satu-satu. Jangan cuma bilang "0,02" tanpa nyebut mesin mana.
   - Kalau EA-nya aslinya auto-lot (risiko %), tulis kalau di uji ini
     dipaksa lot tetap — kalau nggak, pembacanya salah ngira.

   Alasannya: PnL dolar naik terus padahal persennya turun, gara-gara lot
   nggak pernah naik. Kalau modal + konsep lot nggak ditulis, tabelnya
   kebaca kayak "makin lama makin jago" — padahal bukan.

**3. Pelajaran + apa yang dikerjain** — bahasa awam, singkat.

Aturan tambahan:
- Jangan ngubur angka di dalam paragraf. Tabel.
- Kalau ada yang belum diuji, tulis di akhir bagian 3, satu baris per hal.
- Jangan ngulang penjelasan yang udah dikasih di jawaban sebelumnya.

---

---

# ⚠️ FVG: LANTAI LOT NYALA sejak 26 Agt 2026 — modal minimal $1.250

`EA_XAU_FVG_Trend` **v1.80 / v2.10**. Gerbang lama
`if(lot < g_lotMin) { g_nLewatLot++; return; }` **dicabut** atas permintaan Alif.
Sekarang lot yang di bawah minimum broker **DINAIKKAN ke minimum**, gak ada yang
ditolak, supaya semua akun client isi transaksinya SAMA.

**Sebabnya** (log live 26 Agt, dua akun VPS Asia, setelan sama persis): sinyal
13:45 butuh saldo $1.439; akun $1.4rb pasang, akun $1.2rb **lewat tanpa jejak**.
Habis itu yang pasang kekunci di posisi, yang lewat malah bebas nyamber sinyal
16:45 — **sekali beda, gak pernah sinkron lagi.** Terbukti sebab-akibat: matiin
gerbang itu di simulasi → semua saldo hasilnya identik.

**HARGA YANG DIBAYAR — WAJIB DIBACA SEBELUM MASANG DI AKUN KECIL.**
Di lot 0,01 XAUUSD, risiko dolar = jarak SL dalam dolar. Diukur dari 577 trade
FVG asli (`AUTOTEST/JEBLOK/TRADE_JEBLOK.csv`), jeblok terdalam di lot 0,01 =
**$308,77**:

| saldo | risiko 1 trade (SL maks $20) | jeblok terdalam | vonis |
|---|---|---|---|
| $300 | 6,67% | **102,9%** | **AKUN HABIS** |
| $500 | 4,00% | 61,8% | jauh lewat pagar |
| $1.000 | 2,00% | 30,9% | lewat pagar 25% |
| $1.250 | 1,60% | ~24,7% | **batas aman** |
| $2.000 | 1,00% | 15,4% | aman |

**Modal minimal biar jeblok masuk pagar 25% ≈ $1.250.** Jangan pasang FVG di
akun di bawah itu, dan jangan bilang "agak berisiko" — di $300 akunnya habis,
kejadian di data 15 Jan 2026.

`InpLotLantai=false` ngebalikin perilaku LAMA persis — pakai itu kalau mau
ngulang riset lama supaya angkanya bisa dibanding.

Rinciannya: `04_Documentation/VONIS_LANTAI_LOT_26AGT2026.md`

---

---

# ⚠️ MT5 NULIS DUA XML KALAU FORWARD NYALA — DAN INI UDAH MAKAN 3 SCRIPT

*(26 Agt 2026)*

```
Report=Reports\NAMA  +  ForwardMode=4
   ->  Reports\NAMA.xml           = periode BELAKANG
   ->  Reports\NAMA.forward.xml   = periode DEPAN
```

**Kolom `Back Result` / `Forward Result` di XML OTOMATIS itu SERING KOSONG.**
Yang keisi cuma di ekspor MANUAL dari tab optimasi MT5 (makanya XML lama di
`DATA\` kolomnya isi — itu ekspor tangan, bukan hasil `Report=`).

**Gejalanya paling jahat**, dan ini yang kejadian: kolom depan kosong → script
nganggap semua titik bernilai sama → **pemilih juara ngambil titik PERTAMA**,
yang kebetulan setelan paling jelek di peta. **Nol pesan salah.** Angka yang
keluar keliatan sah, dan baru ketahuan pas dibanding sama peta-nya sendiri.

**Obatnya:** baca **DUA-DUANYA**, gabungin lewat nomor `Pass` (urutannya
GAK sejajar, jangan diasumsiin). Fungsinya udah ada: `BacaXmlOptimasi` +
`AmbilAngka` di `AUTOTEST\BREAKOUT.ps1` dan `AUTOTEST\REZIM.ps1`.

**Plus gerbang CEK-KOSONG** — kalau titik yang punya angka depan < 4, script
BERHENTI. Pemilih juara gak boleh jalan di data kosong (aturan 8.5).

**Uji regresinya:** `AUTOTEST\KLIK_UJI_XML.bat` — bikin XML palsu (nomor Pass
sengaja diacak), buktiin penggabungannya bener, dan buktiin gerbangnya teriak
waktu forward-nya gak ada. Cepat, gak jalanin MT5. Klik ini tiap kali script
optimasi diutak-atik, SEBELUM jalanin run yang lama.

| script | status |
|---|---|
| `BREAKOUT.ps1` | ✅ ditambal |
| `REZIM.ps1` | ✅ ditambal |
| **`GRIDOPT.ps1`** | ❌ **MASIH KENA** — pakai `ForwardMode=4` tapi gak pernah baca `.forward.xml` |

---

---

# ⚠️ BARRIER MT5 — pass berikutnya GAK BOLEH mulai selama MT5 lama masih hidup

*(22 Agt 2026)*

`$pr.WaitForExit()` **cuma nungguin proses yang kita panggil.** `terminal64.exe`
itu peluncur — dia bisa nyerahin kerjaan ke proses `terminal64.exe` LAIN terus
keluar duluan. Akibatnya pass berikutnya mulai waktu MT5 lama masih jalan →
proses baru nyerahin diri ke yang lama → **keluar 2 detik, nol hasil, NOL PESAN
SALAH.** Lama-lama `.exe`-nya kekunci dan baru muncul:

> `Start-Process : The process cannot access the file because it is being used by another process.`

Jejaknya di log GABUNG 22 Agt:
```
[20:45:03]  pass 1 APR25 sukses (1 menit 55 detik)
[20:45:05]  MEI25 berkas ringkasan ada 0     <- 2 DETIK
[20:45:22]  JUN25 berkas ringkasan ada 0
```

**Obatnya** (udah dipasang di `GABUNG` `MART` `ADU2` `GRID` `GRIDOPT`):
fungsi `JalankanMt5` — sesudah `WaitForExit`, TUNGGU sampai gak ada lagi
`terminal64.exe` **BARU**, plus coba ulang 5x kalau `.exe`-nya lagi kekunci.
Proses yang udah ada dari SEBELUM script jalan sengaja diabaikan (`$MtAwal`) —
kalau ikut ditungguin, script nunggu selamanya gara-gara MT5 punya Alif yang
kebuka duluan.

**Script AUTOTEST LAMA (ADAPB, ADAPT, ARAH*, AE*, BAB*, dll — 40-an berkas)
BELUM ditambal.** Kalau salah satunya dipakai lagi, pasang `JalankanMt5` dulu.
Gejalanya gampang dikenali: ada pass yang selesai dalam hitungan detik dan
"berkas hasil ada 0".

---

---

# ⚠️ LAPTOP INI RESTART SENDIRI. RANCANG SEMUA RUN PANJANG BUAT MATI DI TENGAH.

*(ditambahin 21 Agt 2026, sesudah optimasi 18 pass ilang total gara-gara restart)*

**Terukur, bukan perasaan** — `LAPORAN_MATI_MENDADAK.txt` (14 Agt, 30 hari ke belakang):

| | |
|---|---|
| mati tanpa shutdown normal (Kernel-Power 41) | **19 kali dalam 30 hari** |
| layar biru | 0x13A tiga kali, 0x50, 0x10E |
| mati TANPA layar biru | 5 dari 10 terakhir |
| error hardware (WHEA) / termal / disk | **0** |
| sisa disk | 199 GB (42%) — **bukan** masalah ruang |

Kode layar biru **campur-campur** (0x13A heap corruption, 0x50 page fault,
0x10E video memory). Kalau satu driver yang rusak, kodenya biasanya itu-itu aja.
Campur begini polanya **RAM**. Dan yang mati tanpa layar biru itu ciri
**listrik/panas** pas semua inti dipaksa kerja — persis yang dilakuin MT5 optimizer.

## Yang harus dilakuin Alif (urusan hardware)
1. **Windows Memory Diagnostic** (ketik "mdsched" di Start) atau **MemTest86** semalaman.
2. **Update driver VGA** — buat yang 0x10E.
3. Jalanin `AUTOTEST\KLIK_CEK_MATI.bat` **klik kanan → Run as administrator**
   buat data terbaru (laporan 14 Agt jalan tanpa admin, sebagian log gak kebaca).

## Yang harus dilakuin SETIAP SESI AI (urusan script) — INI YANG SERING KELEWAT
**Run yang perkiraannya lewat 45 menit WAJIB dipecah, punya `KEMAJUAN.txt`, dan
bisa dilanjut cuma dengan klik ulang .bat yang sama.** Plus: jangan pakai semua
inti — potong kecil bikin agen yang jalan sedikit, panasnya turun.

Contoh yang udah bener: `AUTOTEST\GRIDOPT.ps1` (6 potong × 3 pass).

**Alasan aturan ini ada:** alat `CEK_MATI_MENDADAK.ps1` udah dibikin 2 Agt karena
masalah yang SAMA. Alatnya ada, laporannya ada, tapi tiap sesi baru bikin script
panjang baru yang nganggep laptopnya bakal hidup 4 jam lurus. **Yang bocor bukan
pengetahuannya — tapi cara pengetahuannya nyampe ke sesi berikutnya.**

---

# 🛑 BERHENTI. BACA FILE INI SAMPAI HABIS SEBELUM NYENTUH APA PUN.

**Dibikin 5 Agustus 2026** · buat sesi Claude berikutnya (sesi lama kena limit)
**Folder:** `C:\scalping` (bot straddle) · project sebelah `C:\Trading` (bot Camarilla)

> Kalau lu Claude yang baru buka project ini: **JANGAN ngedit file, JANGAN bikin
> script, JANGAN kasih saran angka** sebelum kelar baca ini. Project ini udah
> berbulan-bulan jalan, udah puluhan kali salah, dan hampir semua kesalahannya
> BERULANG karena AI berikutnya gak tau apa yang udah pernah dicoba.
>
> **Kesalahan paling mahal di project ini bukan salah hitung — tapi ngasih angka
> yang keliatan meyakinkan padahal ngukur hal lain.** Alif nyebutnya "angka bohong".

---

# BAGIAN 1 — SIAPA YANG LU AJAK NGOMONG

**Nama: Alif.** Bahasa Indonesia santai ("lu/gue"). Bukan programmer, bukan
analis. Pinter, kritis, sering nangkep kejanggalan yang gue lewatin — **kalau dia
nunjuk kontradiksi, itu HAMPIR SELALU petunjuk beneran, bukan salah paham.**

### 🗣️ ATURAN BAHASA (WAJIB, bukan saran)

**Tiap istilah teknis WAJIB dijelasin pas pertama muncul.** Contoh yang bener:

- straddle = "ngapit harga dua sisi"
- offset = "jarak order dari harga"
- drawdown = "jeblok" (seberapa dalam modal turun dari puncaknya)
- trailing stop = "stop rugi yang ngikutin harga"
- ATR = "ukuran gerakan rata-rata"
- pending order = "order titipan yang nunggu harga nyampe"

Jangan lempar jargon telanjang. Anggap pembaca cerdas tapi BUKAN ahli.

### 💙 KONTEKS PRIBADI — WAJIB DIPEGANG

Alif **kehilangan uang besar di trading Juni 2026, sebagian DUIT PINJAMAN**, dan
lagi **defisit keuangan**. Modal yang dipakai sekarang **$2.000**.

- **JANGAN** fasilitasi trading pakai duit pinjaman / duit yang gak sanggup ilang.
- **JANGAN** kasih harapan palsu. Edge-nya tipis dan rapuh-rezim.
- Trading **BUKAN** income andalan dia. Jangan diperlakukan gitu.
- Kalau angkanya jelek, **bilang jelek**. Dia lebih milih jujur daripada disenengin.

Detail lengkap: `C:\Trading\Master File\KONTEKS_PRIBADI_ALIF.md`

### 🚫 TANYA ≠ SURUH EKSEKUSI

Kalau Alif cuma **NANYA** ("kenapa gini?", "ini apa?"), **JAWAB dan jelasin doang.**
**JANGAN ngedit file/kode/setelan** kecuali dia EKSPLISIT minta diubah.
(Pernah kejadian: dia nanya cara kerja recenter, Claude malah langsung matiin fiturnya.)

---

# BAGIAN 2 — WAJIB PANGGIL SKILL INI

Ada skill permanen: **`mt5-riset-bot-anti-bohong`**

Isinya jebakan teknis yang **udah pernah bikin angka riset bohong** di project ini,
plus cara verifikasinya. **Panggil skill itu tiap kali** mau:
bikin/edit EA `.mq5` · bikin script otomasi tester `.ps1`/`.bat` · baca hasil backtest.

Jangan kerja tanpa itu. Isinya hasil berbulan-bulan kena batunya.

---

# BAGIAN 3 — POSISI PROJECT SEKARANG (5 Agt 2026)

## 3.1 Yang LAGI JALAN di duit asli

| | |
|---|---|
| EA live | **`Straddle_Trio_LIVE_OLD.mq5` v3.24** di VPS, akun 33942840 |
| lokasi file | `C:\scalping\15_LIVE_OLD\` |
| modal | $2.000 |
| lot | auto-lot, acuan saldo $2.000 |

**⛔ FILE `15_LIVE_OLD\Straddle_Trio_LIVE_OLD.mq5` JANGAN DIEDIT SAMA SEKALI.**
Itu versi yang Alif jalanin live dan jadi patokan pembanding. Sudah dipulihkan
dengan susah payah (24 perubahan dimundurin satu-satu dari catatan sesi, karena
gak ada file cadangan). Kalau perlu perubahan → **file baru**, jangan sentuh yang ini.

## 3.2 Tiga EA yang penting

| file | versi | perannya |
|---|---|---|
| `15_LIVE_OLD\Straddle_Trio_LIVE_OLD.mq5` | 3.24 | **LIVE. JANGAN DIEDIT.** |
| `01_EAs\Straddle_Trio_LIVE.mq5` | 3.32 | build live cabang pengembangan |
| `01_EAs\Straddle_Trio_KELT_OPT.mq5` | **3.50** | **build RISET** — semua uji di sini |
| `01_EAs\Straddle_News_OPT.mq5` | 3.x | bot news (CPI/NFP/FOMC) |

**Aturan:** riset di `KELT_OPT`, kalau menang baru diport ke build live.

## 3.3 Angka juara sekarang (BUKAN ekspektasi — baca Bagian 6 dulu)

Config **D0** (setelan sekarang), 16 bulan (Apr 2025 – Jul 2026), lot 0,02, tick asli:

| | |
|---|---|
| cuan | **$13.130** |
| jeblok (drawdown) | **12,1%** |
| transaksi | 10.693 |
| bulan tanpa transaksi | **5** (Mei–Sep 2025) |

## 3.4 Bot news (terpisah, buat lantai bulanan)

| | |
|---|---|
| cuan | **$3.751** · jeblok **8,8%** |
| event kedagangan | **40 dari 40** (~4/bulan) |
| script | `AUTOTEST\NEWS.ps1` + `KLIK_NEWS.bat` |

Ini **satu-satunya** mekanisme yang jalan-by-jadwal, jadi hadir tiap bulan
apa pun rezimnya. Straddle **gak bisa** kasih volume merata — udah kebukti 2×.

---

# BAGIAN 4 — 🔴🔴 TEMUAN PALING PENTING SEPANJANG PROJECT

## 5% HARI = 90% CUAN

Config juara D0, 341 hari, dipisah per besar gerakan harian:

| gerakan harian | hari | % hari | trade/hari | cuan $ | **% cuan** |
|---|---|---|---|---|---|
| 0–99 pip | 47 | 13,8% | 0,00 | 0 | 0,0% |
| 100–199 | 154 | **45,2%** | 2,61 | +210 | **1,6%** |
| 200–299 | 98 | 28,7% | 17,62 | +537 | 4,1% |
| 300–399 | 25 | 7,3% | 74,40 | +509 | 3,9% |
| **400–499** | **6** | 1,8% | 173,83 | **+3.694** | **28,1%** |
| **500+** | **11** | 3,2% | 514,64 | **+8.180** | **62,3%** |

**17 hari dari 341 = 90,4% dari SELURUH cuan.**

Per bulan:

| bulan | cuan $ | % total |
|---|---|---|
| **Mei–Sep 2025** (5 bulan) | **0** | **0%** |
| Okt–Des 2025 | +207 | 1,6% |
| **Jan 2026** | **+5.479** | **41,7%** |
| **Feb 2026** | **+4.095** | **31,2%** |
| **Mar 2026** | **+2.263** | **17,2%** |
| Apr–Jul 2026 | +1.298 | 9,9% |

**3 bulan dari 16 = 90,1% cuan.**

## Artinya apa (INI YANG HARUS DIPEGANG TIAP NGASIH SARAN)

Ini **BUKAN** strategi "dagang tiap hari dapet dikit-dikit".
Ini strategi **"nungguin ledakan"** yang kebetulan masang order tiap hari.

Order harian di 95% hari itu **bukan sumber cuan — itu ONGKOS BERJAGA.**

**⇒ Target Alif "1 lot per hari merata tiap bulan" BERTENTANGAN sama sumber
cuannya.** Itu ngejar 95% hari yang cuma ngasih 10% cuan. Ini udah dites dari
banyak arah dan hasilnya konsisten. **Jangan janjiin itu bisa.**

---

# BAGIAN 5 — PERJALANAN & SEMUA YANG UDAH DICOBA

## 5.1 Yang TERBUKTI MATI — jangan diulang

| # | dicoba | hasil | bukti |
|---|---|---|---|
| 1 | **Gedein lot buat kejar 1 lot/hari** | jeblok 382% = akun ludes | uji tangga lot |
| 2 | **`Straddle_Rata` v1.00** (risiko per transaksi, lot dihitung mundur dari target volume) | **50 dari 50 setelan RUGI, 44 jeblok ~99,7%** | risiko 1%/transaksi × 45 transaksi/hari = 45% modal/hari; DAN lot digedein justru di hari sepi |
| 3 | **`Straddle_Rata` v2.00** (jatah rugi harian) | **25 dari 25 RUGI**, rugi naik linear ikut besar taruhan | straddle polos = **nol edge** |
| 4 | **Tarik TP keluar biar R:R ≥1** | r_f 78 → 15, jeblok meledak 96–107% | edge-nya mean-reversion TP-tipis |
| 5 | **Buka paksa hari sepi** (semua takaran lot) | **semua RUGI**, makin banyak transaksi makin minus/transaksi (+0,030 → −0,600) | Blok 1, 5 Agt |
| 6 | **Kecilin jarak jebakan pas sepi** (adaptif) | **RUGI, 2×**, % hari cuan turun 59,2% → 36,4% lurus | Blok 2, 5 Agt |
| 7 | **Pengaman biaya diikat ke SPREAD** | **kebalik arah** — spread melebar pas hari besar → jebakan didorong menjauh tepat pas hari yang bayar. Mesin B di hari 500+ pip: **−83% transaksi, cuan +$3.711 → −$127** | Blok 2, 5 Agt |
| 8 | **`Expiry_Mandiri`** (umur order diurus EA, dibikin buat "fix" invalid expiration) | **MERUGIKAN** — transaksi +35,6%, cuan +15,7%, **jeblok +85,5%**. Dan fix-nya ternyata **gak perlu** (lihat 5.2) | 4–5 Agt |

## 5.2 Dua kesalahan diagnosa besar (5 Agt) — PENTING

### (a) "invalid expiration = masalah live-only" → SALAH

Gue kira error `invalid expiration` cuma muncul di live, terus bikin fitur
`Expiry_Mandiri` yang **ngerusak strategi**. Ternyata:

| | Tester (Juli, 16 hari) | Live VPS (10 hari) |
|---|---|---|
| gagal `invalid expiration` | 119.420 | 35.600 |
| berhasil kepasang | 50.068 | 16.008 |
| % berhasil | **29,5%** | **31,0%** |
| **order yang HILANG** | **0** | **0** |
| telat ≤ 60 detik | 100% | 99,99% |

**Mekanismenya (terbukti, bukan tebakan):** broker **membulatkan waktu kedaluwarsa
ke bawah ke menit bulat**, lalu **nolak kalau sisanya < 60 detik**. Umur order 90
detik → cuma yang dikirim di **detik 30–59** yang lolos. EA nyoba ulang tiap 8
detik sampai tembus. **Jadi cacat ini cuma NUNDA (maks 39 detik), bukan ngebatalin.**

**⇒ Backtest = live untuk cacat ini. Angka tester TIDAK melebih-lebihkan.**

**PELAJARAN:** sebelum vonis "ini masalah live-only", **GREP DULU LOG TESTER-nya.**

### (b) "tester gak dagang = EA rusak" → SALAH

Tes 3–5 Agt nol transaksi. Sebabnya bukan bug:

```
2026.08.04 01:00:00   [REZIM] NORMAL -> MATI (ATRavg 159.2 pip)
```

Saringan rezim butuh **24 contoh (1/jam = 24 JAM)** baru berani mutusin. Tes mulai
3 Agt 00:00 → contoh ke-24 jatuh 4 Agt 01:00. Rata-rata gerakan 159,2 < ambang 170
→ vonis SEPI → `Regime_DeadLotMult=0` → lot NOL → diam total. Buat balik NORMAL
butuh > 187 pip; gak kecapai.

**PELAJARAN:** "tester nol transaksi" ≠ EA rusak. **Nyalain `InpDebugLog=true`,
baca alasannya.** Jawabannya keluar dalam 1 baris, 30 detik.

**JANGAN tes EA ini di jendela < 2 minggu.** Saringan rezim butuh 24 jam cuma buat
nyala, dan rata-ratanya pakai jendela 96 jam (4 hari).

### (c) EFEK SAMPING yang baru ketemu

Tiap EA **di-restart**, hitungan rezim **balik NOL** → masa pemanasan → dianggap
NORMAL → mesin dagang lagi 24 jam **walau pasar beneran sepi**. Kebukti di log VPS:
4 Agt Trio jalan lurus 00:00–08:13 = **diam total**; dipasang ulang 16:51 →
**langsung dagang**. **Di live, perilaku EA tergantung KAPAN TERAKHIR DI-RESTART.**

Sudah ditambal di `01_EAs\Straddle_Trio_LIVE.mq5` v3.32 (tombol
`Regime_IsiDariSejarah`, default nyala) — **belum ditambal di build riset v3.40.**

> **⚠ UPDATE 7 Okt 2026 — tambalan di atas (isi-ulang dari sejarah, kebawa ke v1.61)
> cuma nambal SEPARUH.** Angkanya bias +2,4% (baca bar M1 yang udah selesai, padahal
> EA live nyampel di tick pertama tiap jam) dan ingatan histeresis gak kebangun
> (mulai NORMAL, cuma 96 jam). Diukur di data 2 tahun: **10,9% attach ulang bikin A/B
> dagang di jam yang harusnya MATI**, 17% bikin K salah. Kejadian nyata 7 Okt: EA
> diam bener dari 07:00 VPS, attach ulang 11:38 → "172.0 → NORMAL" → dagang lagi.
> Obat yang terbukti (nol jam beda di 2 tahun): putar ulang 3 mesin ~1.000 jam.
> **UPDATE 8 Okt 2026: v1.62 UDAH DIBIKIN + LULUS UJI** (rezim 0 beda dari 12.815 adu,
> NFP 2 Okt + 1 Sep–3 Okt identik v1.61 di dua broker). Siap pasang:
> `SIAP_PASANG_07OKT2026_v1.62\BACA_DULU_v162.txt` · laporan
> `10_Hasil_Vonis\LAPORAN_UJI_v162_07OKT2026.html`. Sampai v1.62 kepasang di VPS:
> jangan attach ulang kalau gak perlu. (Catatan: di tester EA yang jalan terus udah
> MATI dari 23:00 server 6 Okt = 03:00 VPS, 4 jam lebih awal dari EA di VPS.) Laporan: `10_Hasil_Vonis\LAPORAN_REZIM_ATTACH_07OKT2026.html` ·
> alat: `02_Tools\ukur_rezim_attach.py` (pengganti `cermin_rezim.py` yang pakai
> rumus ATR Wilder — MT5 pakai SMA).

## 5.3 Kesalahan KELAS "angka bohong" — yang paling mahal

| # | kesalahan | akibat | ketahuannya |
|---|---|---|---|
| 1 | **Kolom `PersenMenang` di EA news dikutip mentah** | gue lapor "menang 98%", aslinya **63%**. Kode: `if(untung) g_hasilEvt=1` — **satu transaksi cuan doang bikin seluruh event dicap MENANG**, dan capnya gak pernah dicabut. Ada baris PnL **−$3,10** dicap "UNTUNG". | **Alif yang nangkep**, bukan gue |
| 2 | **`$W[$w]` di `[ordered]@{}` balikin KOSONG** | `FromDate`/`ToDate` di .ini kosong → MT5 diam-diam pakai rentang bawaannya → **3 jendela jalan di periode SAMA**, hasil identik, **NOL error** | banding hasil byte-per-byte |
| 3 | **File `.sidik` ditulis pakai `[System.Text.Encoding]::UTF8`** | BOM ikut ketulis, `.Trim()` gak buang BOM → perbandingan resume **SELALU gagal** → **0 blok kelewat dari 26 sesi** | baca log |
| 4 | **Lot 0,015 direkomendasiin** | step broker 0,01 → **tidak sah**. Run itu 9.736 transaksi vs 14.495 config lain | jumlah transaksi janggal |
| 5 | **`[array]::IndexOf` + banding float persis** | crash `op_Subtraction` di tengah run + gagal senyap | crash |
| 6 | **Format Python `{1,>5}` di PowerShell** | crash di baris laporan terakhir SETELAH run kelar berjam-jam | crash |
| 7 | **Salah pakai pembagi**: pakai porsi HARI (5,9%) padahal harusnya porsi TRANSAKSI (56,9%) | prediksi volume ×1,06, aslinya ×1,61 | banding hasil |
| 8 | **Tangga lot D25/D50/D75 hasilnya identik** | lot 0,02 × 0,25/0,50/0,75 semua ke-clamp ke minimum broker 0,01 → **3 anak tangga jadi 1** | hasil sama persis sampai sen |
| 9 | **Klaim "default adaptif dikalibrasi netral"** | di praktek ×0,97 dagang **1.351 vs 759** = 78% lebih agresif. **Gue gak verifikasi ke data sebelum ngomong** | hasil run |
| 10 | **Nuduh perubahan gue sendiri yang ngerusak resume** | padahal log nunjukin 0 skip **jauh sebelum** perubahan itu. Nuduh sebelum ngukur | baca log |
| 11 | **`$tet = @()` dipakai buat NGITUNG** lalu `$sehat / $tet` | crash `op_Division` **di baris vonis paling akhir**, sesudah run 40 menit kelar. Pencacah harus `[int]`, bukan array | crash |
| 12 | **Format `"{0,+10:N2}"`** (tanda + di rata-kanan) | BUKAN format .NET yang sah — rata-kanan cuma nerima angka polos. Meledak di baris terakhir. Tempel tanda +/- manual | ketangkep pas periksa, sebelum jalan |
| 13 | **`Get-Content -Encoding 'utf-16'` / `'utf-8'`** | **Nama itu GAK ADA di PowerShell** (yang sah: `Unicode` / `UTF8`, tanpa strip). Errornya **ketelen `try{}catch{}`** → log kebaca **kosong** → script vonis *"final balance gak ketemu"* alias **nuduh RUN-nya gagal padahal run-nya sukses**. Obat: baca **byte mentah** → deteksi BOM → decode manual | crash gerbang, 5 Agt |

### 🔑 POLA UMUM DARI SEMUA KESALAHAN DI ATAS

1. **Angka yang keliatan meyakinkan tapi ngukur hal lain** → cek DEFINISI kolom sebelum ngutip.
2. **Gagal senyap** — nol error, angka tetap keluar, cuma salah. → pasang palang yang TERIAK.
3. **Fix dipasang di satu file, file sebelahnya kelewat.** → tiap nambal, **sisir file sejenis**.
4. **Gerbang merah jangan ditebak — dikejar.** Ukur dulu, baru nuduh.

## 5.4 Palang wajib di tiap script uji (semuanya ada korbannya)

- `$BASE` diturunin dari **FILE EA**, bukan diketik ulang di script
- **sidik-jari config** disimpan; hasil lama cuma dipakai kalau config SAMA PERSIS
- **jumlah baris per Tag wajib nambah TEPAT 1** sesudah run
- **palang tanggal + baca-balik .ini** (yang dicek isi FILE, bukan variabel di memori)
- CSV EA **diarsipin di awal sesi** (bukan ditimpa, bukan dibiarin numpuk)
- semua angka ke `.set` lewat `Fmt*` (koma ribuan pernah ngeracunin run)
- `InpDebugLog` **selalu false** di run panjang · **nol nyalin `*.log`** (pernah 191 GB)
- `*.gen` dihapus · tick asli `Model=4` · `UseCloud=0`
- nilai parameter **DISIMPAN di CSV**, jangan ditebak balik dari nama tag

Pemeriksa otomatis: `02_Tools\cek_ps1.py` (script) dan `02_Tools\cek_mq5.py` (EA).
**Jalanin sebelum nyerahin file apa pun ke Alif.** Ada uji regresi dua-arah di
`02_Tools\uji_cek_ps1\jalanin_uji.py`.

## 5.5 Cara kerja yang Alif minta (dia yang koreksi, 3 Agt)

> *"cara kerja lu salah, lu backtest satu satu"*

**Pakai OPTIMIZER buat MEMETAKAN dulu** (cari AREA sweet-spot dengan sedikit
kombinasi), **baru backtest tunggal buat FINALIS.** Jangan nembak satu-satu.

**Milih pemenang = cari DATARAN, bukan PUNCAK.** Nilai sebuah setelan = titik
**paling lemah** di sekitarnya. Puncak sendirian di tengah jurang = keberuntungan,
bukan temuan.

**Jatah percobaan itu barang mahal.** Makin banyak setelan dicoba, makin gampang
ada yang keliatan bagus **cuma karena kebetulan**. Penangkalnya: jendela penjaga
(dipakai buat MENOLAK, bukan buat MILIH) + aturan dataran.

---

# BAGIAN 6 — ⚠️ BATAS KEJUJURAN YANG WAJIB DIPEGANG

**Cuan $13.130 itu bertumpu di 17 HARI.** Semua penyeteloan yang ngincer hari-hari
itu = **menjahit baju buat 17 sampel**. Detektor ekspansi nunjukin +80% — tapi
**di 17 hari yang sama**. Nilainya di data yang belum pernah dilihat **BELUM kebukti**.

Konsekuensi praktis yang **wajib** disampaikan kalau Alif nanya soal harapan:

- Angka backtest ini **PLAFON**, bukan ekspektasi.
- Kalau 12 bulan ke depan gak ada bulan kayak Jan–Mar 2026, hasilnya **mendekati
  nol**, bukan $13.000.
- **Ini bukan penghasilan yang bisa diandalkan.**

---

# BAGIAN 7 — TUGAS YANG LAGI NGGANTUNG (lanjutin dari sini)

## 7.1 Yang Alif minta terakhir

> *"gimana kalau di hari sepi logika mesinnya dibalik — yang tadinya buy stop
> sell stop jadi buy limit sell limit... dan apakah butuh TP bukan trailing?"*
>
> *"susun testnya sambil lu terus perhatiin forensik perdaynya untuk cari solusi"*

## ✅✅ UDAH DIJALANIN — DAN BERHASIL. INI TEMUAN POSITIF PERTAMA SETELAH BANYAK GAGAL.

**Ide Alif kebukti bener.** Jendela SEPI (Apr–Des 2025), lot mati 0,02:

| jarak/SL | trade | cuan $ | jeblok | **bulan nol** | hari cuan |
|---|---|---|---|---|---|
| **650 / 1,20** | 660 | **+111,44** | **12,7%** | **0** | 56,7% |
| 850 / 1,00 | 621 | +108,26 | 12,2% | 2 | 61,5% |
| 650 / 1,00 | 665 | +95,56 | 14,8% | 0 | 53,3% |
| 550 / 1,00 | 757 | +93,16 | 17,0% | 0 | 44,6% |
| *D0 mesin dimatiin (patokan)* | 599 | *+16,18* | *12,1%* | ***5*** | — |
| *breakout B nyala (patokan)* | 759 | *−126,66* | *18,8%* | *0* | — |

**Fade 650/SL1,20 = juara.** Ngalahin "mesin dimatiin" ($111 vs $16) **SAMBIL
ngilangin 5 bulan mati**, cuma nambah 0,6 poin jeblok.

**Ini DATARAN, bukan puncak sendirian** — 550 sampai 850 semuanya positif.

### 🔴 TEBING BIAYA: jarak sempit = AKUN LUDES, bukan cuma rugi

| jarak | cuan | jeblok | **ekuitas terendah (modal $2.000)** |
|---|---|---|---|
| 150 · 250 · 350 | ~−$1.990 | **99,4–99,7%** | **$6,73 – $12,26** |
| 450 | −$71 s/d −$683 | 15–37% | $1.231 – $1.684 |
| 550 – 850 | **+$57 s/d +$111** | 12–20% | aman |

Angka −$1.990 itu **BUKAN "rugi $1.990"** — itu **duitnya HABIS**. Semua config
≤350 pip nabrak lantai, makanya angkanya mirip semua. Jangan sampai kebaca
sebagai angka rugi biasa.

**Pelajaran fisik:** biaya spread itu TETAP, gak ikut ngecil. Fade butuh target
LEBAR biar ongkos jadi porsi kecil. TP tipis bukan "kurang untung" — **mematikan**.

---

## (Riwayat) STATUS SEBELUMNYA: EA + SCRIPT UDAH JADI, TINGGAL DIJALANIN

| | |
|---|---|
| EA | `01_EAs\Straddle_Trio_KELT_OPT.mq5` **v3.50** (VERSI_EA 350) |
| tombol baru | `Sepi_Mode` · `Fade_Off_Pips` · `Fade_TP_Ratio` · `Fade_SL_Ratio` · `Fade_Expiry_Sec` |
| script | `AUTOTEST\FADE.ps1` |
| **tombol Alif** | **`AUTOTEST\KLIK_FADE.bat`** ← tinggal diklik, ~30–50 menit |

**Yang harus lu lakuin kalau Alif belum sempat klik: minta dia klik itu, terus
baca hasilnya.** Jangan bikin ulang, udah ada.

### Cara kerja mode fade di EA v3.50

`FadeAktif()` = `Sepi_Mode==1 && g_regime==2 && m_kode==1`
→ **cuma mesin B, cuma pas rezim SEPI, cuma kalau Sepi_Mode=1.**

Kalau aktif: **BuyLimit di BAWAH harga · SellLimit di ATAS harga · TP dipasang di
order · SL dipasang di order · TRAILING DIMATIIN TOTAL** · larangan rezim-sepi
dan pengali lot rezim-sepi DILEWATI (fade emang penggantinya).

`Sepi_Mode=0` → seluruh jalur baru mati, perilaku identik v3.40.

### 🔴 GERBANG REGRESI — angka patokannya

`Sepi_Mode=0` di jendela SEPI **WAJIB** ngasih **PERSIS**:

```
599 transaksi  ·  +$16,18  ·  jeblok 12,10%
```

Beda seangka pun = **kode mode fade BOCOR ke jalur normal → STOP, jangan
diterusin.** Script udah bawa gerbang ini di BLOK 0, dia berhenti sendiri.

### Yang disapu (12 kombinasi)

- `Fade_Off_Pips` **150 · 250 · 350 · 450** — 150 sengaja dimasukin walau
  diperkirakan GAGAL karena ongkos, biar **tebing biayanya kepetakan**
- `Fade_SL_Ratio` **0,70 · 1,00 · 1,50**
- `Fade_TP_Ratio` dikunci 1,00 (balik ke tengah)

### Cara baca vonisnya

| lampu | artinya | tindakan |
|---|---|---|
| **HIJAU** | fade > "mesin dimatiin" (+$16,18) | lanjut: jendela penuh + tangga lot |
| **KUNING** | fade > breakout (−$126,66) tapi < D0 | arah bener, belum cukup |
| **MERAH** | fade < breakout | ide gugur — hari sepi emang gak ada yang bisa dipanen |

Script juga nyetak tabel **TEBING BIAYA** (rata cuan per jarak) dan **PENJAGA
2026** (kalau ada yang bikin 2026 turun >5%, ditandain `<-- 2026 RUSAK`).

### ⚠️ Catatan jujur yang WAJIB disampein ke Alif

Hadiah maksimalnya **~$60/bulan di lot 0,02 (≈4% dari total)**. Nilainya di
**VOLUME dan kehadiran tiap bulan**, BUKAN di cuan. Jangan dijual lebih dari itu.

---

### (Arsip) Rancangan aslinya — buat konteks kalau perlu diulang dari nol

## 7.2 Forensik yang MENDUKUNG ide fade ini

154 hari sepi, semua mesin dibuka:

| | |
|---|---|
| hari **nol transaksi** | 31 (20%) — jebakan gak kesenggol sama sekali |
| hari **cuan** | 63 (51%) · rata **+$7,63** |
| hari **rugi** | 59 (48%) · rata **−$17,43** |

Menang-kalah 50:50 **tapi yang kalah 2,3× lebih gede** = tanda tangan pasar
bolak-balik (harga nyentuh jebakan lalu **balik lagi**). Breakout kena tipu;
fade manen kejadian yang sama. Plus 31 hari nol-transaksi: limit yang dipasang
**di DALAM** kotak bakal kefill di hari-hari itu → **volume nambah**.

## 7.3 Jawaban soal TP vs trailing: **TP. Trailing salah kaprah buat fade.**

1. Hari cuan di pasar sepi rata cuma **+$7,63** — gerakannya gak cukup buat
   trailing nyala (butuh nyentuh ambang) lalu ngasih balik jarak trailing.
2. Setelan sekarang trailing nyala di **86 pip padahal jebakan 580 pip** = 15%
   perjalanan. Buat fade yang targetnya ~sejarak jebakan, itu **motong pemenang
   di sepertiga jalan**.
3. Titik keluar fade **udah ketauan dari awal** (balik ke tengah) = TP menurut
   definisi. Trailing gak nambah apa-apa, cuma ngasih balik cuan.

## 7.4 Yang bakal nentuin hidup-matinya: ONGKOS

Lot 0,02, spread ~40 pip = **$0,80 per transaksi**.

| TP | nilai kotor | porsi ongkos |
|---|---|---|
| 100 pip | $2,00 | **40%** ← mati |
| 200 pip | $4,00 | 20% |
| **300 pip** | $6,00 | **13%** ← baru masuk akal |

**TP tipis = mati sebelum mulai.** Bukan tebakan — project sebelah
(`C:\Trading`, Camarilla) itu **persis** fade TP-tipis. Vonis tercatatnya: butuh
**~63% menang** cuma buat impas, di tick asli jatuh ke 51% → rugi.

Fade **wajib** punya stop rugi (1 hari "sepi" di data meledak: 21+ transaksi, −$208).
Geometrinya nentuin:

| TP / SL | menang minimal buat impas |
|---|---|
| 300 / 300 | **57%** |
| 300 / 600 | 71% |
| 300 / 900 | 78% |

**⇒ SL gak boleh lebar** — kebalikan dari mesin sekarang (SL 638 vs jebakan 580).

## 7.5 Ukuran hadiahnya — jangan berharap kegedean

Hari sepi sekarang netto **−$548** (9 bulan, lot 0,02). Kalau fade jadi cerminan
sempurna, kotornya ~**+$548** = **~$60/bulan**. Bandingin seluruh strategi
$13.131/16 bulan → fade di hari sepi itu **tambahan ~4%**.

**Nilainya di VOLUME dan kehadiran tiap bulan, BUKAN di cuan.** Bilang apa adanya.

## 7.6 Rancangan tes yang disepakati

**Ubah `01_EAs\Straddle_Trio_KELT_OPT.mq5` (build riset), JANGAN yang di `15_LIVE_OLD`.**

Ubahan terkurung di 3 titik kirim order (baris ~990, ~1000, ~1011: `BuyStop`/
`SellStop` → `BuyLimit`/`SellLimit`), plus pasang TP, plus matiin trailing pas
mode fade.

Satu tombol: **`Sepi_Mode`** = `0` mati (sekarang) · `1` fade khusus hari sepi.

**GERBANG REGRESI WAJIB:** `Sepi_Mode=0` harus reproduce angka v3.40 **SAMA PERSIS**.
Beda seangka pun = kodenya salah.

**Yang disapu cuma 2 (jatah percobaan tipis):**
- **jarak fade** — seberapa dalam limit dipasang dari tengah
- **rasio SL** — 0,7 / 1,0 / 1,5 × TP

≈12 kombinasi di jendela SEPI, plus jendela penjaga 2026 buat mastiin mode 0
gak kesenggol.

**Jendela baku:**

| nama | periode | peran |
|---|---|---|
| SEPI | 2025.04.01 – 2025.12.31 | laboratorium |
| PENUH | 2025.04.01 – 2026.07.28 | hakim |
| RAME | 2026.01.01 – 2026.07.28 | **penjaga — 2026 gak boleh rusak** |
| CEPAT | 2026.07.01 – 2026.07.10 | gerbang versi |

Data tick asli tersedia dari **2025.03.10**.

**Template script:** contoh paling rapi & mutakhir = `AUTOTEST\SEPI.ps1` dan
`AUTOTEST\ADAPB.ps1` (5 Agt). Salin strukturnya bulat-bulat — semua palang
udah kepasang di situ.

## 7.6b 🔴 TUGAS YANG LAGI NGGANTUNG SEKARANG — ADU LAWAN EA LIVE

### Permintaan Alif (5 Agt, paling akhir)

> *"gue mau arah dari riset kita ngalahin ea straddle trio yang live sekarang,
> dengan settingan 0.1 lot dan auto lot on dan juga rem 2"*
> *"yang gue maksud file ea yang OLD, tapi jangan lu sentuh — jadi kita harus kalahin dia"*

**Patokan yang harus dikalahin = `15_LIVE_OLD\Straddle_Trio_LIVE_OLD.mq5`**
dengan **autolot ON · rem (`AutoLot_MaxLot`) 2,0 · acuan saldo $2.000.**

⛔ **FILE ITU TETAP HARAM DIEDIT.** Script cuma boleh NYALIN dia ke folder
Experts buat dikompile.

### Status: SCRIPT UDAH JADI, BELUM DIJALANIN

| | |
|---|---|
| script | `AUTOTEST\BANDING.ps1` |
| **tombol Alif** | **`AUTOTEST\KLIK_BANDING.bat`** ← ~40–70 menit |

### Kenapa scriptnya dibikin begitu (jangan disederhanain)

**1. EA OLD GAK BISA nulis CSV hasil.** Dia build live — `OnTester`, skor, dan
penulis CSV semuanya udah dibuang. Jadi hasilnya dibaca dari **LOG TESTER**:
baris `final balance XXXX.XX USD` + jumlah baris `deal #`. Dua-duanya
**bebas-bahasa**. Laporan `.htm` sengaja TIDAK dipakai karena labelnya ikut
setelan bahasa MT5 — gampang salah baca tanpa ketauan.

**2. Ada BLOK EKUIVALENSI, dan itu wajib.** Build riset v3.50 *katanya* jalur
sinyalnya identik sama OLD kalau semua fitur baru dimatiin. **"Katanya" itu
ASUMSI.** Blok 1 ngukur langsung: OLD vs riset-setara, config sama, jendela sama.
Beda cuan >$1 → script **berhenti sendiri**. Kalau gagal, itu temuan besar:
berarti angka riset selama ini ngukur strategi yang BEDA dari yang live.
Kalau lolos, jeblok OLD boleh diambil dari angka riset (urutan transaksi terbukti sama).

**3. Takaran lot diukur DUA-DUANYA** biar gak nebak maksud "0,1 lot":

| | A | B | K | |
|---|---|---|---|---|
| L05 | 0,05 | 0,05 | 0,06 | default LIVE yang beneran jalan |
| L10 | 0,10 | 0,10 | 0,13 | konvensi "validasi riset" di komentar EA |

**4. ⚠️ Autolot ON = lot IKUT NAIK seiring saldo naik.** SEMUA angka riset
sebelumnya diukur di **lot MATI 0,02**. Di setelan ini **jeblok bakal JAUH lebih
besar** — itu konsekuensi autolot, bukan bug. Script nolak otomatis kalau lewat
toleransi 30% walaupun cuannya naik.

### Penantangnya

Fade **650 / SL ×1,20** (juara Blok 3), diadu ulang di setelan autolot yang
SAMA PERSIS kayak patokan — bukan dibandingin apel-ke-jeruk sama angka lot 0,02.

### Kalau BANDING udah jalan, yang harus lu lakuin

1. **Baca BLOK 1 duluan.** Gagal = angka lain jangan dibaca sama sekali.
2. Baca vonis per takaran lot per jendela: `PENANTANG MENANG` / `EA LIVE MASIH MENANG`.
3. Cek jeblok lawan jatah Alif **20%** (toleransi 30%).
4. Catat hasilnya ke `12_Forensik_Audit\`.

---

## 7.7 Rencana lain yang MASIH LAYAK

**Geser taruhan ke hari yang bayar.** Hari 100–299 pip = **74% kalender**, netto
cuma **+$747 (5,7% cuan)** tapi rugi kotornya **$1.442** — itu yang bikin ayunan
ekuitas. Kecilin taruhan di situ → cuan nyaris gak kesentuh, **jeblok turun** →
jatah jeblok yang kebebasin dipakai **naikin lot global**.

Tombolnya **udah ada**: `Exp_WaitMult` (sekarang 1,0) dan `Exp_HarvestMult` (2,0).
Sapu `Exp_WaitMult` {1,0 · 0,8 · 0,6 · 0,4} × `Exp_HarvestMult` {2,0 · 3,0 · 4,0}
= 12 kombinasi. Gak ada kode EA baru.

---

# BAGIAN 8 — PETA FILE

| isi | lokasi |
|---|---|
| **File ini** | `C:\scalping\BACA_DULU_SEBELUM_NGAPA-NGAPAIN.md` |
| forensik terbaru & terpenting | `12_Forensik_Audit\2026-08-05_KENAPA-RUGI-DAN-KENAPA-TRADE-SEDIKIT.md` |
| forensik invalid-expiration | `12_Forensik_Audit\2026-08-05_INVALID-EXPIRATION-ADA-DI-TESTER-JUGA.md` |
| forensik rezim mati di tester | `12_Forensik_Audit\2026-08-05_SARINGAN-REZIM-MATI-DI-TESTER-PENDEK.md` |
| pelajaran & evaluasi cara kerja | `12_Forensik_Audit\PELAJARAN_3AGT2026.md`, `EVALUASI_CARA_KERJA_2AGT2026.md` |
| rencana adaptasi (blok 1–2, udah dijalanin) | `11_Rencana_Usulan\RENCANA_ADAPTASI_PASAR_SEPI_5AGT2026.md` |
| masterfile lama (panjang) | `MASTER_STRADDLE.md`, `04_Documentation\PROJECT_JOURNEY_FULL_REFERENCE.md` |
| indeks folder | `00_INDEX.md` |
| EA | `01_EAs\` · live: `15_LIVE_OLD\` |
| script uji | `AUTOTEST\` (`SEPI.ps1`, `ADAPB.ps1`, `NEWS.ps1` = paling mutakhir) |
| hasil angka | `AUTOTEST\SEPI\SEPI_HASIL.csv`, `AUTOTEST\ADAPB\ADAPB_HASIL.csv` |
| data harian EA | `%APPDATA%\MetaQuotes\Terminal\Common\Files\KELT_HARIAN.csv` |
| pemeriksa | `02_Tools\cek_ps1.py`, `02_Tools\cek_mq5.py` |

---

# BAGIAN 9 — RINGKASAN 10 BARIS (kalau cuma sempet baca ini)

1. Ngomong bahasa Indonesia santai, **tiap istilah teknis WAJIB dijelasin**.
2. Alif habis rugi besar sebagian duit pinjaman. **Jangan kasih harapan palsu.**
3. **Panggil skill `mt5-riset-bot-anti-bohong`** sebelum ngoprek apa pun.
4. **`15_LIVE_OLD\Straddle_Trio_LIVE_OLD.mq5` JANGAN DISENTUH.** Riset di `KELT_OPT` v3.40.
5. **17 hari dari 341 = 90% cuan.** 3 bulan dari 16 = 90% cuan. Ini strategi
   nungguin ledakan, bukan dagang harian.
6. **Target "1 lot/hari merata" bertentangan sama sumber cuannya.** Jangan dijanjiin.
7. **Yang udah MATI:** gedein lot · buka paksa hari sepi · kecilin jarak jebakan ·
   pengaman diikat spread · `Expiry_Mandiri` · straddle polos.
8. **Optimizer buat memetakan, backtest tunggal buat finalis.** Cari DATARAN, bukan puncak.
9. **Pasang palang yang TERIAK.** Kesalahan di sini semuanya "gagal senyap":
   nol error, angka tetap keluar, cuma salah.
10. **MODE FADE BERHASIL** (temuan positif pertama setelah banyak gagal):
    fade **650 pip / SL ×1,20** di hari sepi = **+$111** vs "mesin dimatiin"
    **+$16**, dan **5 bulan mati jadi 0**. Jarak sempit (≤350) = **AKUN LUDES**,
    bukan sekadar rugi. **Tugas berikutnya: Alif KLIK
    `AUTOTEST\KLIK_BANDING.bat`** — adu lawan EA live (file OLD, HARAM diedit)
    di setelan autolot ON rem 2,0. Detail di Bagian 7.6b.

---

**Terakhir diperbarui: 5 Agustus 2026, akhir sesi (kena limit).**
Kalau ada yang gak jelas di file ini, **tanya Alif — jangan nebak.**
