[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-2](PM-2.md) · Berikutnya: [PM-4](PM-4.md)

# PM-3 · Lingkungan uji nyata

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | infrastruktur, tanpa perubahan kode |
| Branch | tidak perlu branch |
| Mulai setelah | [PM-2](PM-2.md) selesai |
| Lanjut ke | [PM-4](PM-4.md) |

## Tujuan

Menyediakan lingkungan yang tidak bisa dibuat sendiri oleh BE: PostgreSQL 16 untuk tes, kunci provider AI, dan staging dengan urutan deploy yang benar.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 10 langkah 1 | Instance PostgreSQL 16 sekali pakai untuk tes, misalnya Docker Compose `postgres:16-alpine` dengan nama database `draftaja_test_*`. Bukan database Azure bersama. | Octaf punya akses dan izin menjalankan tes | 🔒 [doc 10, langkah verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#langkah-verifikasi-yang-harus-dijadwalkan) |
| Doc 10 langkah 2 | Kunci Groq dan Gemini dengan izin pemakaian terbatas, plus nama `GEMINI_MODEL` yang aktif. | Hamid menerima kunci lewat kanal aman; tidak ada kunci di repo atau chat grup terbuka | 🔒 [doc 08 integrasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/08-integrations.md) |
| Doc 17 deploy | Staging dengan urutan deploy: API lebih dulu, migration sebelum container baru menerima lalu lintas, lalu web. | Urutan deploy dicoba sekali di staging | 🔒 [doc 17, urutan merge dan deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#urutan-merge-dan-deploy) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H15 · PM-3 → CORE-5 · PostgreSQL 16 sekali pakai** | Octaf (BE 1), [CORE-5](CORE-5.md) | Instance PostgreSQL 16 sekali pakai (bukan database Azure bersama) dan izin pemakaiannya | Kanal aman, bukan repo |
| **H16 · PM-3 → AI-4 · kunci Groq dan Gemini** | Hamid (BE 2), [AI-4](AI-4.md) | Kunci Groq dan Gemini, izin pemakaian terbatas, nama `GEMINI_MODEL` yang aktif | Kanal aman, bukan chat grup terbuka |

## Selesai bila

- [ ] H15 dan H16 terkirim lewat kanal aman
- [ ] Staging bisa menjalankan pasangan commit `develop`

## Dokumen rujukan lain

- 🔒 [Doc 02 arsitektur, deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/02-system-architecture.md#deploy)
- 🔒 [ENV_CONTRACT API](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/ENV_CONTRACT.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
