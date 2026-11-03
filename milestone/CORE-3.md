[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-2](CORE-2.md) · Berikutnya: [CORE-4](CORE-4.md)

# CORE-3 · Persona dan alur dokumen

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core3-kategori-dokumen` |
| Mulai setelah | [CORE-2](CORE-2.md) selesai, menerima H9 |
| Lanjut ke | [CORE-4](CORE-4.md) |

## Tujuan

Dokumen berhenti berpersona akademik: kategori memakai 8 kunci, identitas dokumen sesuai kantor, dokumen bisa difinalkan dan direvisi, dan ekspor mengikuti bentuk kategorinya.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-05 | Enum `DocumentCategory` dan migrasi nilai kategori lama ke 8 kunci, mengikuti keputusan H9. | Tes: nilai lama termigrasi ke 8 kunci; nilai di luar enum ditolak 422 | 🔒 [doc 12, template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#template-tpl) |
| KF-DOK-04 | Kolom identitas dokumen pengganti kolom akademik, lewat migration baru (bukan mengedit migration lama). | Kolom akademik tidak lagi dikirim ke antarmuka; kolom pengganti tersimpan | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| S-22 | Menghapus jenis dokumen akademik dari `laporan.create` dan `createDefaultSections`. | Tes: `laporan.create` tidak mengirim Skripsi, Makalah, Laporan Praktikum, atau Studi Kasus | 🔒 [S-22](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-05 | `POST /laporan/{id}/finalize` dan `POST /laporan/{id}/revisions`. Bergantung pada BE-01. | Tes: final mengisi `finalized_at`; revisi membuat draf baru dengan `revision_of_id`; dokumen final menolak suntingan 403 | 🔒 [KF-TIN-04](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#tinjau-dan-penomoran-tin) |
| BE-07 | Konkurensi optimistis di `PUT /sections/{id}` dengan field opsional `base_updated_at`. | Tes: dua pembaruan dengan `base_updated_at` sama, yang kedua 409 `revision_changed`; tanpa field, perilaku lama tetap | 🔒 [S-12](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-10 | `MustVerifyEmail` di `User`. | Tes: akun belum terverifikasi menerima 409 `email_unverified` di rute aplikasi | 🔒 [S-17](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#keamanan-dan-data) |
| KF-EKS-02, KF-EKS-03 | Tata letak ekspor per kategori: Surat dan Notula tanpa sampul dan daftar isi. | Tes: DOCX Surat tidak memuat sampul dan daftar isi; DOCX `PRP-01` memuat keduanya | 🔒 [doc 12, ekspor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ekspor-eks) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H9 · PM-2 → CORE-3 · kategori dan kolom identitas** | Raja (PM), [PM-2](PM-2.md) | Keputusan 8 kunci kategori, pemetaan nilai lama, dan kolom identitas pengganti kolom akademik | Enum `DocumentCategory` dan migrasi kolom (KF-DOK-04) |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H12 · CORE-3 → WEB-3 · final, revisi, konflik** | Alvira (FE), [WEB-3](WEB-3.md) | `POST /laporan/{id}/finalize`, `POST /laporan/{id}/revisions`, 409 `revision_changed` | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] Semua tes di kolom kriteria lulus
- [ ] Doc 04 dan 05 diperbarui untuk kolom dan endpoint baru
- [ ] H12 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Roadmap fase 1](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-1-penyelarasan-persona)
- 🔒 [Doc 16 tes per fase](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase)
- 🔒 [Doc 04 basis data](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md)
- 🔒 [Rencana penggantian field](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#rencana-penggantian-field)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
