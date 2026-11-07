[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-1](QA-1.md) · Berikutnya: [QA-3](QA-3.md)

# QA-2 · Uji fase 0

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-qa2-fase-0` |
| Saran urutan | [QA-1](QA-1.md) selesai, menerima H4, H5. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [QA-3](QA-3.md) |

## Tujuan

Membuktikan perbaikan keamanan fase 0 menolak kasus yang salah, dan antarmuka tetap aman ketika API berubah.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 fase 0 | Kasus negatif BE-01, BE-02, BE-09, BE-17: ekspor milik orang lain, judul terlalu panjang, gambar tanpa session, HTML berisi skrip. | Setiap kasus menghasilkan status dan `code` yang diharapkan | 🔒 [doc 16, tes fase 0](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| Web doc 07 | Memicu keadaan error secara manual: kuota habis, provider belum siap, session kedaluwarsa, CSRF kedaluwarsa, origin tidak stateful, DOCX ditolak. | Setiap keadaan menampilkan pesan Indonesia yang sesuai | 🔒 [web doc 07, serah terima ke QA](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#serah-terima-ke-qa) |
| WEB-1 | Memeriksa status tak dikenal, mode baca, dan tanggal dekat tengah malam WIB di 360 px dan desktop. | Tidak ada error render | 🔒 [doc 17, serah terima ke QA](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#serah-terima-ke-qa) |

## Rincian pekerjaan

### Persiapan

1. Jalankan pasangan commit dari H4 dan H5 di lokal: API dengan SQLite, `queue:work`, dan seeder; web di `localhost:3000`.
2. Siapkan dua akun uji: akun A pemilik dokumen, akun B pengguna lain. Daftarkan akun baru, jangan memakai akun seeder, supaya hasil bisa diulang.
3. Siapkan berkas uji: DOCX valid kecil, DOCX di atas 10 MB, berkas ZIP berekstensi `.docx`, gambar PNG kecil, berkas SVG, berkas teks berekstensi `.png`.

### Kasus negatif API

| Kasus | Langkah | Hasil yang diharapkan |
|---|---|---|
| Ekspor dokumen orang lain | Akun B memanggil `GET /api/v1/laporan/{id A}/export/pdf` lewat proxy web | 403 |
| Ekspor dokumen final sendiri | Akun A, dokumen final | 200, berkas PDF dan DOCX terunduh |
| Judul terlalu panjang | `POST /api/v1/templates/{id}/use` dengan `judul` 300 karakter | 422, error di field `judul` |
| Tanpa judul | Body kosong | Dokumen berjudul nama template, tanpa awalan "Laporan:" |
| Gambar tanpa masuk | Buka URL gambar editor di jendela samaran | 401 |
| Gambar akun lain | Akun B membuka URL gambar milik A | 403 |
| Berkas bukan gambar | Unggah berkas teks berekstensi `.png` dan berkas SVG di editor | 422 dengan pesan Indonesia |
| HTML berbahaya di isi bagian | Langkah rinci ada di PR BE-17 | Skrip tidak tersimpan; pratinjau membawa header CSP |
| Akun uji di produksi | Jalankan seeder dengan `APP_ENV=production` di salinan lokal | Tidak ada akun `@drafaja.test` |

Untuk memanggil endpoint langsung, pakai DevTools di tab web yang sudah masuk, supaya cookie session dan `X-XSRF-TOKEN` ikut. Jangan memanggil port 8000 langsung, karena itu bukan jalur pengguna.

### Keadaan error di antarmuka

Cara memicu setiap keadaan ada di web doc 07 bagian serah terima ke QA. Untuk setiap keadaan, catat teks yang tampil dan pastikan tidak menyebut kode HTTP, nama variabel env, atau pesan bawaan Laravel berbahasa Inggris.

| Keadaan | Teks yang diharapkan, dari web doc 04 |
|---|---|
| Kuota habis | "Kuota AI harian kamu sudah habis. Coba lagi besok." |
| Provider belum siap | "Provider AI yang dipilih belum tersedia. Pilih provider lain atau coba lagi nanti." |
| Session kedaluwarsa | Dialihkan ke `/masuk?next=...` |
| CSRF kedaluwarsa | Aksi berhasil tanpa pesan error, karena klien mengulang sekali |
| Origin tidak stateful | "Sesi belum dapat dibuat. Muat ulang halaman, lalu hubungi admin bila masih terjadi." |
| DOCX ditolak | Pesan di kolom berkas |

### Ketahanan antarmuka

Pakai cara memicu dari [H5](../serah-terima/H5.md): ubah status dokumen menjadi `in_review` lewat `php artisan tinker`, ubah `updated_at` menjadi dekat tengah malam WIB, lalu periksa daftar dokumen, beranda, dan editor di 360 px dan desktop.

### Pelaporan

Setiap kegagalan dilaporkan dengan template "Laporan bug", ditautkan ke PR asalnya. Hasil ringkas (tanggal, pasangan commit, lulus atau gagal per kasus) dicatat di rencana uji QA-1.

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H4 · CORE-1 → QA-2 · fase 0 API siap uji**](../serah-terima/H4.md) | Octaf (BE 1), [CORE-1](CORE-1.md) | PR BE-01, BE-02, BE-09, BE-17 yang sudah merge, beserta cara memicu setiap error | Menguji kasus negatif fase 0 |
| [**H5 · WEB-1 → QA-2 · fase 0 web siap uji**](../serah-terima/H5.md) | Alvira (FE), [WEB-1](WEB-1.md) | Pasangan commit API dan web, tangkapan layar 360 px dan desktop | Menguji status tak dikenal, mode baca, zona waktu, pesan error |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Laporan QA dibuat dengan template 🔒 [web `docs/qa`](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/qa/README.md), dicetak ke PDF dengan `npm run qa:report`, dan PDF-nya dilampirkan di issue serah terima yang diuji
- [ ] Semua kasus di atas punya hasil tercatat
- [ ] Bug yang ditemukan sudah dilaporkan, tanpa bug `priority-high` terbuka
- [ ] Hasil ringkas tercatat di rencana uji

## Dokumen rujukan lain

- 🔒 [Web doc 04, pemetaan error](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/04-integrasi-api.md#pemetaan-error)
- 🔒 [Doc 07, risiko](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#risiko)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
