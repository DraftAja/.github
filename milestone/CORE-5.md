[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-4](CORE-4.md) · Berikutnya: [CORE-6](CORE-6.md)

# CORE-5 · Verifikasi PostgreSQL 16

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel |
| Contoh branch | `test/be-core-core5-postgresql-16` |
| Mulai setelah | [CORE-4](CORE-4.md) selesai, menerima H15 |
| Lanjut ke | [CORE-6](CORE-6.md) bila Fase U dikerjakan, selain itu selesai |

## Tujuan

Membuktikan bahwa kode yang selama ini diuji di SQLite juga benar di PostgreSQL 16, basis data produksi.

## Kondisi per 4 November 2026

CI backend sudah punya job "Tes (PostgreSQL 16)" dengan `postgres:16-alpine`, dan job itu tampil hijau. Namun hijaunya semu: langkah PHPUnit diberi `continue-on-error: true` dan hanya memunculkan peringatan bila gagal. Di run `develop` terakhir, PHPUnit mencatat 208 tes dengan 154 error dan 2 failure.

Penyebabnya satu migration: `2026_10_28_000002_add_document_workflow_state`. Sebelum mengubah skema, migration itu mencocokkan daftar kolom, indeks, foreign key, dan check constraint empat tabel secara persis. Bila tidak cocok dengan bentuk yang diharapkan, ia menolak dengan pesan "Unknown or partially reconciled workflow schema; refusing DDL and leaving schema/data unchanged." Pada PostgreSQL yang dibuat dari nol, salah satu pencocokan itu tidak cocok. Akibatnya setiap tes yang menjalankan migrate gagal, dan deploy baru ke PostgreSQL kosong juga akan gagal.

Karena itu milestone ini dimulai dengan memperbaiki migrate di PostgreSQL. Perbaikan itu tidak menunggu H15.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-18 | Memperbaiki penolakan migration `2026_10_28_000002` pada PostgreSQL yang dibuat dari nol, lalu menghapus `continue-on-error` dari job CI. | `php artisan migrate` di PostgreSQL 16 kosong selesai; job CI PostgreSQL gagal bila ada tes gagal | 🔒 [S-23](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#proses-lintas-repo) |
| Doc 10 langkah 1 | Menjalankan seluruh rangkaian tes, migrasi dari kosong, upgrade dari baseline, `documents:schema-check`, dan uji pemulihan di PostgreSQL 16 sekali pakai. | Semua lulus; hasil tercatat di doc 10 bagian verifikasi | 🔒 [doc 10, langkah verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#langkah-verifikasi-yang-harus-dijadwalkan) |
| Konkurensi | Menjalankan tes `*PostgresConcurrencyTest` yang selama ini dilewati di SQLite. | Tes konkurensi lulus | 🔒 [doc 16, lapisan uji](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#lapisan-uji) |
| BE-14 | Menyamakan aturan validasi buat dan ubah dokumen (bila kapasitas ada). | Tes: nilai yang diterima saat buat juga diterima saat ubah | 🔒 [S-18](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-16 | Menghapus parser lama yang tidak dipakai (bila kapasitas ada). | `php artisan test` dan Larastan hijau; entri baseline terkait ikut dihapus | 🔒 [S-20](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| INF-04 | Role PostgreSQL `draftaja_migrator`, `draftaja_app`, `draftaja_readonly` per database staging. | `ca-api` berjalan dengan `draftaja_app`; migrate hanya lewat job | 🔒 [doc 19, role](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#role-dan-hak-akses) |
| INF-05 | Mount Azure Files di `/var/www/html/storage/app` untuk `ca-api` dan `ca-worker`, lalu uji tulis-baca dari kedua peran. | Unggah template di staging dianalisis worker sampai pola siap | 🔒 [doc 19, penyimpanan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#penyimpanan-berkas) |
| INF-06 | `LOG_CHANNEL=stderr` dan variabel baru dari doc 19 masuk `ENV_CONTRACT.md`, serta aturan `EnvContract` bila perlu. | `env:verify` lolos di staging | 🔒 [doc 19, variabel](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#variabel-environment-per-app) |
| INF-10 | Uji pemulihan: PITR ke server baru, staging diarahkan ke sana, buka dokumen dan ekspor PDF. | Hasil tercatat di doc 10 | 🔒 [doc 19, backup](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#backup-restore-dan-pemulihan-bencana) |

## Rincian pekerjaan

### BE-18 · Migrasi dari kosong di PostgreSQL

Langkah:

1. Jalankan `docker compose up -d postgres`, lalu tes PostgreSQL dengan perintah di komentar `docker-compose.yml` (database `draftaja_test_local`, dijaga `TestDatabaseGuard`).
2. Cari pencocokan mana yang gagal. Fungsi `fingerprint()` memeriksa berurutan: kolom persis, lalu indeks, foreign key, dan check constraint sebelum workflow. Tambahkan log sementara di cabang lokal untuk melihat daftar kolom dan constraint yang sebenarnya dibuat PostgreSQL, lalu bandingkan dengan yang diharapkan. Perbedaan yang biasa muncul antara SQLite dan PostgreSQL adalah nama constraint dan indeks yang dibuat otomatis, serta bentuk check constraint.
3. Pilih cara perbaikan bersama Raja, karena kontrak tim melarang mengedit migration yang sudah merge:
   - Migration baru yang berjalan sebelum `2026_10_28_000002` dan menyamakan bentuk skema PostgreSQL. Cara ini sesuai aturan, tetapi hanya mungkin bila perbedaannya bisa diperbaiki dengan DDL yang aman.
   - Pengecualian tertulis atas aturan "jangan edit migration", khusus untuk melonggarkan pencocokan nama constraint di migration itu. Alasan pengecualian: migration itu belum pernah berhasil berjalan di PostgreSQL kosong. Pengecualian dicatat di PR dan disetujui PM.
4. Setelah migrate di PostgreSQL kosong berhasil, hapus `continue-on-error` dan langkah peringatan dari job CI, supaya kegagalan berikutnya memerahkan CI.

Bukti selesai: job CI "Tes (PostgreSQL 16)" hijau tanpa `continue-on-error`, dengan nol error dan nol failure.

Hindari: menghapus pengaman `fingerprint()` begitu saja. Pengaman itu melindungi database bersama yang sudah berisi data dari DDL yang salah.

### Verifikasi terhadap data nyata

Setelah [H15](../serah-terima/H15.md):

1. Upgrade salinan data `draftaja_shared_dev` dengan `php artisan migrate`. Catat jumlah baris `users`, `laporans`, `report_sections`, `templates`, `template_patterns`, `ai_generations` sebelum dan sesudah.
2. Jalankan `php artisan documents:schema-check` (baca-saja) dan simpan keluarannya.
3. Uji pemulihan: pulihkan backup ke instance kosong, arahkan API lokal ke sana, buka satu dokumen dan ekspor PDF-nya.
4. Catat semuanya di doc 10 bagian verifikasi, tanpa kredensial.

### Konkurensi

`AcceptDocumentPostgresConcurrencyTest` dan `PatternLearnerPostgresConcurrencyTest` menguji penguncian baris (`lockForUpdate`) saat dua permintaan menerima usulan atau memesan kuota bersamaan. SQLite tidak mendukung penguncian itu, jadi tes ini hanya bermakna di PostgreSQL. Di log CI terakhir keduanya ikut gagal karena migrate gagal. Setelah migrate diperbaiki, keduanya harus lulus tanpa diubah.

### INF-04 sampai INF-06 · Aplikasi di staging

Tiga pekerjaan ini menyiapkan aplikasi untuk staging yang dibangun Raja di PM-3:

1. INF-04: buat role per database dengan SQL contoh di doc 19. `ca-api`, `ca-worker`, dan job scheduler memakai `draftaja_app` (tanpa DDL); job migrate memakai `draftaja_migrator`.
2. INF-05: mount Azure Files ke `storage/app`. Periksa izin berkas, karena image membuat direktori dengan pemilik `www-data`, sedangkan share SMB punya pemilik dan mode sendiri.
3. INF-06: tambahkan `LOG_CHANNEL=stderr` dan variabel lain dari doc 19 ke `ENV_CONTRACT.md` dan `.env.example`, supaya log Laravel masuk Log Analytics.

INF-10 (uji pemulihan) dijalankan bersama verifikasi data nyata di atas, setelah H15.

### BE-14 · Validasi buat dan ubah dokumen

Masalah sekarang: `tahun_ajaran` maksimal 10 karakter saat buat tetapi 20 saat ubah, dan logo menolak webp saat buat tetapi menerima saat ubah. Bila kolom akademik sudah diganti di CORE-3, butir ini berlaku untuk kolom penggantinya. Pakai satu sumber aturan untuk kedua request.

### BE-16 · Parser lama

`app/Services/TemplateParserService.php`, `WordParserService.php`, dan `app/Traits/ParsesWordDocument.php` tidak dipanggil dari kode atau tes mana pun. Pembacaan DOCX sudah dipegang `DocxDocumentReader`. Hapus ketiganya, lalu hapus entri Larastan baseline yang menunjuk ke berkas itu.

## Urutan kerja yang disarankan

1. Migrasi dari kosong dan job CI, segera, karena memengaruhi semua milestone backend.
2. Konkurensi, yang otomatis ikut diuji begitu langkah 1 selesai.
3. Verifikasi terhadap data nyata setelah H15.
4. BE-14 dan BE-16 bila kapasitas ada.

Kirim [H21](../serah-terima/H21.md) setelah langkah 3.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H15 · PM-3 → CORE-5 · PostgreSQL 16 sekali pakai**](../serah-terima/H15.md) | Raja (PM), [PM-3](PM-3.md) | Instance PostgreSQL 16 sekali pakai (bukan database Azure bersama) dan izin pemakaiannya | Tes konkurensi, migrasi, upgrade, schema check |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H21 · CORE-5 → PM-4 · hasil tes PostgreSQL 16**](../serah-terima/H21.md) | Raja (PM), [PM-4](PM-4.md) | Hasil seluruh rangkaian tes di PostgreSQL 16 | Doc 10 bagian verifikasi |

## Selesai bila

- [ ] INF-04, INF-05, INF-06 selesai di staging dan INF-10 tercatat di doc 10
- [ ] Job CI PostgreSQL hijau tanpa `continue-on-error`
- [ ] Hasil PostgreSQL 16 tercatat di doc 10
- [ ] H21 terkirim ke Raja

## Dokumen rujukan lain

- 🔒 [Doc 04, upgrade, preflight, dan pemulihan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md#upgrade-preflight-dan-pemulihan)
- 🔒 [Doc 09, tes](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/09-setup-development.md#tes)
- 🔒 [Workflow CI](https://github.com/DraftAja/Backend-Laravel/blob/develop/.github/workflows/ci.yml)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
