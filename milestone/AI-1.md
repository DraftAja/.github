[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [AI-2](AI-2.md)

# AI-1 · Kontrak AI terketik

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-ai-ai1-tipe-respons` |
| Saran urutan | menerima H3. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [AI-2](AI-2.md) |

## Tujuan

Respons endpoint AI muncul di `openapi.json`, supaya FE memakai tipe hasil generate, bukan tipe tulisan tangan. Endpoint AI punya kasus error paling beragam di API, jadi semuanya harus tercantum di kontrak sebelum web bercabang berdasarkan `code`.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-04 (AI) | Resource atau anotasi Scramble untuk `Generation`, `CleanupReservation`, dan `ChatReply`. Bentuk JSON tidak berubah. | Bagian AI di "Kontrak manual" `types.ts` web bisa dihapus dan `tsc` tetap lolos | 🔒 [S-07](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| S-10 | Status hasil AI di kontrak sama dengan `AiStatus` (`pending`, `succeeded`, `failed`). | Tipe status di web berasal dari kontrak | 🔒 [S-10](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| S-16 | Menyelaraskan `docs-ai/05-api.md` dengan doc 05 dan menghapus sebutan Inertia dan LaporBot di `docs-ai/01-arsitektur.md`. | Tidak ada pertentangan antara `docs-ai/` dan doc 05 | 🔒 [S-16](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#proses-lintas-repo) |

## Rincian pekerjaan

### BE-04 bagian AI · Respons AI terketik

Masalah sekarang: `AiContentController`, `AiChatController`, dan `DocumentCleanupController` membangun respons dengan `response()->json([...])`. Scramble tidak bisa membaca bentuknya, jadi web menulis tipe `Generation`, `GenerationStatus`, `CleanupReservation`, dan `ChatReply` dengan tangan di `src/lib/api/types.ts`.

Respons yang perlu diketik:

| Endpoint | Field | Catatan |
|---|---|---|
| `GET /ai/generations/{id}` | `generation_id`, `status`, `operation`, `progress_done`, `progress_total`, `accepted_at`, `output` | Untuk `operation = document_cleanup` juga `source_revision`, `expires_at`, `expired`, `can_accept`. Ditandai opsional. |
| `POST /laporan/{id}/tidy` | `generation_id`, `status`, `source_revision`, `progress_done`, `progress_total`, `reused` | 202 untuk baru, 200 untuk dipakai ulang |
| `POST /ai/chat` | `reply`, `generation_id` | |
| `POST /ai/generate` | `result`, `generation_id` | Tidak dipakai web, tetap dikontrakkan |
| `POST /ai/generations/{id}/accept` | `accepted_at` | |

Langkah:

1. Tulis proposal kontrak berisi tabel di atas dan semua kasus error, mention Louis dan Alvira, lalu langsung kerjakan. Persetujuan Louis ([H3](../serah-terima/H3.md)) boleh menyusul; bila ditolak, sesuaikan proposal dan kode.
2. Buat Resource datar (`public static $wrap = null;`) per respons, misalnya `GenerationStatusResource`, `CleanupReservationResource`, `ChatReplyResource`. Field yang hanya ada untuk perapihan memakai `$this->when(...)`, supaya Scramble menandainya opsional.
3. Cantumkan semua status error yang disetujui di H3. Bila CORE-2 sudah merge, rujuk skema `ErrorResponse` miliknya. Bila belum, koordinasikan dengan Octaf supaya kedua PR memakai skema yang sama.
4. Lengkapi `code` yang masih kosong di controller AI: `proposal_not_ready` dan `revision_changed` di accept, `retention_misconfigured` di status perapihan.
5. Jalankan `composer api:spec`.

Bukti selesai: semua tes di `tests/Feature/Ai` dan tes perapihan di `tests/Feature/Documents` tetap lulus tanpa diubah, karena bentuk JSON tidak berubah. Di checkout web lokal, keempat tipe manual bisa diganti alias dari `components['schemas']` dan `npm run typecheck` lolos.

### S-10 · Enum status di kontrak

Masalah sekarang: tipe `GenerationStatus` di web memuat `processing`, padahal `AiStatus` hanya `pending`, `succeeded`, `failed`. Status `processing` hanya ada di `PatternStatus`, untuk analisis pola.

Langkah: pastikan `status` di skema respons generation memakai enum `AiStatus`, sehingga `openapi.json` menuliskannya sebagai enum tiga nilai. Analisis pola tetap memakai `analysis_status` dan `PatternStatus` di respons template.

### S-16 · Dokumentasi AI selaras

Masalah sekarang: `docs-ai/` tidak ditulis ulang saat split. `docs-ai/01-arsitektur.md` masih menyebut Inertia dan LaporBot, dan `docs-ai/05-api.md` belum dicocokkan dengan doc 05.

Langkah: perbarui `docs-ai/01-arsitektur.md` untuk arsitektur dua repo, lalu ubah `docs-ai/05-api.md` menjadi rujukan ke doc 05 ditambah hal yang khusus modul AI, seperti cara kuota dihitung dan bentuk keluaran terstruktur. Hindari menyalin tabel endpoint, supaya tidak ada dua sumber.

## Urutan kerja yang disarankan

1. Proposal kontrak, mention reviewer, lalu langsung lanjut.
2. S-16.
3. BE-04 bagian AI dan S-10 dalam satu PR.

Kirim [H7](../serah-terima/H7.md) setelah PR itu merge.

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H3 · QA-1 → AI-1 · kasus error AI disetujui**](../serah-terima/H3.md) | Louis (QA), [QA-1](QA-1.md) | Persetujuan kasus error endpoint AI (generate, tidy, chat) | Mengunci bentuk respons AI di BE-04 |

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H7 · AI-1 → WEB-2 · tipe generate, tidy, chat**](../serah-terima/H7.md) | Alvira (FE), [WEB-2](WEB-2.md) | Tipe `Generation`, `CleanupReservation`, `ChatReply` di `openapi.json` | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] `composer api:spec` dijalankan dan `openapi.json` ikut di-commit
- [ ] `docs-ai/01` dan `docs-ai/05` tidak bertentangan dengan doc 05
- [ ] H7 dilaporkan di issue dan reviewer di-mention ke Alvira

## Dokumen rujukan lain

- 🔒 [Backlog backend](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-backend)
- 🔒 [Doc 05, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#ai)
- 🔒 [docs-ai](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/README.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
