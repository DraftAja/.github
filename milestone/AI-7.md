[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-6](AI-6.md)

# AI-7 · Ringkasan, teks ke tabel, dan pengingat

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-ai-ai7-teks-ke-tabel` |
| Saran urutan | [AI-6](AI-6.md) selesai, menerima H30. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | selesai |

## Tujuan

Pemilik usaha melihat tujuh angka utama dan rekap tanpa menghitung, menempelkan catatan harian lalu mendapat usulan baris tabel tanpa angka karangan, dan diingatkan tepat waktu saat stok menipis, retur perlu diambil, atau invoice perlu dikirim.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-28 | `/summary`, `/recaps` (`USH-08`), `report_templates` | Tes U08, U09, U12, U14, U16; `penjualan_titip` sama dengan jumlah invoice periode itu; ukuran KN-11 | 🔒 [doc 21, angka ringkasan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/21-perhitungan-usaha.md#angka-ringkasan) |
| BE-29 | Operasi `sheet_tidy`, `SheetTidyPrompt`, `SheetOutputValidator`, terima sebagian baris | Regresi prompt nol angka tambahan untuk Groq dan Gemini; kolom `sistem` tidak pernah terisi | 🔒 [aturan AI umum](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/09-usaha/README.md#aturan-ai-umum) |
| BE-30 | `reminder_schedules`, `usaha:reminders`, peringatan stok, ringkasan stok harian, `notifications` | Tes waktu dibekukan; U10 menghasilkan tepat dua peringatan; tidak ada pengingat ganda | 🔒 [doc 06, W16](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#w16-peringatan-dan-pengingat) |

## Rincian pekerjaan

### BE-28 · Ringkasan dan rekap

Masalah sekarang: rekap mingguan dan bulanan dihitung tangan, dan angka bulan sering tidak sama dengan jumlah minggunya karena dibulatkan berbeda.

Langkah:

1. Tes kasus U08, U09, U12, U14, dan U16 lebih dulu, termasuk selisih Rp1 di U12.
2. Perluas `BusinessFigures` dari H30. Satu rumus untuk ringkasan, rekap, dan invoice.
3. Periode dihitung di zona waktu usaha dari `occurred_on`.
4. `report_templates`: kolom dan filter divalidasi terhadap daftar tetap per sumber. Template tidak menyimpan angka.
5. `UsahaLoadSeeder` dan ukur KN-11. Batasi jumlah query per permintaan di tes.

Bukti selesai: tes kasus hijau di kedua basis data, dan hasil ukur KN-11 dicatat di doc 10.

Hindari: tabel ringkasan yang disimpan (cache angka). Bila performa kurang, tambah indeks dulu dan ukur ulang.

### BE-29 · Teks mentah ke tabel

Masalah sekarang: perapihan AI hanya untuk bagian dokumen. Catatan harian pemilik usaha penuh angka ("laku 18, retur 2, kirim lagi 20") yang tidak boleh diubah atau ditambah.

Langkah:

1. Data regresi dari bagian Contoh `USH-01` sampai `USH-05` dan `USH-07`, ditambah minimal 4 teks lain yang disamarkan.
2. `SheetTidyPrompt` memuat 7 aturan AI umum di katalog `USH`.
3. `SheetOutputValidator`: setiap angka di usulan harus ada di teks setelah normalisasi, kolom `sistem` kosong, dan `ref` yang tidak dikenal ditandai.
4. Operasi `sheet_tidy` lewat `AiGenerationService`, 1 unit kuota per potongan 3.000 karakter, dengan aturan `request_key` yang sama.
5. `accept` menyimpan baris terpilih lewat service modul.
6. Jalankan regresi dengan Groq dan Gemini, lalu kirim [H33](../serah-terima/H33.md).

Bukti selesai: regresi nol angka tambahan, dan tes validator untuk keluaran yang mengarang angka.

Hindari: mengirim alamat, telepon, rekening, atau angka usaha lain ke provider. Yang dikirim hanya teks, kolom, serta kode dan nama barang dan mitra.

### BE-30 · Peringatan dan pengingat

Masalah sekarang: tidak ada notifikasi apa pun di kode. Doc 04 merencanakan tabel `notifications` di fase 3, yang mungkin belum ada saat Fase U.

Langkah:

1. Bila fase 3 belum membuatnya, buat tabel `notifications` dan `GET /notifications` dengan bentuk dari doc 05.
2. Peringatan stok memakai penanda "baru menipis" dari service mutasi CORE-6, dibuat di transaksi yang sama.
3. `reminder_schedules` dan penghitungan `next_run_at` di zona usaha.
4. `usaha:reminders` setiap 5 menit, idempoten per jadwal dan waktu.
5. Email hanya bila `notify_business_email` aktif dan SMTP tersedia.

Bukti selesai: tes dengan `travelTo` untuk setiap aturan KN-15, dan U10 menghasilkan dua peringatan.

## Urutan kerja yang disarankan

1. BE-28 setelah H30.
2. BE-30.
3. BE-29, karena regresi prompt butuh kunci provider dan waktu.
4. Kirim [H32](../serah-terima/H32.md) setelah BE-28 dan BE-30 merge, lalu perbarui H32 saat BE-29 merge.

## Serah terima masuk

Tidak ada yang perlu ditunggu. Mulai saja; serah terima yang belum dilaporkan diganti data contoh, tipe sementara, atau provider palsu, sesuai bagian "Selama belum dilaporkan" di halaman serah terimanya. Bila ada laporan masuk, kamu yang di-mention sebagai reviewer.


| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H30 · CORE-7 → AI-7 · data invoice untuk rekap dan pengingat**](../serah-terima/H30.md) | Octaf (BE 1), [CORE-7](CORE-7.md) | `BusinessFigures` dan status invoice | BE-28 dan pengingat invoice di BE-30 |

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H32 · AI-7 → WEB-7 · openapi ringkasan, teks ke tabel, pengingat**](../serah-terima/H32.md) | Alvira (FE), [WEB-7](WEB-7.md) | `openapi.json` dengan endpoint BE-28 sampai BE-30 | PR API merge, label `butuh-fe` di issue web |
| [**H33 · AI-7 → QA-7 · hasil regresi prompt teks ke tabel**](../serah-terima/H33.md) | Louis (QA), [QA-7](QA-7.md) | Keluaran regresi untuk Groq dan Gemini | Dicatat di doc 10 dan `docs-ai/07-testing.md` |

## Selesai bila

- [ ] Tes BE-28 sampai BE-30 lulus di SQLite dan PostgreSQL 16
- [ ] Regresi prompt nol angka tambahan tercatat
- [ ] Ukuran KN-11 dan KN-15 tercatat di doc 10
- [ ] KF-USH-09, 10, 12, 13, 14, 16 berlabel Sudah ada di doc 10, 12, dan 20
- [ ] H32 dan H33 dilaporkan di issue dan reviewer di-mention

## Dokumen rujukan lain

- 🔒 [Doc 06, W15](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/06-business-workflows.md#w15-teks-mentah-ke-tabel)
- 🔒 [ADR 0004, AI hanya merapikan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/adr/0004-ai-hanya-merapikan.md)
- 🔒 [Doc 16, regresi prompt](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
