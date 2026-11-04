[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-1](AI-1.md) · Berikutnya: [AI-3](AI-3.md)

# AI-2 · Provider palsu untuk E2E

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `test/be-ai-ai2-stub-provider` |
| Mulai setelah | [AI-1](AI-1.md) selesai |
| Lanjut ke | [AI-3](AI-3.md) |

## Tujuan

E2E web bisa menjalankan perapihan sampai terima usulan tanpa memanggil Groq atau Gemini sungguhan. Tanpa ini, fitur yang paling membedakan DraftAja, yaitu merapikan catatan mentah, tidak pernah diuji dari antarmuka.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-23 | Menyepakati dengan Alvira cara mengarahkan `AI_*` ke provider palsu lokal untuk CI web, lalu menyediakannya di API. | Job E2E web bisa menjalankan perapihan tanpa kunci sungguhan | 🔒 [web doc 06, pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#pengujian) |
| Dokumentasi | Menulis caranya di doc 09 API dan merujuknya dari doc 07 web. | Louis bisa memakai cara yang sama untuk uji manual | 🔒 [Doc 09 setup](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/09-setup-development.md) |

## Rincian pekerjaan

### FE-23 · Provider palsu

Masalah sekarang: job E2E web menjalankan API `develop` sungguhan dengan SQLite dan seeder, tetapi tanpa kunci provider. `GET /templates` melaporkan kedua provider belum siap, sehingga unggah template, perapihan, dan chat tidak bisa diuji. Tes PHPUnit memakai HTTP palsu di dalam proses, dan cara itu tidak bisa dipakai oleh proses API yang dijalankan Playwright.

Pilihan yang dipertimbangkan:

| Cara | Kelebihan | Kekurangan |
|---|---|---|
| Mengarahkan `GROQ_BASE_URL` ke server HTTP palsu di repo web | Kode API tidak berubah | Server palsu harus memahami prompt dan skema JSON perapihan, termasuk mempertahankan `id` blok dan token `{{P:n}}`. Rapuh setiap kali prompt berubah. |
| Kelas `FakeAiClient` di API yang dipilih `AiClientFactory` saat flag lokal menyala | Satu tempat, dirawat `be-ai`, mengikuti perubahan prompt | Menambah kode yang harus dijaga agar tidak aktif di produksi |

Usulan: cara kedua. Langkah:

1. Sepakati nama variabel dengan Alvira di issue, misalnya `AI_FAKE=true`.
2. Buat `App\Services\Ai\Clients\FakeAiClient` yang mengimplementasikan `AiClient` (`provider`, `model`, `isReady`, `readiness`, `complete`). `provider()` mengembalikan provider yang diminta, sehingga enum `groq` dan `gemini` di kontrak tidak berubah. `model()` mengembalikan `fake`.
3. `complete()` memilih jawaban dari bentuk permintaan:
   - analisis pola: JSON pola tetap yang lolos validasi `PatternLearner`;
   - perapihan: blok yang sama dengan masukan (`id`, urutan, dan token `{{P:n}}` dipertahankan), dengan perubahan kecil yang bisa dikenali tes, misalnya huruf awal kalimat dijadikan kapital;
   - chat dan generate: kalimat tetap berbahasa Indonesia.
4. Sediakan cara memicu kegagalan untuk kasus negatif, misalnya catatan yang memuat penanda khusus membuat `FakeAiClient` melempar pengecualian provider, sehingga API membalas `provider_failed`.
5. `AiClientFactory::make()` mengembalikan `FakeAiClient` hanya bila flag menyala dan `app()->environment()` bernilai `local` atau `testing`.
6. Tambah aturan di `app/Support/EnvContract.php`: flag ini ditolak di staging dan produksi, sehingga `env:verify` gagal dan container tidak mau start.
7. Tambah variabel ke `.env.example` tanpa nilai dan ke `docs/ENV_CONTRACT.md`.

Bukti selesai: tes unit `FakeAiClient` untuk ketiga jenis permintaan; tes `EnvContract` menolak flag di `production`; tes fitur perapihan sampai accept dengan flag menyala.

Hindari: menambah nilai `fake` ke enum `AiProvider`. Nilai itu akan muncul di `openapi.json` dan tipe web, padahal hanya untuk uji.

### Dokumentasi

Langkah: tulis bagian "Provider palsu untuk uji" di doc 09 API berisi cara menyalakan, perilaku setiap operasi, dan cara memicu kegagalan. Minta Alvira menautkannya dari web doc 07 dan menambah variabel itu di langkah "Siapkan API" job E2E web.

## Urutan kerja yang disarankan

1. Issue kesepakatan dengan Alvira.
2. `FakeAiClient`, pemilihan di factory, dan aturan `EnvContract` dalam satu PR.
3. Doc 09.

Kirim [H8](../serah-terima/H8.md) setelah PR merge.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H8 · AI-2 → WEB-5 · stub provider untuk E2E**](../serah-terima/H8.md) | Alvira (FE), [WEB-5](WEB-5.md) | Cara mengarahkan `AI_*` ke provider palsu lokal, tertulis di doc 09 API. Juga dipakai Louis. | Issue kesepakatan `be-ai` dan `fe` |

## Selesai bila

- [ ] Provider palsu aktif hanya di `local` dan `testing`, dibuktikan tes
- [ ] Doc 09 API dan `docs/ENV_CONTRACT.md` diperbarui
- [ ] H8 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Doc 08, Groq dan Gemini](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/08-integrations.md#groq-dan-gemini)
- 🔒 [docs-ai, layanan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/04-service-layer.md)
- 🔒 [Web doc 07, aturan menulis tes E2E](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#aturan-menulis-tes-e2e)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
