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

Antarmuka tidak rusak ketika API menambah status atau kode baru, dan tampilan memenuhi aturan dasar, tanpa menunggu perubahan API.

## Yang dikerjakan

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

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H5 · WEB-1 → QA-2 · fase 0 web siap uji** | Louis (QA), [QA-2](QA-2.md) | Pasangan commit API dan web, tangkapan layar 360 px dan desktop | Daftar periksa serah terima QA di PR web |

## Selesai bila

- [ ] Semua tes Vitest di kolom kriteria lulus
- [ ] Diuji di 360 px dan desktop
- [ ] H5 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Web doc 06 backlog](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md)
- 🔒 [Web doc 05 sistem desain](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
