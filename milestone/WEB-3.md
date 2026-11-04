[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-2](WEB-2.md) · Berikutnya: [WEB-4](WEB-4.md)

# WEB-3 · Final, revisi, kuota, konflik

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-web3-final-dan-revisi` |
| Mulai setelah | [WEB-2](WEB-2.md) selesai, menerima H11, H12, H13 |
| Lanjut ke | [WEB-4](WEB-4.md) |

## Tujuan

Alur dokumen fase 1 tampil di antarmuka: menandai final, membuat revisi, melihat kuota, menangani konflik, dan halaman dasar yang belum ada.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-11 | Sorot penanda `[perlu dilengkapi]` di editor, hitung di bilah status, klik membawa ke penanda pertama. | E2E: bagian berisi dua penanda menampilkan hitungan 2 | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-12 | Tombol "Tandai final" dan "Buat revisi" di kepala editor, setelah H12. | Dokumen final terbuka dalam mode baca; revisi membuka draf baru | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-13 | Kartu kuota AI di beranda dan panel perapihan; tombol AI nonaktif sampai `resets_at`, setelah H13. | Kuota tampil sesuai `GET /ai/quota` | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-14 | Dialog konflik saat bagian disunting di tab lain (409 `revision_changed`). | Dialog muncul dan isi terbaru tidak hilang | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-15 | Halaman `not-found.tsx`, `error.tsx`, dan `/kebijakan-privasi` dengan teks dari H11. | Ketiga halaman tampil di 360 px dan desktop | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |
| FE-18 | Usulan perapihan dibaca dari field `proposal`; hapus `JSON.parse`. | Tipe usulan dari `schema.d.ts` | 🔒 [web doc 06, ketahanan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#ketahanan-terhadap-perubahan-api) |
| KF-DOK-04 | Dialog data dokumen memakai kolom identitas baru. | Kolom akademik tidak tampil | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |

## Rincian pekerjaan

### FE-11 · Penanda `[perlu dilengkapi]`

Masalah sekarang: AI menulis `[perlu dilengkapi]` di tempat informasi yang tidak ada di catatan pengguna. Penanda itu tampil sebagai teks biasa, sehingga mudah terlewat dan terbawa ke dokumen final.

Langkah:

1. Buat ekstensi TipTap berbasis decoration yang mencari teks `[perlu dilengkapi]` di isi bagian dan memberi kelas sorotan berlatar `warning-bg`. Pakai decoration, bukan mark, supaya HTML yang disimpan ke API tidak berubah.
2. Bilah status editor menampilkan jumlah penanda di seluruh dokumen, dihitung dari `content` semua bagian.
3. Klik pada jumlah membawa ke bagian dan posisi penanda pertama. Penanda harus bisa dicapai dengan keyboard.
4. Sebelum unduh dan sebelum "Tandai final", tampilkan peringatan bila masih ada penanda. Pengguna tetap boleh melanjutkan.

Bukti selesai: E2E bagian berisi dua penanda menampilkan hitungan 2. Tes Vitest untuk fungsi penghitung.

### FE-12 · Tandai final dan buat revisi

Langkah, setelah [H12](../serah-terima/H12.md):

1. Kepala editor menampilkan tombol sesuai status: draf menampilkan "Tandai final", final menampilkan "Buat revisi".
2. "Tandai final" membuka `ConfirmDialog` yang menjelaskan bahwa dokumen tidak bisa disunting lagi, lalu memanggil `POST /laporan/{id}/finalize`. Setelah sukses, perbarui cache detail dan daftar dokumen. Editor otomatis masuk mode baca berkat FE-02.
3. "Buat revisi" memanggil `POST /laporan/{id}/revisions`, lalu `router.push(routes.document(id_baru))`.
4. Daftar dokumen dan beranda menampilkan status baru tanpa muat ulang.

Bukti selesai: E2E draf ke final ke revisi. Dokumen final terbuka dalam mode baca dan tombol unduh tetap ada.

### FE-13 · Kuota AI

Langkah, setelah [H13](../serah-terima/H13.md):

1. Hook `useAiQuota` untuk `GET /ai/quota` di modul `ai`, dengan invalidasi setelah setiap permintaan AI selesai.
2. Kartu di beranda: "Kuota AI hari ini: {used} dari {limit}", dengan waktu pulih dalam WIB. Kartu disembunyikan bila `limit` 0.
3. Panel perapihan menampilkan sisa kuota. Bila `remaining` 0, tombol "Rapikan dengan AI" nonaktif sampai `resets_at`, dengan keterangan "Kuota pulih pukul 07.00 WIB."

### FE-14 · Dialog konflik

Langkah, setelah H12:

1. Simpan `updated_at` setiap bagian saat dimuat. Kirim sebagai `base_updated_at` di setiap `PUT /sections/{id}`.
2. Bila API membalas 409 `revision_changed`, hentikan simpan otomatis untuk bagian itu dan tampilkan dialog berisi dua pilihan: "Muat versi terbaru" (membuang perubahan di tab ini) atau "Timpa dengan versi ini" (simpan ulang dengan `base_updated_at` terbaru dari respons).
3. Teks yang sedang diketik tidak boleh hilang sebelum pengguna memilih.

Bukti selesai: E2E dengan dua halaman Playwright pada dokumen yang sama.

### FE-15 · Halaman dasar

| Berkas | Isi |
|---|---|
| `src/app/not-found.tsx` | "Halaman tidak ditemukan." dengan tautan ke Beranda bila masuk, atau ke landing bila belum |
| `src/app/(app)/error.tsx` | "Terjadi kesalahan." dengan tombol muat ulang. Tidak menampilkan jejak galat. |
| `/kebijakan-privasi` | Teks dari [H11](../serah-terima/H11.md), ditautkan dari formulir daftar dan kaki landing |

Path baru ditambahkan ke `src/lib/routes.ts`. `/kebijakan-privasi` bersifat publik, jadi tidak masuk `matcher` di `src/proxy.ts`.

### FE-18 · Usulan dari `proposal`

Langkah: ganti `parseBlocks(generation.output)` di `CleanupPanel.tsx` dengan `generation.proposal?.blocks`. Hapus `JSON.parse` dan penanganan string rusak.

### KF-DOK-04 · Data dokumen kantor

Langkah: ganti kolom di `DocumentDataDialog.tsx` dan `CreateDocumentForm.tsx` dengan kolom identitas baru dari CORE-3, misalnya nomor dokumen, tanggal dokumen, unit kerja, nama dan jabatan penandatangan. Hapus "Tahun" yang berasal dari `tahun_ajaran`. Kirim `category` dari 8 kunci.

## Urutan kerja yang disarankan

1. FE-11 dan FE-15 (tanpa `/kebijakan-privasi`), yang tidak menunggu API.
2. FE-12 dan FE-14 setelah H12.
3. FE-13 dan FE-18 setelah H13.
4. `/kebijakan-privasi` setelah H11.
5. KF-DOK-04 setelah kolom identitas CORE-3 merge.

Kirim [H14](../serah-terima/H14.md) setelah semua merge.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H11 · PM-2 → WEB-3 · teks kebijakan privasi**](../serah-terima/H11.md) | Raja (PM), [PM-2](PM-2.md) | Isi halaman kebijakan privasi | Halaman `/kebijakan-privasi` (FE-15) |
| [**H12 · CORE-3 → WEB-3 · final, revisi, konflik**](../serah-terima/H12.md) | Octaf (BE 1), [CORE-3](CORE-3.md) | `POST /laporan/{id}/finalize`, `POST /laporan/{id}/revisions`, 409 `revision_changed` | FE-12 tombol final dan revisi, FE-14 dialog konflik |
| [**H13 · AI-3 → WEB-3 · kuota dan proposal**](../serah-terima/H13.md) | Hamid (BE 2), [AI-3](AI-3.md) | `GET /ai/quota` dan field `proposal` berbentuk objek | FE-13 kartu kuota, FE-18 tanpa `JSON.parse` |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H14 · WEB-3 → QA-3 · fase 1 siap uji**](../serah-terima/H14.md) | Louis (QA), [QA-3](QA-3.md) | Fase 1 lengkap di kedua repo: PR web merujuk PR API yang sudah merge | Repo gabungan mencatat pasangan commit, daftar periksa serah terima QA |

## Selesai bila

- [ ] Semua kriteria terima lulus di profil desktop dan ponsel
- [ ] Web doc 03 diperbarui untuk editor dan halaman baru
- [ ] H14 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Web doc 03, editor](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#editor-dokumenid)
- 🔒 [Web doc 01, status dokumen](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/01-konteks-produk.md#status-dokumen)
- 🔒 [Web doc 05, teks antarmuka](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md#teks-antarmuka)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
