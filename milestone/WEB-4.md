[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [WEB-3](WEB-3.md) · Berikutnya: [WEB-5](WEB-5.md)

# WEB-4 · Katalog 8 kategori dan kolom isian

| | |
|---|---|
| Pemilik | Alvira · FE · `fe` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Frontend-Nextjs |
| Contoh branch | `feat/fe-web4-katalog-template` |
| Mulai setelah | [WEB-3](WEB-3.md) selesai, menerima H17 |
| Lanjut ke | [WEB-5](WEB-5.md) |

## Tujuan

Pengguna menelusuri 64 template per kategori, melihat strukturnya sebelum dipakai, dan mengisi kolom isian dokumen.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| FE-17 | Katalog 8 kategori, pencarian nama, detail template, dan pratinjau struktur, memakai `GET /categories` dari H17. | Filter memakai 8 kunci tetap; pratinjau tidak membuat dokumen | 🔒 [doc 12, template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#template-tpl) |
| KF-DOK-05 | Penanda kolom isian wajib yang kosong sebelum ekspor. | Ekspor tetap diizinkan, pengguna melihat daftar kolom yang belum diisi | 🔒 [doc 12, dokumen dan editor](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#dokumen-dan-editor-dok) |
| FE-04 lanjutan | Form buat dokumen memakai 8 kategori, bukan `report_types`. | Tidak ada istilah akademik yang tampil | 🔒 [web doc 06, kebenaran tampilan](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#kebenaran-tampilan) |
| FE-21 | Ringkasan `stats` di beranda: draf, final, template siap (bila kapasitas ada). | Angka sesuai data dari API | 🔒 [web doc 06, alur yang menunggu API](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/06-backlog-frontend.md#alur-yang-menunggu-api) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H17 · CORE-4 → WEB-4 · kategori dan kolom isian** | Octaf (BE 1), [CORE-4](CORE-4.md) | `GET /categories`, `template_fields`, `laporans.field_values` di `openapi.json` | FE-17 katalog dan kolom isian |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Katalog dan kolom isian diuji di 360 px dan desktop
- [ ] Peta halaman web doc 03 diperbarui

## Dokumen rujukan lain

- 🔒 [Roadmap fase 2](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-2-katalog-64-template)
- 🔒 [Web doc 05 sistem desain](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/05-sistem-desain.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
