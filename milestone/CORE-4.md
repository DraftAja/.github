[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-3](CORE-3.md) · Berikutnya: [CORE-5](CORE-5.md)

# CORE-4 · Katalog 64 template

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core4-katalog-template` |
| Mulai setelah | [CORE-3](CORE-3.md) selesai |
| Lanjut ke | [CORE-5](CORE-5.md) |

## Tujuan

Pengguna bisa memilih dari 64 template bawaan, 8 per kategori, lengkap dengan kolom isian yang disimpan per dokumen. Ini fitur inti MVP: sekretaris tidak perlu mencari dokumen lama untuk disalin, karena struktur dan kolom isiannya sudah tersedia.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-01 | Definisi 64 template di `TemplateSeeder` dari berkas spesifikasi di `docs/templates/`. Seeder bisa dijalankan ulang tanpa duplikasi. | `TemplateSeedTest` mengharapkan 64, kode unik, tepat 8 per kategori | 🔒 [Katalog template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md) |
| KF-TPL-01 | Tes kesesuaian: membaca bagian "Struktur" setiap spesifikasi dan membandingkannya dengan `sections` di seeder. | Tes gagal bila spesifikasi atau seeder diubah tanpa yang lain | 🔒 [doc 16, tes fase 2](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |
| KF-DOK-01, KF-DOK-05 | Tabel `template_fields` dan kolom `laporans.field_values`. | Nilai tersimpan di `field_values`; kolom wajib yang kosong menghasilkan peringatan sebelum ekspor | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| Fase 2 | `GET /categories` sebagai pengganti `report_types` di `GET /laporan/options`. | `report_types` ditandai deprecated sesuai rencana penggantian field | 🔒 [Rencana penggantian field](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#rencana-penggantian-field) |
| BE-12 | Respons buka template memakai `laporan_id`; `url` ditandai deprecated. | Web tidak lagi membaca `url` | 🔒 [S-09](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-13 | Paginasi `GET /laporan` (`?page=`, `per_page` maksimal 50). Tanpa parameter, perilaku lama dipertahankan. | Uji beban 200 dokumen di bawah 1 detik p95 dicatat di doc 10 | 🔒 [S-11](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |

## Rincian pekerjaan

### KF-TPL-01 · 64 template di seeder

Masalah sekarang: `TemplateSeeder::definitions()` berisi 3 template (laporan kegiatan sekolah, laporan berkala, notulen rapat) dengan kategori lama, dan `TemplateSeedTest` mengunci jumlahnya dengan `assertCount(3, $builtins)`. Spesifikasi 64 template sudah ditulis di `docs/templates/`, satu berkas per template. Setiap berkas punya bagian Kapan dipakai, Struktur, Kolom isian, Panduan isi, Contoh, Variasi instansi, dan Dokumen terkait.

Langkah:

1. Tambah kolom `code` (nullable, unique) dan `review_mode` (nullable) di `templates` lewat migration baru. Template unggahan pengguna tidak punya `code`.
2. Ubah bentuk definisi seeder: `code`, `category` (enum dari CORE-3), `name`, `description` dari "Kapan dipakai", `sections` dari "Struktur", dan `fields` dari "Kolom isian".
3. Tulis 64 definisi. Nama bagian harus sama persis dengan spesifikasi, karena tes kesesuaian membandingkan teksnya.
4. Seeder tetap memakai `updateOrCreate` berdasarkan `code`, supaya bisa dijalankan ulang tanpa menggandakan template, upload, atau pola, dan tidak menimpa metadata yang sudah diubah pengguna.
5. Ubah label Status di setiap berkas spesifikasi dan di katalog `docs/templates/README.md` dari Rencana ke Sudah ada, dalam PR yang sama.

Bukti selesai: `TemplateSeedTest` mengharapkan 64 template, kode unik, tepat 8 per kategori, dan menjalankan seeder dua kali tanpa duplikasi.

Hindari: memecah 64 template ke banyak PR yang mengubah `assertCount` berkali-kali. Bila terlalu besar untuk satu PR, pecah per kategori dan naikkan angka tes 8 per PR.

### KF-TPL-01 · Tes kesesuaian spesifikasi dan seeder

Langkah: tes yang membaca setiap berkas `docs/templates/0N-*/XXX-NN-*.md`, mengambil daftar bagian dari "Struktur" dan kolom dari "Kolom isian", lalu membandingkannya dengan definisi seeder untuk `code` yang sama.

Bukti selesai: mengubah satu nama bagian di spesifikasi tanpa mengubah seeder membuat tes gagal.

### KF-DOK-01, KF-DOK-05 · Kolom isian

Langkah:

1. Tabel `template_fields`: `template_id` (FK dengan `->index()`), `key` (snake_case, unik per template), `label`, `type` (`teks`, `teks_panjang`, `tanggal`, `waktu`, `angka`, `rupiah`, `orang`, `daftar`), `required`, `source` (`input`, `profil_instansi`, `sistem`), `sort`.
2. Kolom `laporans.field_values` (json). Disimpan di dokumen, bukan tabel terpisah, karena selalu dibaca bersama dokumen (alasan di doc 04).
3. Dokumen baru dari template mewarisi kategori dan daftar kolom.
4. Validasi `field_values` di FormRequest terhadap `template_fields`: tipe sesuai, `key` dikenal.
5. Detail dokumen mengirim daftar `key` kolom wajib yang masih kosong. Ekspor tetap diizinkan.

Bukti selesai: tes nilai tersimpan di `field_values`; tes `key` tak dikenal ditolak 422; tes daftar kolom wajib kosong muncul di detail dokumen.

### Kategori lewat API

Langkah: `GET /categories` membalas 8 kategori dengan `key`, `label`, `color`, `ink`, dan urutan, dari `DocumentCategory`. Warna disalin dari landing web (`src/features/landing/content.ts`), sehingga API menjadi satu sumber. `GET /templates` menerima `category` dan `q`. `GET /templates/{id}` membalas detail dengan bagian dan kolom isian. `report_types` di `GET /laporan/options` ditandai deprecated.

### BE-12 · `url` absolut di respons buka template

Masalah sekarang: respons `templates.document` dan `templates.use` berisi `url` absolut `FRONTEND_URL/dokumen/{id}`. API jadi mengetahui path halaman web. Web sudah tidak membaca field ini.

Langkah: tandai `url` deprecated di Resource dan `openapi.json`, lalu hapus satu sprint kemudian.

### BE-13 · Paginasi daftar dokumen

Masalah sekarang: `GET /laporan` membalas semua dokumen milik pengguna sekaligus. KN-04 menargetkan akun dengan 200 dokumen tetap di bawah 1 detik.

Langkah: bila ada `?page=`, pakai paginasi bawaan Laravel dengan `per_page` maksimal 50 dan sertakan `meta` serta `links`. Tanpa parameter, perilaku lama dipertahankan sampai web pindah. Ukur dengan 200 dokumen di lokal, catat p95 di doc 10.

## Urutan kerja yang disarankan

1. Migration `code`, `template_fields`, dan `field_values`.
2. Seeder 64 template dan kedua tes, per kategori.
3. `GET /categories`, saringan, dan detail template.
4. BE-12 dan BE-13.

Kirim [H17](../serah-terima/H17.md) setelah langkah 3 merge, dan [H18](../serah-terima/H18.md) setelah langkah 2 selesai untuk 8 kategori.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H17 · CORE-4 → WEB-4 · kategori dan kolom isian**](../serah-terima/H17.md) | Alvira (FE), [WEB-4](WEB-4.md) | `GET /categories`, `template_fields`, `laporans.field_values` di `openapi.json` | PR API merge, label `butuh-fe` di issue web |
| [**H18 · CORE-4 → QA-4 · 64 template di seeder**](../serah-terima/H18.md) | Louis (QA), [QA-4](QA-4.md) | 64 template di `TemplateSeeder` sesuai `docs/templates/` | PR API merge |

## Selesai bila

- [ ] 64 template ter-seed dan tes kesesuaian lulus
- [ ] `openapi.json` memuat endpoint dan field baru
- [ ] H17 dan H18 terkirim

## Dokumen rujukan lain

- 🔒 [Spesifikasi template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md)
- 🔒 [Doc 04, skema target](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md#skema-target)
- 🔒 [Doc 09, menambah template bawaan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/09-setup-development.md#menambah-template-bawaan)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
