[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-6](CORE-6.md)

# CORE-7 · Mitra, titip jual, dan invoice

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core7-invoice-mitra` |
| Mulai setelah | [CORE-6](CORE-6.md) selesai |
| Lanjut ke | selesai |

## Tujuan

Pemilik usaha mencatat kiriman, penjualan, dan retur di setiap mitra, lalu mendapat invoice per mitra per periode yang angkanya sama dengan doc 21, bernomor unik, dan bisa diunduh sebagai PDF dan XLSX.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-22 | Mitra, harga mitra, catatan kirim, terjual, sisa terlihat, retur, saldo per mitra | Tes kasus U01, U06, U07, U15 | 🔒 [doc 21, sisa di mitra](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/21-perhitungan-usaha.md#sisa-di-mitra) |
| BE-23 | Invoice: pratinjau, kirim dengan nomor, lunas, batalkan pelunasan, batalkan, kunci periode | Tes U01 sampai U05, U13, U15; 20 `send` bersamaan di PostgreSQL tanpa nomor ganda | 🔒 [doc 21, invoice](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/21-perhitungan-usaha.md#invoice) |
| BE-24 | PDF invoice lewat Browsershot dan XLSX lewat `SheetExporter` | PDF memuat nomor, total, dan terbilang yang benar; XLSX sesuai tata letak | 🔒 [USH-06](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/09-usaha/USH-06-invoice-mitra.md) |

## Rincian pekerjaan

### BE-22 · Mitra dan catatan titip jual

Masalah sekarang: pemilik menghitung sisa di setiap lokasi secara manual. Urutan hitung, retur, lalu kirim dalam satu kunjungan menentukan sisa yang benar.

Langkah:

1. Tulis tes U01, U06, U07, dan U15 lebih dulu.
2. Migrasi `business_partners`, `partner_prices`, dan `consignment_entries` (satu tabel, kolom `step` untuk urutan).
3. Pencatatan `kirim` memanggil service mutasi CORE-6 (`keluar_titip`) dalam transaksi yang sama. Retur layak memanggil `retur_layak`.
4. `consignment_count_invalid` untuk sisa terlihat yang melebihi sisa tercatat dan untuk retur yang melebihi sisa.
5. `GET /partners/{id}/balance?period=` dihitung dari catatan.

Bukti selesai: tes U01, U06, U07, dan U15 hijau; urutan permintaan yang terbalik dalam satu tanggal tetap menghasilkan sisa yang sama.

Hindari: menyimpan sisa yang diketik pengguna sebagai sumber kebenaran. Sisa selalu dihitung.

### BE-23 · Invoice

Masalah sekarang: tagihan disusun manual per lokasi selama 30 sampai 60 menit.

Langkah:

1. Tes U01 sampai U05, U13, dan U15 lebih dulu, termasuk pembulatan komisi per baris.
2. Migrasi `partner_invoices`, `partner_invoice_lines`, dan `invoice_sequences`.
3. Rumus satu sumber di service `BusinessFigures`, yang juga akan dipakai AI-7 untuk rekap (bicarakan nama dan letaknya dengan Hamid di H30).
4. `send` mengunci `invoice_sequences` dengan `lockForUpdate()`, memberi nomor dari pola usaha, dan menyalin baris.
5. Kunci periode berlaku untuk `terkirim` dan `lunas`. `pay` membuat uang masuk `pelunasan_invoice` lewat service CORE-6.
6. `invoice_empty` untuk periode tanpa penjualan.

Bukti selesai: tes kasus hijau, tes konkurensi nomor di PostgreSQL, dan tes yang memastikan baris invoice terkirim tidak berubah setelah harga mitra diubah.

Hindari: menghitung ulang invoice terkirim dari catatan saat dibuka.

### BE-24 · Berkas invoice

Masalah sekarang: belum ada view invoice. Ekspor dokumen memakai Browsershot, dan XLSX bergantung pada `SheetExporter` dari AI-6 (BE-27).

Langkah:

1. View Blade `usaha/invoice.blade.php` dengan tata letak di spesifikasi `USH-06`, termasuk tanda air DRAF dan BATAL.
2. `Terbilang::rupiah()` dengan tes untuk 0, ribuan, jutaan, dan angka yang mengandung "sebelas" dan "seratus".
3. XLSX lewat `SheetExporter` setelah BE-27 merge.

Bukti selesai: PDF U01 memuat "tiga ratus dua puluh ribu rupiah"; XLSX dua sheet lulus tes tata letak.

## Urutan kerja yang disarankan

1. BE-22.
2. BE-23, lalu kirim [H29](../serah-terima/H29.md) dan [H30](../serah-terima/H30.md).
3. BE-24, PDF lebih dulu, XLSX setelah BE-27 dari AI-6.

## Serah terima masuk

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H25 · QA-6 → CORE-6 · kasus uji rumus disetujui**](../serah-terima/H25.md) | Louis (QA), [QA-6](QA-6.md) | Lembar hitung manual U01 sampai U17 | Tes BE-22 dan BE-23. Diterima saat CORE-6. |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H29 · CORE-7 → WEB-7 · openapi mitra dan invoice**](../serah-terima/H29.md) | Alvira (FE), [WEB-7](WEB-7.md) | `openapi.json` dengan endpoint BE-22 dan BE-23, serta unduh BE-24 | PR API merge, label `butuh-fe` di issue web |
| [**H30 · CORE-7 → AI-7 · data invoice untuk rekap dan pengingat**](../serah-terima/H30.md) | Hamid (BE 2), [AI-7](AI-7.md) | Service `BusinessFigures` dan status invoice | PR API merge, issue kesepakatan `be-core` dan `be-ai` |

## Selesai bila

- [ ] Tes U01 sampai U07, U13, U15 lulus di SQLite dan PostgreSQL 16
- [ ] Tes konkurensi nomor invoice lulus di PostgreSQL 16
- [ ] KF-USH-03 sampai 06 berlabel Sudah ada di doc 10, 12, dan 20
- [ ] H29 dan H30 terkirim

## Dokumen rujukan lain

- 🔒 [Doc 06, W13 dan status invoice](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#w13-siklus-titip-jual-sampai-invoice)
- 🔒 [USH-03, USH-04, USH-05](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/09-usaha/README.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
