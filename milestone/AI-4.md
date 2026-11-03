[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-3](AI-3.md) · Berikutnya: [AI-5](AI-5.md)

# AI-4 · Uji provider sungguhan

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel (dokumentasi) |
| Contoh branch | `docs/be-ai-hasil-smoke-test` |
| Mulai setelah | [AI-3](AI-3.md) selesai, menerima H16 |
| Lanjut ke | [AI-5](AI-5.md) |

## Tujuan

Membuktikan analisis pola dan perapihan berjalan dengan Groq dan Gemini sungguhan, dan mengetahui biayanya.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| docs-ai 08 butir 2 | Smoke test Groq dan Gemini untuk analisis pola dan perapihan, dengan kunci dari H16. | Kedua provider berhasil; hasil tanpa rahasia tercatat | 🔒 [Roadmap modul AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/08-roadmap.md) |
| Doc 10 langkah 2 | Mencatat token rata-rata per dokumen untuk perhitungan biaya. | Angka token tercatat di doc 10 dan dipakai doc 14 | 🔒 [doc 14, struktur biaya](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/14-model-bisnis.md#struktur-biaya) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H16 · PM-3 → AI-4 · kunci Groq dan Gemini** | Raja (PM), [PM-3](PM-3.md) | Kunci Groq dan Gemini, izin pemakaian terbatas, nama `GEMINI_MODEL` yang aktif | Smoke test provider dan regresi prompt |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Hasil tercatat di doc 10 dan `docs-ai/07-testing.md`
- [ ] Tidak ada kunci di repo atau log

## Dokumen rujukan lain

- 🔒 [Doc 08 integrasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/08-integrations.md)
- 🔒 [docs-ai 07 testing](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/07-testing.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
