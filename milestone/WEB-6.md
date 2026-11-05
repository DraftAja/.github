[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-5](WEB-5.md) · Berikutnya: [WEB-7](WEB-7.md)

# WEB-6 · Kerangka Dashboard Usaha

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-web6-kerangka-usaha` |
| Mulai setelah | [WEB-5](WEB-5.md) selesai, menerima H27 |
| Lanjut ke | [WEB-7](WEB-7.md) |

## Tujuan

Pengguna menemukan Dashboard usaha dari Beranda dan menu, membuat usaha, berganti usaha bila punya add-on, dan mencatat uang serta stok dari ponsel maupun laptop.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-28 | Menu Usaha, kartu Beranda, kerangka `/usaha`, `BusinessSwitcher`, Buat usaha, panel add-on | E2E: usaha kedua tanpa add-on menampilkan panel add-on | 🔒 [web doc 03, Usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#usaha) |
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

1. FE-28 dan FE-32 dengan kontrak manual dari proposal, sebelum H27.
2. Setelah H27: `npm run api:sync && npm run api:types`, kosongkan kontrak manual, lalu FE-30 dan FE-31.

## Serah terima masuk

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H27 · CORE-6 → WEB-6 · openapi usaha, uang, stok**](../serah-terima/H27.md) | Octaf (BE 1), [CORE-6](CORE-6.md) | `openapi.json` dengan endpoint BE-19 sampai BE-21 | FE-28 sampai FE-32 tanpa tipe tulisan tangan |

## Serah terima keluar

Tidak ada. Hasil WEB-6 diserahkan ke QA bersama WEB-7 lewat H34.

## Selesai bila

- [ ] `npm run api:check && npm run typecheck && npm run lint && npm run format:check && npm test && npm run build` lulus
- [ ] E2E FE-28 dan FE-30 lulus di desktop dan ponsel
- [ ] web doc 03 dan 06 diperbarui

## Dokumen rujukan lain

- 🔒 [web doc 04, angka usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#waktu-dan-angka)
- 🔒 [web doc 01, Dashboard usaha](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/01-konteks-produk.md#dashboard-usaha)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
