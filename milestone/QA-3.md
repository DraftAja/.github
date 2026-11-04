[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-2](QA-2.md) · Berikutnya: [QA-4](QA-4.md)

# QA-3 · Uji fase 1

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-qa3-fase-1` |
| Mulai setelah | [QA-2](QA-2.md) selesai, menerima H14 |
| Lanjut ke | [QA-4](QA-4.md) |

## Tujuan

Membuktikan dokumen sudah berpersona kantor dan alur final serta revisi berjalan benar dari antarmuka.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 fase 1 | Transisi final dan revisi: dokumen final menolak suntingan, revisi membuat draf baru. | Sesuai tes fase 1 di doc 16 | 🔒 [doc 16, tes fase 1](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| KN-09 | Mencari istilah kolom "Hindari" di `src/` web dan `resources/views` API. | Tidak ditemukan, kecuali di path API dan nama variabel | 🔒 [web doc 05 sistem desain](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md) |
| KF-EKS-02, KF-EKS-03 | Ekspor per kategori: Surat tanpa sampul, proposal dengan sampul. | Berkas sesuai kategori | 🔒 [doc 12, ekspor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ekspor-eks) |
| Doc 17 peran | Menulis E2E kasus negatif untuk alur fase 1. | E2E kasus negatif lulus di CI web | 🔒 [doc 17, peran](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#peran) |

## Rincian pekerjaan

### Transisi final dan revisi

| Kasus | Hasil yang diharapkan |
|---|---|
| Draf ditandai final | Status Final tampil di editor, daftar, dan beranda; editor masuk mode baca; tombol unduh tetap ada |
| Final ditandai final lagi lewat DevTools | 403 `document_final` |
| Menyunting bagian dokumen final lewat DevTools | 403 `document_final` |
| Buat revisi | Draf baru terbuka dengan bagian yang sama; dokumen asal tetap final |
| Revisi dari dokumen akun lain | 403 |
| Unduh dokumen final | PDF tanpa tanda "Draf"; DOCX terunduh |
| Unduh dokumen draf | PDF dengan tanda "Draf" |

### Konflik suntingan

Buka dokumen yang sama di dua tab. Ubah bagian yang sama di tab pertama, tunggu tersimpan, lalu ubah di tab kedua. Dialog konflik harus muncul di tab kedua, dan teks yang sedang diketik tidak hilang sebelum pilihan dibuat.

### Kuota

Set `AI_DAILY_QUOTA=3` di `.env` API. Lakukan satu perapihan dokumen dengan 3 potongan, lalu periksa kartu kuota menunjukkan 3 dari 3 dan tombol AI nonaktif dengan keterangan waktu pulih 07.00 WIB.

### Persona dan istilah (KN-09)

1. Cari istilah dari kolom "Hindari" di web doc 05 bagian teks antarmuka, di `src/` web dan `resources/views` API. Contoh di Git Bash:

   ```bash
   grep -rniE "skripsi|makalah|praktikum|nim\b|prodi|dosen pembimbing|tahun ajaran|laporbot|lapor\.in" draftaja-web/src draftaja-api/resources/views
   ```

2. Hasil di nama path API, nama variabel, dan komentar kode boleh tersisa. Hasil di teks yang tampil ke pengguna dilaporkan sebagai bug.
3. Ajukan tiga pertanyaan ke asisten chat: satu dalam batas, satu di luar batas, satu meminta mengarang angka. Jawaban harus mengikuti persona yang ditetapkan PM di PM-2. Uji ini memakai provider palsu kecuali kunci sudah ada.

### Ekspor per kategori

Buat satu dokumen untuk setiap kategori yang sudah punya template, unduh DOCX dan PDF, lalu periksa bentuknya. Surat, Notula, SOP, dan Ringkasan tanpa sampul dan tanpa daftar isi. Proposal kegiatan (`PRP-01`) dengan sampul dan daftar isi.

### E2E kasus negatif

Tulis di `tests/e2e` repo web dengan aturan di web doc 07: akun baru per tes, pilih elemen dengan peran dan nama, berjalan di kedua profil. Minimal: dokumen final tidak menampilkan tombol sunting, dan revisi membuka dokumen baru. Branch dan PR di repo web, dengan Alvira sebagai reviewer.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H14 · WEB-3 → QA-3 · fase 1 siap uji**](../serah-terima/H14.md) | Alvira (FE), [WEB-3](WEB-3.md) | Fase 1 lengkap di kedua repo: PR web merujuk PR API yang sudah merge | Menguji transisi status, persona, ekspor per kategori |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Semua kasus punya hasil tercatat
- [ ] E2E kasus negatif merge dan hijau di CI web
- [ ] Tidak ada bug `priority-high` terbuka untuk fase 1

## Dokumen rujukan lain

- 🔒 [Doc 06, status dokumen](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#status-dokumen)
- 🔒 [Web doc 07, aturan menulis tes E2E](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#aturan-menulis-tes-e2e)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
