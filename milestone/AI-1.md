[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [AI-2](AI-2.md)

# AI-1 · Kontrak AI terketik

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-ai-tipe-respons` |
| Mulai setelah | menerima H3 |
| Lanjut ke | [AI-2](AI-2.md) |

## Tujuan

Respons endpoint AI muncul di `openapi.json`, supaya FE memakai tipe hasil generate, bukan tipe tulisan tangan.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-04 (AI) | Resource atau anotasi Scramble untuk `Generation`, `CleanupReservation`, dan `ChatReply`. Bentuk JSON tidak berubah. | Bagian AI di "Kontrak manual" `types.ts` web bisa dihapus dan `tsc` tetap lolos | 🔒 [S-07](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| S-10 | Status hasil AI di kontrak sama dengan `AiStatus` (`pending`, `succeeded`, `failed`). | Tipe status di web berasal dari kontrak | 🔒 [S-10](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| S-16 | Menyelaraskan `docs-ai/05-api.md` dengan doc 05 dan menghapus sebutan Inertia dan LaporBot di `docs-ai/01-arsitektur.md`. | Tidak ada pertentangan antara `docs-ai/` dan doc 05 | 🔒 [S-16](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#proses-lintas-repo) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H3 · QA-1 → AI-1 · kasus error AI disetujui** | Louis (QA), [QA-1](QA-1.md) | Persetujuan kasus error endpoint AI (generate, tidy, chat) | Mengunci bentuk respons AI di BE-04 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H7 · AI-1 → WEB-2 · tipe generate, tidy, chat** | Alvira (FE), [WEB-2](WEB-2.md) | Tipe `Generation`, `CleanupReservation`, `ChatReply` di `openapi.json` | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] `composer api:spec` dijalankan dan `openapi.json` ikut di-commit
- [ ] H7 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Backlog backend](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-backend)
- 🔒 [docs-ai](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/README.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
