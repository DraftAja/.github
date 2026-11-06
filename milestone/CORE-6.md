[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-5](CORE-5.md) · Berikutnya: [CORE-7](CORE-7.md)

# CORE-6 · Usaha, uang, dan stok

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core6-usaha-dan-stok` |
| Mulai setelah | [CORE-5](CORE-5.md) selesai, menerima H23 dan H25 |
| Lanjut ke | [CORE-7](CORE-7.md) |

## Tujuan

Pengguna baru memilih tujuan pemakaian dan hanya melihat menu yang ia butuhkan. Pengguna bisa membuat usaha, berganti usaha bila punya add-on, mencatat modal, pemasukan, pengeluaran, dan stok dengan batas minimum. Semua angka cocok dengan kasus uji doc 21 di SQLite dan PostgreSQL 16.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-19 | `businesses`, kolom usaha di `users`, `BusinessPolicy`, `business.writable`, `PlanLimits::maxBusinesses()`, `BusinessPlanService`, switch, perintah aktivasi add-on | Usaha kedua tanpa add-on 403 `business_limit_reached`; usaha orang lain 404; usaha `baca_saja` 403 `business_read_only` | 🔒 [doc 18, Backlog Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-fase-u) |
| BE-20 | `cash_categories`, `cash_transactions`, endpoint uang | Tes kasus U08 dan U09 | 🔒 [KF-USH-01, KF-USH-02](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#usaha-ush) |
| BE-31 | Kolom orientasi di `users`, `sales_channels` di `businesses`, `PATCH /me/preferences`, `POST /me/onboarding/complete`, `GET /getting-started` | Akun lama tidak melihat orientasi; `areas` kosong 422; status Langkah mulai sesuai data; area tidak membatasi endpoint | 🔒 [doc 22](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/22-orientasi-dan-panduan-fitur.md) |
| BE-21 | `products`, `stock_movements`, mutasi, produksi, `stock_negative`, penanda menipis, `usaha:stock-check` | Tes kasus U10, U11, dan U17 di PostgreSQL | 🔒 [doc 21, stok](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/21-perhitungan-usaha.md#stok) |

## Rincian pekerjaan

### BE-19 · Usaha dan batas add-on

Masalah sekarang: tidak ada konsep usaha di kode. `users.instansi` hanya teks bebas, dan `PlanLimits` belum ada.

Langkah:

1. Proposal kontrak untuk `/businesses`, `/businesses/{id}`, dan `/businesses/{id}/switch` dengan template doc 17.
2. Migrasi langkah 1 di urutan migrasi Fase U doc 04. Setiap FK diberi `->index()`, dan `status` serta kolom add-on tidak masuk `$fillable`.
3. `PlanLimits::maxBusinesses()` membaca angka uji coba dari H23 lewat `config/`, bukan `env()` langsung.
4. `BusinessPolicy` menjawab 404 untuk usaha milik orang lain. Route model binding bersarang memastikan anak (mitra, barang) milik usaha di path.
5. Middleware `business.writable` untuk semua aksi menulis.
6. Perintah artisan untuk admin: aktifkan dan akhiri add-on untuk satu akun.
7. `composer api:spec`.

Bukti selesai: tes batas, isolasi (termasuk usaha lain milik pengguna yang sama), baca saja, dan hapus akun menghapus usaha.

Hindari: memakai `workspace_id` atau tabel `workspaces` untuk usaha (ADR 0007).

### BE-20 · Uang masuk dan keluar

Masalah sekarang: belum ada tabel uang. Rumus penjualan, pengeluaran, selisih, dan saldo kas hanya ada di doc 21.

Langkah:

1. Tulis tes kasus U08 dan U09 lebih dulu (merah).
2. Migrasi `cash_categories` dan `cash_transactions` dengan rupiah `bigint`, `occurred_on` `date`, dan indeks `(business_id, occurred_on)`.
3. Kategori bawaan dibuat saat usaha dibuat.
4. Penjualan langsung yang menyebut barang membuat mutasi `keluar_jual` di transaksi yang sama (menunggu BE-21, atau kerjakan BE-21 lebih dulu).
5. `pelunasan_invoice` ditolak bila dikirim lewat API.

Bukti selesai: U08 dan U09 hijau di SQLite dan PostgreSQL.

Hindari: kolom `decimal` atau `float` untuk rupiah.

### BE-21 · Barang, mutasi, dan batas minimum

Masalah sekarang: belum ada stok. Stok menipis adalah dasar pengingat KF-USH-12 milik AI-7.

Langkah:

1. Tulis tes U10, U11, dan U17 lebih dulu.
2. Migrasi `products` dan `stock_movements` dengan jumlah dalam milli.
3. Satu service mutasi yang mengunci baris barang (`lockForUpdate()`), memeriksa stok tidak negatif, memperbarui `stock_milli` dan `is_low`, dan mengembalikan penanda "baru menipis" agar AI-7 bisa membuat notifikasi.
4. Endpoint "catat produksi" membuat dua mutasi dalam satu transaksi.
5. `usaha:stock-check` membandingkan `stock_milli` dengan jumlah mutasi tanpa mengubah data.

Bukti selesai: U10 menghasilkan tepat dua penanda baru menipis; U17 lulus di PostgreSQL.

Hindari: mengizinkan stok diedit langsung lewat `PATCH /products/{id}`.

### BE-31 · Orientasi dan area fitur

Masalah sekarang: setelah daftar, semua pengguna langsung masuk Beranda dokumen. Tidak ada tempat untuk menyimpan tujuan pemakaian, sehingga pemilik usaha harus mencari menu Usaha sendiri dan sekretaris melihat menu yang tidak ia pakai.

Langkah:

1. Proposal kontrak untuk field baru di `GET /me`, `PATCH /me/preferences`, `POST /me/onboarding/complete`, dan `GET /getting-started`.
2. Migrasi kolom `areas`, `onboarded_at`, `preferred_categories`, `dismissed_hints` di `users`. Migrasi yang sama mengisi akun lama dengan `areas = ["dokumen"]` dan `onboarded_at` waktu migrasi.
3. Kolom `sales_channels` dan `first_recap_exported_at` masuk migrasi `businesses` (BE-19), selama migrasi itu belum merge.
4. `GET /getting-started` menghitung `done` dari data, satu method per langkah, mengikuti tabel Langkah mulai di doc 22.
5. Pastikan tidak ada middleware atau policy yang membaca `areas`. Area hanya tampilan.

Bukti selesai: tes akun lama, validasi `areas`, setiap langkah `done` sebelum dan sesudah datanya dibuat, dan tes bahwa endpoint usaha tetap 200 untuk pengguna yang area usahanya tidak aktif.

Hindari: mengirim path halaman dari API. Web membangun tujuan dari `key` langkah.

## Urutan kerja yang disarankan

1. BE-19 setelah H23, bersama BE-31 karena keduanya mengubah `users` dan `businesses`.
2. BE-21, lalu BE-20, karena penjualan langsung membuat mutasi stok.
3. Kirim [H27](../serah-terima/H27.md) dan [H28](../serah-terima/H28.md) setelah ketiganya merge.

## Serah terima masuk

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H23 · PM-6 → CORE-6 · ADR usaha dan batas add-on**](../serah-terima/H23.md) | Raja (PM), [PM-6](PM-6.md) | ADR 0007 Diterima, angka batas uji coba | BE-19 |
| [**H25 · QA-6 → CORE-6 · kasus uji rumus disetujui**](../serah-terima/H25.md) | Louis (QA), [QA-6](QA-6.md) | Lembar hitung manual U01 sampai U17 yang cocok dengan doc 21 | Tes BE-20, BE-21, dan CORE-7 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H27 · CORE-6 → WEB-6 · openapi usaha, uang, stok**](../serah-terima/H27.md) | Alvira (FE), [WEB-6](WEB-6.md) | `openapi.json` dengan endpoint BE-19 sampai BE-21 dan BE-31 | PR API merge, label `butuh-fe` di issue web |
| [**H28 · CORE-6 → AI-6 · service uang dan stok**](../serah-terima/H28.md) | Hamid (BE 2), [AI-6](AI-6.md) | Service uang dan mutasi yang bisa dipanggil lembar kerja dan impor | PR API merge, issue kesepakatan `be-core` dan `be-ai` |

## Selesai bila

- [ ] Tes U08 sampai U11 dan U17 lulus di SQLite dan PostgreSQL 16
- [ ] `composer api:spec` dijalankan dan `openapi.json` di-commit
- [ ] KF-USH-01, 02, 07, 08, 19 dan KF-ORI-01 sampai 06 berlabel Sudah ada di doc 10, 12, 20, dan 22
- [ ] H27 dan H28 terkirim

## Dokumen rujukan lain

- 🔒 [Doc 04, skema Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md#skema-target-fase-u)
- 🔒 [Doc 05, endpoint Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#endpoint-fase-u-dashboard-usaha)
- 🔒 [Doc 07, berkas dan data usaha](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#berkas-dan-data-usaha)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
