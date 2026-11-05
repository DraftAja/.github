[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-6](WEB-6.md)

# WEB-7 · Mitra, invoice, lembar kerja, dan impor

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-web7-lembar-kerja` |
| Mulai setelah | [WEB-6](WEB-6.md) selesai, menerima H29, H31, H32. `@tanstack/react-table` boleh dipasang setelah H24. |
| Lanjut ke | selesai |

## Tujuan

Pemilik usaha mencatat kunjungan mitra dari ponsel, membuat dan mengunduh invoice, mengisi lembar kerja seperti di Excel, mengimpor berkas lama, menempelkan catatan untuk dirapikan AI, dan melihat Ringkasan serta rekap.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-29 | Ringkasan: `StatCard`, `PeriodPicker`, peringatan, grafik batang | Angka sama dengan `GET /summary`; persentase retur `null` ditulis "tidak ada pengiriman" | 🔒 [web doc 03, Ringkasan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#ringkasan-usaha) |
| FE-33 | Mitra dan Catat kunjungan berurutan | E2E kunjungan; `consignment_count_invalid` tampil di field | 🔒 [web doc 03, Mitra](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#mitra-usahamitra-dan-detail-usahamitraid) |
| FE-34 | Invoice dan aksi per status | E2E buat, kirim, unduh PDF; `InvoiceStatusPill` punya cabang bawaan | 🔒 [web doc 03, Invoice](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#invoice-usahainvoice-dan-detail-usahainvoiceid) |
| FE-35 | `DataGrid` dan Lembar kerja | Navigasi keyboard, tempel dari Excel, 360 px tanpa gulir halaman | 🔒 [web doc 05, DataGrid](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md#aturan-datagrid) |
| FE-36 | Wizard Impor dan panel Rapikan teks | E2E impor `USH-04.csv` | 🔒 [web doc 03, Impor](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#impor-usahaimpor) |
| FE-37 | Laporan dan template laporan | Unduh XLSX dari rekap | 🔒 [web doc 06](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md) |
| FE-38 | Pengingat dan notifikasi usaha | Tautan notifikasi usaha lain berganti usaha dulu | 🔒 [web doc 03, Pengingat](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#pengingat-usahapengingat) |

## Rincian pekerjaan

### FE-35 · DataGrid

Masalah sekarang: tidak ada komponen tabel di `src/components/ui/`. Editor dokumen memakai TipTap, yang tidak cocok untuk data baris.

Langkah:

1. Pasang `@tanstack/react-table` setelah H24, dengan versi dipin.
2. `DataGrid` headless: `columns` dan `data` di-memo, `getRowId` dari id API, operasi `manual*`.
3. `ColumnMeta` membawa tipe, sumber, dan perataan. Kolom `sistem` hanya baca.
4. Pola `role="grid"`, panah, Enter, Tab, Esc, Home, End.
5. Tempel dari Excel: parser teks tab dengan tes Vitest, kirim per baris, tandai baris gagal.
6. Virtualisasi baris dengan `@tanstack/react-virtual` hanya bila lembar lebih dari 100 baris terasa lambat dan dependensinya disetujui.

Bukti selesai: Vitest parser, E2E keyboard, dan tangkapan layar 360 px.

Hindari: menghitung total atau sisa di grid. Kolom hitungan selalu dari API.

### FE-33 · Catat kunjungan

Masalah sekarang: pemilik mencatat di lokasi mitra dengan ponsel. Urutan langkah menentukan sisa yang benar.

Langkah:

1. Form tiga langkah (terjual atau sisa terlihat, retur, kirim baru), dikirim berurutan.
2. Di bawah 1024 px, langkah ditampilkan bertahap.
3. Error `consignment_count_invalid` dan `invoice_period_locked` mengikuti teks web doc 04.

Bukti selesai: E2E kasus U01 dari antarmuka, sisa akhir 8.

### FE-36 · Impor dan Rapikan teks

Langkah:

1. Wizard dengan `langkah` di URL. Polling status impor 1.500 ms.
2. `ColumnMapper` bisa dipakai dengan keyboard.
3. Panel Rapikan teks memakai pola usulan yang sama dengan `CleanupPanel`: usulan berdampingan, terima atau buang, `request_key` per permintaan.

Bukti selesai: E2E impor `USH-04.csv` dari folder contoh API. E2E Rapikan teks memakai provider palsu dari H8.

### FE-29, FE-34, FE-37, FE-38

Ikuti spesifikasi layar di web doc 03. Pemeriksaan utamanya: angka tidak dihitung di klien, enum punya cabang bawaan, dan setiap layar menangani keadaan kosong, memuat, dan error.

## Urutan kerja yang disarankan

1. FE-33 dan FE-34 setelah H29.
2. FE-35 setelah H24 dan H31, lalu FE-36.
3. FE-29, FE-37, FE-38 setelah H32.
4. Kirim [H34](../serah-terima/H34.md).

## Serah terima masuk

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H29 · CORE-7 → WEB-7 · openapi mitra dan invoice**](../serah-terima/H29.md) | Octaf (BE 1), [CORE-7](CORE-7.md) | Endpoint BE-22 sampai BE-24 | FE-33, FE-34 |
| [**H31 · AI-6 → WEB-7 · openapi lembar kerja, impor, ekspor**](../serah-terima/H31.md) | Hamid (BE 2), [AI-6](AI-6.md) | Endpoint BE-25 sampai BE-27 | FE-35, FE-36 |
| [**H32 · AI-7 → WEB-7 · openapi ringkasan, teks ke tabel, pengingat**](../serah-terima/H32.md) | Hamid (BE 2), [AI-7](AI-7.md) | Endpoint BE-28 sampai BE-30 | FE-29, FE-36, FE-37, FE-38 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H34 · WEB-7 → QA-7 · Fase U siap uji**](../serah-terima/H34.md) | Louis (QA), [QA-7](QA-7.md) | Pasangan commit API dan web dengan Fase U lengkap, tangkapan layar 360 px dan desktop | Repo gabungan mencatat pasangan commit, daftar periksa serah terima QA |

## Selesai bila

- [ ] Gerbang verifikasi web lulus
- [ ] E2E Fase U di web doc 07 lulus di desktop dan ponsel
- [ ] web doc 03 sampai 07 diperbarui
- [ ] H34 terkirim

## Dokumen rujukan lain

- 🔒 [web doc 04, kode error Fase U](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#kode-yang-akan-datang)
- 🔒 [web doc 07](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
