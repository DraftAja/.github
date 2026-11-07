[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [QA-2](QA-2.md)

# QA-1 · Rencana uji dan tinjauan kontrak

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel (dokumentasi), issue di kedua repo |
| Contoh branch | `docs/qa-qa1-rencana-uji` |
| Saran urutan | Repo dipisah, 3 November 2026. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [QA-2](QA-2.md) |

## Tujuan

Menyiapkan rencana uji per fase dan memastikan setiap proposal kontrak sudah memikirkan kasus error sebelum kode ditulis. Kesalahan kontrak yang ditemukan di tahap proposal hanya butuh komentar. Kesalahan yang ditemukan setelah merge butuh PR di dua repo.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 | Rencana uji per fase dari tabel "Tes yang wajib ditambah per fase" dan skenario peramban. | Rencana uji fase 0 sampai 2 tersedia dan disetujui Raja | 🔒 [doc 16, tes per fase](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| Doc 17 | Meninjau kolom Error di setiap proposal kontrak; memberi persetujuan sebelum Alvira memasang `kontrak-disetujui`. | Setiap proposal terbuka sudah ditinjau dalam 1×24 jam hari kerja | 🔒 [doc 17, proposal kontrak](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#proposal-kontrak) |
| Kontrak bagian 12 | Memakai template issue "Laporan bug": langkah reproduksi, hasil diharapkan dan aktual, lingkungan, bukti. | Bug fase 0 dilaporkan dengan template | 🔒 [kontrak bagian 12](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#12-issue-dan-pelacakan-pekerjaan) |

## Rincian pekerjaan

### Rencana uji per fase

Rencana ditulis di repo API, misalnya `docs/qa/rencana-uji.md`, karena folder `docs/qa/` milik QA menurut kontrak tim. Isinya per fase:

| Bagian | Isi |
|---|---|
| Ruang lingkup | Milestone dan butir yang diuji di fase itu, diambil dari peta milestone |
| Kasus uji | Satu baris per kasus: ID (misalnya `QA-F0-01`), butir asal (BE-xx atau FE-xx), langkah, data uji, hasil yang diharapkan, cara memicu |
| Kasus negatif | Minimal satu per butir yang menolak sesuatu: akses pengguna lain, input terlalu panjang, berkas salah, sesi habis |
| Lingkungan | Pasangan commit yang diuji, akun uji, cara menjalankan stack lokal, provider palsu dari AI-2 |
| Peramban dan lebar | Chromium di 360 px dan 1440 px setiap fase; Firefox dan WebKit sekali sebelum rilis |
| Kriteria keluar | Kapan fase dinyatakan lolos uji: semua kasus P0 lulus, tidak ada bug `priority-high` terbuka |

Mulai dari fase 0 (dipakai di QA-2), lalu fase 1 dan 2. Kasus untuk butir yang belum punya proposal ditandai "menunggu proposal".

### Tinjauan proposal kontrak

Setiap issue berlabel `kontrak` ditinjau dengan pertanyaan ini:

1. Apakah setiap status error punya `code`, dan setiap `code` punya kondisi pemicu yang bisa dijalankan di lokal?
2. Apakah akses pengguna lain (403) dan tamu (401) tercantum?
3. Untuk endpoint asinkron: apakah status 202, pemakaian ulang `request_key` (200, `reused: true`), dan konflik isi berbeda (409) tercantum?
4. Apakah teks yang ditampilkan FE untuk setiap `code` sudah diisi Alvira?
5. Apakah perubahan bersifat menambah, atau memecah kontrak dan butuh tahap deprecated?

Tulis persetujuan sebagai komentar berisi daftar `code` yang disetujui, kondisi pemicunya, kasus yang ditambahkan, dan kasus yang ditolak beserta alasannya. Format lengkapnya ada di [H2](../serah-terima/H2.md) dan [H3](../serah-terima/H3.md). Proposal pertama yang ditinjau adalah BE-03 dari Octaf dan kontrak AI dari Hamid.

### Laporan bug

Template issue "Laporan bug" sudah tersedia otomatis di semua repo organisasi, dengan kolom area, prioritas, pasangan commit yang diuji, langkah reproduksi, hasil yang diharapkan, hasil aktual, lingkungan, dan bukti. Aturan tambahan:

- Bug dibuat di repo tempat perbaikannya akan dikerjakan. Bila belum jelas, buat di repo API dan beri label `be-core`, `be-ai`, atau `fe` setelah diperiksa.
- Bukti tidak memuat data pribadi sungguhan.
- Bug keamanan tidak ditulis rinci di issue publik. Repo kode privat, tetapi bila ragu, sampaikan langsung ke pemilik kode dan Raja.

## Urutan kerja yang disarankan

1. Rencana uji fase 0, karena QA-2 bergantung padanya.
2. Tinjauan proposal BE-03 dan kontrak AI, segera setelah dibuka.
3. Rencana uji fase 1 dan 2.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H2 · QA-1 → CORE-2 · kasus error inti disetujui**](../serah-terima/H2.md) | Octaf (BE 1), [CORE-2](CORE-2.md) | Persetujuan daftar `code` error di proposal kontrak endpoint inti | Komentar di issue `kontrak`, label `kontrak-disetujui` |
| [**H3 · QA-1 → AI-1 · kasus error AI disetujui**](../serah-terima/H3.md) | Hamid (BE 2), [AI-1](AI-1.md) | Persetujuan kasus error endpoint AI (generate, tidy, chat) | Komentar di issue `kontrak`, label `kontrak-disetujui` |

## Selesai bila

- [ ] Rencana uji fase 0 sampai 2 merge dan disetujui Raja
- [ ] H2 dan H3 dilaporkan di issue dan reviewer di-mention
- [ ] Template laporan bug dipakai untuk bug pertama

## Dokumen rujukan lain

- 🔒 [Doc 16, strategi pengujian](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md)
- 🔒 [Doc 05, katalog error](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#katalog-error)
- 🔒 [Web doc 07, pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
