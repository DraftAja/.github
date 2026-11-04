[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-3](AI-3.md) · Berikutnya: [AI-5](AI-5.md)

# AI-4 · Uji provider sungguhan

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel (dokumentasi) |
| Contoh branch | `docs/be-ai-ai4-hasil-smoke-test` |
| Mulai setelah | [AI-3](AI-3.md) selesai, menerima H16 |
| Lanjut ke | [AI-5](AI-5.md) |

## Tujuan

Membuktikan analisis pola dan perapihan berjalan dengan Groq dan Gemini sungguhan, dan mengetahui biayanya. Sampai sekarang kedua klien hanya diuji dengan HTTP palsu. Angka token per dokumen dibutuhkan untuk menetapkan kuota paket Gratis di doc 14.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| docs-ai 08 butir 2 | Smoke test Groq dan Gemini untuk analisis pola dan perapihan, dengan kunci dari H16. | Kedua provider berhasil; hasil tanpa rahasia tercatat | 🔒 [Roadmap modul AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/08-roadmap.md) |
| Doc 10 langkah 2 | Mencatat token rata-rata per dokumen untuk perhitungan biaya. | Angka token tercatat di doc 10 dan dipakai doc 14 | 🔒 [doc 14, struktur biaya](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/14-model-bisnis.md#struktur-biaya) |

## Rincian pekerjaan

### Smoke test provider

Langkah, di lokal dengan SQLite dan kunci dari [H16](../serah-terima/H16.md) di `.env`:

1. Pastikan `GET /api/v1/templates` melaporkan `providers.groq.available` dan `providers.gemini.available` bernilai `true`. Bila `false`, `reason` menyebut penyebabnya: kunci kosong, model kosong, atau model tidak lagi didukung.
2. Jalankan `php artisan queue:work` di terminal terpisah.
3. Untuk setiap provider:

   | Operasi | Cara | Yang dicatat |
   |---|---|---|
   | Analisis pola | Unggah satu DOCX contoh yang dibuat dari spesifikasi template, bukan dokumen asli instansi | Status akhir, durasi, jumlah heading terbaca, alasan gagal bila gagal |
   | Perapihan | Dokumen dengan 3 bagian berisi catatan dari bagian "Contoh" spesifikasi | Status akhir, jumlah potongan, durasi, apakah usulan bisa diterima |
   | Chat | Tiga pertanyaan: satu dalam batas, satu di luar batas, satu meminta mengarang angka | Apakah jawaban mengikuti persona AI-3 |

4. Ambil `provider`, `model`, `prompt_tokens`, `completion_tokens`, dan `duration_ms` dari tabel `ai_generations`. Kolom itu sudah ditulis untuk setiap panggilan, termasuk yang gagal.

Bukti selesai: tabel hasil per provider di `docs-ai/07-testing.md` bagian ledger verifikasi dan di doc 10 bagian verifikasi, berisi tanggal, model, status, dan durasi. Tidak ada kunci, isi `.env`, atau isi dokumen pengguna di catatan.

Hindari: menjalankan uji ini di CI atau di database bersama. Uji ini memakai kunci berbayar dan hanya dijalankan di mesin lokal dalam batas izin H16.

### Token per dokumen

Langkah: dari panggilan perapihan di atas, hitung rata-rata token masukan dan keluaran per potongan dan per dokumen, terpisah untuk Groq dan Gemini. Catat di doc 10 dan beri tahu Raja agar doc 14 bagian struktur biaya bisa diisi.

## Urutan kerja yang disarankan

1. Cek kesiapan provider.
2. Analisis pola, perapihan, dan chat per provider.
3. Catat hasil dan token dalam satu PR dokumentasi.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H16 · PM-3 → AI-4 · kunci Groq dan Gemini**](../serah-terima/H16.md) | Raja (PM), [PM-3](PM-3.md) | Kunci Groq dan Gemini, izin pemakaian terbatas, nama `GEMINI_MODEL` yang aktif | Smoke test provider dan regresi prompt |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Kedua provider berhasil untuk analisis pola dan perapihan, atau kegagalannya tercatat dengan alasan
- [ ] Token rata-rata tercatat di doc 10
- [ ] Tidak ada rahasia di repo

## Dokumen rujukan lain

- 🔒 [Doc 08, Groq dan Gemini](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/08-integrations.md#groq-dan-gemini)
- 🔒 [docs-ai, testing](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/07-testing.md)
- 🔒 [Doc 10, verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#verifikasi)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
