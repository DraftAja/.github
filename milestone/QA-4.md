[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-3](QA-3.md) · Berikutnya: [QA-5](QA-5.md)

# QA-4 · Uji katalog dan regresi prompt

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-qa4-katalog-dan-regresi` |
| Mulai setelah | [QA-3](QA-3.md) selesai, menerima H18, H19 |
| Lanjut ke | [QA-5](QA-5.md) |

## Tujuan

Memastikan 64 template sesuai spesifikasi dan keluaran AI tidak menambah fakta, lalu menyiapkan uji pengguna.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-01 | Memeriksa 64 template di aplikasi terhadap berkas spesifikasi. | Struktur dan kolom isian sama dengan spesifikasi | 🔒 [Katalog template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md) |
| KF-AI-02 | Pemeriksaan manual keluaran regresi dari H19: apakah gaya mengikuti pola dan tidak ada fakta tambahan. | Nol fakta tambahan disetujui | 🔒 [doc 16, regresi prompt AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai) |
| Doc 16 uji pengguna | Menyiapkan sesi uji pengguna setelah fase 2 bersama sekretaris atau staf TU. | Jadwal, tugas, dan ukuran sesi ditetapkan bersama Raja | 🔒 [doc 16, uji pengguna](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#uji-pengguna) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H18 · CORE-4 → QA-4 · 64 template di seeder** | Octaf (BE 1), [CORE-4](CORE-4.md) | 64 template di `TemplateSeeder` sesuai `docs/templates/` | Memeriksa kesesuaian spesifikasi dan seeder |
| **H19 · AI-5 → QA-4 · hasil regresi prompt** | Hamid (BE 2), [AI-5](AI-5.md) | Keluaran minimal 10 pasang catatan untuk Groq dan Gemini | Pemeriksaan manual fakta tambahan dan penanda `[perlu dilengkapi]` |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Hasil pemeriksaan katalog dan regresi tercatat di doc 10

## Dokumen rujukan lain

- 🔒 [docs-ai 07 testing](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs-ai/07-testing.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
