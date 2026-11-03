[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-4](QA-4.md)

# QA-5 · Gerbang rilis MVP

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-qa5-gerbang-rilis` |
| Mulai setelah | [QA-4](QA-4.md) selesai, menerima H20 |
| Lanjut ke | selesai |

## Tujuan

Memberi tanda bahwa satu pasangan commit API dan web layak dirilis sebagai MVP.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 16 | Memeriksa 7 kriteria rilis MVP. | Setiap kriteria punya bukti tertulis | 🔒 [doc 16, kriteria rilis MVP](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#kriteria-rilis-mvp) |
| Doc 16 uji peramban | Skenario peramban di Chromium pada 360 px dan 1440 px, lalu sekali di Firefox dan WebKit. | Semua lulus; hasil ringkas tercatat di doc 10 | 🔒 [doc 16, uji peramban](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#uji-peramban) |
| Doc 17 | Memasang label `siap-rilis` pada pasangan commit. | Label terpasang di issue repo gabungan | 🔒 [doc 17, label issue](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#label-issue) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H20 · WEB-5 → QA-5 · E2E MVP hijau** | Alvira (FE), [WEB-5](WEB-5.md) | E2E P1 dan "P2 bagian template" lulus di 360 px dan 1440 px | Kriteria rilis butir 5 |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H22 · QA-5 → PM-4 · label siap-rilis** | Raja (PM), [PM-4](PM-4.md) | Label `siap-rilis` pada pasangan commit API dan web | Issue di repo gabungan DraftAja |

## Selesai bila

- [ ] H22 terkirim ke Raja

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
