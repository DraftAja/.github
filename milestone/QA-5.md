[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-4](QA-4.md) · Berikutnya: [QA-6](QA-6.md)

# QA-5 · Gerbang rilis MVP

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-qa5-gerbang-rilis` |
| Mulai setelah | [QA-4](QA-4.md) selesai, menerima H20 |
| Lanjut ke | [QA-7](QA-7.md) bila Fase U dikerjakan, selain itu selesai |

## Tujuan

Memberi tanda bahwa satu pasangan commit API dan web layak dirilis sebagai MVP. Tanda ini satu-satunya dasar bagi Raja untuk membuat tag rilis.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 | Memeriksa 7 kriteria rilis MVP. | Setiap kriteria punya bukti tertulis | 🔒 [doc 16, kriteria rilis MVP](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#kriteria-rilis-mvp) |
| Doc 16 uji peramban | Skenario peramban di Chromium pada 360 px dan 1440 px, lalu sekali di Firefox dan WebKit. | Semua lulus; hasil ringkas tercatat di doc 10 | 🔒 [doc 16, uji peramban](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#uji-peramban) |
| Doc 17 | Memasang label `siap-rilis` pada pasangan commit. | Label terpasang di issue repo gabungan | 🔒 [doc 17, label issue](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#label-issue) |

## Rincian pekerjaan

### Tujuh kriteria rilis

| No | Kriteria, dari doc 16 | Bukti yang dikumpulkan |
|---|---|---|
| 1 | Semua tes lulus di SQLite, dan seluruh rangkaian sudah dijalankan sekali di PostgreSQL 16 dengan hasil tercatat | Tautan run CI `develop` (job SQLite dan PostgreSQL hijau tanpa `continue-on-error`) dan hasil dari [H21](../serah-terima/H21.md) |
| 2 | Risiko R1 sampai R9 dan temuan P0 di doc 18 selesai atau diterima tertulis oleh PM | Daftar per risiko dengan tautan PR atau komentar penerimaan Raja |
| 3 | Tidak ada istilah kolom "Hindari" di teks yang tampil | Hasil pencarian seperti di QA-3 |
| 4 | Regresi prompt lulus dengan nol fakta tambahan untuk Groq dan Gemini | Hasil AI-5 dan pemeriksaan manual QA-4 |
| 5 | Skenario peramban P1 dan "P2 bagian template" lulus di 360 px dan 1440 px | Tautan run E2E dari [H20](../serah-terima/H20.md) dan hasil uji manual Firefox dan WebKit |
| 6 | Label status di doc 01, 10, dan 12 sesuai kode | Pemeriksaan acak 10 baris terhadap kode |
| 7 | `.env.example` memuat semua variabel baru tanpa nilai | `php artisan env:verify` lolos di CI; perbandingan `.env.example` dengan `docs/ENV_CONTRACT.md` |

Kriteria yang tidak terpenuhi tidak boleh ditandai lulus. Tulis statusnya dan minta Raja memutuskan: perbaiki dulu atau terima risiko tertulis.

### Uji peramban

| Skenario | Peramban | Lebar |
|---|---|---|
| P1: notula dari catatan rapat sampai unduh PDF | Chromium | 360 px dan 1440 px |
| P2 bagian template: unggah DOCX, tunggu analisis, buat dokumen | Chromium | 360 px dan 1440 px |
| P1 dan P2 bagian template | Firefox, WebKit | 1440 px, sekali |

Bagian Chromium sudah dibuktikan E2E dari H20. Firefox dan WebKit diuji manual, atau dengan `npx playwright test --project` bila Alvira menambahkan profilnya. Hasil ringkas (tanggal, peramban, lebar, lulus atau gagal) dicatat di doc 10.

### Pasangan commit

1. Ambil commit terbaru repo gabungan DraftAja setelah semua milestone fase 2 merge.
2. Catat SHA `backend` dan `frontend` dari commit itu.
3. Jalankan semua pemeriksaan di atas pada pasangan itu. Bila ada merge baru ke `develop` di salah satu repo selama pemeriksaan, ulangi pemeriksaan yang terdampak.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H20 · WEB-5 → QA-5 · E2E MVP hijau**](../serah-terima/H20.md) | Alvira (FE), [WEB-5](WEB-5.md) | E2E P1 dan "P2 bagian template" lulus di 360 px dan 1440 px | Kriteria rilis butir 5 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H22 · QA-5 → PM-4 · label siap-rilis**](../serah-terima/H22.md) | Raja (PM), [PM-4](PM-4.md) | Label `siap-rilis` pada pasangan commit API dan web | Issue di repo gabungan DraftAja |

## Selesai bila

- [ ] Laporan QA dibuat dengan template 🔒 [web `docs/qa`](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/qa/README.md), dicetak ke PDF dengan `npm run qa:report`, dan PDF-nya dilampirkan di issue serah terima yang diuji
- [ ] Tujuh kriteria punya bukti atau penerimaan risiko tertulis
- [ ] Hasil uji peramban tercatat di doc 10
- [ ] H22 terkirim ke Raja

## Dokumen rujukan lain

- 🔒 [Doc 10, verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#verifikasi)
- 🔒 [Doc 18, temuan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#temuan)
- [Repo gabungan DraftAja](https://github.com/DraftAja/DraftAja) 🔒

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
