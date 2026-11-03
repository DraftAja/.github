[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-4](WEB-4.md)

# WEB-5 · E2E MVP dan aksesibilitas

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Frontend-Nextjs |
| Contoh branch | `test/fe-web5-e2e-mvp` |
| Mulai setelah | [WEB-4](WEB-4.md) selesai, menerima H8 |
| Lanjut ke | selesai |

## Tujuan

Alur MVP dibuktikan otomatis di peramban, desktop dan ponsel, termasuk pemeriksaan aksesibilitas dasar.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-22 | E2E: unggah template sampai `ready`, lalu buat dokumen darinya. | Lulus di profil desktop dan ponsel | 🔒 [web doc 07, yang wajib ditambah](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#yang-wajib-ditambah) |
| FE-23 | E2E: perapihan sampai terima usulan, memakai provider palsu dari H8. | Lulus tanpa memanggil Groq atau Gemini sungguhan | 🔒 [web doc 07, aturan E2E](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#aturan-menulis-tes-e2e) |
| FE-24 | E2E: unduh PDF dari antarmuka. | Lulus di profil desktop dan ponsel | 🔒 [web doc 07, yang wajib ditambah](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/07-pengujian.md#yang-wajib-ditambah) |
| FE-26 | Pemeriksaan aksesibilitas otomatis di 5 halaman utama (KN-02). Dependensi baru seperti `@axe-core/playwright` butuh persetujuan tim. | Tidak ada pelanggaran kontras, label, atau fokus | 🔒 [doc 12, nonfungsional](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#kebutuhan-nonfungsional) |
| FE-25 | E2E: hapus akun (bila kapasitas ada). | Lulus di kedua profil | 🔒 [web doc 06, pengujian](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#pengujian) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H8 · AI-2 → WEB-5 · stub provider untuk E2E** | Hamid (BE 2), [AI-2](AI-2.md) | Cara mengarahkan `AI_*` ke provider palsu lokal, tertulis di doc 09 API. Juga dipakai Louis. | E2E perapihan tanpa kunci sungguhan (FE-23) |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H20 · WEB-5 → QA-5 · E2E MVP hijau** | Louis (QA), [QA-5](QA-5.md) | E2E P1 dan "P2 bagian template" lulus di 360 px dan 1440 px | CI web hijau di `develop` |

## Selesai bila

- [ ] Skenario P1 dan "P2 bagian template" lulus di 360 px dan 1440 px
- [ ] H20 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Doc 16 uji peramban](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#uji-peramban)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
