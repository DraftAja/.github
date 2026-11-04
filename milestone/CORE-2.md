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

Setiap error API punya `code` dan pesan Indonesia, dan setiap respons inti terketik di `openapi.json`, supaya FE tidak lagi menulis tipe dengan tangan. Setelah milestone ini, perubahan bentuk respons inti langsung memecahkan `tsc` di web, bukan baru ketahuan saat aplikasi dipakai.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-03 | Renderer error tunggal di `bootstrap/app.php` dengan `code` dan pesan Indonesia untuk 401, 403, 404, 409 verifikasi, 419, 422, 429 throttle. Tambah skema `ErrorResponse` dan `ValidationErrorResponse`. | Tes per status memeriksa `code`; tes katalog memastikan setiap error `api/*` punya `message` dan `code` | 🔒 [S-03, S-04, S-05](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-04 (inti) | Resource atau anotasi Scramble untuk respons dashboard, options, dan template yang kini ditulis tangan di web. Bentuk JSON tidak berubah. | Bagian inti di "Kontrak manual" `types.ts` web bisa dihapus dan `tsc` tetap lolos | 🔒 [S-07](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-11 | `securitySchemes` cookie session dan header CSRF di `openapi.json` lewat `Scramble::extendOpenApi`. | `openapi.json` memuat skema dan setiap rute non-publik memakainya | 🔒 [S-06](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |

## Rincian pekerjaan

### BE-03 · Renderer error tunggal

Masalah sekarang: error dari controller alur dokumen sudah membawa `code` (misalnya `quota_exceeded`, `invalid_document`), tetapi error bawaan Laravel tidak. 401 membalas `Unauthenticated.`, 403 `This action is unauthorized.`, 404 menyebut nama model, 419 `CSRF token mismatch.`, dan 429 throttle `Too Many Attempts.`, semuanya berbahasa Inggris dan tanpa `code`. Middleware `EnsureEmailIsVerified` membalas 409 berbahasa Inggris tanpa `code`, padahal 409 juga dipakai untuk konflik alur. Web terpaksa menebak dari status HTTP, misalnya menganggap setiap 409 dari `GET /me` sebagai email belum terverifikasi.

Langkah:

1. Tulis proposal kontrak dengan template "Proposal kontrak API". Kolom Error berisi semua `code` di bawah, lalu tunggu persetujuan Louis ([H2](../serah-terima/H2.md)) dan label `kontrak-disetujui`.
2. Di `bootstrap/app.php` bagian `withExceptions`, daftarkan callback `render` yang hanya berlaku untuk `api/*`. Callback ini menerima pengecualian yang sudah disiapkan Laravel: `AuthorizationException` sudah menjadi `AccessDeniedHttpException`, `ModelNotFoundException` menjadi `NotFoundHttpException`, `TokenMismatchException` menjadi `HttpException` 419. `AuthenticationException` dan `ValidationException` tetap utuh. Periksa urutan ini di tes, jangan hanya mengandalkan catatan ini.
3. Pemetaan yang dipasang:

   | Kondisi | Status | `code` |
   |---|---|---|
   | Belum masuk | 401 | `unauthenticated` |
   | Policy menolak | 403 | `forbidden` |
   | Dokumen final dibuka untuk diubah | 403 | `document_final` |
   | Model atau rute tidak ada | 404 | `not_found` |
   | Email belum terverifikasi | 409 | `email_unverified` |
   | Token CSRF tidak cocok | 419 | `csrf_mismatch` |
   | Validasi | 422 | `validation_failed`, `errors` tetap ada |
   | Throttle per menit | 429 | `rate_limited`, header `Retry-After` dipertahankan |

4. Untuk `document_final`, policy mengembalikan `Response::deny('Dokumen ini sudah final.', 'document_final')` dan renderer membaca kode itu dari pengecualian asal.
5. Ubah `EnsureEmailIsVerified` agar membalas `{message, code: "email_unverified"}` dengan pesan Indonesia.
6. Lengkapi `code` yang kosong di controller alur: `pattern_not_ready` dan `source_missing` di `TemplateController`, `revision_changed` dan `proposal_not_ready` di accept, `retention_misconfigured` di status perapihan. Koordinasikan baris AI dengan Hamid, karena controller AI miliknya.
7. Tambah skema `ErrorResponse` dan `ValidationErrorResponse` lewat `Scramble::extendOpenApi`, lalu rujuk dari respons 4xx dan 5xx.
8. Pindahkan baris di doc 05 API dari "Kode yang direncanakan" ke "Kode yang sudah dikirim".

Bukti selesai: satu tes per baris tabel di atas. Ditambah satu tes katalog yang memanggil setiap rute `api/*` dengan kondisi gagal yang paling mudah (misalnya tanpa session) dan memeriksa respons punya `message` dan `code`.

Hindari: mengubah status HTTP yang sudah ada. Menambah `code` aman di v1, mengubah status tidak (doc 05 bagian versi).

### BE-04 bagian inti · Respons terketik

Masalah sekarang: Scramble hanya bisa membaca bentuk respons dari Resource. Respons yang dibangun dengan `response()->json([...])` muncul tanpa skema, sehingga web menulis tipenya dengan tangan di `src/lib/api/types.ts` bagian "Kontrak manual".

Respons inti yang perlu diketik:

| Endpoint | Tipe manual di web |
|---|---|
| `GET /dashboard` | `Dashboard` |
| `GET /laporan/options` | `LaporanOptions` |
| `document_style` di `GET /laporan/{id}` | `DocumentStyle`, `LaporanDetail` |
| `GET /templates` | `TemplateIndex`, `ProviderReadiness` |
| `GET /templates/{id}/analysis` | `TemplateAnalysis` |
| `POST /templates`, `POST /laporan/{id}/import-template` | `UploadReservation` |
| `POST /templates/{id}/document`, `/use` | `OpenedDocument` |

Langkah:

1. Untuk setiap respons, buat Resource kecil (misalnya `DashboardResource`, `UploadReservationResource`) atau beri anotasi tipe yang dibaca Scramble. Resource lebih mudah dirawat.
2. Respons yang sekarang datar (tanpa `data`) harus tetap datar. Pasang `public static $wrap = null;` di Resource tersebut saja. Jangan memanggil `JsonResource::withoutWrapping()`, karena berlaku global dan akan menghapus `data` dari semua respons lain. Bentuk JSON tidak boleh berubah.
3. Respons 202 dan 200 untuk unggah template harus tercantum keduanya.
4. Jalankan `composer api:spec`, lalu coba sinkron di checkout web lokal untuk memastikan tipe yang dihasilkan cocok dengan tipe manual.

Bukti selesai: tes fitur yang sudah ada tetap lulus tanpa diubah, karena bentuk tidak berubah. Di web, mengganti tipe manual inti dengan alias `components['schemas'][...]` lolos `npm run typecheck`.

### BE-11 · Skema keamanan di kontrak

Masalah sekarang: `components.securitySchemes` di `openapi.json` kosong, jadi kontrak tidak menyatakan bahwa rute butuh session dan token CSRF.

Langkah:

1. Di `Scramble::extendOpenApi`, daftarkan skema `apiKey` di cookie bernama `draftaja_session` dan skema `apiKey` di header `X-XSRF-TOKEN`.
2. Terapkan sebagai `security` bawaan. Rute publik (`/auth/login`, `/auth/register`, `/auth/forgot-password`, `/auth/reset-password`) diberi `security: []`.

Bukti selesai: `openapi.json` memuat kedua skema, dan setiap rute di grup `auth:sanctum` memakainya.

## Urutan kerja yang disarankan

1. Proposal kontrak BE-03, sambil menunggu H1 dan H2.
2. BE-04 bagian inti, yang bisa dikerjakan sambil menunggu persetujuan karena bentuk JSON tidak berubah.
3. BE-03 setelah label `kontrak-disetujui`.
4. BE-11.

Kirim [H6](../serah-terima/H6.md) setelah ketiganya merge.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H1 · PM-1 → CORE-2 · kontrak tim v1.1, CODEOWNERS**](../serah-terima/H1.md) | Raja (PM), [PM-1](PM-1.md) | `TEAM_CONTRACT.md` v1.1, CODEOWNERS per repo, branch protection, label. Berlaku untuk semua anggota. | Reviewer wajib sudah berlaku sebelum BE-03 mengubah kontrak error |
| [**H2 · QA-1 → CORE-2 · kasus error inti disetujui**](../serah-terima/H2.md) | Louis (QA), [QA-1](QA-1.md) | Persetujuan daftar `code` error di proposal kontrak endpoint inti | Menulis satu tes per `code` di BE-03 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H6 · CORE-2 → WEB-2 · openapi.json inti baru**](../serah-terima/H6.md) | Alvira (FE), [WEB-2](WEB-2.md) | `openapi.json` dengan skema `ErrorResponse` dan tipe respons dashboard, options, template | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] Proposal kontrak berlabel `kontrak-disetujui` sebelum kode
- [ ] `composer api:spec` dijalankan dan `openapi.json` ikut di-commit
- [ ] Doc 05 API diperbarui
- [ ] H6 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Doc 05, katalog error](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#katalog-error)
- 🔒 [Doc 17, proposal kontrak](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#proposal-kontrak)
- 🔒 [Web doc 04, pemetaan error](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#pemetaan-error)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
