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

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-AI-05 | Prompt chat berpersona asisten DraftAja untuk pekerjaan administrasi, mengikuti keputusan H10. Lokasi: `app/Services/Ai/Prompts/ChatPrompt.php`. | Tes: prompt sistem tidak memuat "LaporBot" atau "Lapor.in" | 🔒 [doc 12, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#ai) |
| BE-08 | `GET /ai/quota` mengembalikan `{limit, used, remaining, resets_at}`. | Tes: setelah satu perapihan 3 potongan, `used` bertambah 3 | 🔒 [KF-AI-04](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#backlog-backend) |
| BE-06 | Field baru `proposal` berbentuk `{blocks: [{id, text}]}`; `output` untuk perapihan ditandai deprecated. | Web memakai `proposal`; `output` untuk perapihan boleh dihapus satu sprint kemudian | 🔒 [S-08](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#bentuk-kontrak) |

## Rincian pekerjaan

### KF-AI-05 · Persona asisten

Masalah sekarang: prompt sistem di `ChatPrompt.php` dibuka dengan "Kamu adalah 'LaporBot', asisten AI untuk mahasiswa IT di aplikasi 'Lapor.in'" dan menawarkan bantuan menyusun laporan, skripsi, dan makalah.

Langkah:

1. Tulis ulang prompt sistem dari keputusan [H10](../serah-terima/H10.md): nama, pengguna sasaran, yang boleh dibantu, yang tidak boleh, gaya bahasa, dan kalimat penolakan.
2. Masukkan aturan dasar AI DraftAja ke prompt: tidak membuat nama, angka, tanggal, nominal, atau nomor dokumen; menyarankan `[perlu dilengkapi]` untuk informasi yang tidak ada; tidak menulis dokumen dari nol tanpa catatan.
3. Bila `laporan_id` dikirim, konteks yang disertakan hanya judul, kategori, dan nama bagian, bukan seluruh isi dokumen (minimalisasi data, doc 07).

Bukti selesai: tes unit yang memeriksa prompt sistem tidak memuat istilah terlarang dari H10 (LaporBot, Lapor.in, mahasiswa, skripsi, makalah, NIM, prodi, dosen pembimbing). Pemeriksaan jawaban dengan provider sungguhan masuk AI-4.

### BE-08 · Sisa kuota

Masalah sekarang: kuota dihitung `AiGenerationService` dari jumlah `quota_units` milik pengguna sejak awal hari, dibandingkan dengan `AI_DAILY_QUOTA` (bawaan 100). Satu perapihan memakai satu unit per potongan (20 blok atau 3.000 karakter per potongan), jadi dokumen panjang bisa memakai beberapa unit sekaligus. Pengguna baru tahu kuota habis setelah ditolak 429.

Langkah:

1. Proposal kontrak untuk `GET /api/v1/ai/quota`.
2. Pindahkan perhitungan `used` ke satu method di `AiGenerationService` yang dipakai bersama oleh reservasi dan endpoint baru, supaya angkanya tidak pernah berbeda.
3. Respons: `limit` (0 berarti tanpa batas), `used`, `remaining` (null bila tanpa batas), dan `resets_at` (awal hari berikutnya waktu aplikasi, yaitu UTC, sama dengan 07.00 WIB).
4. Rancang agar `limit` nanti bisa dibaca dari `PlanLimits` per paket (doc 14) tanpa mengubah bentuk respons.
5. Opsional untuk panel perapihan: perkiraan unit di respons atau endpoint terpisah, dihitung dari jumlah potongan dokumen saat ini.
6. `composer api:spec`.

Bukti selesai: tes `used` bertambah 3 setelah perapihan 3 potongan; tes permintaan ulang dengan `request_key` sama tidak menambah `used`; tes `limit: 0` saat `AI_DAILY_QUOTA=0`.

### BE-06 · Usulan sebagai objek

Masalah sekarang: untuk perapihan, `output` di `GET /ai/generations/{id}` adalah string JSON di dalam JSON, berbentuk `"{\"blocks\":[...]}"`. Web harus `JSON.parse` dan menebak bentuknya, dan bentuk itu tidak ada di kontrak.

Langkah:

1. Tambah field `proposal` (objek `{blocks: [{id, text}]}` atau null) hanya untuk `operation = document_cleanup`. Null sampai `succeeded` dan saat usulan kedaluwarsa, sama dengan aturan `output` sekarang.
2. `output` untuk perapihan tetap dikirim dan ditandai deprecated di `openapi.json`.
3. Catat tanggal deprecated di rencana penggantian field doc 18. Hapus `output` untuk perapihan paling cepat satu sprint setelah web memakai `proposal`.

Bukti selesai: tes bahwa `proposal.blocks` berisi `id` dan teks yang sama dengan isi `output` lama.

## Urutan kerja yang disarankan

1. BE-06 dan BE-08, yang tidak menunggu keputusan produk.
2. KF-AI-05 setelah [H10](../serah-terima/H10.md).

Kirim [H13](../serah-terima/H13.md) setelah BE-06 dan BE-08 merge.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H10 · PM-2 → AI-3 · persona asisten**](../serah-terima/H10.md) | Raja (PM), [PM-2](PM-2.md) | Persona dan batas asisten DraftAja | Mengganti prompt "LaporBot" (KF-AI-05) |

## Serah terima keluar

| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H13 · AI-3 → WEB-3 · kuota dan proposal**](../serah-terima/H13.md) | Alvira (FE), [WEB-3](WEB-3.md) | `GET /ai/quota` dan field `proposal` berbentuk objek | PR API merge, label `butuh-fe` di issue web |

## Selesai bila

- [ ] Semua tes di kolom kriteria lulus
- [ ] `composer api:spec` dijalankan
- [ ] KF-AI-05 berlabel Sudah ada di doc 01, 10, dan 12
- [ ] H13 terkirim ke Alvira

## Dokumen rujukan lain

- 🔒 [Doc 14, paket](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/14-model-bisnis.md#paket)
- 🔒 [Doc 05, AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#ai)
- 🔒 [Rencana penggantian field](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/18-audit-pasca-split.md#rencana-penggantian-field)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
