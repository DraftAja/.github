[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-2](QA-2.md) · Berikutnya: [QA-4](QA-4.md)

# QA-3 · Uji fase 1

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-fase-1` |
| Mulai setelah | [QA-2](QA-2.md) selesai, menerima H14 |
| Lanjut ke | [QA-4](QA-4.md) |

## Tujuan

Membuktikan dokumen sudah berpersona kantor dan alur final serta revisi berjalan benar dari antarmuka.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 fase 1 | Transisi final dan revisi: dokumen final menolak suntingan, revisi membuat draf baru. | Sesuai tes fase 1 di doc 16 | 🔒 [doc 16, tes fase 1](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| KN-09 | Mencari istilah kolom "Hindari" di `src/` web dan `resources/views` API. | Tidak ditemukan, kecuali di path API dan nama variabel | 🔒 [web doc 05 sistem desain](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md) |
| KF-EKS-02, KF-EKS-03 | Ekspor per kategori: Surat tanpa sampul, proposal dengan sampul. | Berkas sesuai kategori | 🔒 [doc 12, ekspor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ekspor-eks) |
| Doc 17 peran | Menulis E2E kasus negatif untuk alur fase 1. | E2E kasus negatif lulus di CI web | 🔒 [doc 17, peran](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#peran) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H14 · WEB-3 → QA-3 · fase 1 siap uji** | Alvira (FE), [WEB-3](WEB-3.md) | Fase 1 lengkap di kedua repo: PR web merujuk PR API yang sudah merge | Menguji transisi status, persona, ekspor per kategori |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Tidak ada bug prioritas tinggi terbuka untuk fase 1

## Dokumen rujukan lain

- 🔒 [Doc 11 perjalanan pengguna](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/11-persona-dan-perjalanan-pengguna.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
