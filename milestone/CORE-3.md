[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [CORE-2](CORE-2.md) · Berikutnya: [CORE-4](CORE-4.md)

# CORE-3 · Persona dan alur dokumen

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-core-core3-kategori-dokumen` |
| Saran urutan | [CORE-2](CORE-2.md) selesai, menerima H9. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [CORE-4](CORE-4.md) |

## Tujuan

Dokumen berhenti berpersona akademik: kategori memakai 8 kunci, identitas dokumen sesuai kantor, dokumen bisa difinalkan dan direvisi, dan ekspor mengikuti bentuk kategorinya. Milestone ini juga menutup dua celah yang muncul begitu dokumen bisa final: konflik suntingan dari dua tab, dan verifikasi email yang belum diwajibkan.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-05 | Enum `DocumentCategory` dan migrasi nilai kategori lama ke 8 kunci, mengikuti keputusan H9. | Tes: nilai lama termigrasi ke 8 kunci; nilai di luar enum ditolak 422 | 🔒 [doc 12, template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#template-tpl) |
| KF-DOK-04 | Kolom identitas dokumen pengganti kolom akademik, lewat migration baru (bukan mengedit migration lama). | Kolom akademik tidak lagi dikirim ke antarmuka; kolom pengganti tersimpan | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| S-22 | Menghapus jenis dokumen akademik dari `laporan.create` dan `createDefaultSections`. | Tes: `laporan.create` tidak mengirim Skripsi, Makalah, Laporan Praktikum, atau Studi Kasus | 🔒 [S-22](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |
| BE-05 | `POST /laporan/{id}/finalize` dan `POST /laporan/{id}/revisions`. Bergantung pada BE-01. | Tes: final mengisi `finalized_at`; revisi membuat draf baru dengan `revision_of_id`; dokumen final menolak suntingan 403 | 🔒 [KF-TIN-04](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#tinjau-dan-penomoran-tin) |
| BE-07 | Konkurensi optimistis di `PUT /sections/{id}` dengan field opsional `base_updated_at`. | Tes: dua pembaruan dengan `base_updated_at` sama, yang kedua 409 `revision_changed`; tanpa field, perilaku lama tetap | 🔒 [S-12](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-10 | `MustVerifyEmail` di `User`. | Tes: akun belum terverifikasi menerima 409 `email_unverified` di rute aplikasi | 🔒 [S-17](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#keamanan-dan-data) |
| KF-EKS-02, KF-EKS-03 | Tata letak ekspor per kategori: Surat dan Notula tanpa sampul dan daftar isi. | Tes: DOCX Surat tidak memuat sampul dan daftar isi; DOCX `PRP-01` memuat keduanya | 🔒 [doc 12, ekspor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ekspor-eks) |
| INF-08 | Azure Communication Services Email dan domain pengirim untuk development, sebelum BE-10 mewajibkan verifikasi email. | Email atur ulang kata sandi diterima dari development | 🔒 [doc 19, email](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#email) |

## Rincian pekerjaan

### KF-TPL-05 · Kategori 8 kunci

Masalah sekarang: `templates.category` adalah teks bebas dengan nilai lama `laporan_kegiatan`, `laporan_berkala`, dan `notulen`. Dokumen tidak punya kategori, hanya `report_type` berisi nilai akademik.

Langkah:

1. Buat enum `App\Enums\DocumentCategory` dengan 8 kasus: `laporan`, `proposal`, `notula`, `dokumentasi_ti`, `sop`, `surat`, `ringkasan`, `rencana_kerja`. Tambah method `label()` untuk teks tampilan.
2. Migration baru: tambah kolom `category` di `laporans`, isi dari `report_type` memakai tabel pemetaan dari [H9](../serah-terima/H9.md). Migration lain memetakan nilai lama `templates.category`.
3. Cast `category` ke enum di model `Laporan` dan `Template`. Ulangi nilai bawaan migration di `$attributes` model (aturan di `CLAUDE.md`).
4. Validasi `Rule::enum(DocumentCategory::class)` di semua request yang menerima kategori.
5. Tambah `category` ke `LaporanResource` dan `TemplateResource`. `report_type` tetap dikirim dan ditandai deprecated (rencana penggantian field di doc 18).

Bukti selesai: tes migrasi data lama ke kunci baru; tes 422 untuk nilai di luar enum.

### KF-DOK-04 · Kolom identitas kantor

Masalah sekarang: `laporans` masih memuat `nim`, `prodi`, `mata_kuliah`, `dosen_pembimbing`, dan `tahun_ajaran`. `User::$fillable` juga menyebut `nim` dan `prodi`, padahal tabel `users` tidak punya kolom itu.

Langkah:

1. Migration baru menambah kolom pengganti sesuai H9. Usulan doc 04: `nomor_dokumen`, `tanggal_dokumen`, `unit_kerja`, `penandatangan_nama`, `penandatangan_jabatan`.
2. Salin nilai yang bisa dipetakan dalam migration yang sama (`prodi` ke `unit_kerja`).
3. Tambah kolom baru ke `LaporanResource` dan ke validasi `store` serta `update`.
4. Berhenti mengirim kolom akademik di Resource setelah web memakai kolom baru. Drop kolom di migration terpisah, paling cepat satu sprint kemudian.
5. Hapus `nim` dan `prodi` dari `User::$fillable`.

Bukti selesai: tes `POST` dan `PUT /laporan` menyimpan kolom baru; tes `LaporanResource` tidak memuat kolom akademik setelah langkah 4.

Hindari: mengedit migration lama, termasuk merapikannya dengan Pint.

### S-22 · Jenis dokumen akademik dihapus

Masalah sekarang: `LaporanController::REPORT_TYPES` berisi Makalah, Proposal, Laporan Praktikum, Studi Kasus, Skripsi, dan dikirim apa adanya ke web lewat `GET /laporan/options`. `createDefaultSections` membuat bab akademik (Pendahuluan, Pembahasan, Penutup, Daftar Pustaka).

Langkah: ganti sumber pilihan dengan `DocumentCategory`, dan ganti bab bawaan per kategori dengan struktur minimal yang netral (misalnya satu bagian "Isi" untuk dokumen tanpa template). Pesan error yang memakai kata "pattern" atau "templat" ikut diganti menjadi "pola" dan "template".

Bukti selesai: tes `GET /laporan/options` tidak memuat istilah akademik; tes dokumen baru tanpa template tidak berisi "Daftar Pustaka".

### BE-05 · Final dan revisi

Masalah sekarang: model dan policy sudah mengunci dokumen final, dan `markAsFinal()` ada, tetapi tidak ada endpoint yang memanggilnya.

Langkah:

1. Proposal kontrak untuk dua endpoint, disetujui Alvira dan Louis.
2. `POST /laporan/{id}/finalize`: otorisasi pemilik dan status `draft`, panggil `markAsFinal()`, balas `{data: LaporanResource}`. Dokumen yang sudah final membalas 403 `document_final`.
3. Migration: kolom `revision_of_id` (FK ke `laporans`, nullable, dengan `->index()`) dan `revision_no` (integer, bawaan 0).
4. `POST /laporan/{id}/revisions`: dokumen sumber harus final dan milik pengguna. Dalam satu transaksi, salin dokumen dan semua bagiannya (termasuk hubungan induk dan anak), isi `revision_of_id`, naikkan `revision_no`, status `draft`. Balas 201.
5. Ekspor dokumen final sudah bisa sejak BE-01 di CORE-1. Periksa ulang dengan tes.
6. `composer api:spec`, lalu perbarui doc 05 dan doc 06 API.

Bukti selesai: tes final mengisi `finalized_at`; tes final kedua kali 403 `document_final`; tes suntingan dokumen final 403; tes revisi membuat draf baru dengan jumlah bagian sama dan dokumen asal tetap final.

### BE-07 · Konflik suntingan dua tab

Masalah sekarang: `PUT /sections/{id}` tidak memeriksa versi. Editor menyimpan otomatis 1,5 detik setelah ketikan terakhir, jadi dua tab yang membuka bagian sama saling menimpa tanpa peringatan.

Langkah:

1. Terima field opsional `base_updated_at` (ISO 8601, nilai `updated_at` bagian saat dimuat klien).
2. Bila dikirim dan tidak sama dengan `updated_at` di basis data, balas 409 `revision_changed` dengan isi bagian terbaru di body.
3. Bila tidak dikirim, simpan seperti sekarang supaya web versi lama tetap jalan.
4. Pakai field body, bukan header `If-Match`, karena proxy web hanya meneruskan daftar header tertentu.

Bukti selesai: tes dua pembaruan dengan `base_updated_at` yang sama, yang kedua 409; tes tanpa field tetap 200.

### BE-10 · Verifikasi email diwajibkan

Masalah sekarang: `User` belum mengimplementasikan `MustVerifyEmail`, jadi middleware `verified` tidak pernah menolak dan `must_verify_email` selalu `false`. Undangan ruang kerja (fase 3) tidak aman tanpa verifikasi.

Langkah: implementasikan `MustVerifyEmail` di `User`. Pastikan email verifikasi menaut ke `FRONTEND_URL/verifikasi-email?url=...`, sama dengan alur yang sudah ada. Akun uji di `UserSeeder` diberi `email_verified_at` supaya E2E dan uji manual tidak terhalang.

Bukti selesai: tes akun belum terverifikasi menerima 409 `email_unverified` di `GET /me`; tes akun terverifikasi 200; tes E2E web yang mendaftar akun baru tetap lulus atau disesuaikan bersama Alvira.

### KF-EKS-02, KF-EKS-03 · Ekspor per kategori

Masalah sekarang: ekspor DOCX selalu menyisipkan sampul dan daftar isi, sehingga surat dan notula hasil ekspor salah bentuk.

Langkah: tentukan tata letak dari kategori dan kode template. Surat, Notula, SOP, dan Ringkasan tanpa sampul dan tanpa daftar isi. Daftar isi hanya untuk template yang menyebutnya di spesifikasi, misalnya `PRP-01`, `LAP-02`, `DTI-01` (daftar lengkap di doc 08 API). Terapkan aturan yang sama di PDF.

Bukti selesai: tes DOCX dokumen kategori Surat tidak memuat sampul dan daftar isi; tes DOCX `PRP-01` memuat keduanya. Tes ini bisa memakai definisi template sementara sampai 64 template masuk di CORE-4.

### INF-08 · Email keluar

Masalah sekarang: `MAIL_MAILER=log`, jadi email verifikasi dan atur ulang kata sandi hanya tertulis di log. Begitu BE-10 mewajibkan verifikasi, pengguna development tidak bisa masuk tanpa email yang benar-benar terkirim.

Langkah: ikuti doc 19 bagian email. Buat resource Email Communication Services dengan domain Azure-managed untuk development, tautkan ke Communication Services, lalu buat app Entra untuk kredensial SMTP. Isi `MAIL_MAILER=smtp`, `MAIL_HOST=smtp.azurecomm.net`, `MAIL_PORT=587`, dan `MAIL_USERNAME`, dengan `MAIL_PASSWORD` di Key Vault. Periksa dulu pemberitahuan pensiun layanan Azure Communication Services. Bila Email terdampak, pakai penyedia SMTP lain di port 587.

Bukti selesai: email atur ulang kata sandi dari development diterima di kotak masuk sungguhan.

## Urutan kerja yang disarankan

1. BE-07 dan BE-10, yang tidak menunggu keputusan produk.
2. BE-05, yang bergantung pada BE-01 di CORE-1.
3. Setelah [H9](../serah-terima/H9.md): KF-TPL-05, lalu KF-DOK-04 dan S-22.
4. KF-EKS-02 dan KF-EKS-03, yang membutuhkan kategori.

Kirim [H12](../serah-terima/H12.md) setelah BE-05 dan BE-07 merge. Alvira bisa mulai tanpa menunggu butir kategori.

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H9 · PM-2 → CORE-3 · kategori dan kolom identitas**](../serah-terima/H9.md) | Raja (PM), [PM-2](PM-2.md) | Keputusan 8 kunci kategori, pemetaan nilai lama, dan kolom identitas pengganti kolom akademik | Enum `DocumentCategory` dan migrasi kolom (KF-DOK-04) |

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H12 · CORE-3 → WEB-3 · final, revisi, konflik**](../serah-terima/H12.md) | Alvira (FE), [WEB-3](WEB-3.md) | `POST /laporan/{id}/finalize`, `POST /laporan/{id}/revisions`, 409 `revision_changed` | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] Semua tes di kolom kriteria lulus, termasuk di job CI PostgreSQL
- [ ] Doc 04, 05, dan 06 diperbarui untuk kolom dan endpoint baru
- [ ] H12 dilaporkan di issue dan reviewer di-mention ke Alvira

## Dokumen rujukan lain

- 🔒 [Doc 04, kolom yang harus diganti](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/04-database-design.md)
- 🔒 [Doc 06, status dokumen](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#status-dokumen)
- 🔒 [Rencana penggantian field](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#rencana-penggantian-field)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
