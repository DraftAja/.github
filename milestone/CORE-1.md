[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [CORE-2](CORE-2.md)

# CORE-1 · Keamanan fase 0

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `fix/be-core-core1-policy-ekspor` |
| Mulai setelah | Repo dipisah, 3 November 2026 |
| Lanjut ke | [CORE-2](CORE-2.md) |

## Tujuan

Menutup celah keamanan dan kebenaran perilaku yang menjadi dasar semua fitur berikutnya, sebelum fitur lain dibangun di atasnya. Keempat butir utama bisa dikerjakan paralel dan tidak mengubah bentuk kontrak API kecuali BE-09, yang mengubah cara URL gambar dilayani.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-01 | Tambah `LaporanPolicy::export`: pemilik boleh mengekspor semua status. PDF draf diberi tanda "Draf". Hapus atau ganti `canExportPdf()`. | Tes fitur: ekspor PDF dan DOCX dokumen `final` milik sendiri 200; milik orang lain 403 | 🔒 [S-01](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-02 | Validasi `templates.use` dengan FormRequest yang sama dengan `templates.document`. Judul bawaan = nama template tanpa awalan, tanpa `report_type` akademik. | Tes: `judul` 300 karakter ditolak 422; tanpa body menghasilkan dokumen berjudul nama template | 🔒 [S-02](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-09 | Gambar editor dan logo pindah ke disk privat, disajikan lewat rute yang memeriksa kepemilikan dokumen. URL tetap relatif. | Tes: tamu 401, pengguna lain 403, berkas bukan gambar dan SVG ditolak 422, ekspor PDF tetap memuat gambar | 🔒 [S-13](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#keamanan-dan-data) |
| BE-17 | Bersihkan `content` bagian dengan HTMLPurifier (konfigurasi sama dengan `CleanupOutputRestorer`) dan kirim `Content-Security-Policy: script-src 'none'` pada pratinjau. | Tes: `<script>` dan atribut `onerror` hilang saat disimpan; pratinjau memuat header CSP; format TipTap tetap utuh | 🔒 [S-21](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#keamanan-dan-data) |
| R6 | `UserSeeder` hanya berjalan di lokal dan tes. | Tes: seeder tidak membuat akun uji saat `APP_ENV=production` | 🔒 [doc 16, tes fase 0](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |

## Rincian pekerjaan

### BE-01 · Ekspor dokumen final

Masalah sekarang: `LaporanController::downloadPdf` dan `downloadDocx` memeriksa izin `update`. `LaporanPolicy::update` menolak dokumen berstatus `final`, jadi dokumen final akan mendapat 403 saat diunduh. Masalah ini belum terasa karena belum ada endpoint untuk memfinalkan dokumen, tetapi langsung muncul begitu BE-05 di [CORE-3](CORE-3.md) masuk. Selain itu, `LaporanStatus::canExportPdf()` hanya mengizinkan status `final`, bertentangan dengan doc 06 yang mengizinkan draf diekspor dengan tanda "Draf". Method itu tidak dipanggil di mana pun.

Langkah:

1. Tambah method `export(User $user, Laporan $laporan): bool` di `LaporanPolicy` yang hanya memeriksa kepemilikan, tanpa memeriksa status.
2. Ganti `authorize('update', ...)` menjadi `authorize('export', ...)` di kedua method ekspor.
3. Di `resources/views/reports/preview.blade.php`, tampilkan tanda "Draf" untuk dokumen berstatus selain `final`. Tanda ini ikut ke PDF karena PDF dirender dari view yang sama. DOCX tidak diberi tanda.
4. Hapus `canExportPdf()` dari `LaporanStatus` dan `Laporan`, atau ubah artinya sesuai doc 06. Pilih menghapus bila tidak ada pemanggil.

Bukti selesai: tes fitur untuk empat kombinasi, yaitu draf dan final milik sendiri (200, PDF dan DOCX) serta milik orang lain (403). Karena belum ada endpoint final, tes membuat dokumen final dengan `markAsFinal()`.

Hindari: memakai `view` sebagai pengganti `update`. Arti keduanya akan berbeda begitu ruang kerja masuk (peninjau boleh melihat dokumen orang lain, tetapi aturan ekspor belum diputuskan).

### BE-02 · Validasi buka template bawaan

Masalah sekarang: `TemplateController::useTemplate` membaca `judul`, `report_type`, dan `instansi` lewat `$request->input()` tanpa validasi. Panjang tidak dibatasi, `report_type` bisa berisi nilai di luar daftar, dan nilai bawaannya akademik: `report_type` menjadi `Makalah` dan judul diawali "Laporan: ".

Langkah:

1. Pindahkan aturan validasi `templates.document` (`request_key` UUID, `judul` maksimal 255, `report_type` maksimal 100, `instansi` maksimal 255) ke satu FormRequest, misalnya `OpenTemplateDocumentRequest`, lalu pakai di kedua method. `request_key` tetap opsional di `use`.
2. Judul bawaan menjadi nama template tanpa awalan.
3. `report_type` bawaan tidak lagi `Makalah`. Sampai kategori 8 kunci masuk di CORE-3, pakai satu nilai yang disepakati Raja di [PM-2](PM-2.md), atau `Proposal` sebagai nilai teknis sementara yang paling netral di daftar lama.
4. Jalankan `composer api:spec`, karena FormRequest baru menambah skema request di `openapi.json`.

Bukti selesai: tes `judul` 300 karakter ditolak 422 dengan error di field `judul`; tes tanpa body menghasilkan dokumen berjudul persis nama template.

### BE-09 · Gambar editor dan logo privat

Masalah sekarang: gambar yang disisipkan di editor dan logo sampul disimpan di disk `public`, lalu dilayani lewat `/storage/*`. Siapa pun yang mengetahui URL-nya bisa membuka gambar tanpa masuk. Dokumen kantor bisa memuat tanda tangan, foto kegiatan, atau data siswa. Validasi tipe dan ukuran sudah ditambahkan saat split, tetapi belum punya tes.

Langkah:

1. Simpan unggahan baru di disk `local` (privat) dan catat di `uploaded_files` dengan `purpose` `editor_image` atau `logo`, beserta `laporan_id` atau pemiliknya.
2. Buat rute GET baru di `/api/v1`, misalnya `/api/v1/files/{uploadedFile}`, yang memeriksa session dan kepemilikan, lalu mengalirkan berkas dengan `Content-Type` dari data tersimpan dan `X-Content-Type-Options: nosniff`.
3. URL yang dikirim ke web tetap relatif, sekarang berupa path API tersebut. Web sudah meneruskan `/api/v1/*` lewat proxy, jadi tidak perlu perubahan proxy.
4. Ubah `ReportExportImages` agar mengenali URL baru sebagai berkas lokal saat membuat PDF dan DOCX.
5. Tulis migration data yang memindahkan berkas lama dari `public` ke `local` dan memperbarui `content` bagian yang memuat URL lama. Bila terlalu berisiko, pertahankan URL lama untuk dokumen lama dan catat keputusannya di doc 18.
6. Jalankan `composer api:spec`, karena ada rute baru.

Bukti selesai: tes tamu 401, pengguna lain 403, pemilik 200; berkas bukan gambar dan SVG ditolak 422 saat unggah; tes ekspor PDF dengan gambar tetap memuat gambar.

Hindari: URL bertanda tangan yang berlaku lama. URL itu tetap bisa dibagikan, sehingga tidak menutup masalahnya.

### BE-17 · HTML isi bagian dibersihkan

Masalah sekarang: isi bagian dokumen disimpan sebagai HTML apa adanya, lalu dirender tanpa escape di pratinjau dan PDF. HTMLPurifier sudah dipakai, tetapi hanya untuk hasil perapihan AI (`CleanupOutputRestorer`). Pratinjau disajikan dari origin web yang sama dengan cookie session. Dampaknya terbatas selama hanya pemilik yang membuka pratinjau dokumennya sendiri, tetapi wajib ditutup sebelum peninjau ruang kerja (fase 4) membuka dokumen orang lain.

Langkah:

1. Ekstrak konfigurasi HTMLPurifier dari `CleanupOutputRestorer` ke satu kelas yang dipakai bersama, supaya daftar tag dan atribut yang diizinkan sama di kedua jalur.
2. Pastikan daftar itu mencakup semua keluaran TipTap yang dipakai editor: paragraf, heading, daftar, kutipan, tabel, gambar dengan URL relatif, tebal, miring, perataan teks, dan jenis huruf.
3. Bersihkan `content` di `ReportSectionController::update` dan `store`, serta di jalur impor DOCX yang menulis isi bagian.
4. Tambahkan header `Content-Security-Policy` pada respons pratinjau (`laporan.preview` dan `laporan.preview.live`) yang melarang skrip.

Bukti selesai: tes yang menyimpan isi berisi tag skrip dan atribut event, lalu memeriksa keduanya hilang; tes bahwa contoh HTML TipTap lengkap tersimpan tanpa perubahan; tes header CSP ada di kedua respons pratinjau. Contoh muatan uji ditulis di tes dan PR, tidak di halaman publik ini.

### R6 · Akun uji hanya di lokal

Masalah sekarang: `UserSeeder` membuat tiga akun berpassword `password`. Bila seeder ikut dijalankan di produksi, akun itu menjadi pintu masuk.

Langkah: di awal `UserSeeder::run()`, lewati pembuatan akun bila `app()->environment()` bukan `local` atau `testing`, dan tulis pesan ke konsol.

Bukti selesai: tes yang menjalankan seeder dengan environment `production` dan memeriksa tidak ada akun `@drafaja.test`.

## Urutan kerja yang disarankan

1. BE-01 dan R6, yang kecil dan tanpa ketergantungan.
2. BE-02.
3. BE-17, karena menyentuh jalur simpan yang juga dipakai CORE-3.
4. BE-09, yang paling besar karena ada migration data.

Setiap butir satu branch dan satu PR. Kirim [H4](../serah-terima/H4.md) setelah keempat PR utama merge.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H4 · CORE-1 → QA-2 · fase 0 API siap uji**](../serah-terima/H4.md) | Louis (QA), [QA-2](QA-2.md) | PR BE-01, BE-02, BE-09, BE-17 yang sudah merge, beserta cara memicu setiap error | Daftar periksa serah terima QA di PR |

## Selesai bila

- [ ] Semua tes di kolom kriteria ada dan lulus
- [ ] S-01, S-02, S-13, S-21 ditandai selesai di doc 18
- [ ] `composer api:spec` dijalankan untuk BE-02 dan BE-09
- [ ] H4 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Backlog backend](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-backend)
- 🔒 [Roadmap fase 0](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-0-keamanan-dan-stabilisasi-pasca-split)
- 🔒 [Doc 07 risiko](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#risiko)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
