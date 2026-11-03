[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-4](CORE-4.md)

# CORE-5 · Verifikasi PostgreSQL 16

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel |
| Contoh branch | `test/be-core-core5-postgresql-16` |
| Mulai setelah | [CORE-4](CORE-4.md) selesai, menerima H15 |
| Lanjut ke | selesai |

## Tujuan

Membuktikan bahwa kode yang selama ini diuji di SQLite juga benar di PostgreSQL 16, basis data produksi.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 10 langkah 1 | Menjalankan seluruh rangkaian tes, migrasi dari kosong, upgrade dari baseline, `documents:schema-check`, dan uji pemulihan di PostgreSQL 16 sekali pakai. | Semua lulus; hasil tercatat di doc 10 bagian verifikasi | 🔒 [doc 10, langkah verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#langkah-verifikasi-yang-harus-dijadwalkan) |
| Konkurensi | Menjalankan tes `*PostgresConcurrencyTest` yang selama ini dilewati di SQLite. | Tes konkurensi lulus | 🔒 [doc 16, lapisan uji](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#lapisan-uji) |
| BE-14 | Menyamakan aturan validasi buat dan ubah dokumen (bila kapasitas ada). | Tes: nilai yang diterima saat buat juga diterima saat ubah | 🔒 [S-18](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-16 | Menghapus parser lama yang tidak dipakai (bila kapasitas ada). | `php artisan test` dan Larastan hijau; entri baseline terkait ikut dihapus | 🔒 [S-20](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H15 · PM-3 → CORE-5 · PostgreSQL 16 sekali pakai** | Raja (PM), [PM-3](PM-3.md) | Instance PostgreSQL 16 sekali pakai (bukan database Azure bersama) dan izin pemakaiannya | Tes konkurensi, migrasi, upgrade, schema check |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H21 · CORE-5 → PM-4 · hasil tes PostgreSQL 16** | Raja (PM), [PM-4](PM-4.md) | Hasil seluruh rangkaian tes di PostgreSQL 16 | Doc 10 bagian verifikasi |

## Selesai bila

- [ ] Hasil PostgreSQL 16 tercatat di doc 10
- [ ] H21 terkirim ke Raja

## Dokumen rujukan lain

- 🔒 [Roadmap modul AI, bukti PostgreSQL](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/08-roadmap.md)
- 🔒 [Doc 09 setup dan tes](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/09-setup-development.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
