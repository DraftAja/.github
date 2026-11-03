[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-1](PM-1.md) · Berikutnya: [PM-3](PM-3.md)

# PM-2 · Keputusan produk fase 1

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Backend-Laravel (dokumentasi) |
| Contoh branch | `docs/pm-keputusan-fase-1` |
| Mulai setelah | [PM-1](PM-1.md) selesai |
| Lanjut ke | [PM-3](PM-3.md) |

## Tujuan

Keputusan produk yang menghalangi fase 1 diambil dan ditulis, supaya BE dan FE tidak menebak isi kategori, kolom identitas, persona asisten, dan kebijakan privasi.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-05, A10 | Mengesahkan 8 kunci kategori (`laporan`, `proposal`, `notula`, `dokumentasi_ti`, `sop`, `surat`, `ringkasan`, `rencana_kerja`) dan pemetaan nilai `report_type` lama ke kunci baru. | Tabel pemetaan nilai lama ke kunci baru tercatat dan siap dipakai migrasi | 🔒 [doc 01, delapan kategori](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/01-project-overview.md#delapan-kategori-dokumen) |
| KF-DOK-04 | Menetapkan kolom identitas pengganti `nim`, `prodi`, `mata_kuliah`, `dosen_pembimbing`, `tahun_ajaran`. Usulan doc 12: nomor dokumen, tanggal dokumen, unit kerja, penandatangan, jabatan penandatangan. | Keputusan tercatat di doc 12 dan doc 04 | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| KF-AI-05 | Menetapkan persona dan batas asisten DraftAja untuk pekerjaan administrasi. | Teks persona tercatat dan tidak menyebut mahasiswa atau skripsi | 🔒 [doc 12, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ai) |
| FE-15 | Menyediakan isi halaman kebijakan privasi, mengacu UU Nomor 27 Tahun 2022 (keputusan A8). | Teks final diserahkan ke FE | 🔒 [doc 07 keamanan dan privasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md) |
| Definition of Ready | Memastikan setiap requirement fase 1 punya kriteria terima yang bisa diuji sebelum BE dan FE mulai. | Louis menyatakan setiap requirement fase 1 bisa diuji | 🔒 [doc 17, Definition of Ready](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#definition-of-ready) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H9 · PM-2 → CORE-3 · kategori dan kolom identitas** | Octaf (BE 1), [CORE-3](CORE-3.md) | Keputusan 8 kunci kategori, pemetaan nilai lama, dan kolom identitas pengganti kolom akademik | Issue keputusan ditutup, doc 12 diperbarui |
| **H10 · PM-2 → AI-3 · persona asisten** | Hamid (BE 2), [AI-3](AI-3.md) | Persona dan batas asisten DraftAja | Issue keputusan ditutup, doc 01 atau 11 diperbarui |
| **H11 · PM-2 → WEB-3 · teks kebijakan privasi** | Alvira (FE), [WEB-3](WEB-3.md) | Isi halaman kebijakan privasi | Issue keputusan ditutup |

## Selesai bila

- [ ] Empat keputusan tercatat sebagai issue tertutup
- [ ] Doc 01, 11, dan 12 diperbarui
- [ ] H9, H10, H11 terkirim

## Dokumen rujukan lain

- 🔒 [Keputusan A1 sampai A10](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/README.md#keputusan-yang-diambil-saat-menyusun-ulang-dokumen)
- 🔒 [Roadmap fase 1](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-1-penyelarasan-persona)
- 🔒 [Doc 11 persona](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/11-persona-dan-perjalanan-pengguna.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
