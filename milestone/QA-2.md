[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-1](QA-1.md) · Berikutnya: [QA-3](QA-3.md)

# QA-2 · Uji fase 0

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-fase-0` |
| Mulai setelah | [QA-1](QA-1.md) selesai, menerima H4, H5 |
| Lanjut ke | [QA-3](QA-3.md) |

## Tujuan

Membuktikan perbaikan keamanan fase 0 menolak kasus yang salah, dan antarmuka tetap aman ketika API berubah.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 fase 0 | Kasus negatif BE-01, BE-02, BE-09, BE-17: ekspor milik orang lain, judul terlalu panjang, gambar tanpa session, HTML berisi skrip. | Setiap kasus menghasilkan status dan `code` yang diharapkan | 🔒 [doc 16, tes fase 0](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| Web doc 07 | Memicu keadaan error secara manual: kuota habis, provider belum siap, session kedaluwarsa, CSRF kedaluwarsa, origin tidak stateful, DOCX ditolak. | Setiap keadaan menampilkan pesan Indonesia yang sesuai | 🔒 [web doc 07, serah terima ke QA](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#serah-terima-ke-qa) |
| WEB-1 | Memeriksa status tak dikenal, mode baca, dan tanggal dekat tengah malam WIB di 360 px dan desktop. | Tidak ada error render | 🔒 [doc 17, serah terima ke QA](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#serah-terima-ke-qa) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H4 · CORE-1 → QA-2 · fase 0 API siap uji** | Octaf (BE 1), [CORE-1](CORE-1.md) | PR BE-01, BE-02, BE-09, BE-17 yang sudah merge, beserta cara memicu setiap error | Menguji kasus negatif fase 0 |
| **H5 · WEB-1 → QA-2 · fase 0 web siap uji** | Alvira (FE), [WEB-1](WEB-1.md) | Pasangan commit API dan web, tangkapan layar 360 px dan desktop | Menguji status tak dikenal, mode baca, zona waktu, pesan error |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Temuan P0 tertutup atau tercatat sebagai issue `bug`

## Dokumen rujukan lain

- 🔒 [Doc 07 risiko](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#risiko)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
