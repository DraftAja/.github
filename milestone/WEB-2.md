[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-1](WEB-1.md) · Berikutnya: [WEB-3](WEB-3.md)

# WEB-2 · Sinkron kontrak baru

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Frontend-Nextjs |
| Contoh branch | `refactor/fe-web2-tipe-dari-kontrak` |
| Saran urutan | [WEB-1](WEB-1.md) selesai, menerima H6, H7. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [WEB-3](WEB-3.md) |

## Tujuan

Web memakai tipe yang dibangkitkan dari `openapi.json` dan bercabang berdasarkan `code` error, bukan menebak dari status HTTP. Setelah milestone ini, perubahan bentuk respons di API langsung memecahkan `npm run typecheck` di web.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-03 | Layout area kerja bercabang berdasarkan `code === 'email_unverified'`; 409 lain ditangani sebagai error umum. Bergantung pada BE-03 dan BE-10. | `serverApi` mengembalikan `code`; E2E akun belum terverifikasi diarahkan ke `/verifikasi-email` | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |
| FE-08 | Mengganti sepuluh tipe di "Kontrak manual" dengan tipe dari `schema.d.ts`, setelah H6 dan H7. | "Kontrak manual" kosong atau hanya berisi tipe sementara bernomor issue; `npm run typecheck` lolos | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |

## Rincian pekerjaan

### Sinkron kontrak

Langkah ini dilakukan setiap kali PR API yang mengubah kontrak merge, dan pertama kali di milestone ini setelah [H6](../serah-terima/H6.md) dan [H7](../serah-terima/H7.md):

```bash
cd draftaja-web
git switch develop && git pull
npm run api:sync      # menyalin docs/api/openapi.json dari ../draftaja-api atau ../Backend-Laravel
npm run api:types     # membangkitkan src/lib/api/schema.d.ts
npm run typecheck
```

Commit `contracts/openapi.json` dan `src/lib/api/schema.d.ts` bersama, dengan pesan `chore(api): sinkron kontrak <sha API>`.

### FE-08 · Kontrak manual dikosongkan

Masalah sekarang: sepuluh tipe di `src/lib/api/types.ts` bagian "Kontrak manual" ditulis tangan, yaitu `Dashboard`, `LaporanOptions`, `DocumentStyle` dan `LaporanDetail`, `TemplateIndex` dan `ProviderReadiness`, `TemplateAnalysis`, `UploadReservation`, `OpenedDocument`, `Generation` dan `GenerationStatus`, `CleanupReservation`, serta `ChatReply`. Tipe-tipe ini tidak diperiksa terhadap API.

Langkah:

1. Ganti setiap tipe manual dengan alias ke `components['schemas'][...]` atau ke tipe respons operasi di `paths`.
2. Jalankan `npm run typecheck`. Bila ada error, bandingkan bentuk baru dengan doc 05 API. Bila API berbeda dari proposal kontrak, tulis komentar di issue proposal. Jangan menyesuaikan kode web diam-diam.
3. `GenerationStatus` tidak lagi memuat `processing`. Periksa `CleanupPanel.tsx`, yang memeriksa `['pending', 'processing']`, dan sederhanakan.
4. Tipe yang belum tersedia di kontrak tetap di "Kontrak manual" dengan komentar nomor issue.

Bukti selesai: bagian "Kontrak manual" kosong, atau hanya berisi tipe sementara bernomor issue; `npm run typecheck`, `npm run lint`, `npm test`, dan E2E lulus.

### FE-03 · Bercabang berdasarkan `code`

Masalah sekarang: `src/app/(app)/(workspace)/layout.tsx` memanggil `serverApi('/me')` dan mengalihkan ke `/verifikasi-email` bila status 409. `serverApi` hanya mengembalikan `{status, data}` tanpa isi error. Begitu `GET /me` punya sumber 409 lain, pengguna akan dialihkan ke halaman yang salah.

Langkah:

1. Ubah `src/lib/api/server.ts` agar mengembalikan `code` dari body error bila ada.
2. Di layout, alihkan ke `/verifikasi-email` hanya bila `code === 'email_unverified'`. 409 lain menampilkan halaman galat umum.
3. Tambah cabang di `src/lib/api/errors.ts` untuk kode baru dari CORE-2 sesuai tabel "Kode yang akan datang" di web doc 04: `document_final`, `forbidden`, `revision_changed`, `pattern_not_ready`, `source_missing`, `rate_limited`.
4. Pindahkan baris dari "Kode yang akan datang" ke tabel "Per `code`" di web doc 04.

Bukti selesai: tes Vitest `userMessage` untuk setiap kode baru; E2E akun belum terverifikasi diarahkan ke `/verifikasi-email`. E2E ini baru bisa ditulis setelah BE-10 (verifikasi email diwajibkan) merge di CORE-3. Sebelum itu, cukup tes Vitest untuk fungsi pemilih tujuan.

## Urutan kerja yang disarankan

1. Sinkron kontrak setelah H6. Ganti tipe inti dan jalankan pemeriksaan.
2. Sinkron lagi setelah H7. Ganti tipe AI.
3. FE-03.

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H6 · CORE-2 → WEB-2 · openapi.json inti baru**](../serah-terima/H6.md) | Octaf (BE 1), [CORE-2](CORE-2.md) | `openapi.json` dengan skema `ErrorResponse` dan tipe respons dashboard, options, template | `npm run api:sync && npm run api:types`, lalu mengosongkan "Kontrak manual" |
| [**H7 · AI-1 → WEB-2 · tipe generate, tidy, chat**](../serah-terima/H7.md) | Hamid (BE 2), [AI-1](AI-1.md) | Tipe `Generation`, `CleanupReservation`, `ChatReply` di `openapi.json` | FE-08 tanpa tipe tulisan tangan |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] `contracts/openapi.json` dan `schema.d.ts` sama dengan `develop` API
- [ ] "Kontrak manual" kosong atau hanya berisi tipe sementara bernomor issue
- [ ] Web doc 04 diperbarui

## Dokumen rujukan lain

- 🔒 [Web doc 04, alur kontrak](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#alur-kontrak)
- 🔒 [Web doc 04, pemetaan error](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#pemetaan-error)
- 🔒 [Doc 05 API, katalog error](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#katalog-error)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
