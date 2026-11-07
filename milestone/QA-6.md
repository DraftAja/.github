[Profil organisasi](https://github.com/DraftAja) · [Semua milestone](README.md) · Sebelumnya: [QA-5](QA-5.md) · Berikutnya: [QA-7](QA-7.md)

# QA-6 · Rencana uji dan lembar hitung Fase U

| | |
|---|---|
| Pemilik | Louis · QA · `qa` |
| Fase | Fase U · Dashboard usaha (Lanjutan, bila kapasitas) |
| Repo | Backend-Laravel dan Frontend-Nextjs (dokumen uji) |
| Contoh branch | `docs/qa-qa6-lembar-hitung` |
| Saran urutan | [QA-4](QA-4.md) selesai. Dikerjakan berdampingan dengan QA-5, karena CORE-6 dan AI-6 butuh hasilnya sebelum menulis tes. Tidak wajib: boleh dimulai kapan saja, serah terima yang belum dilaporkan diganti data contoh atau tipe sementara. |
| Lanjut ke | [QA-7](QA-7.md) |

## Tujuan

Sebelum kode Fase U ditulis, setiap angka di doc 21 sudah dihitung ulang oleh orang selain penulisnya, dan setiap risiko berkas impor dan ekspor sudah punya kasus uji.

## Ringkasan pekerjaan

| Butir | Pekerjaan | Kriteria terima | Sumber |
|---|---|---|---|
| Lembar hitung | Hitung ulang kasus U01 sampai U17 secara manual di lembar terpisah | Setiap hasil sama dengan doc 21, atau selisihnya dilaporkan dan doc 21 diperbaiki | 🔒 [doc 21, kasus uji](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/21-perhitungan-usaha.md#kasus-uji) |
| Kasus berkas | Daftar berkas uji impor dan ekspor beserta hasil yang diharapkan | Mencakup semua baris impor dan ekspor di doc 16 Fase U | 🔒 [doc 16, Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/16-strategi-pengujian.md#fase-u-dashboard-usaha) |
| Isolasi | Daftar endpoint usaha yang wajib punya tes 404 lintas usaha | Semua path di doc 05 Endpoint Fase U tercakup | 🔒 [doc 05, endpoint Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/05-api-documentation.md#endpoint-fase-u-dashboard-usaha) |
| Uji pengguna | Rencana uji 5 pemilik UMKM untuk KN-10 | Tugas, ukuran, dan cara menyamarkan data tertulis | 🔒 [KN-10](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/12-kebutuhan-sistem.md#kebutuhan-nonfungsional) |

## Rincian pekerjaan

### Lembar hitung · U01 sampai U17

Masalah sekarang: rumus dan kasus uji ditulis oleh orang yang sama. Kesalahan aritmetika di dokumen akan terbawa ke tes, sehingga tes lulus tetapi angkanya salah (NFR-03).

Langkah:

1. Buat lembar hitung di spreadsheet, dengan satu baris per kasus.
2. Hitung setiap hasil dari masukan saja, tanpa membaca kolom hasil di doc 21 lebih dulu.
3. Bandingkan. Laporkan setiap perbedaan sebagai issue `bug` berlabel `docs`.
4. Data contoh di `docs/templates/09-usaha/contoh/` ikut dicocokkan dengan U01, U02, U08, dan U09.
5. Kirim [H25](../serah-terima/H25.md).

Bukti selesai: lembar hitung terlampir di issue dan semua baris cocok.

Hindari: menghitung dengan kalkulator yang membulatkan otomatis. Tulis pembulatan setengah ke atas secara eksplisit.

### Kasus berkas · Impor dan ekspor

Langkah:

1. Daftar berkas uji: berkas rusak, `.xlsm`, berkas terenkripsi, di atas 5 MB, 5.001 baris, ZIP bom, sel rumus, header berbeda, bentuk lebar, pemisah desimal titik dan koma.
2. Hasil yang diharapkan per berkas: kode error, atau jumlah baris valid dan bermasalah.
3. Daftar pemeriksaan ekspor: tipe sel, format rupiah, escape rumus, BOM, dan berkas terbuka di Excel dan LibreOffice.
4. Kirim [H26](../serah-terima/H26.md).

Bukti selesai: daftar disetujui Hamid di issue `kontrak`.

### Isolasi dan uji pengguna

Langkah:

1. Tabel semua endpoint Fase U dengan kolom: pengguna lain, usaha lain milik pengguna sama, tamu, usaha baca saja.
2. Rencana uji pengguna: tugas "catat satu kunjungan mitra dan buat invoice" dan tugas orientasi "mulai dari akun baru sesuai pekerjaanmu" (KN-16), ukuran waktu dan bantuan, data contoh Tapeku, bukan data asli peserta.

Bukti selesai: kedua tabel tercatat di issue QA Fase U.

## Urutan kerja yang disarankan

1. Lembar hitung, lalu kirim [H25](../serah-terima/H25.md).
2. Kasus berkas, lalu kirim [H26](../serah-terima/H26.md).
3. Isolasi dan uji pengguna.

## Serah terima masuk

Tidak ada. QA-6 memakai doc 16, 20, dan 21 yang sudah ada.

## Serah terima keluar

Setelah butirnya selesai, laporkan di issue **Laporan selesai** dan mention penerima. Lanjut ke milestone berikutnya tanpa menunggu tanggapan. Bila penerima tidak setuju, perbaiki lalu laporkan ulang di issue yang sama.


| Label di diagram | Ke | Yang diserahkan | Kanal |
|---|---|---|---|
| [**H25 · QA-6 → CORE-6 · kasus uji rumus disetujui**](../serah-terima/H25.md) | Octaf (BE 1), [CORE-6](CORE-6.md) | Lembar hitung U01 sampai U17 | Komentar di issue `kontrak`, label `kontrak-disetujui` |
| [**H26 · QA-6 → AI-6 · kasus uji impor dan ekspor disetujui**](../serah-terima/H26.md) | Hamid (BE 2), [AI-6](AI-6.md) | Daftar berkas uji dan hasil yang diharapkan | Komentar di issue `kontrak`, label `kontrak-disetujui` |

## Selesai bila

- [ ] Laporan QA dibuat dengan template 🔒 [web `docs/qa`](https://github.com/DraftAja/Frontend-Nextjs/blob/develop/docs/qa/README.md), dicetak ke PDF dengan `npm run qa:report`, dan PDF-nya dilampirkan di issue serah terima yang diuji
- [ ] Semua kasus U01 sampai U17 cocok atau doc 21 sudah diperbaiki
- [ ] Daftar berkas uji disetujui
- [ ] Tabel isolasi dan rencana uji pengguna tercatat
- [ ] H25 dan H26 dilaporkan di issue dan reviewer di-mention

## Dokumen rujukan lain

- 🔒 [Doc 20, kriteria selesai Fase U](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/20-dashboard-usaha.md#kriteria-selesai-fase-u)
- 🔒 [Berkas contoh](https://github.com/DraftAja/Backend-Laravel/blob/develop/docs/templates/09-usaha/README.md#berkas-contoh)

---

🔒 = berkas di repo privat, hanya bisa dibuka anggota organisasi yang sudah masuk ke GitHub.
