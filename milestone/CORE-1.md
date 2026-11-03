[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Berikutnya: [CORE-2](CORE-2.md)

# CORE-1 · Keamanan fase 0

| | |
|---|---|
| Pemilik | Octaf · BE 1 · `be-core` |
| Fase | Fase 0 · Keamanan dan stabilisasi pasca-split |
| Repo | Backend-Laravel |
| Contoh branch | `fix/be-core-core1-policy-ekspor` |
| Mulai setelah | Repo dipisah, 3 November 2026 |
| Lanjut ke | [CORE-2](CORE-2.md) |

## Tujuan

Menutup celah keamanan dan kebenaran perilaku yang menjadi dasar semua fitur berikutnya, sebelum fitur lain dibangun di atasnya.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| BE-01 | Tambah `LaporanPolicy::export`: pemilik boleh mengekspor semua status. PDF draf diberi tanda "Draf". Hapus atau ganti `canExportPdf()`. | Tes fitur: ekspor PDF dan DOCX dokumen `final` milik sendiri 200; milik orang lain 403 | 🔒 [S-01](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-02 | Validasi `templates.use` dengan FormRequest yang sama dengan `templates.document`. Judul bawaan = nama template tanpa awalan, tanpa `report_type` akademik. | Tes: `judul` 300 karakter ditolak 422; tanpa body menghasilkan dokumen berjudul nama template | 🔒 [S-02](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#kebenaran-perilaku) |
| BE-09 | Gambar editor dan logo pindah ke disk privat, disajikan lewat rute yang memeriksa kepemilikan dokumen. URL tetap relatif. | Tes: tamu 401, pengguna lain 403, berkas bukan gambar dan SVG ditolak 422, ekspor PDF tetap memuat gambar | 🔒 [S-13](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#keamanan-dan-data) |
| BE-17 | Bersihkan `content` bagian dengan HTMLPurifier (konfigurasi sama dengan `CleanupOutputRestorer`) dan kirim `Content-Security-Policy: script-src 'none'` pada pratinjau. | Tes: `<script>` dan atribut `onerror` hilang saat disimpan; pratinjau memuat header CSP; format TipTap tetap utuh | 🔒 [S-21](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#keamanan-dan-data) |
| R6 | `UserSeeder` hanya berjalan di lokal dan tes. | Tes: seeder tidak membuat akun uji saat `APP_ENV=production` | 🔒 [doc 16, tes fase 0](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#tes-yang-wajib-ditambah-per-fase) |

## Serah terima masuk

Tidak ada. Milestone ini bisa dikerjakan tanpa menunggu anggota lain.

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H4 · CORE-1 → QA-2 · fase 0 API siap uji** | Louis (QA), [QA-2](QA-2.md) | PR BE-01, BE-02, BE-09, BE-17 yang sudah merge, beserta cara memicu setiap error | Daftar periksa serah terima QA di PR |

## Selesai bila

- [ ] Semua tes di kolom kriteria ada dan lulus
- [ ] S-01, S-02, S-13, S-21 ditandai selesai di doc 18
- [ ] H4 terkirim ke Louis

## Dokumen rujukan lain

- 🔒 [Backlog backend](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-backend)
- 🔒 [Roadmap fase 0](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-0-keamanan-dan-stabilisasi-pasca-split)
- 🔒 [Doc 07 risiko](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/07-authentication-security.md#risiko)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
