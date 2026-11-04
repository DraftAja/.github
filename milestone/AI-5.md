[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-4](AI-4.md)

# AI-5 · Regresi prompt nol fakta tambahan

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel |
| Contoh branch | `test/be-ai-ai5-regresi-prompt` |
| Mulai setelah | [AI-4](AI-4.md) selesai |
| Lanjut ke | selesai |

## Tujuan

Membuktikan aturan dasar AI: AI tidak menambah fakta yang tidak ada di catatan pengguna. Ini kriteria rilis MVP butir 4. Bila satu fakta tambahan ditemukan, prompt diperbaiki dan seluruh uji diulang.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-AI-02 | Minimal 10 pasang catatan dari bagian "Contoh" spesifikasi template, sebaran kategori merata. Tidak memakai dokumen asli instansi. | Data uji tersedia | 🔒 [doc 16, regresi prompt AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai) |
| KF-AI-02 | Pemeriksaan otomatis: setiap angka, tanggal, dan kata berhuruf kapital di keluaran harus ada di catatan masukan. | Nol fakta tambahan untuk Groq dan Gemini | 🔒 [doc 12, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ai) |
| KF-AI-02 | Pemeriksaan penanda: catatan yang tidak menyebut informasi wajib menghasilkan `[perlu dilengkapi]`. | Tidak ada isian karangan | 🔒 [doc 16, regresi prompt AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai) |

## Rincian pekerjaan

### Data uji

Setiap spesifikasi di `docs/templates/` punya bagian "Contoh" berisi catatan mentah dan hasil yang diharapkan. Langkah:

1. Pilih minimal 10 contoh, minimal satu dari setiap kategori yang paling sering dipakai persona (Notula, Surat, Laporan, Proposal), sisanya merata.
2. Untuk minimal 3 contoh, buat versi yang sengaja tidak menyebut informasi wajib template (misalnya tanggal rapat), untuk menguji penanda `[perlu dilengkapi]`.
3. Simpan sebagai berkas data di `tests/Fixtures/ai-regression/`, satu berkas per contoh, berisi catatan, kode template, dan daftar informasi wajib yang sengaja dihilangkan.

### Pemeriksaan otomatis

Uji ini memanggil provider sungguhan, jadi dipisah dari `php artisan test`. Langkah:

1. Buat perintah konsol, misalnya `php artisan ai:regression {provider}`, yang menolak berjalan bila `APP_ENV` bukan `local`.
2. Untuk setiap contoh: buat dokumen sementara dari template, isi bagian dengan catatan, jalankan perapihan lewat `DocumentCleanupService`, tunggu selesai, ambil usulan.
3. Dari keluaran, kumpulkan semua angka, tanggal, dan kata berhuruf kapital yang bukan awal kalimat. Setiap item harus muncul di catatan masukan. Item yang tidak ada dihitung sebagai fakta tambahan.
4. Untuk contoh yang sengaja tidak lengkap, periksa `[perlu dilengkapi]` muncul dan tidak ada isian karangan di tempat itu.
5. Cetak tabel hasil dan simpan keluaran mentah ke berkas di luar repo, supaya bisa diserahkan ke Louis.
6. Jalankan perintah itu lebih dulu dengan provider palsu dari AI-2, untuk memastikan logikanya benar sebelum memakai kunci berbayar.

Ambang lulus: nol fakta tambahan di semua contoh, untuk Groq dan Gemini. Usulan yang ditolak validator karena angka atau tanggal berubah dihitung sebagai gagal, bukan dilewati, karena di antarmuka pengguna akan melihat perapihan gagal.

### Bila ada fakta tambahan

1. Catat contoh, provider, dan item yang ditambahkan.
2. Perbaiki `CleanupPrompt`, atau perketat `DocumentOutputValidator::validateCleanup`. Validator itu sudah menolak keluaran yang mengubah angka, tanggal, URL, atau sitasi di satu blok dibanding sumbernya, serta keluaran yang memuat HTML atau mengubah token terlindungi `{{P:n}}`. Validator belum memeriksa nama orang atau instansi yang muncul baru, dan tidak bisa menilai perubahan makna. Bila fakta tambahannya berupa nama, pertimbangkan pemeriksaan kata berhuruf kapital yang sama dengan pemeriksaan otomatis di atas.
3. Ulangi seluruh rangkaian, bukan hanya contoh yang gagal.
4. Catat riwayat perbaikan di `docs-ai/07-testing.md`.

## Urutan kerja yang disarankan

1. Data uji dan perintah konsol, diuji dengan provider palsu.
2. Putaran pertama dengan Groq dan Gemini.
3. Perbaikan prompt bila perlu, lalu putaran ulang.
4. Catat hasil di doc 10 dan `docs-ai/07-testing.md`.

Kirim [H19](../serah-terima/H19.md) setelah putaran yang lulus tercatat.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain. Kunci provider sudah diterima di AI-4 lewat H16.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H19 · AI-5 → QA-4 · hasil regresi prompt**](../serah-terima/H19.md) | Louis (QA), [QA-4](QA-4.md) | Keluaran minimal 10 pasang catatan untuk Groq dan Gemini | Dicatat di doc 10 dan `docs-ai/07-testing.md` |

## Selesai bila

- [ ] Nol fakta tambahan untuk kedua provider
- [ ] Hasil tercatat di doc 10 dan `docs-ai/07-testing.md`
- [ ] H19 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Aturan AI di katalog template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md)
- 🔒 [docs-ai, testing](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/07-testing.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
