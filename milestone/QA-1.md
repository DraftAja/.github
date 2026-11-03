[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [QA-2](QA-2.md)

# QA-1 · Rencana uji dan tinjauan kontrak

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel (dokumentasi), issue di kedua repo |
| Contoh branch | `docs/qa-qa1-rencana-uji` |
| Mulai setelah | Repo dipisah, 3 November 2026 |
| Lanjut ke | [QA-2](QA-2.md) |

## Tujuan

Menyiapkan rencana uji per fase dan memastikan setiap proposal kontrak sudah memikirkan kasus error sebelum kode ditulis.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 | Rencana uji per fase dari tabel "Tes yang wajib ditambah per fase" dan skenario peramban. | Rencana uji fase 0 sampai 2 tersedia dan disetujui Raja | 🔒 [doc 16, tes per fase](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| Doc 17 | Meninjau kolom Error di setiap proposal kontrak; memberi persetujuan sebelum Alvira memasang `kontrak-disetujui`. | Setiap proposal terbuka sudah ditinjau dalam 1×24 jam hari kerja | 🔒 [doc 17, proposal kontrak](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#proposal-kontrak) |
| Kontrak bagian 12 | Memakai template issue "Laporan bug": langkah reproduksi, hasil diharapkan dan aktual, lingkungan, bukti. | Bug fase 0 dilaporkan dengan template | 🔒 [kontrak bagian 12](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#12-issue-dan-pelacakan-pekerjaan) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H2 · QA-1 → CORE-2 · kasus error inti disetujui** | Octaf (BE 1), [CORE-2](CORE-2.md) | Persetujuan daftar `code` error di proposal kontrak endpoint inti | Komentar di issue `kontrak`, label `kontrak-disetujui` |
| **H3 · QA-1 → AI-1 · kasus error AI disetujui** | Hamid (BE 2), [AI-1](AI-1.md) | Persetujuan kasus error endpoint AI (generate, tidy, chat) | Komentar di issue `kontrak`, label `kontrak-disetujui` |

## Selesai bila

- [ ] Rencana uji tersedia
- [ ] H2 dan H3 terkirim

## Dokumen rujukan lain

- 🔒 [Doc 16 strategi pengujian](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md)
- 🔒 [Web doc 07 pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
