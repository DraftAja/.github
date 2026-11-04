[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-3](PM-3.md) · Berikutnya: [PM-5](PM-5.md)

# PM-4 · Feature freeze dan rilis MVP, minggu 14

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Rilis · minggu 14, 15, 16 |
| Repo | Backend-Laravel, Frontend-Nextjs, DraftAja |
| Contoh branch | `release/mvp` |
| Mulai setelah | [PM-3](PM-3.md) selesai, menerima H21, H22 |
| Lanjut ke | [PM-5](PM-5.md) |

## Tujuan

MVP (fase 0 sampai 2) dibekukan dan dirilis sebagai pasangan versi API dan web yang sudah lolos QA.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Kriteria rilis | Memeriksa 7 kriteria rilis MVP di doc 16 berdasarkan H21 dan H22. | Semua terpenuhi, atau risikonya diterima tertulis | 🔒 [doc 16, kriteria rilis MVP](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#kriteria-rilis-mvp) |
| Risiko | Menerima tertulis risiko R1 sampai R9 dan temuan P0 yang belum selesai. | Daftar penerimaan risiko tercatat di doc 10 | 🔒 [doc 07, risiko](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#risiko) |
| Freeze | Mengumumkan feature freeze: setelah ini hanya perbaikan bug dan dokumentasi. | PR fitur baru ditahan sampai setelah rilis | 🔒 [kontrak bagian 11](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#11-rilis-milestone-dan-code-freeze) |
| Rilis | Membuat `release/*` dari `develop`, memberi tag dengan nama sama di kedua repo, dan mengisi tabel pasangan rilis di doc 17. | Tag ada di kedua repo; repo gabungan mencatat pasangan commit | 🔒 [doc 17, urutan merge dan deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#urutan-merge-dan-deploy) |

## Rincian pekerjaan

### Keputusan rilis

1. Baca issue `siap-rilis` dari [H22](../serah-terima/H22.md) dan bagian verifikasi doc 10 dari [H21](../serah-terima/H21.md).
2. Untuk setiap kriteria yang tidak terpenuhi, pilih: tunda rilis sampai diperbaiki, atau terima risiko tertulis. Penerimaan risiko ditulis di doc 10 dengan alasan dan rencana perbaikannya.
3. Untuk risiko R1 sampai R9 di doc 07 dan temuan P0 di doc 18 yang belum selesai, lakukan hal yang sama.

### Feature freeze

Umumkan di grup dan di issue rilis: mulai minggu 14, PR yang menambah fitur tidak di-merge sampai setelah rilis. Yang boleh merge: perbaikan bug, dokumentasi, dan perubahan yang dibutuhkan kriteria rilis. Pasang label `blocked` pada PR fitur yang masuk selama freeze.

### Membuat rilis

1. Di kedua repo, buat `release/mvp` dari commit `develop` yang tercatat di issue `siap-rilis`, bukan dari ujung `develop` terbaru.
2. Perbaikan selama masa rilis masuk ke `release/mvp` lewat PR, lalu juga ke `develop`.
3. Setelah stabil, buat tag dengan nama sama di kedua repo, misalnya `v1.0-mvp`. Deploy API lebih dulu, lalu web.
4. Isi tabel pasangan rilis di doc 17 API: nama tag, SHA API, SHA web, tanggal.
5. Pastikan repo gabungan DraftAja mencatat pasangan yang sama.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H21 · CORE-5 → PM-4 · hasil tes PostgreSQL 16**](../serah-terima/H21.md) | Octaf (BE 1), [CORE-5](CORE-5.md) | Hasil seluruh rangkaian tes di PostgreSQL 16 | Kriteria rilis butir 1 |
| [**H22 · QA-5 → PM-4 · label siap-rilis**](../serah-terima/H22.md) | Louis (QA), [QA-5](QA-5.md) | Label `siap-rilis` pada pasangan commit API dan web | Membuat `release/*` dan tag pasangan versi |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] 7 kriteria rilis diperiksa
- [ ] Tag pasangan versi dibuat
- [ ] Tabel pasangan rilis di doc 17 terisi

## Dokumen rujukan lain

- 🔒 [Doc 17, urutan merge dan deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#urutan-merge-dan-deploy)
- 🔒 [Doc 10, verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#verifikasi)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
