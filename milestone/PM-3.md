[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [PM-2](PM-2.md) · Berikutnya: [PM-4](PM-4.md)

# PM-3 · Lingkungan uji nyata

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | Backend-Laravel (`infra/`, `deploy.yml`), Frontend-Nextjs (`deploy.yml`), Azure |
| Contoh branch | `chore/pm-pm3-infra-staging` |
| Mulai setelah | [PM-2](PM-2.md) selesai |
| Lanjut ke | [PM-4](PM-4.md), dan [PM-6](PM-6.md) bila Fase U dikerjakan |

## Tujuan

Menyediakan lingkungan yang tidak bisa dibuat sendiri oleh BE: PostgreSQL 16 untuk tes, kunci provider AI, dan staging dengan urutan deploy yang benar. Ketiganya menyangkut biaya, akses akun Azure, dan rahasia, sehingga menjadi tanggung jawab PM.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Doc 10 langkah 1 | Instance PostgreSQL 16 sekali pakai untuk tes, misalnya Docker Compose `postgres:16-alpine` dengan nama database `draftaja_test_*`. Bukan database Azure bersama. | Octaf punya akses dan izin menjalankan tes | 🔒 [doc 10, langkah verifikasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#langkah-verifikasi-yang-harus-dijadwalkan) |
| Doc 10 langkah 2 | Kunci Groq dan Gemini dengan izin pemakaian terbatas, plus nama `GEMINI_MODEL` yang aktif. | Hamid menerima kunci lewat kanal aman; tidak ada kunci di repo atau chat grup terbuka | 🔒 [doc 08 integrasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/08-integrations.md) |
| Doc 17 deploy | Staging dengan urutan deploy: API lebih dulu, migration sebelum container baru menerima lalu lintas, lalu web. | Urutan deploy dicoba sekali di staging | 🔒 [doc 17, urutan merge dan deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#urutan-merge-dan-deploy) |
| INF-03 | Bicep untuk staging di `infra/` Backend-Laravel: VNet, Container Apps environment, app dan job, PostgreSQL atau database di server dev, Key Vault, Storage, ACR, managed identity, Log Analytics. Dikerjakan bersama Octaf. | `az bicep build` bersih; deploy ke resource group kosong berhasil | 🔒 [doc 19, pekerjaan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#pekerjaan-yang-dibutuhkan) |
| INF-07 | Workflow `deploy.yml` di kedua repo dengan OIDC dan urutan migrate, API, scheduler, web. | Push ke `develop` men-deploy staging tanpa langkah manual | 🔒 [doc 19, alur deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#alur-deploy) |
| INF-12 | Domain kustom dan managed certificate untuk web staging. | `https://<domain>` melayani web staging | 🔒 [doc 19, layanan](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#layanan-yang-dipilih) |

## Rincian pekerjaan

### PostgreSQL 16 untuk uji

PostgreSQL di Docker Compose (`postgres:16-alpine`, database `draftaja_test_local`) sudah ada dan bisa dijalankan sendiri oleh Octaf. Yang dibutuhkan dari PM adalah salinan yang menyerupai produksi:

1. Buat instance Azure Database for PostgreSQL Flexible Server 16 sekali pakai dengan tingkat terkecil, atau minta izin memakai server bersama dengan database terpisah. Nama database tidak boleh `draftaja_shared_dev`, `draftaja_staging`, atau `draftaja_prod`.
2. Pulihkan backup terbaru `draftaja_shared_dev` ke instance itu.
3. Buka firewall untuk IP Octaf selama periode uji.
4. Serahkan lewat [H15](../serah-terima/H15.md), lalu hapus instance setelah CORE-5 selesai.

### Kunci provider AI

1. Buat kunci Groq dan Gemini atas nama akun proyek.
2. Pasang batas anggaran di dasbor provider bila tersedia.
3. Tetapkan nama `GEMINI_MODEL` yang aktif saat itu. Tidak ada nilai bawaan di kode.
4. Tulis izin pemakaian: jumlah panggilan atau anggaran maksimal, hanya untuk AI-4 dan AI-5, hanya contoh dari spesifikasi template.
5. Serahkan lewat [H16](../serah-terima/H16.md). Cabut kunci setelah pameran.

### Staging dan urutan deploy

Rancangan lengkap staging, termasuk layanan, jaringan, role database, rahasia, dan alur deploy, ada di 🔒 doc 19. Keputusan hosting dan region menunggu persetujuan Raja di ADR 0006. Ringkasnya: Container Apps di Indonesia Central dengan `ca-web` eksternal, `ca-api` internal, `ca-worker`, serta job scheduler dan migrate; PostgreSQL Flexible Server Burstable; Azure Files untuk `storage/app`; Key Vault dengan managed identity.

INF-03 menulis semua itu sebagai Bicep, INF-07 menambahkan workflow deploy, dan INF-12 memasang domain. Octaf mengerjakan bagian aplikasinya di CORE-5 (INF-04 sampai INF-06).

Staging dipakai untuk mencoba urutan deploy sebelum rilis MVP. Variabel yang mengikat kedua repo ada di `docs/ENV_CONTRACT.md` API, dan salah satu saja keliru membuat login gagal.

| Variabel API | Variabel web | Nilai staging |
|---|---|---|
| `FRONTEND_URL` | `APP_URL` | `https://staging.<domain>` |
| `APP_URL` | `API_INTERNAL_URL` | alamat internal container API |
| `SANCTUM_STATEFUL_DOMAINS` | | host `FRONTEND_URL` |
| `SESSION_COOKIE` | `SESSION_COOKIE_NAME` | `draftaja_session` |

Urutan yang dicoba sekali:

1. Deploy container API dengan peran `web`, `worker`, dan `scheduler` dari satu image. Entrypoint menjalankan `app:runtime-check` dan `env:verify`, dan container tidak start bila gagal.
2. Jalankan migration sebelum container API baru menerima lalu lintas.
3. Deploy container web.
4. Uji masuk, buat dokumen, dan unduh DOCX di staging.
5. Coba rollback web saja, lalu pastikan API baru tetap melayani web lama.

Peran `web` dan `worker` API harus berbagi direktori `storage/app` yang sama, karena worker membaca DOCX yang diunggah lewat peran `web`.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H15 · PM-3 → CORE-5 · PostgreSQL 16 sekali pakai**](../serah-terima/H15.md) | Octaf (BE 1), [CORE-5](CORE-5.md) | Instance PostgreSQL 16 sekali pakai (bukan database Azure bersama) dan izin pemakaiannya | Kanal aman, bukan repo |
| [**H16 · PM-3 → AI-4 · kunci Groq dan Gemini**](../serah-terima/H16.md) | Hamid (BE 2), [AI-4](AI-4.md) | Kunci Groq dan Gemini, izin pemakaian terbatas, nama `GEMINI_MODEL` yang aktif | Kanal aman, bukan chat grup terbuka |

## Selesai bila

- [ ] INF-03, INF-07, dan INF-12 selesai; staging bisa di-deploy ulang dari nol dengan Bicep
- [ ] H15 dan H16 terkirim lewat kanal aman
- [ ] Staging bisa menjalankan pasangan commit `develop`
- [ ] Urutan deploy dan rollback web dicoba sekali

## Dokumen rujukan lain

- 🔒 [Kontrak environment API](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/ENV_CONTRACT.md)
- 🔒 [Doc 02, deploy](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/02-system-architecture.md#deploy)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
