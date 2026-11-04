[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-4](WEB-4.md)

# WEB-5 · E2E MVP dan aksesibilitas

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Frontend-Nextjs |
| Contoh branch | `test/fe-web5-e2e-mvp` |
| Mulai setelah | [WEB-4](WEB-4.md) selesai, menerima H8 |
| Lanjut ke | selesai |

## Tujuan

Alur MVP dibuktikan otomatis di peramban, desktop dan ponsel, termasuk pemeriksaan aksesibilitas dasar. Hasilnya menjadi bukti kriteria rilis butir 5.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-22 | E2E: unggah template sampai `ready`, lalu buat dokumen darinya. | Lulus di profil desktop dan ponsel | 🔒 [web doc 07, yang wajib ditambah](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#yang-wajib-ditambah) |
| FE-23 | E2E: perapihan sampai terima usulan, memakai provider palsu dari H8. | Lulus tanpa memanggil Groq atau Gemini sungguhan | 🔒 [web doc 07, aturan E2E](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#aturan-menulis-tes-e2e) |
| FE-24 | E2E: unduh PDF dari antarmuka. | Lulus di profil desktop dan ponsel | 🔒 [web doc 07, yang wajib ditambah](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#yang-wajib-ditambah) |
| FE-26 | Pemeriksaan aksesibilitas otomatis di 5 halaman utama (KN-02). Dependensi baru seperti `@axe-core/playwright` butuh persetujuan tim. | Tidak ada pelanggaran kontras, label, atau fokus | 🔒 [doc 12, nonfungsional](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#kebutuhan-nonfungsional) |
| FE-25 | E2E: hapus akun (bila kapasitas ada). | Lulus di kedua profil | 🔒 [web doc 06, pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#pengujian) |

## Kondisi sekarang

`tests/e2e/smoke.spec.ts` punya 4 skenario yang berjalan di profil `desktop` (Desktop Chrome) dan `phone` (Pixel 7): daftar sampai unduh DOCX lalu keluar, dokumen dari pola template bawaan, tamu diarahkan ke Masuk, dan landing untuk tamu. Setiap tes juga gagal bila browser memanggil port 8000 secara langsung. Job E2E di CI menjalankan API `develop` dengan SQLite dan seeder, tanpa kunci AI.

## Rincian pekerjaan

### FE-22 · Unggah template sampai pola siap

Prasyarat CI: langkah "Siapkan API" di job E2E saat ini hanya menjalankan `php artisan serve`. Analisis pola dan perapihan dikerjakan worker antrean, jadi tambahkan `php artisan queue:work --sleep=1 > ../worker.log 2>&1 &` di langkah itu. Tanpa worker, status tetap `analyzing` dan tes habis waktu.

Langkah: masuk dengan akun baru, buka `/template`, unggah DOCX fixture kecil, tunggu badge "Pola siap", tekan "Gunakan", isi judul, lalu pastikan editor terbuka dengan bab dari heading DOCX. Analisis pola memanggil provider AI, jadi skenario ini memakai provider palsu dari [H8](../serah-terima/H8.md). Fixture DOCX disimpan di `tests/e2e/fixtures/`, dibuat dari spesifikasi template, tanpa data pribadi.

### FE-23 · Perapihan sampai terima

Langkah: dari dokumen hasil FE-22 atau dokumen baru, tulis catatan di satu bagian, tekan "Rapikan dengan AI", tunggu usulan, lalu periksa "Sebelum" dan "Usulan" tampil berdampingan. Tekan terima, lalu pastikan isi bagian berubah sesuai perubahan yang dikenali dari provider palsu. Tambah satu skenario negatif: provider palsu dipicu gagal, dan pesan "Perapihan gagal. Dokumen kamu tetap seperti semula." muncul.

### FE-24 · Unduh PDF

Langkah: pakai `page.waitForEvent('download')` saat menekan "Unduh PDF", lalu periksa nama berkas berakhiran `.pdf` dan ukurannya lebih dari 0. PDF dibuat Browsershot di sisi API dan butuh Chrome dari Puppeteer. Langkah "Siapkan API" di job E2E saat ini belum menjalankan `npm ci` di direktori `api`, jadi tambahkan langkah itu, sama dengan job tes di CI API. Unduh DOCX tidak membutuhkannya, karena itu skenario DOCX yang ada sekarang sudah lulus.

### FE-26 · Aksesibilitas otomatis

Langkah:

1. Ajukan dependensi baru `@axe-core/playwright` di grup dan dapatkan persetujuan sesuai aturan dependensi di kontrak tim. Versi dipin persis (`.npmrc` `save-exact`).
2. Jalankan pemeriksaan di 5 halaman: `/masuk`, `/beranda`, `/dokumen`, `/template`, `/dokumen/[id]`.
3. Tes gagal untuk pelanggaran berdampak `serious` dan `critical`. Pelanggaran lain dicatat sebagai issue.

### FE-25 · Hapus akun

Langkah: daftar akun baru, buka `/profil`, hapus akun dengan kata sandi, lalu pastikan pengguna kembali ke landing dan tidak bisa masuk dengan akun itu lagi.

### Profil 1440 px

Kriteria rilis meminta 360 px dan 1440 px. Profil `desktop` memakai lebar bawaan Desktop Chrome di Playwright. Tambahkan `viewport: { width: 1440, height: 900 }` di profil desktop, atau tambahkan profil ketiga, lalu sepakati dengan Louis agar laporan E2E bisa dipakai sebagai bukti.

## Urutan kerja yang disarankan

1. FE-24, yang tidak butuh AI.
2. FE-22 dan FE-23 setelah H8.
3. Profil 1440 px.
4. FE-26 setelah persetujuan dependensi.
5. FE-25 bila kapasitas ada.

Kirim [H20](../serah-terima/H20.md) setelah job E2E hijau di `develop` dengan semua skenario MVP.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H8 · AI-2 → WEB-5 · stub provider untuk E2E**](../serah-terima/H8.md) | Hamid (BE 2), [AI-2](AI-2.md) | Cara mengarahkan `AI_*` ke provider palsu lokal, tertulis di doc 09 API. Juga dipakai Louis. | E2E perapihan tanpa kunci sungguhan (FE-23) |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H20 · WEB-5 → QA-5 · E2E MVP hijau**](../serah-terima/H20.md) | Louis (QA), [QA-5](QA-5.md) | E2E P1 dan "P2 bagian template" lulus di 360 px dan 1440 px | CI web hijau di `develop` |

## Selesai bila

- [ ] Job E2E hijau di `develop` dengan FE-22, FE-23, FE-24
- [ ] Web doc 07 diperbarui untuk skenario baru
- [ ] H20 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Web doc 07, pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md)
- 🔒 [Doc 16, uji peramban](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#uji-peramban)
- 🔒 [Doc 11, perjalanan pengguna](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/11-persona-dan-perjalanan-pengguna.md#perjalanan-pengguna)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
