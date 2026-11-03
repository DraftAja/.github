[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [AI-2](AI-2.md) · Berikutnya: [AI-4](AI-4.md)

# AI-3 · Persona asisten, kuota, proposal

| | |
|---|---|
| Pemilik | Hamid · BE 2 · `be-ai` |
| Fase | Fase 1 · Penyelarasan persona |
| Repo | Backend-Laravel |
| Contoh branch | `feat/be-ai-ai3-persona-kuota` |
| Mulai setelah | [AI-2](AI-2.md) selesai, menerima H10 |
| Lanjut ke | [AI-4](AI-4.md) |

## Tujuan

Asisten berbicara sebagai DraftAja, pengguna bisa melihat sisa kuota, dan usulan perapihan dikirim sebagai objek.

## Yang dikerjakan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-AI-05 | Prompt chat berpersona asisten DraftAja untuk pekerjaan administrasi, mengikuti keputusan H10. Lokasi: `app/Services/Ai/Prompts/ChatPrompt.php`. | Tes: prompt sistem tidak memuat "LaporBot" atau "Lapor.in" | 🔒 [doc 12, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ai) |
| BE-08 | `GET /ai/quota` mengembalikan `{limit, used, remaining, resets_at}`. | Tes: setelah satu perapihan 3 potongan, `used` bertambah 3 | 🔒 [KF-AI-04](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-backend) |
| BE-06 | Field baru `proposal` berbentuk `{blocks: [{id, text}]}`; `output` untuk perapihan ditandai deprecated. | Web memakai `proposal`; `output` untuk perapihan boleh dihapus satu sprint kemudian | 🔒 [S-08](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| **H10 · PM-2 → AI-3 · persona asisten** | Raja (PM), [PM-2](PM-2.md) | Persona dan batas asisten DraftAja | Mengganti prompt "LaporBot" (KF-AI-05) |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| **H13 · AI-3 → WEB-3 · kuota dan proposal** | Alvira (FE), [WEB-3](WEB-3.md) | `GET /ai/quota` dan field `proposal` berbentuk objek | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] Proposal kontrak untuk BE-06 dan BE-08 disetujui sebelum kode
- [ ] `openapi.json` ikut di-commit
- [ ] H13 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Roadmap fase 1](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/10-project-status-and-gaps.md#fase-1-penyelarasan-persona)
- 🔒 [Rencana penggantian field](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#rencana-penggantian-field)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
