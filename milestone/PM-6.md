[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-5](PM-5.md)

# PM-6 · Keputusan produk Dashboard Usaha

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel (`docs/`), .github |
| Contoh branch | `docs/pm-pm6-keputusan-usaha` |
| Saran urutan | [PM-3](PM-3.md) selesai. Berjalan berdampingan dengan fase 2, tidak menunggu PM-4. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [PM-4](PM-4.md), yang memutuskan bagian Fase U mana yang ikut rilis |

## Tujuan

Semua keputusan yang menahan Fase U diambil sebelum Octaf dan Hamid menulis kode: model usaha, dependensi baru, batas add-on Multi-usaha untuk uji coba, dan jawaban pertanyaan terbuka di doc 20.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| ADR 0007 | Putuskan usaha sebagai entitas sendiri, terpisah dari ruang kerja | Status ADR menjadi Diterima atau Ditolak di PR | 🔒 [ADR 0007](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/adr/0007-usaha-entitas-terpisah.md) |
| ADR 0008 | Putuskan PhpSpreadsheet dan TanStack Table, setujui dependensinya | Status ADR Diterima, persetujuan dependensi tercatat sesuai `TEAM_CONTRACT.md` | 🔒 [ADR 0008](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/adr/0008-pustaka-lembar-kerja.md) |
| Q1 sampai Q6 | Jawab pertanyaan terbuka doc 20 lewat wawancara 5 pemilik UMKM | Tabel pertanyaan terbuka diperbarui; rumus doc 21 disesuaikan bila jawaban Q1 atau Q5 berbeda dari bawaan | 🔒 [doc 20, pertanyaan terbuka](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/20-dashboard-usaha.md#pertanyaan-terbuka) |
| Add-on | Tetapkan batas usaha untuk akun uji coba dan cara admin mengaktifkannya | Angka uji coba tertulis di doc 14 dengan catatan "uji coba, bukan harga" | 🔒 [doc 14, add-on](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/14-model-bisnis.md#add-on-multi-usaha) |
| Validasi | Rencana survei Van Westendorp untuk 20 pemilik UMKM | Daftar pertanyaan dan cara merekrut responden tercatat di issue | 🔒 [doc 14, validasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/14-model-bisnis.md#validasi) |

## Rincian pekerjaan

### ADR 0007 · Usaha sebagai entitas sendiri

Masalah sekarang: doc 13 merancang ruang kerja instansi dengan peran dokumen dan penagihan per ruang kerja. Dashboard usaha butuh satu usaha gratis dan usaha tambahan berbayar per pengguna. ADR 0007 mengusulkan tabel `businesses` sendiri, dan statusnya masih Diusulkan.

Langkah:

1. Baca ADR 0007 bersama Octaf dan Hamid. Periksa alternatif yang ditolak.
2. Putuskan di PR yang mengubah status ADR. Bila ditolak, tulis ADR pengganti dan beri tahu QA bahwa doc 04 dan 20 akan berubah.

Bukti selesai: ADR 0007 berstatus Diterima atau Ditolak di `develop`.

Hindari: memutuskan lewat chat tanpa mengubah berkas ADR.

### ADR 0008 · Pustaka lembar kerja

Masalah sekarang: `composer.json` tidak punya pustaka spreadsheet, dan `package.json` web tidak punya pustaka tabel. Kontrak tim mewajibkan persetujuan untuk dependensi baru.

Langkah:

1. Minta Hamid dan Alvira mengonfirmasi bahwa lisensi MIT dan versi yang akan dipin sudah diperiksa.
2. Setujui `phpoffice/phpspreadsheet`, `@tanstack/react-table`, dan bila perlu `@tanstack/react-virtual` di PR ADR.
3. Kirim [H24](../serah-terima/H24.md).

Bukti selesai: ADR 0008 Diterima, dan persetujuan tercatat di PR.

### Q1 sampai Q6 · Pertanyaan terbuka

Masalah sekarang: skema bagi hasil, produksi, pajak, awal minggu, dan kanal pengingat masih memakai bawaan sementara.

Langkah:

1. Wawancarai 5 pemilik UMKM yang menitip jual di 3 lokasi atau lebih. Pakai daftar pertanyaan di doc 14 bagian Validasi.
2. Tulis jawabannya di tabel pertanyaan terbuka doc 20.
3. Bila Q1 (skema) atau Q5 (awal minggu) berbeda dari bawaan, minta Octaf memperbarui doc 21 dan kasus ujinya sebelum CORE-6 mulai.

Bukti selesai: semua baris Q1 sampai Q6 punya jawaban atau keputusan "tetap memakai bawaan".

Hindari: memakai data pribadi pemilik UMKM di dokumen. Tulis ringkasannya saja.

### Add-on · Batas uji coba

Masalah sekarang: doc 14 sengaja tidak mencantumkan harga dan batas add-on. Octaf butuh satu angka untuk `PlanLimits::maxBusinesses()` selama uji coba.

Langkah:

1. Tetapkan angka batas usaha untuk akun uji coba dan tulis di doc 14 sebagai angka uji coba.
2. Tetapkan siapa yang boleh mengaktifkan add-on (admin platform) dan berapa lama uji coba berlaku.
3. Kirim [H23](../serah-terima/H23.md).

Bukti selesai: doc 14 memuat angka uji coba dan cara aktivasinya.

Hindari: menulis angka harga sebelum survei.

## Urutan kerja yang disarankan

1. ADR 0007 dan add-on, lalu kirim [H23](../serah-terima/H23.md).
2. ADR 0008, lalu kirim [H24](../serah-terima/H24.md).
3. Wawancara Q1 sampai Q6, paralel dengan CORE-6.
4. Rencana survei harga.

## Serah terima masuk

Tidak ada. PM-6 memakai dokumen Fase U yang sudah ada di `docs/`.

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H23 · PM-6 → CORE-6 · ADR usaha dan batas add-on**](../serah-terima/H23.md) | Octaf (BE 1), [CORE-6](CORE-6.md) | ADR 0007 Diterima, angka batas uji coba, cara aktivasi admin | PR docs merge, pengumuman di grup |
| [**H24 · PM-6 → AI-6 · ADR 0008 dan dependensi disetujui**](../serah-terima/H24.md) | Hamid (BE 2), [AI-6](AI-6.md). Juga berlaku untuk Alvira di [WEB-7](WEB-7.md). | ADR 0008 Diterima, izin dependensi | PR docs merge, pengumuman di grup |

## Selesai bila

- [ ] ADR 0007 dan 0008 tidak lagi berstatus Diusulkan
- [ ] Q1 sampai Q6 terjawab di doc 20
- [ ] Angka batas uji coba tertulis di doc 14
- [ ] H23 dan H24 dilaporkan di issue dan reviewer di-mention

## Dokumen rujukan lain

- 🔒 [Doc 20 Dashboard usaha](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/20-dashboard-usaha.md)
- 🔒 [Doc 10, Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-u-dashboard-usaha)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
