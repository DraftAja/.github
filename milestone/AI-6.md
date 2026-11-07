[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-5](AI-5.md) · Berikutnya: [AI-7](AI-7.md)

# AI-6 · Lembar kerja, impor, dan ekspor

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-ai-ai6-impor-lembar-kerja` |
| Saran urutan | [AI-5](AI-5.md) selesai, menerima H24 dan H26. BE-25 untuk `USH-01`, `USH-02`, dan `USH-07` memakai service dari H28, sebelum itu service sementara. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [AI-7](AI-7.md) |

## Tujuan

Pengguna yang terbiasa dengan Excel bisa mengisi lembar kerja di situs, mengimpor berkas CSV atau XLSX lamanya, dan mengunduh semua data usaha sebagai berkas yang terbuka benar di Excel.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-25 | `SheetTemplates` untuk 8 template `USH`, `SheetTemplateSpecTest`, `sheets`, `sheet_rows`, templat kosong | Tes spesifikasi lulus; sel lembar bertipe sama dengan endpoint modul; 409 `revision_changed` | 🔒 [templates/09-usaha](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/09-usaha/README.md) |
| BE-26 | PhpSpreadsheet, impor dengan pencocokan kolom, pratinjau, commit, batal, `usaha:imports-prune` | Berkas `contoh/` terimpor bersih; berkas berbahaya ditolak; tidak ada baris tersimpan sebelum commit | 🔒 [doc 06, W14](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#w14-impor-csv-atau-xlsx) |
| BE-27 | `SheetExporter` XLSX dan CSV dengan format angka, BOM, dan escape rumus | Tes ekspor doc 16 lulus; uji manual Excel dan LibreOffice tercatat | 🔒 [tata letak XLSX](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/09-usaha/README.md#tata-letak-xlsx) |

## Rincian pekerjaan

### BE-25 · Template tabel dan lembar kerja

Masalah sekarang: template di kode hanya template dokumen (`TemplateSeeder`). Template tabel `USH` baru ada di spesifikasi Markdown.

Langkah:

1. `SheetTemplateSpecTest` lebih dulu: baca tabel Kolom setiap `docs/templates/09-usaha/USH-*.md` dan bandingkan kunci, label, urutan, tipe, serta sumbernya dengan `SheetTemplates`.
2. Isi `SheetTemplates` sampai tes itu hijau.
3. Migrasi `sheets` dan `sheet_rows`. Lembar bertipe tidak menyimpan baris. Penyuntingan sel memanggil service modul dari H28 (dan dari CORE-7 untuk `USH-03` sampai `USH-05`).
4. Lembar bebas: maksimal 30 kolom dan 5.000 baris, `revision` per baris untuk 409.
5. Templat kosong XLSX dan CSV per `USH` lewat `SheetExporter` (BE-27).

Bukti selesai: tes spesifikasi, tes sel lembar `USH-01` membuat catatan uang yang sama dengan `POST /cash-transactions`, dan tes 409.

Hindari: menyalin aturan validasi modul ke lembar kerja. Selalu panggil service modulnya.

### BE-26 · Impor CSV dan XLSX

Masalah sekarang: pengguna punya bertahun-tahun data di Excel buatan sendiri dengan header yang berbeda-beda.

Langkah:

1. Tambah `phpoffice/phpspreadsheet` setelah H24, dengan versi dipin di PR.
2. Tulis tes keamanan lebih dulu, dengan contoh berkas yang dibuat di tes: berkas rusak, `.xlsm`, berkas terenkripsi, di atas 5 MB, lebih dari 5.000 baris, ZIP bom, dan sel rumus.
3. Migrasi `business_imports`, `business_import_rows`, dan `import_mappings`. `request_key` mengikuti aturan idempotensi doc 05.
4. Job `ParseBusinessImport`: baca nilai rumus terakhir, jangan pernah menghitung rumus, deteksi pemisah, tebak pencocokan dari header dan nama lain di spesifikasi.
5. `PATCH /imports/{id}` membaca ulang baris saat pencocokan atau pemisah desimal berubah.
6. `commit` dalam satu transaksi, urut tanggal, memakai service modul.
7. `usaha:imports-prune` harian.

Bukti selesai: semua berkas di `docs/templates/09-usaha/contoh/` terimpor tanpa baris bermasalah, dan semua tes keamanan hijau.

Hindari: membaca berkas di dalam permintaan HTTP, dan membuat mitra atau barang baru diam-diam dari nama yang tidak dikenal.

### BE-27 · Ekspor XLSX dan CSV

Masalah sekarang: ekspor yang ada hanya DOCX dan PDF.

Langkah:

1. `SheetExporter` mengikuti tabel tata letak XLSX: tanggal Excel, `"Rp"#,##0`, header dibekukan, sheet `Keterangan`.
2. Escape teks berawalan `=`, `+`, `-`, `@`, tab, dan carriage return.
3. CSV: UTF-8 BOM, titik koma, tanggal ISO, jumlah dengan koma desimal.
4. Tes membaca ulang berkas dengan PhpSpreadsheet dan memeriksa tipe sel.
5. Uji manual di Microsoft Excel dan LibreOffice, catat di doc 10.

Bukti selesai: tes ekspor doc 16 hijau, dan catatan uji manual ada di doc 10.

Hindari: menulis angka sebagai teks berformat ("Rp320.000") di sel.

## Urutan kerja yang disarankan

1. `SheetTemplateSpecTest` dan `SheetTemplates` (tidak butuh H28).
2. BE-27, karena BE-25 (templat kosong) dan BE-24 milik Octaf memakainya.
3. BE-25 lembar kerja setelah H28.
4. BE-26.
5. Kirim [H31](../serah-terima/H31.md).

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H24 · PM-6 → AI-6 · ADR 0008 dan dependensi disetujui**](../serah-terima/H24.md) | Raja (PM), [PM-6](PM-6.md) | Izin PhpSpreadsheet | BE-26, BE-27 |
| [**H26 · QA-6 → AI-6 · kasus uji impor dan ekspor disetujui**](../serah-terima/H26.md) | Louis (QA), [QA-6](QA-6.md) | Daftar berkas uji dan hasil yang diharapkan | Tes BE-26 dan BE-27 |
| [**H28 · CORE-6 → AI-6 · service uang dan stok**](../serah-terima/H28.md) | Octaf (BE 1), [CORE-6](CORE-6.md) | Service uang dan mutasi | Lembar bertipe dan commit impor |

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H31 · AI-6 → WEB-7 · openapi lembar kerja, impor, ekspor**](../serah-terima/H31.md) | Alvira (FE), [WEB-7](WEB-7.md) | `openapi.json` dengan endpoint BE-25 sampai BE-27 | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] `SheetTemplateSpecTest` dan semua tes impor dan ekspor lulus
- [ ] Uji manual Excel dan LibreOffice tercatat di doc 10
- [ ] `composer api:spec` dijalankan
- [ ] KF-USH-11, 15, 17, 18 berlabel Sudah ada di doc 10, 12, dan 20
- [ ] H31 dilaporkan di issue dan reviewer di-mention

## Dokumen rujukan lain

- 🔒 [ADR 0008](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/adr/0008-pustaka-lembar-kerja.md)
- 🔒 [Doc 07, berkas dan data usaha](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#berkas-dan-data-usaha)
- 🔒 [Doc 16, Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#fase-u-dashboard-usaha)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
