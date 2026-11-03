[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-1](AI-1.md) · Berikutnya: [AI-3](AI-3.md)

# AI-2 · Provider palsu untuk E2E

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `test/be-ai-stub-provider` |
| Mulai setelah | [AI-1](AI-1.md) selesai |
| Lanjut ke | [AI-3](AI-3.md) |

## Tujuan

E2E web bisa menjalankan perapihan sampai terima usulan tanpa memanggil Groq atau Gemini sungguhan.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-23 | Menyepakati dengan Alvira cara mengarahkan `AI_*` ke provider palsu lokal untuk CI web, lalu menyediakannya di API. | Job E2E web bisa menjalankan perapihan tanpa kunci sungguhan | 🔒 [web doc 06, pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#pengujian) |
| Dokumentasi | Menulis caranya di doc 09 API dan merujuknya dari doc 07 web. | Louis bisa memakai cara yang sama untuk uji manual | 🔒 [Doc 09 setup](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/09-setup-development.md) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H8 · AI-2 → WEB-5 · stub provider untuk E2E** | Alvira (FE), [WEB-5](WEB-5.md) | Cara mengarahkan `AI_*` ke provider palsu lokal, tertulis di doc 09 API. Juga dipakai Louis. | Issue kesepakatan `be-ai` dan `fe` |

## Selesai bila

- [ ] Cara stub tertulis di doc 09
- [ ] H8 terkirim ke Alvira dan Louis

## Dokumen rujukan lain

- 🔒 [Aturan menulis tes E2E](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#aturan-menulis-tes-e2e)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
