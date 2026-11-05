[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-6](QA-6.md)

# QA-7 · Uji dan gerbang Fase U

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel dan Frontend-Nextjs |
| Contoh branch | `test/qa-qa7-uji-fase-u` |
| Mulai setelah | [QA-6](QA-6.md) selesai, menerima H33 dan H34 |
| Lanjut ke | selesai |

## Tujuan

Membuktikan bahwa angka usaha benar, data antarusaha terpisah, berkas terbuka di Excel, layar bisa dipakai pemilik UMKM, dan pengingat tepat waktu. Lalu memberi label `siap-rilis` Fase U, atau menyebut bagian yang harus pindah ke fase 5.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KN-12 | Jalankan tes rumus di PostgreSQL 16 dan bandingkan dengan lembar hitung QA-6 | U01 sampai U17 lulus, hasil sama dengan lembar hitung | 🔒 [doc 21](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/21-perhitungan-usaha.md#kasus-uji) |
| KN-14 | Buka berkas ekspor setiap template di Excel dan LibreOffice | Tanpa pesan perbaikan, `=SUM` sama dengan baris total | 🔒 [KN-14](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#kebutuhan-nonfungsional) |
| KN-11, KN-13 | Ukur performa dengan `UsahaLoadSeeder` | p95 sesuai KN-11, dicatat di doc 10 | 🔒 [doc 16, Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#fase-u-dashboard-usaha) |
| KN-15 | Periksa pengingat dengan jadwal nyata di lingkungan uji | Terkirim paling lambat 10 menit setelah jadwal, tidak ganda | 🔒 [doc 06, W16](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#w16-peringatan-dan-pengingat) |
| KN-10 | Uji pengguna dengan 5 pemilik UMKM | Hasil dibandingkan dengan target usulan KN-10 | Rencana uji QA-6 |
| Gerbang | Kriteria selesai Fase U di doc 20 | Label `siap-rilis` Fase U atau daftar bagian yang pindah ke fase 5 | 🔒 [doc 20](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/20-dashboard-usaha.md#kriteria-selesai-fase-u) |

## Rincian pekerjaan

### KN-12 · Akurasi

Masalah sekarang: tes rumus ditulis BE dari doc 21. QA-6 sudah memastikan doc 21 benar, tetapi belum ada bukti bahwa layar dan berkas menampilkan angka yang sama.

Langkah:

1. Jalankan `php vendor/bin/phpunit -c phpunit.pgsql.xml --filter=Usaha` di PostgreSQL 16 sekali pakai.
2. Masukkan data kasus U01, U02, U08, dan U09 lewat antarmuka, lalu bandingkan Ringkasan, invoice, rekap, dan berkas XLSX dengan lembar hitung.

Bukti selesai: tangkapan layar dan berkas terlampir di issue gerbang.

### KN-14 · Kompatibilitas berkas

Langkah:

1. Unduh XLSX dan CSV untuk setiap template dan invoice.
2. Buka di Microsoft Excel dan LibreOffice Calc. Periksa tanggal, rupiah, `=SUM`, dan nama mitra `=HYPERLINK("x")` yang harus tampil sebagai teks.

Bukti selesai: tabel hasil per berkas di doc 10.

### Gerbang Fase U

Langkah:

1. Periksa delapan kriteria selesai Fase U di doc 20.
2. Pasang label `siap-rilis-fase-u` pada pasangan commit yang lulus, atau tulis bagian yang gagal beserta usulan pindah ke fase 5.
3. Kirim [H35](../serah-terima/H35.md).

Hindari: memberi label untuk sebagian Fase U tanpa menyebut bagian yang tidak ikut.

## Urutan kerja yang disarankan

1. KN-12 dan KN-14 setelah H34.
2. Regresi prompt manual setelah H33.
3. KN-11, KN-13, KN-15, lalu uji pengguna.
4. Kirim [H35](../serah-terima/H35.md).

## Serah terima masuk

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H33 · AI-7 → QA-7 · hasil regresi prompt teks ke tabel**](../serah-terima/H33.md) | Hamid (BE 2), [AI-7](AI-7.md) | Keluaran regresi Groq dan Gemini | Pemeriksaan manual angka tambahan |
| [**H34 · WEB-7 → QA-7 · Fase U siap uji**](../serah-terima/H34.md) | Alvira (FE), [WEB-7](WEB-7.md) | Pasangan commit API dan web | Semua uji di halaman ini |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H35 · QA-7 → PM-4 · label siap-rilis Fase U**](../serah-terima/H35.md) | Raja (PM), [PM-4](PM-4.md) | Label `siap-rilis-fase-u` atau daftar bagian yang pindah ke fase 5 | Issue di repo gabungan DraftAja |

## Selesai bila

- [ ] Hasil KN-10 sampai KN-15 tercatat di doc 10
- [ ] Bug P0 dan P1 Fase U selesai atau diterima tertulis oleh PM
- [ ] H35 terkirim

## Dokumen rujukan lain

- 🔒 [Kriteria rilis MVP](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#kriteria-rilis-mvp)
- 🔒 [web doc 07](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
