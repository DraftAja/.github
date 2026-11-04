[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-3](WEB-3.md) · Berikutnya: [WEB-5](WEB-5.md)

# WEB-4 · Katalog 8 kategori dan kolom isian

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-web4-katalog-template` |
| Mulai setelah | [WEB-3](WEB-3.md) selesai, menerima H17 |
| Lanjut ke | [WEB-5](WEB-5.md) |

## Tujuan

Pengguna menelusuri 64 template per kategori, melihat strukturnya sebelum dipakai, dan mengisi kolom isian dokumen. Ini perjalanan utama persona di doc 11: sekretaris membuka kategori Notula, memilih template, mengisi tanggal dan tempat rapat, lalu menempel catatan.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-17 | Katalog 8 kategori, pencarian nama, detail template, dan pratinjau struktur, memakai `GET /categories` dari H17. | Filter memakai 8 kunci tetap; pratinjau tidak membuat dokumen | 🔒 [doc 12, template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#template-tpl) |
| KF-DOK-05 | Penanda kolom isian wajib yang kosong sebelum ekspor. | Ekspor tetap diizinkan, pengguna melihat daftar kolom yang belum diisi | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| FE-04 lanjutan | Form buat dokumen memakai 8 kategori, bukan `report_types`. | Tidak ada istilah akademik yang tampil | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-21 | Ringkasan `stats` di beranda: draf, final, template siap (bila kapasitas ada). | Angka sesuai data dari API | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |

## Rincian pekerjaan

### FE-17 · Katalog

Halaman `/template` diperluas:

| Elemen | Isi | Data |
|---|---|---|
| Tab kategori | 8 kategori dengan warna map yang sama dengan landing, ditambah "Semua". Pilihan disimpan di URL `?kategori=notula` agar bisa dibagikan. | `GET /categories` |
| Kotak cari | Nama dan kode, misalnya `SRT-01` atau "undangan". Debounce 300 ms. | `GET /templates?category=&q=` |
| Kelompok | Template milik saya, lalu template bawaan. Template resmi ruang kerja menyusul di fase 3. | |
| Kartu | Kode, nama, kalimat "Kapan dipakai", badge status pola untuk template unggahan | |
| Detail `/template/[id]` | Daftar bagian, kolom isian wajib, tata letak ekspor (sampul, daftar isi), dokumen terkait, tombol "Pakai template" | `GET /templates/{id}` |
| Kosong | Menyarankan kategori lain dan tombol unggah template | |

Membuka detail tidak membuat dokumen. Dokumen baru dibuat hanya saat "Pakai template" ditekan. Path `/template/[id]` ditambahkan ke `routes.ts`, dan `matcher` di `proxy.ts` sudah mencakup `/template/:path*`.

### KF-DOK-01 dan KF-DOK-05 · Kolom isian

Langkah:

1. Setelah "Pakai template", form buat dokumen menampilkan langkah kolom isian yang dibangun dari kolom template. Pemetaan tipe ke komponen: `teks` ke `Input`, `teks_panjang` ke `Textarea`, `tanggal` dan `waktu` ke input tanggal dan waktu, `angka` ke input angka, `rupiah` ke input angka dengan format "Rp1.250.000" saat tampil, `orang` dan `daftar` ke daftar baris yang bisa ditambah.
2. Kolom bersumber `sistem`, seperti nomor dokumen, tampil terkunci dengan keterangan "diisi saat final".
3. Pengguna boleh melewati langkah ini.
4. Dialog "Data dokumen" di editor memakai komponen yang sama untuk mengubah `field_values`.
5. Sebelum unduh, bila detail dokumen menyebut kolom wajib yang kosong, tampilkan daftar kolom itu dengan tombol "Lengkapi" dan "Tetap unduh".

Bukti selesai: E2E membuat dokumen dari template bawaan dengan satu kolom wajib kosong, lalu peringatan muncul sebelum unduh.

### FE-04 lanjutan · Form memakai kategori

Langkah: ganti sumber pilihan jenis dokumen dari `report_types` ke `GET /categories`, kirim `category`, dan hapus pemakaian `report_types`. Setelah web tidak lagi membacanya, beri tahu Octaf agar `report_types` bisa dihapus sesuai rencana penggantian field.

### FE-21 · Ringkasan beranda

`GET /dashboard` sudah mengirim `stats` (`laporan_draft`, `laporan_final`, `template_total`, `template_ready`), tetapi beranda belum menampilkannya. Tampilkan sebagai tiga angka ringkas di atas "Lanjutkan pekerjaanmu". Kerjakan bila kapasitas ada.

## Urutan kerja yang disarankan

1. Tata letak katalog dengan tipe sementara, sambil menunggu H17.
2. Sambungkan ke API setelah [H17](../serah-terima/H17.md).
3. Kolom isian dan penanda kolom wajib.
4. FE-04 lanjutan, lalu FE-21.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H17 · CORE-4 → WEB-4 · kategori dan kolom isian**](../serah-terima/H17.md) | Octaf (BE 1), [CORE-4](CORE-4.md) | `GET /categories`, `template_fields`, `laporans.field_values` di `openapi.json` | FE-17 katalog dan kolom isian |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Katalog dan kolom isian tampil di 360 px dan desktop
- [ ] Tidak ada pemakaian `report_types` di `src/`
- [ ] Web doc 03 diperbarui untuk katalog, detail template, dan kolom isian

## Dokumen rujukan lain

- 🔒 [Web doc 03, template](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/03-peta-halaman.md#template-template)
- 🔒 [Web doc 01, delapan kategori](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/01-konteks-produk.md#delapan-kategori-dokumen)
- 🔒 [Spesifikasi template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
