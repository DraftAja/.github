[Profil organisasi](https://github.com/DraftAja)

# Semua milestone

Setiap anggota memegang milestone di lajurnya sendiri. Halaman setiap milestone berisi tujuan, ringkasan butir pekerjaan beserta kriteria terima, lalu rincian setiap butir: masalah yang ada di kode sekarang, langkah pengerjaan, bukti selesai, dan hal yang perlu dihindari. Halaman itu bisa dibaca tanpa membuka dokumentasi di repo privat. Di bagian akhir ada urutan kerja yang disarankan, serah terima masuk dan keluar, serta dokumen rujukan.

| Fase | Raja · PM | Octaf · BE 1 | Hamid · BE 2 | Alvira · FE | Louis · QA |
|---|---|---|---|---|---|
| Fase 0 · Keamanan dan stabilisasi pasca-split | [PM-1](PM-1.md) Fondasi kolaborasi dua repo | [CORE-1](CORE-1.md) Keamanan fase 0<br>[CORE-2](CORE-2.md) Kontrak error dan tipe respons | [AI-1](AI-1.md) Kontrak AI terketik<br>[AI-2](AI-2.md) Provider palsu untuk E2E | [WEB-1](WEB-1.md) Ketahanan tanpa menunggu API<br>[WEB-2](WEB-2.md) Sinkron kontrak baru | [QA-1](QA-1.md) Rencana uji dan tinjauan kontrak<br>[QA-2](QA-2.md) Uji fase 0 |
| Fase 1 · Penyelarasan persona | [PM-2](PM-2.md) Keputusan produk fase 1 | [CORE-3](CORE-3.md) Persona dan alur dokumen | [AI-3](AI-3.md) Persona asisten, kuota, proposal | [WEB-3](WEB-3.md) Final, revisi, kuota, konflik | [QA-3](QA-3.md) Uji fase 1 |
| Fase 2 · Katalog 64 template | [PM-3](PM-3.md) Lingkungan uji nyata | [CORE-4](CORE-4.md) Katalog 64 template<br>[CORE-5](CORE-5.md) Verifikasi PostgreSQL 16 | [AI-4](AI-4.md) Uji provider sungguhan<br>[AI-5](AI-5.md) Regresi prompt nol fakta tambahan | [WEB-4](WEB-4.md) Katalog 8 kategori dan kolom isian<br>[WEB-5](WEB-5.md) E2E MVP dan aksesibilitas | [QA-4](QA-4.md) Uji katalog dan regresi prompt<br>[QA-5](QA-5.md) Gerbang rilis MVP |
| Rilis · minggu 14, 15, 16 | [PM-4](PM-4.md) Feature freeze dan rilis MVP, minggu 14<br>[PM-5](PM-5.md) v1.0-final minggu 15, pameran minggu 16 |   |   |   |   |

## Serah terima

Klik label untuk membuka halaman serah terimanya: isi paket, syarat sebelum dikirim, cara mengirim, cara penerima memeriksa, dan yang dilakukan bila terlambat. Aturan umum dan siklus serah terima ada di [folder serah terima](../serah-terima/README.md).

#### Fase 0 · Keamanan dan stabilisasi pasca-split

| Label di diagram | Dari | Ke | Yang diserahkan | Kanal | Dipakai untuk |
|---|---|---|---|---|---|
| [**H1 · PM-1 → CORE-2 · kontrak tim v1.1, CODEOWNERS**](../serah-terima/H1.md) | Raja (PM)<br>[PM-1](PM-1.md) | Octaf (BE 1)<br>[CORE-2](CORE-2.md) | `TEAM_CONTRACT.md` v1.1, CODEOWNERS per repo, branch protection, label. Berlaku untuk semua anggota. | PR ke `develop`, pengumuman di grup | Reviewer wajib sudah berlaku sebelum BE-03 mengubah kontrak error |
| [**H2 · QA-1 → CORE-2 · kasus error inti disetujui**](../serah-terima/H2.md) | Louis (QA)<br>[QA-1](QA-1.md) | Octaf (BE 1)<br>[CORE-2](CORE-2.md) | Persetujuan daftar `code` error di proposal kontrak endpoint inti | Komentar di issue `kontrak`, label `kontrak-disetujui` | Menulis satu tes per `code` di BE-03 |
| [**H3 · QA-1 → AI-1 · kasus error AI disetujui**](../serah-terima/H3.md) | Louis (QA)<br>[QA-1](QA-1.md) | Hamid (BE 2)<br>[AI-1](AI-1.md) | Persetujuan kasus error endpoint AI (generate, tidy, chat) | Komentar di issue `kontrak`, label `kontrak-disetujui` | Mengunci bentuk respons AI di BE-04 |
| [**H4 · CORE-1 → QA-2 · fase 0 API siap uji**](../serah-terima/H4.md) | Octaf (BE 1)<br>[CORE-1](CORE-1.md) | Louis (QA)<br>[QA-2](QA-2.md) | PR BE-01, BE-02, BE-09, BE-17 yang sudah merge, beserta cara memicu setiap error | Daftar periksa serah terima QA di PR | Menguji kasus negatif fase 0 |
| [**H5 · WEB-1 → QA-2 · fase 0 web siap uji**](../serah-terima/H5.md) | Alvira (FE)<br>[WEB-1](WEB-1.md) | Louis (QA)<br>[QA-2](QA-2.md) | Pasangan commit API dan web, tangkapan layar 360 px dan desktop | Daftar periksa serah terima QA di PR web | Menguji status tak dikenal, mode baca, zona waktu, pesan error |
| [**H6 · CORE-2 → WEB-2 · openapi.json inti baru**](../serah-terima/H6.md) | Octaf (BE 1)<br>[CORE-2](CORE-2.md) | Alvira (FE)<br>[WEB-2](WEB-2.md) | `openapi.json` dengan skema `ErrorResponse` dan tipe respons dashboard, options, template | PR API merge, label `butuh-fe` di issue web | `npm run api:sync && npm run api:types`, lalu mengosongkan "Kontrak manual" |
| [**H7 · AI-1 → WEB-2 · tipe generate, tidy, chat**](../serah-terima/H7.md) | Hamid (BE 2)<br>[AI-1](AI-1.md) | Alvira (FE)<br>[WEB-2](WEB-2.md) | Tipe `Generation`, `CleanupReservation`, `ChatReply` di `openapi.json` | PR API merge, label `butuh-fe` di issue web | FE-08 tanpa tipe tulisan tangan |
| [**H8 · AI-2 → WEB-5 · stub provider untuk E2E**](../serah-terima/H8.md) | Hamid (BE 2)<br>[AI-2](AI-2.md) | Alvira (FE)<br>[WEB-5](WEB-5.md) | Cara mengarahkan `AI_*` ke provider palsu lokal, tertulis di doc 09 API. Juga dipakai Louis. | Issue kesepakatan `be-ai` dan `fe` | E2E perapihan tanpa kunci sungguhan (FE-23) |

#### Fase 1 · Penyelarasan persona

| Label di diagram | Dari | Ke | Yang diserahkan | Kanal | Dipakai untuk |
|---|---|---|---|---|---|
| [**H9 · PM-2 → CORE-3 · kategori dan kolom identitas**](../serah-terima/H9.md) | Raja (PM)<br>[PM-2](PM-2.md) | Octaf (BE 1)<br>[CORE-3](CORE-3.md) | Keputusan 8 kunci kategori, pemetaan nilai lama, dan kolom identitas pengganti kolom akademik | Issue keputusan ditutup, doc 12 diperbarui | Enum `DocumentCategory` dan migrasi kolom (KF-DOK-04) |
| [**H10 · PM-2 → AI-3 · persona asisten**](../serah-terima/H10.md) | Raja (PM)<br>[PM-2](PM-2.md) | Hamid (BE 2)<br>[AI-3](AI-3.md) | Persona dan batas asisten DraftAja | Issue keputusan ditutup, doc 01 atau 11 diperbarui | Mengganti prompt "LaporBot" (KF-AI-05) |
| [**H11 · PM-2 → WEB-3 · teks kebijakan privasi**](../serah-terima/H11.md) | Raja (PM)<br>[PM-2](PM-2.md) | Alvira (FE)<br>[WEB-3](WEB-3.md) | Isi halaman kebijakan privasi | Issue keputusan ditutup | Halaman `/kebijakan-privasi` (FE-15) |
| [**H12 · CORE-3 → WEB-3 · final, revisi, konflik**](../serah-terima/H12.md) | Octaf (BE 1)<br>[CORE-3](CORE-3.md) | Alvira (FE)<br>[WEB-3](WEB-3.md) | `POST /laporan/{id}/finalize`, `POST /laporan/{id}/revisions`, 409 `revision_changed` | PR API merge, label `butuh-fe` di issue web | FE-12 tombol final dan revisi, FE-14 dialog konflik |
| [**H13 · AI-3 → WEB-3 · kuota dan proposal**](../serah-terima/H13.md) | Hamid (BE 2)<br>[AI-3](AI-3.md) | Alvira (FE)<br>[WEB-3](WEB-3.md) | `GET /ai/quota` dan field `proposal` berbentuk objek | PR API merge, label `butuh-fe` di issue web | FE-13 kartu kuota, FE-18 tanpa `JSON.parse` |
| [**H14 · WEB-3 → QA-3 · fase 1 siap uji**](../serah-terima/H14.md) | Alvira (FE)<br>[WEB-3](WEB-3.md) | Louis (QA)<br>[QA-3](QA-3.md) | Fase 1 lengkap di kedua repo: PR web merujuk PR API yang sudah merge | Repo gabungan mencatat pasangan commit, daftar periksa serah terima QA | Menguji transisi status, persona, ekspor per kategori |

#### Fase 2 · Katalog 64 template

| Label di diagram | Dari | Ke | Yang diserahkan | Kanal | Dipakai untuk |
|---|---|---|---|---|---|
| [**H15 · PM-3 → CORE-5 · PostgreSQL 16 sekali pakai**](../serah-terima/H15.md) | Raja (PM)<br>[PM-3](PM-3.md) | Octaf (BE 1)<br>[CORE-5](CORE-5.md) | Instance PostgreSQL 16 sekali pakai (bukan database Azure bersama) dan izin pemakaiannya | Kanal aman, bukan repo | Tes konkurensi, migrasi, upgrade, schema check |
| [**H16 · PM-3 → AI-4 · kunci Groq dan Gemini**](../serah-terima/H16.md) | Raja (PM)<br>[PM-3](PM-3.md) | Hamid (BE 2)<br>[AI-4](AI-4.md) | Kunci Groq dan Gemini, izin pemakaian terbatas, nama `GEMINI_MODEL` yang aktif | Kanal aman, bukan chat grup terbuka | Smoke test provider dan regresi prompt |
| [**H17 · CORE-4 → WEB-4 · kategori dan kolom isian**](../serah-terima/H17.md) | Octaf (BE 1)<br>[CORE-4](CORE-4.md) | Alvira (FE)<br>[WEB-4](WEB-4.md) | `GET /categories`, `template_fields`, `laporans.field_values` di `openapi.json` | PR API merge, label `butuh-fe` di issue web | FE-17 katalog dan kolom isian |
| [**H18 · CORE-4 → QA-4 · 64 template di seeder**](../serah-terima/H18.md) | Octaf (BE 1)<br>[CORE-4](CORE-4.md) | Louis (QA)<br>[QA-4](QA-4.md) | 64 template di `TemplateSeeder` sesuai `docs/templates/` | PR API merge | Memeriksa kesesuaian spesifikasi dan seeder |
| [**H19 · AI-5 → QA-4 · hasil regresi prompt**](../serah-terima/H19.md) | Hamid (BE 2)<br>[AI-5](AI-5.md) | Louis (QA)<br>[QA-4](QA-4.md) | Keluaran minimal 10 pasang catatan untuk Groq dan Gemini | Dicatat di doc 10 dan `docs-ai/07-testing.md` | Pemeriksaan manual fakta tambahan dan penanda `[perlu dilengkapi]` |
| [**H20 · WEB-5 → QA-5 · E2E MVP hijau**](../serah-terima/H20.md) | Alvira (FE)<br>[WEB-5](WEB-5.md) | Louis (QA)<br>[QA-5](QA-5.md) | E2E P1 dan "P2 bagian template" lulus di 360 px dan 1440 px | CI web hijau di `develop` | Kriteria rilis butir 5 |
| [**H21 · CORE-5 → PM-4 · hasil tes PostgreSQL 16**](../serah-terima/H21.md) | Octaf (BE 1)<br>[CORE-5](CORE-5.md) | Raja (PM)<br>[PM-4](PM-4.md) | Hasil seluruh rangkaian tes di PostgreSQL 16 | Doc 10 bagian verifikasi | Kriteria rilis butir 1 |
| [**H22 · QA-5 → PM-4 · label siap-rilis**](../serah-terima/H22.md) | Louis (QA)<br>[QA-5](QA-5.md) | Raja (PM)<br>[PM-4](PM-4.md) | Label `siap-rilis` pada pasangan commit API dan web | Issue di repo gabungan DraftAja | Membuat `release/*` dan tag pasangan versi |