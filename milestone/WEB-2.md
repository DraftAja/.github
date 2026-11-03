[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-1](WEB-1.md) · Berikutnya: [WEB-3](WEB-3.md)

# WEB-2 · Sinkron kontrak baru

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Frontend-Nextjs |
| Contoh branch | `refactor/fe-tipe-dari-kontrak` |
| Mulai setelah | [WEB-1](WEB-1.md) selesai, menerima H6, H7 |
| Lanjut ke | [WEB-3](WEB-3.md) |

## Tujuan

Web memakai tipe yang dibangkitkan dari `openapi.json` dan bercabang berdasarkan `code` error, bukan menebak dari status HTTP.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-03 | Layout area kerja bercabang berdasarkan `code === 'email_unverified'`; 409 lain ditangani sebagai error umum. Bergantung pada BE-03 dan BE-10. | `serverApi` mengembalikan `code`; E2E akun belum terverifikasi diarahkan ke `/verifikasi-email` | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |
| FE-08 | Mengganti sepuluh tipe di "Kontrak manual" dengan tipe dari `schema.d.ts`, setelah H6 dan H7. | "Kontrak manual" kosong atau hanya berisi tipe sementara bernomor issue; `npm run typecheck` lolos | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H6 · CORE-2 → WEB-2 · openapi.json inti baru** | Octaf (BE 1), [CORE-2](CORE-2.md) | `openapi.json` dengan skema `ErrorResponse` dan tipe respons dashboard, options, template | `npm run api:sync && npm run api:types`, lalu mengosongkan "Kontrak manual" |
| **H7 · AI-1 → WEB-2 · tipe generate, tidy, chat** | Hamid (BE 2), [AI-1](AI-1.md) | Tipe `Generation`, `CleanupReservation`, `ChatReply` di `openapi.json` | FE-08 tanpa tipe tulisan tangan |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] `npm run api:sync && npm run api:types` dijalankan
- [ ] PR web merujuk PR API yang sudah merge

## Dokumen rujukan lain

- 🔒 [Doc 17, bekerja sebelum API siap](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#bekerja-sebelum-api-siap)
- 🔒 [Web doc 04 integrasi API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
