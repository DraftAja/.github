[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [PM-2](PM-2.md)

# PM-1 · Fondasi kolaborasi dua repo

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel, Frontend-Nextjs, DraftAja, .github |
| Contoh branch | `docs/pm-pm1-kontrak-tim-v1.1` |
| Mulai setelah | Repo dipisah, 3 November 2026 |
| Lanjut ke | [PM-2](PM-2.md) |

## Tujuan

Aturan main untuk dua repo berlaku sebelum kontrak API mulai berubah. Setelah milestone ini, setiap PR otomatis meminta review pemilik kode dan setiap anggota bisa diberi tugas di GitHub.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-15 | Revisi `TEAM_CONTRACT.md` v1.1: struktur kepemilikan per repo, aturan PR lintas repo, Definition of Done tambahan, sesuai usulan di doc 17. Pemetaan BE 1 = `be-core` (Octaf) dan BE 2 = `be-ai` (Hamid) sudah diselaraskan di v1.0.1. | Disetujui minimal 4 dari 5 anggota (kontrak bagian 14) | 🔒 [doc 17, usulan revisi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#usulan-revisi-team_contractmd-v11) |
| BE-15 | Membuat `.github/CODEOWNERS` di Backend-Laravel dan Frontend-Nextjs. Di API, `docs/api/openapi.json` dimiliki bersama `be-core`, `be-ai`, dan `fe`. | PR yang mengubah `openapi.json` otomatis meminta review FE | 🔒 [Lampiran A](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#lampiran-a-contoh-githubcodeowners) |
| Lampiran B | Branch protection `main` dan `develop` di kedua repo, termasuk status check `api:check` di web. | Push langsung ke `develop` ditolak; approval 1 untuk `develop` dan 2 untuk `main`; review pemilik kode wajib | 🔒 [Lampiran B](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#lampiran-b-pengaturan-branch-protection-github) |
| Label | Membuat label dari kontrak bagian 12 dan doc 17 di ketiga repo: `be-core`, `be-ai`, `fe`, `qa`, `docs`, `bug`, `feature`, `blocked`, `priority-high`, `kontrak`, `kontrak-disetujui`, `memecah-kontrak`, `butuh-api`, `butuh-fe`, `siap-rilis`. | Semua label tersedia dan dipakai template issue | 🔒 [doc 17, label issue](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#label-issue) |
| Organisasi | Mengundang Alvira dan Louis ke organisasi DraftAja, lalu membuat GitHub Milestone atau Projects untuk 25 milestone ini dengan tanggal jatuh tempo. | Kelima anggota bisa di-assign ke issue dan menjadi pemilik kode | 🔒 [kontrak bagian 12](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#12-issue-dan-pelacakan-pekerjaan) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H1 · PM-1 → CORE-2 · kontrak tim v1.1, CODEOWNERS** | Octaf (BE 1), [CORE-2](CORE-2.md) | `TEAM_CONTRACT.md` v1.1, CODEOWNERS per repo, branch protection, label. Berlaku untuk semua anggota. | PR ke `develop`, pengumuman di grup |

## Selesai bila

- [ ] Kontrak v1.1 merge ke `develop` dengan 4 persetujuan
- [ ] CODEOWNERS dan branch protection aktif di kedua repo
- [ ] Label tersedia di ketiga repo
- [ ] Alvira dan Louis menjadi anggota organisasi

## Dokumen rujukan lain

- 🔒 [TEAM_CONTRACT.md](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md)
- 🔒 [Temuan S-19](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#proses-lintas-repo)
- 🔒 [Doc 17 kolaborasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
