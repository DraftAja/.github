[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-1](CORE-1.md) · Berikutnya: [CORE-3](CORE-3.md)

# CORE-2 · Kontrak error dan tipe respons

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core2-error-code` |
| Mulai setelah | [CORE-1](CORE-1.md) selesai, menerima H1, H2 |
| Lanjut ke | [CORE-3](CORE-3.md) |

## Tujuan

Setiap error API punya `code` dan pesan Indonesia, dan setiap respons inti terketik di `openapi.json`, supaya FE tidak lagi menulis tipe dengan tangan.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-03 | Renderer error tunggal di `bootstrap/app.php` dengan `code` dan pesan Indonesia untuk 401, 403, 404, 409 verifikasi, 419, 422, 429 throttle. Tambah skema `ErrorResponse` dan `ValidationErrorResponse`. | Tes per status memeriksa `code`; tes katalog memastikan setiap error `api/*` punya `message` dan `code` | 🔒 [S-03, S-04, S-05](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-04 (inti) | Resource atau anotasi Scramble untuk respons dashboard, options, dan template yang kini ditulis tangan di web. Bentuk JSON tidak berubah. | Bagian inti di "Kontrak manual" `types.ts` web bisa dihapus dan `tsc` tetap lolos | 🔒 [S-07](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-11 | `securitySchemes` cookie session dan header CSRF di `openapi.json` lewat `Scramble::extendOpenApi`. | `openapi.json` memuat skema dan setiap rute non-publik memakainya | 🔒 [S-06](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H1 · PM-1 → CORE-2 · kontrak tim v1.1, CODEOWNERS** | Raja (PM), [PM-1](PM-1.md) | `TEAM_CONTRACT.md` v1.1, CODEOWNERS per repo, branch protection, label. Berlaku untuk semua anggota. | Reviewer wajib sudah berlaku sebelum BE-03 mengubah kontrak error |
| **H2 · QA-1 → CORE-2 · kasus error inti disetujui** | Louis (QA), [QA-1](QA-1.md) | Persetujuan daftar `code` error di proposal kontrak endpoint inti | Menulis satu tes per `code` di BE-03 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H6 · CORE-2 → WEB-2 · openapi.json inti baru** | Alvira (FE), [WEB-2](WEB-2.md) | `openapi.json` dengan skema `ErrorResponse` dan tipe respons dashboard, options, template | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] Proposal kontrak berlabel `kontrak-disetujui` sebelum kode
- [ ] `composer api:spec` dijalankan dan `openapi.json` ikut di-commit
- [ ] H6 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Doc 17 proposal kontrak](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#proposal-kontrak)
- 🔒 [Doc 16 tes kontrak](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-kontrak)
- 🔒 [Doc 05 API](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
