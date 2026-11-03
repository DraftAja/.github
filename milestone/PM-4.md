[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-3](PM-3.md) · Berikutnya: [PM-5](PM-5.md)

# PM-4 · Feature freeze dan rilis MVP, minggu 14

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Rilis · minggu 14, 15, 16 |
| Repo | Backend-Laravel, Frontend-Nextjs, DraftAja |
| Contoh branch | `release/mvp` |
| Mulai setelah | [PM-3](PM-3.md) selesai, menerima H21, H22 |
| Lanjut ke | [PM-5](PM-5.md) |

## Tujuan

MVP (fase 0 sampai 2) dibekukan dan dirilis sebagai pasangan versi API dan web yang sudah lolos QA.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Kriteria rilis | Memeriksa 7 kriteria rilis MVP di doc 16 berdasarkan H21 dan H22. | Semua terpenuhi, atau risikonya diterima tertulis | 🔒 [doc 16, kriteria rilis MVP](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#kriteria-rilis-mvp) |
| Risiko | Menerima tertulis risiko R1 sampai R9 dan temuan P0 yang belum selesai. | Daftar penerimaan risiko tercatat di doc 10 | 🔒 [doc 07, risiko](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#risiko) |
| Freeze | Mengumumkan feature freeze: setelah ini hanya perbaikan bug dan dokumentasi. | PR fitur baru ditahan sampai setelah rilis | 🔒 [kontrak bagian 11](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#11-rilis-milestone-dan-code-freeze) |
| Rilis | Membuat `release/*` dari `develop`, memberi tag dengan nama sama di kedua repo, dan mengisi tabel pasangan rilis di doc 17. | Tag ada di kedua repo; repo gabungan mencatat pasangan commit | 🔒 [doc 17, urutan merge dan deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#urutan-merge-dan-deploy) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H21 · CORE-5 → PM-4 · hasil tes PostgreSQL 16** | Octaf (BE 1), [CORE-5](CORE-5.md) | Hasil seluruh rangkaian tes di PostgreSQL 16 | Kriteria rilis butir 1 |
| **H22 · QA-5 → PM-4 · label siap-rilis** | Louis (QA), [QA-5](QA-5.md) | Label `siap-rilis` pada pasangan commit API dan web | Membuat `release/*` dan tag pasangan versi |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] 7 kriteria rilis diperiksa
- [ ] Tag pasangan versi dibuat
- [ ] Tabel pasangan rilis di doc 17 terisi

## Dokumen rujukan lain

- 🔒 [Doc 10 verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#verifikasi)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
