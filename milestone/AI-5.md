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

Membuktikan aturan dasar AI: AI tidak menambah fakta yang tidak ada di catatan pengguna.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-AI-02 | Minimal 10 pasang catatan dari bagian "Contoh" spesifikasi template, sebaran kategori merata. Tidak memakai dokumen asli instansi. | Data uji tersedia | 🔒 [doc 16, regresi prompt AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai) |
| KF-AI-02 | Pemeriksaan otomatis: setiap angka, tanggal, dan kata berhuruf kapital di keluaran harus ada di catatan masukan. | Nol fakta tambahan untuk Groq dan Gemini | 🔒 [doc 12, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ai) |
| KF-AI-02 | Pemeriksaan penanda: catatan yang tidak menyebut informasi wajib menghasilkan `[perlu dilengkapi]`. | Tidak ada isian karangan | 🔒 [doc 16, regresi prompt AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H19 · AI-5 → QA-4 · hasil regresi prompt** | Louis (QA), [QA-4](QA-4.md) | Keluaran minimal 10 pasang catatan untuk Groq dan Gemini | Dicatat di doc 10 dan `docs-ai/07-testing.md` |

## Selesai bila

- [ ] Ambang nol fakta tambahan tercapai di kedua provider
- [ ] Hasil tercatat di doc 10 dan `docs-ai/07-testing.md`
- [ ] H19 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Katalog template, bagian Contoh](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
