[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-4](PM-4.md) · Berikutnya: [PM-6](PM-6.md)

# PM-5 · v1.0-final minggu 15, pameran minggu 16

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Rilis · minggu 14, 15, 16 |
| Repo | Backend-Laravel, Frontend-Nextjs, DraftAja |
| Branch | hotfix/* saja |
| Mulai setelah | [PM-4](PM-4.md) selesai |
| Lanjut ke | selesai |

## Tujuan

Versi akhir untuk presentasi, penyerahan dokumen mata kuliah, dan pameran.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Minggu 15 | Merge `develop` ke `main` dengan merge commit, tag `v1.0-final` di kedua repo, penyerahan dokumen mata kuliah. | Tag `v1.0-final` ada di kedua repo dan repo gabungan | 🔒 [kontrak bagian 11](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#11-rilis-milestone-dan-code-freeze) |
| Minggu 16 | Pameran memakai `v1.0-final`. Hanya `hotfix/*` yang diizinkan, di-merge ke `main` dan `develop`. | Tidak ada perubahan di luar hotfix | 🔒 [kontrak bagian 4](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#4-strategi-branch) |

## Rincian pekerjaan

### Minggu 15 · v1.0-final

1. Pastikan `develop` di kedua repo sama dengan pasangan rilis MVP ditambah perbaikan bug selama masa rilis, tanpa fitur baru.
2. Buka PR `develop` ke `main` di kedua repo. Branch protection meminta 2 approval untuk `main`. Merge dengan merge commit, bukan squash, supaya riwayat `develop` tetap terbaca.
3. Tag `v1.0-final` di commit `main` hasil merge, di kedua repo.
4. Repo gabungan DraftAja menyinkronkan `develop`, bukan `main`. Catat pasangan `v1.0-final` dengan commit manual di repo gabungan atau di tabel pasangan rilis doc 17, supaya penguji dan penilai tahu pasangan mana yang dirilis.
5. Deploy `v1.0-final`: API lebih dulu, lalu web.
6. Siapkan dokumen mata kuliah dari `docs/`: PRD, SRS, arsitektur, dan hasil uji. Ambil angka hanya dari doc 10 dan doc 16, yang sudah diverifikasi.

### Minggu 16 · Pameran

1. Pameran memakai deploy `v1.0-final`. Siapkan akun demo terpisah dari akun uji seeder, dengan data contoh dari spesifikasi template, tanpa data pribadi sungguhan.
2. Siapkan rencana cadangan bila koneksi atau provider AI bermasalah, misalnya video demo, atau API lokal dengan provider palsu dari AI-2 yang dijelaskan sebagai mode demo.
3. Perubahan selama minggu 16 hanya lewat `hotfix/<deskripsi>` dari `main`, di-merge ke `main` dan `develop`, lalu tag `v1.0-final.1` dan seterusnya.

### Setelah pameran

Cabut kunci provider AI dari H16 dan hapus instance PostgreSQL uji dari H15 bila masih ada.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] `v1.0-final` ditag di kedua repo
- [ ] Pasangan `v1.0-final` tercatat
- [ ] Dokumen mata kuliah diserahkan
- [ ] Kunci dan instance uji sudah dicabut

## Dokumen rujukan lain

- 🔒 [`TEAM_CONTRACT.md`](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md)
- 🔒 [Doc 17, urutan merge dan deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#urutan-merge-dan-deploy)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
