<div align="center">

# DraftAja

### ✍️ Your Academic Writing, Made Easier.

Aplikasi web untuk membantu mahasiswa menyusun dokumen akademik dengan lebih praktis, mulai dari penulisan hingga ekspor dokumen.

<p align="center">
  <img src="https://media.giphy.com/media/CiZ9e5IUPqeVFzc8Mp/giphy.gif" width="380" alt="Jujutsu Kaisen GIF">
</p>

<p>
  <img src="https://img.shields.io/badge/Laravel-12-FF2D20?style=for-the-badge&logo=laravel&logoColor=white" alt="Laravel 12">
  <img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black" alt="React 18">
  <img src="https://img.shields.io/badge/PHP-8.2%2B-777BB4?style=for-the-badge&logo=php&logoColor=white" alt="PHP 8.2+">
  <img src="https://img.shields.io/badge/Tailwind_CSS-3-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" alt="Tailwind CSS">
</p>

<p>
  <strong>Built for students, designed to simplify academic writing.</strong>
</p>

</div>

---

## 📌 About DraftAja

**DraftAja** adalah aplikasi web yang membantu mahasiswa menyusun dokumen akademik dengan lebih terstruktur. Pengguna dapat membuat dokumen, mengisi bagian-bagian laporan melalui editor rich-text, menggunakan bantuan AI, serta mengimpor template Word dan mengekspor dokumen ke PDF maupun DOCX.

DraftAja juga dilengkapi dengan **Questify**, modul pembelajaran berbasis permainan untuk mengasah kemampuan CSS, SQL, matematika, dan logika.

## ✨ Main Features

| Fitur | Deskripsi |
|---|---|
| 📝 Document Builder | Membuat dokumen akademik dengan cover dan struktur bab. |
| 🤖 AI Assistance | Membantu proses penulisan melalui integrasi AI. |
| 🖊️ Rich-Text Editor | Menyusun dan mengedit konten setiap bab. |
| 📄 Word Template Import | Mengimpor template dokumen `.docx`. |
| 📥 PDF & DOCX Export | Mengekspor hasil dokumen ke format PDF dan Word. |
| 🎮 Questify | Belajar CSS, SQL, matematika, dan logika melalui permainan. |

## 🛠️ Technology Stack

### Backend
- [Laravel 12](https://laravel.com/)
- PHP 8.2+
- Inertia.js v2
- Laravel Breeze
- Laravel Sanctum

### Frontend
- [React 18](https://react.dev/)
- [Vite 5](https://vite.dev/)
- [Tailwind CSS 3](https://tailwindcss.com/)
- [TipTap](https://tiptap.dev/)
- dnd-kit
- FilePond
- Ziggy

### Database & Integrations
- **Database:** SQLite
- **Planned Database:** PostgreSQL 16 on Azure
- **AI:** Google Gemini and Groq
- **PDF Export:** Spatie Browsershot
- **DOCX Export:** PHPWord

---

## 🚀 Getting Started

Ikuti langkah berikut untuk menjalankan DraftAja di lingkungan lokal.

### Prerequisites

Pastikan perangkat sudah memiliki:
- PHP 8.2 atau lebih baru
- Composer
- Node.js dan npm
- Database SQLite

### 1. Clone Repository

```bash
git clone <REPOSITORY_URL>
cd DraftAja
```

Ganti `<REPOSITORY_URL>` dengan URL repository GitHub proyek.

### 2. Install Dependencies

```bash
composer install
npm install
```

### 3. Configure Environment

Buat file `.env` berdasarkan template yang tersedia.

**Windows PowerShell:**

```powershell
Copy-Item .env.example .env
```

**Linux / macOS:**

```bash
cp .env.example .env
```

Kemudian jalankan:

```bash
php artisan key:generate
```

### 4. Setup Database

Buat file database SQLite.

**Windows PowerShell:**

```powershell
New-Item -ItemType File -Force database/database.sqlite
```

**Linux / macOS:**

```bash
touch database/database.sqlite
```

Pastikan konfigurasi database di `.env` sudah sesuai, lalu jalankan migrasi dan seeder:

```bash
php artisan migrate --seed
php artisan storage:link
```

### 5. Configure AI API Keys

Tambahkan API key berikut ke file `.env` untuk mengaktifkan fitur AI yang menggunakannya.

```env
GEMINI_API_KEY=your_gemini_api_key
GROQ_API_KEY=your_groq_api_key
```

Ganti nilai tersebut dengan API key milik sendiri. Jangan mengunggah file `.env` atau API key ke repository publik.

### 6. Build Assets and Run Application

Build aset frontend:

```bash
npm run build
```

Jalankan aplikasi beserta layanan development:

```bash
composer run dev
```

Ikuti alamat lokal yang ditampilkan di terminal untuk membuka aplikasi.

> **Catatan:** Build aset dengan `npm run build` diperlukan sebelum melakukan ekspor PDF menggunakan Browsershot. Pastikan kebutuhan headless Chrome untuk Browsershot juga sudah terpenuhi.

### Default Seed Account

Jika akun bawaan masih tersedia setelah menjalankan seeder:

| Field | Value |
|---|---|
| Email | `alvira@laporin.com` |
| Password | `password` |

Akun tersebut hanya untuk development. Ganti atau nonaktifkan kredensial bawaan sebelum aplikasi digunakan di lingkungan produksi.

---

## 📚 Documentation

Dokumentasi teknis tersedia di folder [`docs/`](docs/).

| Dokumen | Deskripsi |
|---|---|
| [01 — Project Overview](docs/01-project-overview.md) | Tujuan, fitur, aktor, dan teknologi proyek. |
| [02 — System Architecture](docs/02-system-architecture.md) | Arsitektur sistem, layer, dan alur request. |
| [03 — Codebase Structure](docs/03-codebase-structure.md) | Struktur folder dan tanggung jawab modul. |
| [04 — Database Design](docs/04-database-design.md) | Desain database dan ERD. |
| [05 — API Documentation](docs/05-api-documentation.md) | Endpoint, validasi, dan respons API. |
| [06 — Business Workflows](docs/06-business-workflows.md) | Alur bisnis, sequence diagram, dan flowchart. |
| [07 — Authentication & Security](docs/07-authentication-security.md) | Autentikasi, otorisasi, validasi, dan keamanan. |
| [08 — Integrations](docs/08-integrations.md) | Integrasi AI dan pemrosesan dokumen. |
| [09 — Setup & Development](docs/09-setup-development.md) | Instalasi, konfigurasi, pengujian, dan troubleshooting. |
| [10 — Project Status & Gaps](docs/10-project-status-and-gaps.md) | Status implementasi, kekurangan, risiko, dan rekomendasi. |

**Quick links:** [Architecture](docs/02-system-architecture.md) · [API](docs/05-api-documentation.md) · [Database](docs/04-database-design.md) · [Workflows](docs/06-business-workflows.md)

---

## 💻 Useful Commands

| Command | Description |
|---|---|
| `composer run dev` | Menjalankan layanan development. |
| `npm run dev` | Menjalankan Vite development server. |
| `npm run build` | Build aset frontend. |
| `composer run test` | Menjalankan automated tests menggunakan PHPUnit. |
| `vendor/bin/pint` | Memformat kode PHP. |

---

## 📊 Project Status

Dokumentasi teknis ini menggambarkan kondisi repositori pada branch `main` per **13 Oktober 2026**, berdasarkan verifikasi source code.

Informasi mengenai fitur yang sudah diimplementasikan, fitur parsial, temuan teknis, dan rekomendasi pengembangan dapat dilihat di [Project Status & Gaps](docs/10-project-status-and-gaps.md).

---

<div align="center">

### DraftAja — Make Academic Writing Simpler.

*Developed as part of the Information Technology Project course.*

</div>
