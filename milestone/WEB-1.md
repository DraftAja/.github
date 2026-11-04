[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [WEB-2](WEB-2.md)

# WEB-1 · Ketahanan tanpa menunggu API

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Frontend-Nextjs |
| Contoh branch | `fix/fe-web1-status-bawaan` |
| Mulai setelah | Repo dipisah, 3 November 2026 |
| Lanjut ke | [WEB-2](WEB-2.md) |

## Tujuan

Antarmuka tidak rusak ketika API menambah status atau kode baru, dan tampilan memenuhi aturan dasar, tanpa menunggu perubahan API. Semua butir di milestone ini bisa dikerjakan hari ini dengan API yang ada.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-01 | Label untuk 4 status rencana dan cabang bawaan "Status tidak dikenal" di `StatusPill` dan `TemplateCard`. | Tes Vitest: status tak dikenal menghasilkan label bawaan tanpa error | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |
| FE-02 | `readOnly = status !== 'draft'` di editor. | Tes: `in_review` dan nilai tak dikenal membuka mode baca | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |
| FE-04 | Sembunyikan pilihan "Jenis dokumen" akademik sampai `category` tersedia. | Tidak ada istilah kolom "Hindari" yang tampil | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-05 | Batas waktu polling: 5 menit analisis, 10 menit perapihan, lalu pesan "Proses ini lebih lama dari biasanya." | Pesan tampil setelah batas waktu | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-06 | `timeZone: 'Asia/Jakarta'` di `formatDate` dan `formatDateTime`. | Tes Vitest: `2026-11-03T18:30:00Z` tampil sebagai 4 November 2026 | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-07 | Token `line-strong` dengan kontras minimal 3:1 untuk border input dan checkbox. | Rasio kontras dihitung ulang dan dicatat di web doc 05 | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-09 | Langkah peringatan drift kontrak di job E2E (contoh YAML di doc 17 API). | PR web yang tertinggal kontrak memunculkan anotasi peringatan | 🔒 [doc 17, menjaga kontrak](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#menjaga-kontrak-tetap-sinkron) |
| FE-16 | Petakan 400 `stateful_origin_required` ke teks umum. | Tes Vitest untuk `userMessage` | 🔒 [web doc 04 integrasi API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md) |

## Rincian pekerjaan

### FE-01 · Status bawaan

Masalah sekarang: `src/components/ui/StatusPill.tsx` membaca `STATUS[status]` dari objek yang hanya berisi `draft` dan `final`. API boleh menambah nilai enum di v1 (doc 05 API), dan `in_review` serta `cancelled` sudah direncanakan. Begitu salah satunya dikirim, `entry` bernilai `undefined` dan komponen gagal render, sehingga daftar dokumen dan beranda ikut rusak.

Langkah:

1. Tambah label untuk keempat status sesuai web doc 01: `draft` Draf (warning), `in_review` Ditinjau (info), `final` Final (success), `cancelled` Dibatalkan (danger).
2. Tambah cabang bawaan: nilai lain tampil "Status tidak dikenal" dengan tone `neutral`.
3. Ubah tipe prop menjadi `LaporanStatus | (string & {})`, supaya nilai baru dari API tidak memecahkan `tsc` sebelum kontrak disinkron.
4. Terapkan pola yang sama di peta `analysis_status` di `TemplateCard.tsx` (`uploaded`, `analyzing`, `ready`, `failed`).

Bukti selesai: tes Vitest untuk fungsi pemetaan label (pisahkan dari komponen supaya bisa diuji tanpa DOM) dengan nilai dikenal dan tak dikenal.

### FE-02 · Mode baca untuk semua status selain draf

Masalah sekarang: `DocumentEditor.tsx` menetapkan `readOnly = laporan.status === 'final'`. Dokumen `in_review` akan terbuka untuk disunting, lalu setiap simpan otomatis ditolak API dengan 403.

Langkah: ganti menjadi `readOnly = laporan.status !== 'draft'`. Setelah API mengirim izin per dokumen (misalnya `can.update`, rencana fase 3), pakai izin itu.

Bukti selesai: tes Vitest untuk fungsi penentu mode baca dengan `draft`, `final`, `in_review`, dan nilai acak.

### FE-04 · Jenis dokumen akademik disembunyikan

Masalah sekarang: `CreateDocumentForm.tsx` dan `UsePatternDialog.tsx` menampilkan `report_types` dari API apa adanya: Makalah, Proposal, Laporan Praktikum, Studi Kasus, Skripsi. Ini melanggar KN-09 dan persona produk.

Langkah: sembunyikan pilihan "Jenis dokumen". Kirim satu nilai teknis yang disepakati dengan Octaf, misalnya `Proposal`, sampai `category` tersedia di CORE-3. Setelah itu, form memakai 8 kategori (WEB-4).

Bukti selesai: tidak ada istilah dari kolom "Hindari" web doc 05 yang tampil di `/dokumen/baru` dan di dialog "Gunakan pola template".

### FE-05 · Batas waktu polling

Masalah sekarang: polling analisis pola (`useTemplateAnalysis`, interval naik dari 1 sampai 8 detik) dan perapihan (`CleanupPanel`, 2 detik) berjalan tanpa batas selama status belum berubah. Bila worker antrean mati, layar menunggu selamanya. API sendiri memulihkan pekerjaan macet setelah 3.060 detik.

Langkah: simpan waktu mulai polling. Setelah 5 menit untuk analisis atau 10 menit untuk perapihan, hentikan polling dan tampilkan "Proses ini lebih lama dari biasanya. Muat ulang halaman untuk memeriksa lagi." Jangan menawarkan "Coba lagi" yang membuat `request_key` baru, karena pekerjaan lama masih bisa selesai.

Bukti selesai: tes Vitest untuk fungsi `refetchInterval` dengan waktu tiruan.

### FE-06 · Zona waktu WIB

Masalah sekarang: `src/lib/format.ts` membuat `Intl.DateTimeFormat('id-ID', ...)` tanpa `timeZone`. Server Next berjalan dalam UTC dan browser dalam WIB, jadi tanggal dekat tengah malam bisa berbeda antara render server dan klien. Perbedaan itu bisa menimbulkan peringatan hydration.

Langkah: tambahkan `timeZone: 'Asia/Jakarta'` ke kedua formatter. Periksa `timeAgo` tetap benar.

Bukti selesai: tes Vitest bahwa `2026-11-03T18:30:00Z` tampil sebagai 4 November 2026.

### FE-07 · Kontras border input

Masalah sekarang: border input dan checkbox memakai token `line` (`#dce3ed`), kontras 1,29:1 terhadap putih. WCAG 1.4.11 meminta minimal 3:1 untuk batas komponen yang perlu dikenali.

Langkah: tambah token `line-strong` di `@theme` `globals.css` dengan kontras minimal 3:1 terhadap `surface`, lalu pakai untuk border input, select, textarea, dan checkbox di `Field.tsx`. `line` tetap untuk garis pemisah dekoratif. Hitung ulang rasio dan catat di tabel kontras web doc 05.

### FE-09 · Peringatan drift kontrak

Masalah sekarang: `npm run api:check` hanya memastikan `schema.d.ts` dibangkitkan dari `contracts/openapi.json` milik web. Tidak ada pemeriksaan apakah salinan itu sama dengan `docs/api/openapi.json` di `develop` API, jadi web bisa tertinggal kontrak tanpa CI merah.

Langkah: di job E2E `ci.yml`, setelah langkah checkout API ke direktori `api`, tambahkan langkah yang membandingkan `contracts/openapi.json` dengan `api/docs/api/openapi.json` dan mencetak `::warning::` bila berbeda. Contoh YAML ada di doc 17 API. Langkah ini sengaja hanya memberi peringatan, karena web boleh tertinggal sementara selama perubahan API bersifat menambah.

### FE-16 · Pesan 400 stateful origin

Masalah sekarang: bila Origin web tidak terdaftar di `SANCTUM_STATEFUL_DOMAINS`, API membalas 400 `stateful_origin_required` dengan pesan teknis yang menyebut nama variabel env. `userMessage()` meneruskan pesan itu ke pengguna.

Langkah: tambah cabang di `src/lib/api/errors.ts` untuk `code === 'stateful_origin_required'` dengan teks "Sesi belum dapat dibuat. Muat ulang halaman, lalu hubungi admin bila masih terjadi.", lalu catat `console.error` dengan pesan asli.

Bukti selesai: tes Vitest untuk `userMessage` dengan `ApiError` berstatus 400 dan kode tersebut.

## Urutan kerja yang disarankan

1. FE-01 dan FE-02 dalam satu PR, karena keduanya tentang status.
2. FE-06 dan FE-16, yang kecil dan langsung punya tes.
3. FE-04 dan FE-07.
4. FE-05 dan FE-09.

Kirim [H5](../serah-terima/H5.md) setelah semua merge.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H5 · WEB-1 → QA-2 · fase 0 web siap uji**](../serah-terima/H5.md) | Louis (QA), [QA-2](QA-2.md) | Pasangan commit API dan web, tangkapan layar 360 px dan desktop | Daftar periksa serah terima QA di PR web |

## Selesai bila

- [ ] Tes Vitest untuk FE-01, FE-02, FE-05, FE-06, FE-16 lulus
- [ ] Tangkapan layar 360 px dan desktop terlampir di PR yang mengubah tampilan
- [ ] Baris FE terkait ditandai selesai di web doc 06
- [ ] H5 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Web doc 04, nilai enum](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#nilai-enum)
- 🔒 [Web doc 05, kontras terukur](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md#kontras-terukur)
- 🔒 [Web doc 03, editor](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#editor-dokumenid)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
