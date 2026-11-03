[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-2](WEB-2.md) · Berikutnya: [WEB-4](WEB-4.md)

# WEB-3 · Final, revisi, kuota, konflik

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-final-dan-revisi` |
| Mulai setelah | [WEB-2](WEB-2.md) selesai, menerima H11, H12, H13 |
| Lanjut ke | [WEB-4](WEB-4.md) |

## Tujuan

Alur dokumen fase 1 tampil di antarmuka: menandai final, membuat revisi, melihat kuota, menangani konflik, dan halaman dasar yang belum ada.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-11 | Sorot penanda `[perlu dilengkapi]` di editor, hitung di bilah status, klik membawa ke penanda pertama. | E2E: bagian berisi dua penanda menampilkan hitungan 2 | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-12 | Tombol "Tandai final" dan "Buat revisi" di kepala editor, setelah H12. | Dokumen final terbuka dalam mode baca; revisi membuka draf baru | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-13 | Kartu kuota AI di beranda dan panel perapihan; tombol AI nonaktif sampai `resets_at`, setelah H13. | Kuota tampil sesuai `GET /ai/quota` | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-14 | Dialog konflik saat bagian disunting di tab lain (409 `revision_changed`). | Dialog muncul dan isi terbaru tidak hilang | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-15 | Halaman `not-found.tsx`, `error.tsx`, dan `/kebijakan-privasi` dengan teks dari H11. | Ketiga halaman tampil di 360 px dan desktop | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-18 | Usulan perapihan dibaca dari field `proposal`; hapus `JSON.parse`. | Tipe usulan dari `schema.d.ts` | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |
| KF-DOK-04 | Dialog data dokumen memakai kolom identitas baru. | Kolom akademik tidak tampil | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H11 · PM-2 → WEB-3 · teks kebijakan privasi** | Raja (PM), [PM-2](PM-2.md) | Isi halaman kebijakan privasi | Halaman `/kebijakan-privasi` (FE-15) |
| **H12 · CORE-3 → WEB-3 · final, revisi, konflik** | Octaf (BE 1), [CORE-3](CORE-3.md) | `POST /laporan/{id}/finalize`, `POST /laporan/{id}/revisions`, 409 `revision_changed` | FE-12 tombol final dan revisi, FE-14 dialog konflik |
| **H13 · AI-3 → WEB-3 · kuota dan proposal** | Hamid (BE 2), [AI-3](AI-3.md) | `GET /ai/quota` dan field `proposal` berbentuk objek | FE-13 kartu kuota, FE-18 tanpa `JSON.parse` |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H14 · WEB-3 → QA-3 · fase 1 siap uji** | Louis (QA), [QA-3](QA-3.md) | Fase 1 lengkap di kedua repo: PR web merujuk PR API yang sudah merge | Repo gabungan mencatat pasangan commit, daftar periksa serah terima QA |

## Selesai bila

- [ ] Semua keadaan di peta halaman ditangani
- [ ] Diuji di 360 px dan desktop
- [ ] H14 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Web doc 03 peta halaman](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md)
- 🔒 [Roadmap fase 1](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-1-penyelarasan-persona)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
