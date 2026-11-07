[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-5](WEB-5.md) · Berikutnya: [WEB-7](WEB-7.md)

# WEB-6 · Kerangka Dashboard Usaha

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-web6-kerangka-usaha` |
| Saran urutan | [WEB-5](WEB-5.md) selesai, menerima H27. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [WEB-7](WEB-7.md) |

## Tujuan

Pengguna baru memilih tujuan di orientasi dan hanya melihat menu yang ia pilih. Pengguna menemukan Dashboard usaha dari Beranda dan menu, menambahkannya di tengah jalan, membuat usaha lewat form terpandu, berganti usaha bila punya add-on, dan mencatat uang serta stok dari ponsel maupun laptop.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-28 | Menu Usaha, kartu Beranda, kerangka `/usaha`, `BusinessSwitcher`, panel add-on | E2E: usaha kedua tanpa add-on menampilkan panel add-on | 🔒 [web doc 03, Usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#usaha) |
| FE-39 | Orientasi `/mulai`, aturan area di cangkang, `routes.home(me)`, Fitur yang dipakai, baris "Tambahkan ke menu" | E2E: pilih dokumen tanpa menu Usaha; pilih usaha mendarat di `/usaha` | 🔒 [web doc 03, Orientasi](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#orientasi-mulai) |
| FE-40 | Form terpandu Buat usaha empat layar, dipakai orientasi dan aktivasi area | E2E: dari ajakan Beranda sampai barang pertama | 🔒 [web doc 03, Buat usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#buat-usaha-usahabaru) |
| FE-30 | Transaksi dengan `MoneyInput` | E2E: modal dan penjualan langsung tercatat, angka Ringkasan API sama dengan U08 | 🔒 [web doc 06](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md) |
| FE-31 | Stok, kartu stok, catat produksi, penyesuaian, `QuantityInput` | Vitest `formatQuantity`; `stock_negative` tampil di field | 🔒 [web doc 05, komponen usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md#komponen-dashboard-usaha) |
| FE-32 | Pengaturan usaha dan pita baca saja | Tombol menulis tidak dirender di usaha `baca_saja` | 🔒 [web doc 03, Pengaturan usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#pengaturan-usaha-usahapengaturan) |

## Rincian pekerjaan

### FE-28 · Kerangka area usaha

Masalah sekarang: `NAV_ITEMS` di `AppShell.tsx` hanya berisi Beranda, Dokumen, dan Template. Tidak ada pemilih selain menu akun.

Langkah:

1. Tambah `routes.usaha.*` di `src/lib/routes.ts` dan path `/usaha` ke `matcher` di `src/proxy.ts` (perubahan `proxy.ts` ditanyakan dulu sesuai `CLAUDE.md`).
2. `(workspace)/usaha/layout.tsx` dengan `BusinessShell`: pemilih, tab, pita baca saja.
3. Ganti usaha: `POST /businesses/{id}/switch`, lalu hapus query berawalan `['usaha']` saja.
4. Kartu Dashboard usaha di Beranda.
5. Panel add-on untuk `business_limit_reached`.

Bukti selesai: E2E di 360 px dan desktop, dan tangkapan layar di PR.

Hindari: memakai `queryClient.clear()` saat ganti usaha, karena itu ikut membuang cache dokumen.

### FE-39 · Orientasi dan area fitur

Masalah sekarang: `NAV_ITEMS` statis, dan setelah daftar pengguna selalu dibawa ke `/beranda`. Grup `(onboarding)` belum ada di kode.

Langkah:

1. Grup `(app)/(onboarding)` dengan `mulai/page.tsx`. `(workspace)/layout.tsx` mengalihkan sekali ke `/mulai` bila `onboarded_at` kosong.
2. `routes.home(me)` dan tes Vitest untuk setiap kombinasi `areas`. Ganti semua tujuan tetap `/beranda` (logo, setelah masuk, `safeNextPath`).
3. `NAV_ITEMS` disaring dari `areas`. Area tidak aktif tetap bisa dibuka lewat tautan, dengan `InlineNotice` "Tambahkan ke menu".
4. Layar `/profil/fitur` dengan dua sakelar dan "Tampilkan lagi panduan".

Bukti selesai: E2E orientasi untuk pilihan dokumen, usaha, dan lewati, di 360 px dan dengan keyboard saja.

Hindari: menolak atau mengalihkan diam-diam layar dari area yang tidak aktif.

### FE-40 · Form terpandu Buat usaha

Masalah sekarang: rancangan sebelumnya hanya form satu layar. Pengguna baru tidak tahu harus mulai dari barang, mitra, atau impor.

Langkah:

1. `BusinessSetup` empat layar dengan `FormSteps`, `ChoiceCard`, dan `FieldHint`, sesuai tabel layar di web doc 03.
2. Usaha disimpan di layar 1, dan `sales_channels` di layar 2.
3. Layar 4 mengarah ke Impor, lembar kerja dengan Rapikan teks, atau form barang pertama.
4. Komponen yang sama dipakai di `/mulai` dan `/usaha/baru`.

Bukti selesai: E2E dari ajakan Beranda sampai Ringkasan dengan Langkah mulai, dan tangkapan layar setiap layar di 360 px.

Hindari: menanyakan rekening, logo, mitra, atau jadwal di form ini.

### FE-30 · Transaksi

Masalah sekarang: tidak ada input uang. Format rupiah di `src/lib/format.ts` belum menangani masukan "Rp320.000".

Langkah:

1. `MoneyInput` dan tes Vitest untuk "Rp320.000", "320.000", dan "320000".
2. Form dengan react-hook-form dan zod. Error 422 lewat `applyApiErrors()`.
3. Nominal untuk penjualan dengan barang ditampilkan dari respons API, tidak dihitung di klien.

Bukti selesai: Vitest dan E2E kasus U08.

### FE-31 · Stok

Langkah:

1. `formatQuantity` untuk string `"2.500"` menjadi "2,5", tanpa `Number()`.
2. Daftar barang dengan `Badge` Menipis, kartu stok, form produksi dua bagian.

Bukti selesai: Vitest `formatQuantity`, dan E2E catat produksi.

### FE-32 · Pengaturan usaha

Langkah:

1. Form profil, logo (2 MB), rekening, zona waktu, pola nomor, kategori uang.
2. Pilihan usaha yang tetap aktif saat add-on berakhir.
3. Hapus usaha dengan `ConfirmDialog` dan konfirmasi kata sandi.

Bukti selesai: tangkapan layar 360 px dan desktop.

## Urutan kerja yang disarankan

1. FE-28, FE-32, dan FE-39 dengan kontrak manual dari proposal, sebelum H27.
2. FE-40 setelah FE-39, karena orientasi memakai form yang sama.
3. Setelah H27: `npm run api:sync && npm run api:types`, kosongkan kontrak manual, lalu FE-30 dan FE-31.

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H27 · CORE-6 → WEB-6 · openapi usaha, uang, stok**](../serah-terima/H27.md) | Octaf (BE 1), [CORE-6](CORE-6.md) | `openapi.json` dengan endpoint BE-19 sampai BE-21 dan BE-31 | FE-28 sampai FE-32, FE-39, FE-40 tanpa tipe tulisan tangan |

## Serah terima keluar

Tidak ada. Hasil WEB-6 diserahkan ke QA bersama WEB-7 lewat H34.

## Selesai bila

- [ ] `npm run api:check && npm run typecheck && npm run lint && npm run format:check && npm test && npm run build` lulus
- [ ] E2E FE-28, FE-30, FE-39, dan FE-40 lulus di desktop dan ponsel
- [ ] web doc 03 dan 06 diperbarui

## Dokumen rujukan lain

- 🔒 [web doc 04, angka usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#waktu-dan-angka)
- 🔒 [web doc 01, Dashboard usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/01-konteks-produk.md#dashboard-usaha)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
