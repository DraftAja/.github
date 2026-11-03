[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-3](CORE-3.md) · Berikutnya: [CORE-5](CORE-5.md)

# CORE-4 · Katalog 64 template

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core4-katalog-template` |
| Mulai setelah | [CORE-3](CORE-3.md) selesai |
| Lanjut ke | [CORE-5](CORE-5.md) |

## Tujuan

Pengguna bisa memilih dari 64 template bawaan, 8 per kategori, lengkap dengan kolom isian yang disimpan per dokumen.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-01 | Definisi 64 template di `TemplateSeeder` dari berkas spesifikasi di `docs/templates/`. Seeder bisa dijalankan ulang tanpa duplikasi. | `TemplateSeedTest` mengharapkan 64, kode unik, tepat 8 per kategori | 🔒 [Katalog template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md) |
| KF-TPL-01 | Tes kesesuaian: membaca bagian "Struktur" setiap spesifikasi dan membandingkannya dengan `sections` di seeder. | Tes gagal bila spesifikasi atau seeder diubah tanpa yang lain | 🔒 [doc 16, tes fase 2](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| KF-DOK-01, KF-DOK-05 | Tabel `template_fields` dan kolom `laporans.field_values`. | Nilai tersimpan di `field_values`; kolom wajib yang kosong menghasilkan peringatan sebelum ekspor | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| Fase 2 | `GET /categories` sebagai pengganti `report_types` di `GET /laporan/options`. | `report_types` ditandai deprecated sesuai rencana penggantian field | 🔒 [Rencana penggantian field](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#rencana-penggantian-field) |
| BE-12 | Respons buka template memakai `laporan_id`; `url` ditandai deprecated. | Web tidak lagi membaca `url` | 🔒 [S-09](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-13 | Paginasi `GET /laporan` (`?page=`, `per_page` maksimal 50). Tanpa parameter, perilaku lama dipertahankan. | Uji beban 200 dokumen di bawah 1 detik p95 dicatat di doc 10 | 🔒 [S-11](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H17 · CORE-4 → WEB-4 · kategori dan kolom isian** | Alvira (FE), [WEB-4](WEB-4.md) | `GET /categories`, `template_fields`, `laporans.field_values` di `openapi.json` | PR API merge, label `butuh-fe` di issue web |
| **H18 · CORE-4 → QA-4 · 64 template di seeder** | Louis (QA), [QA-4](QA-4.md) | 64 template di `TemplateSeeder` sesuai `docs/templates/` | PR API merge |

## Selesai bila

- [ ] 64 template ter-seed dan tes kesesuaian lulus
- [ ] `openapi.json` memuat endpoint dan field baru
- [ ] H17 dan H18 terkirim

## Dokumen rujukan lain

- 🔒 [Roadmap fase 2](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-2-katalog-64-template)
- 🔒 [Doc 04 basis data](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
