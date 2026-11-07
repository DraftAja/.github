[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [PM-2](PM-2.md)

# PM-1 · Fondasi kolaborasi dua repo

| | |
|---|---|
| Pemilik | Raja · PM · `pm` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel, Frontend-Nextjs, DraftAja, .github |
| Contoh branch | `docs/pm-pm1-kontrak-tim-v1.1` |
| Saran urutan | Repo dipisah, 3 November 2026. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [PM-2](PM-2.md) |

## Tujuan

Aturan main untuk dua repo berlaku sebelum kontrak API mulai berubah. Setelah milestone ini, setiap PR otomatis meminta review pemilik kode dan setiap anggota bisa diberi tugas di GitHub.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-15 | Revisi `TEAM_CONTRACT.md` v1.1: struktur kepemilikan per repo, aturan PR lintas repo, Definition of Done tambahan, sesuai usulan di doc 17. Pemetaan BE 1 = `be-core` (Octaf) dan BE 2 = `be-ai` (Hamid) sudah diselaraskan di v1.0.1. | Disetujui minimal 4 dari 5 anggota (kontrak bagian 14) | 🔒 [doc 17, usulan revisi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#usulan-revisi-team_contractmd-v11) |
| BE-15 | Membuat `.github/CODEOWNERS` di Backend-Laravel dan Frontend-Nextjs. Di API, `docs/api/openapi.json` dimiliki bersama `be-core`, `be-ai`, dan `fe`. | PR yang mengubah `openapi.json` otomatis meminta review FE | 🔒 [Lampiran A](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#lampiran-a-contoh-githubcodeowners) |
| Lingkungan | GitHub Environment `development` (branch `develop`) dan `production` (branch `main`, persetujuan Raja) di kedua repo, rahasia dipisah per Environment. Workflow `Deploy` dan `Aturan branch` sudah ada di kedua repo. | Rahasia production tidak bisa dibaca job dari `develop`; PR ke `main` dari branch selain `develop` atau `hotfix/*` gagal | 🔒 [kontrak bagian 4](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#4-strategi-branch-dan-lingkungan) |
| Lampiran B | Branch protection `main` dan `develop` di kedua repo, termasuk status check `api:check` di web. Butuh paket GitHub Team atau repo publik; selama organisasi memakai paket Free, aturan dijaga workflow `Aturan branch` dan review. | Push langsung ke `develop` ditolak; approval 1 untuk `develop` dan 2 untuk `main`; review pemilik kode wajib | 🔒 [Lampiran B](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#lampiran-b-pengaturan-branch-protection-github) |
| Label | Membuat label dari kontrak bagian 12 dan doc 17 di ketiga repo: `be-core`, `be-ai`, `fe`, `qa`, `docs`, `bug`, `feature`, `priority-high`, `kontrak`, `kontrak-disetujui`, `memecah-kontrak`, `butuh-api`, `butuh-fe`, `siap-rilis`, serta label laporan `laporan`, `perlu-review`, `perlu-perbaikan`, `disetujui`. | Semua label tersedia dan dipakai template issue | 🔒 [doc 17, label issue](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md#label-issue) |
| Organisasi | Mengundang Alvira dan Louis ke organisasi DraftAja, lalu membuat GitHub Milestone atau Projects untuk 25 milestone ini dengan tanggal jatuh tempo. | Kelima anggota bisa di-assign ke issue dan menjadi pemilik kode | 🔒 [kontrak bagian 12](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md#12-issue-dan-pelacakan-pekerjaan) |
| INF-01 | Audit server PostgreSQL dev bersama dengan perintah baca-saja di doc 19, lalu perbaiki firewall, TLS, lock, dan akun admin. Dikerjakan bersama Octaf. | Tabel audit di doc 19 terisi; tidak ada aturan firewall terbuka; lock `CanNotDelete` terpasang | 🔒 [doc 19, audit](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#audit-kondisi-sekarang) |
| INF-02 | Budget di subscription Azure dengan peringatan 50, 80, dan 100 persen ke Raja dan Octaf. | Budget aktif | 🔒 [doc 19, biaya](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/19-infrastruktur-azure.md#biaya) |

## Rincian pekerjaan

### Kontrak tim v1.1

Masalah sekarang: `TEAM_CONTRACT.md` v1.0.1 masih menulis "seluruh kode berada dalam satu monorepo". Bagian 3 menggambarkan struktur Laravel + Inertia (`resources/js`, `vite.config.js`) yang sudah tidak ada di repo API. Lampiran A berisi CODEOWNERS untuk folder yang sudah pindah ke repo web atau tidak pernah ada, misalnya `PatternLearningController.php` dan `ExportController.php`.

Perubahan yang diusulkan, dari doc 17 API:

| Bagian | Perubahan |
|---|---|
| Pembuka | Dua repo kode ditambah repo gabungan dan repo `.github`, menggantikan "satu monorepo" |
| 3 | Dua tabel kepemilikan, satu per repo. Seluruh Frontend-Nextjs milik `fe`. Di Backend-Laravel, area `be-core` dan `be-ai` mengikuti pembagian di doc 05 API. |
| 4, 7 | Aturan PR lintas repo: proposal kontrak lebih dulu, PR API dengan `openapi.json` baru merge sebelum PR web, PR saling merujuk |
| 10 | `package.json` di repo API hanya untuk Puppeteer; paket JS antarmuka sepenuhnya milik `fe` di repo web |
| 12 | Label tambahan dari doc 17 |
| 13 | Definition of Done tambahan: `composer api:spec` di PR API, tipe dari `schema.d.ts` di PR web, tes untuk setiap `code` error baru |
| Lampiran A | CODEOWNERS per repo, lihat di bawah |
| Lampiran B | Branch protection yang sama di kedua repo, dengan nama status check yang sebenarnya |

Langkah: buka PR ke `develop` di Backend-Laravel, minta persetujuan kelima anggota di PR, lalu merge setelah minimal 4 setuju. Naikkan versi di kepala dokumen menjadi 1.1 dengan tanggal.

### CODEOWNERS

Isi minimal Backend-Laravel `.github/CODEOWNERS`. Username Hamid, Octaf, dan Raja tertulis di halaman profil. Username Alvira dan Louis diisi setelah keduanya masuk organisasi.

| Path | Pemilik |
|---|---|
| `*` | Raja |
| `/app/Services/Ai/`, `/app/Jobs/Ai/`, `/app/Http/Controllers/Ai*`, `/app/Http/Controllers/DocumentCleanupController.php`, `/app/Http/Requests/Ai/`, `/config/ai.php`, `/docs-ai/`, `/tests/Unit/Ai/` | Hamid |
| `/app/Http/Controllers/Auth/`, `LaporanController.php`, `TemplateController.php`, `ReportSectionController.php`, `SectionController.php`, `/app/Policies/`, `/app/Services/Documents/`, `/app/Services/Export/`, `/app/Http/Resources/` | Octaf |
| `/docs/api/openapi.json` | Octaf, Hamid, Alvira |
| `/routes/`, `/database/migrations/`, `/composer.json` | Raja, Octaf, Hamid |
| `/tests/Feature/`, `/tests/Fixtures/`, `/docs/qa/` | Louis |

Frontend-Nextjs: `*` milik Alvira; `/contracts/` dan `/src/lib/api/` juga milik Octaf dan Hamid; `/tests/e2e/` juga milik Louis.

Pemilik di CODEOWNERS harus punya akses tulis ke repo, kalau tidak GitHub mengabaikan barisnya.

### Branch protection

Di Settings, bagian Branches, untuk `main` dan `develop` kedua repo:

| Aturan | `develop` | `main` |
|---|---|---|
| Wajib PR | ya | ya |
| Jumlah approval | 1 | 2 |
| Review pemilik kode | wajib | wajib |
| Status check wajib, API | "Audit, gaya, analisis statis, kontrak", "Tes (SQLite)", "Tes (PostgreSQL 16)", "Build image" | sama |
| Status check wajib, web | "Audit, tipe, lint, tes, build", "E2E terhadap draftaja-api", "Build image" | sama |
| Branch harus terbaru sebelum merge | ya | ya |
| Force push dan hapus branch | dilarang | dilarang |

Job "Tes (PostgreSQL 16)" saat ini hijau karena `continue-on-error`, meskipun tesnya gagal. Status check itu baru bermakna setelah Octaf memperbaikinya di [CORE-5](CORE-5.md).

### Label

Buat 18 label di Backend-Laravel, Frontend-Nextjs, dan DraftAja. Template issue di repo `.github` sudah memakai `feature`, `kontrak`, dan `bug`, jadi label itu harus ada lebih dulu. Cara cepat dengan GitHub CLI, misalnya:

```bash
for repo in DraftAja/Backend-Laravel DraftAja/Frontend-Nextjs DraftAja/DraftAja; do
  gh label create kontrak-disetujui --repo "$repo" --color 0E8A16 --description "Proposal kontrak sudah ditinjau" --force
  gh label create laporan --repo "$repo" --color 1D76DB --description "Laporan selesai satu butir atau milestone" --force
  gh label create perlu-review --repo "$repo" --color FBCA04 --description "Menunggu tanggapan reviewer yang di-mention" --force
  gh label create perlu-perbaikan --repo "$repo" --color D93F0B --description "Reviewer tidak setuju, pengirim memperbaiki" --force
  gh label create disetujui --repo "$repo" --color 0E8A16 --description "Reviewer setuju" --force
done
```

### Organisasi dan pelacakan

1. Undang Alvira dan Louis ke organisasi dengan akses tulis ke kedua repo kode.
2. Buat GitHub Milestone di repo yang sesuai untuk 25 milestone, dengan tanggal jatuh tempo. Milestone lintas repo (PM-1, QA-x) dibuat di repo API.
3. Atau buat satu GitHub Project tingkat organisasi dengan kolom `Backlog`, `To Do`, `In Progress`, `In Review`, `Done` (kontrak bagian 12), dan field Milestone.
4. Buat satu issue per butir pekerjaan dengan template "Tugas milestone".
### INF-01 · Audit server database dev

Masalah sekarang: server Azure Database for PostgreSQL yang menampung `draftaja_shared_dev` sudah dipakai tim, tetapi konfigurasinya tidak tercatat di repo mana pun. Aturan firewall, TLS, retensi backup, lock, dan siapa yang memegang login admin belum diketahui. Menghapus server ikut menghapus semua backup-nya.

Langkah:

1. Jalankan perintah audit baca-saja di doc 19 bagian "Audit kondisi sekarang". Perintah itu tidak mengubah apa pun.
2. Isi tabel audit di doc 19 lewat PR dokumentasi, tanpa menyalin kata sandi atau IP pribadi.
3. Perbaiki bersama Octaf: hapus aturan firewall `0.0.0.0` sampai `255.255.255.255` dan aturan "izinkan semua layanan Azure" bila ada, pastikan `require_secure_transport` bernilai `on`, pasang lock `CanNotDelete`, dan pindahkan kata sandi admin ke kanal aman.
4. Buat role `draftaja_readonly` untuk anggota yang hanya perlu membaca data dev (doc 19 bagian role dan hak akses).

Bukti selesai: tabel audit terisi di doc 19 dan `az lock list` menampilkan lock pada server.

### INF-02 · Budget

Masalah sekarang: subscription memakai kredit Azure for Students sebesar USD 100 untuk 12 bulan. Bila kredit habis, subscription dinonaktifkan, dan sumber daya bisa dihapus bila tidak di-upgrade dalam 90 hari. Itu bisa terjadi tepat sebelum pameran.

Langkah: di Cost Management, buat budget sebesar sisa kredit dengan tiga ambang (50, 80, 100 persen) dan kirim ke email Raja dan Octaf. Periksa dengan `az consumption budget list`.

## Urutan kerja yang disarankan

1. Undang Alvira dan Louis, karena CODEOWNERS butuh akun mereka.
2. Label.
3. PR kontrak v1.1 dan CODEOWNERS.
4. Branch protection setelah CODEOWNERS merge.
5. Milestone dan issue.

Kirim [H1](../serah-terima/H1.md) setelah langkah 4.

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H1 · PM-1 → CORE-2 · kontrak tim v1.1, CODEOWNERS**](../serah-terima/H1.md) | Octaf (BE 1), [CORE-2](CORE-2.md) | `TEAM_CONTRACT.md` v1.1, CODEOWNERS per repo, branch protection, label. Berlaku untuk semua anggota. | PR ke `develop`, pengumuman di grup |

## Selesai bila

- [ ] Tabel audit INF-01 terisi dan budget INF-02 aktif
- [ ] Kontrak v1.1 merge ke `develop` dengan 4 persetujuan
- [ ] CODEOWNERS dan branch protection aktif di kedua repo
- [ ] Label tersedia di ketiga repo
- [ ] Alvira dan Louis menjadi anggota organisasi

## Dokumen rujukan lain

- 🔒 [`TEAM_CONTRACT.md`](https://github.com/DraftAja/Backend-Laravel/blob/develop/TEAM_CONTRACT.md)
- 🔒 [Doc 17, kolaborasi dan serah terima](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/17-kolaborasi-dan-serah-terima.md)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
