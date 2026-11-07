[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-1](PM-1.md) · Berikutnya: [PM-3](PM-3.md)

# PM-2 · Keputusan produk fase 1

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Backend-Laravel (dokumentasi) |
| Contoh branch | `docs/pm-pm2-keputusan-fase-1` |
| Saran urutan | [PM-1](PM-1.md) selesai. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [PM-3](PM-3.md) |

## Tujuan

Keputusan produk yang menghalangi fase 1 diambil dan ditulis, supaya BE dan FE tidak menebak isi kategori, kolom identitas, persona asisten, dan kebijakan privasi. Keputusan kategori dan kolom identitas paling mendesak, karena keduanya dipakai migration dan migration yang sudah merge tidak boleh diedit.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-05, A10 | Mengesahkan 8 kunci kategori (`laporan`, `proposal`, `notula`, `dokumentasi_ti`, `sop`, `surat`, `ringkasan`, `rencana_kerja`) dan pemetaan nilai `report_type` lama ke kunci baru. | Tabel pemetaan nilai lama ke kunci baru tercatat dan siap dipakai migrasi | 🔒 [doc 01, delapan kategori](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/01-project-overview.md#delapan-kategori-dokumen) |
| KF-DOK-04 | Menetapkan kolom identitas pengganti `nim`, `prodi`, `mata_kuliah`, `dosen_pembimbing`, `tahun_ajaran`. Usulan doc 12: nomor dokumen, tanggal dokumen, unit kerja, penandatangan, jabatan penandatangan. | Keputusan tercatat di doc 12 dan doc 04 | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| KF-AI-05 | Menetapkan persona dan batas asisten DraftAja untuk pekerjaan administrasi. | Teks persona tercatat dan tidak menyebut mahasiswa atau skripsi | 🔒 [doc 12, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ai) |
| FE-15 | Menyediakan isi halaman kebijakan privasi, mengacu UU Nomor 27 Tahun 2022 (keputusan A8). | Teks final diserahkan ke FE | 🔒 [doc 07 keamanan dan privasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md) |
| Definition of Ready | Memastikan setiap requirement fase 1 punya kriteria terima yang bisa diuji sebelum BE dan FE mulai. | Louis menyatakan setiap requirement fase 1 bisa diuji | 🔒 [doc 17, Definition of Ready](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#definition-of-ready) |

## Rincian pekerjaan

Setiap keputusan dibuat sebagai satu issue di repo API berjudul "Keputusan: <hal>", berisi pilihan yang dipertimbangkan, keputusan, dan alasan. Issue ditutup setelah dokumen terkait diperbarui.

### Kategori dan pemetaan nilai lama

Yang diputuskan:

| Hal | Usulan yang sudah ada di dokumentasi | Yang harus diputuskan |
|---|---|---|
| 8 kunci kategori | Keputusan A10 di README docs API, sama dengan landing | Pengesahan saja |
| Label tampilan | Laporan, Proposal, Notula, Dokumentasi TI, SOP, Surat, Ringkasan, Rencana kerja | Pengesahan saja |
| `report_type` lama di dokumen | Makalah, Proposal, Laporan Praktikum, Studi Kasus, Skripsi | Kunci tujuan untuk setiap nilai. Proposal jelas ke `proposal`; empat lainnya belum punya padanan dan perlu satu kunci bawaan. |
| `templates.category` lama | `laporan_kegiatan` dan `laporan_berkala` ke `laporan`, `notulen` ke `notula` (doc 04) | Pengesahan saja |
| Kategori bawaan dokumen tanpa template | Belum ada | Satu kunci |

### Kolom identitas

Doc 04 sudah mengusulkan pengganti:

| Kolom lama | Pengganti usulan |
|---|---|
| `nim` | dihapus |
| `prodi` | `unit_kerja` |
| `mata_kuliah` | dihapus |
| `dosen_pembimbing` | `penandatangan_nama`, `penandatangan_jabatan` |
| `tahun_ajaran` | `tanggal_dokumen` |
| tidak ada | `nomor_dokumen` |

Yang diputuskan: setuju atau ubah, kolom mana yang wajib, dan kolom mana yang tampil untuk kategori tertentu saja (misalnya nomor dokumen untuk Surat dan SOP).

### Persona asisten

Isi yang harus ditetapkan ada di [H10](../serah-terima/H10.md): nama, pengguna sasaran, yang boleh dan tidak boleh dibantu, gaya bahasa, kalimat penolakan, dan istilah terlarang. Tulis di doc 01 atau doc 11.

### Kebijakan privasi

Isi minimal ada di [H11](../serah-terima/H11.md): data yang diproses, tujuan, subprosesor dan negaranya, masa simpan, hak pengguna, kontak, tanggal berlaku. Teks belum ditinjau ahli hukum, jadi tidak boleh mengklaim kepatuhan.

### Definition of Ready fase 1

Untuk setiap requirement fase 1 (KF-TPL-05, KF-DOK-04, KF-TIN-04, KF-AI-04, KF-AI-05, KF-EKS-02, KF-EKS-03), pastikan kriteria terima di doc 12 bisa diuji, lalu minta Louis menyatakannya di issue.

## Urutan kerja yang disarankan

1. Kategori dan kolom identitas, karena memblokir migration CORE-3. Kirim [H9](../serah-terima/H9.md).
2. Persona asisten. Kirim [H10](../serah-terima/H10.md).
3. Kebijakan privasi. Kirim [H11](../serah-terima/H11.md).
4. Definition of Ready, berjalan bersama langkah 1 sampai 3.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H9 · PM-2 → CORE-3 · kategori dan kolom identitas**](../serah-terima/H9.md) | Octaf (BE 1), [CORE-3](CORE-3.md) | Keputusan 8 kunci kategori, pemetaan nilai lama, dan kolom identitas pengganti kolom akademik | Issue keputusan ditutup, doc 12 diperbarui |
| [**H10 · PM-2 → AI-3 · persona asisten**](../serah-terima/H10.md) | Hamid (BE 2), [AI-3](AI-3.md) | Persona dan batas asisten DraftAja | Issue keputusan ditutup, doc 01 atau 11 diperbarui |
| [**H11 · PM-2 → WEB-3 · teks kebijakan privasi**](../serah-terima/H11.md) | Alvira (FE), [WEB-3](WEB-3.md) | Isi halaman kebijakan privasi | Issue keputusan ditutup |

## Selesai bila

- [ ] Empat keputusan tercatat sebagai issue tertutup
- [ ] Doc 01, 04, 11, dan 12 diperbarui
- [ ] H9, H10, H11 dilaporkan di issue dan reviewer di-mention

## Dokumen rujukan lain

- 🔒 [Doc 04, kolom yang harus diganti](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md)
- 🔒 [Doc 07, pelindungan data pribadi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#pelindungan-data-pribadi)
- 🔒 [Doc 11, persona](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/11-persona-dan-perjalanan-pengguna.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
