[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-3](QA-3.md) · Berikutnya: [QA-5](QA-5.md)

# QA-4 · Uji katalog dan regresi prompt

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase 2 · Katalog 64 template |
| Repo | uji terhadap pasangan commit `develop` |
| Contoh branch | `test/qa-qa4-katalog-dan-regresi` |
| Mulai setelah | [QA-3](QA-3.md) selesai, menerima H18, H19 |
| Lanjut ke | [QA-5](QA-5.md), dan [QA-6](QA-6.md) bila Fase U dikerjakan |

## Tujuan

Memastikan 64 template sesuai spesifikasi dan keluaran AI tidak menambah fakta, lalu menyiapkan uji pengguna.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| KF-TPL-01 | Memeriksa 64 template di aplikasi terhadap berkas spesifikasi. | Struktur dan kolom isian sama dengan spesifikasi | 🔒 [Katalog template](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/README.md) |
| KF-AI-02 | Pemeriksaan manual keluaran regresi dari H19: apakah gaya mengikuti pola dan tidak ada fakta tambahan. | Nol fakta tambahan disetujui | 🔒 [doc 16, regresi prompt AI](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#regresi-prompt-ai) |
| Doc 16 uji pengguna | Menyiapkan sesi uji pengguna setelah fase 2 bersama sekretaris atau staf TU. | Jadwal, tugas, dan ukuran sesi ditetapkan bersama Raja | 🔒 [doc 16, uji pengguna](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#uji-pengguna) |

## Rincian pekerjaan

### Pemeriksaan 64 template

Tes otomatis di CORE-4 sudah membandingkan nama bagian di seeder dengan spesifikasi. Pemeriksaan QA melihat hal yang tidak dicakup tes itu, yaitu hasil di aplikasi.

Untuk setiap kategori, periksa semua template, atau minimal 3 template acak per kategori bila waktu terbatas:

| Yang diperiksa | Dibandingkan dengan |
|---|---|
| Template muncul di tab kategori yang benar | Kunci kategori di nama folder spesifikasi |
| Kode dan nama | Judul berkas spesifikasi |
| Daftar bagian di detail template | Bagian "Struktur" |
| Kolom isian, termasuk tanda wajib | Bagian "Kolom isian" |
| Dokumen baru dari template berisi bagian yang sama | Bagian "Struktur" |
| Ekspor DOCX dan PDF: sampul dan daftar isi | Tabel identitas spesifikasi dan tata letak ekspor di katalog template |

Catat hasil di lembar periksa per kategori. Ketidaksesuaian dilaporkan sebagai bug ke `be-core`.

### Pemeriksaan manual regresi prompt

Untuk setiap keluaran dari [H19](../serah-terima/H19.md), yaitu minimal 10 contoh dikali 2 provider:

1. Baca catatan masukan, lalu keluaran.
2. Tandai setiap nama, angka, tanggal, nominal, atau nomor di keluaran yang tidak ada di masukan. Pemeriksaan otomatis menangkap sebagian besar, tetapi tidak semua, misalnya nama jabatan atau tempat yang ditulis tanpa huruf kapital.
3. Tandai perubahan makna: keputusan yang menjadi usulan, siapa yang menjadi berbeda, tenggat yang hilang.
4. Untuk contoh yang sengaja tidak lengkap, periksa `[perlu dilengkapi]` ada di tempat informasi yang hilang.
5. Nilai apakah gaya mengikuti pola template: bahasa formal, sudut pandang, pola kalimat pembuka bagian.

Ambang lulus: nol fakta tambahan dan nol perubahan makna. Satu temuan berarti AI-5 diulang setelah prompt diperbaiki.

### Persiapan uji pengguna

Bersama Raja, tetapkan:

| Hal | Isi dari doc 16 |
|---|---|
| Peserta | Sekretaris atau staf TU yang membuat notula dan surat setiap minggu |
| Tugas | Membuat notula dari catatan rapat milik peserta sendiri yang sudah disamarkan |
| Yang diukur | Waktu sampai unduh, jumlah suntingan setelah perapihan, fakta yang salah |
| Data | Peserta tidak mengunggah dokumen asli yang memuat data pribadi; contoh asli disamarkan dulu |
| Jumlah peserta dan jadwal | Diputuskan tim bersama rencana validasi harga di doc 14 |

Siapkan lembar tugas, akun uji untuk peserta, dan formulir pencatatan.

## Serah terima masuk

Label sama persis dengan panah di diagram profil.

| Label di diagram | Dari | Yang diterima | Dipakai untuk |
|---|---|---|---|
| [**H18 · CORE-4 → QA-4 · 64 template di seeder**](../serah-terima/H18.md) | Octaf (BE 1), [CORE-4](CORE-4.md) | 64 template di `TemplateSeeder` sesuai `docs/templates/` | Memeriksa kesesuaian spesifikasi dan seeder |
| [**H19 · AI-5 → QA-4 · hasil regresi prompt**](../serah-terima/H19.md) | Hamid (BE 2), [AI-5](AI-5.md) | Keluaran minimal 10 pasang catatan untuk Groq dan Gemini | Pemeriksaan manual fakta tambahan dan penanda `[perlu dilengkapi]` |

## Serah terima keluar

Tidak ada serah terima keluar. Hasilnya dipakai langsung di milestone berikutnya milik pemilik yang sama.

## Selesai bila

- [ ] Laporan QA dibuat dengan template 🔒 [web `docs/qa`](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/qa/README.md), dicetak ke PDF dengan `npm run qa:report`, dan PDF-nya dilampirkan di issue serah terima yang diuji
- [ ] Lembar periksa 8 kategori terisi
- [ ] Pemeriksaan manual regresi tercatat dengan nol temuan
- [ ] Rencana uji pengguna disetujui Raja

## Dokumen rujukan lain

- 🔒 [Doc 11, persona](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/11-persona-dan-perjalanan-pengguna.md)
- 🔒 [Doc 14, validasi](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/14-model-bisnis.md#validasi)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
