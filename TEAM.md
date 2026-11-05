# Panduan Tim Pengembang – KimiaKu

Dokumen ini memuat struktur tim pengembang, kesepakatan pembagian peran, distribusi issue kerja, serta protokol kolaborasi di repositori resmi **KimiaKu**.

---

## 1. Identitas Proyek & Baseline Pembelajaran

* **Nama Media Pembelajaran**: KimiaKu
* **Mata Pelajaran & Sasaran**: Kimia SMA Kelas XI (Fase F – Kurikulum Merdeka)
* **Topik Inti Pembelajaran**: Laju Reaksi & Teori Tumbukan dengan Pendekatan POE (*Predict, Observe, Explain*)
* **Tumpukan Teknologi**: React + Vite (Arsitektur *Local-First* tanpa backend server)
* **Baseline Proyek**: Halaman Beranda Interaktif dari sesi awal lokakarya, dikembangkan secara modular untuk multi-modul dan simulasi praktikum digital.

---

## 2. Susunan Anggota Tim & Pembagian Peran

| No | Nama Anggota | Akun GitHub | Peran Utama | Fokus Tanggung Jawab & Fitur | Issue Terkait |
|:---:|:---|:---|:---|:---|:---:|
| 1 | **Dhila** | `@dhilaa666-cmd` | **Ketua / Lead Developer** | Mengelola baseline repositori, arsitektur integrasi React + Vite, struktur layout utama (`Navbar`, `HeroSection`, `TopicCards`, `ModuleModal`), manajemen state global di `App.jsx`, serta alur merge branch kelompok. | **#1** |
| 2 | **Aulia** | `@wandaaulia55` | **Kurator Materi Kimia** | Mengurasi keilmiahan konten kimia di `src/data/modulesData.js` (4 faktor laju reaksi, teori tumbukan, fenomena makroskopis), menyusun naskah skenario POE, bank instrumen kuis 15 butir di `src/data/quizData.js`, serta menjamin ketepatan notasi kimia baku ($E_a$, $\Delta T$, $\text{Na}_2\text{S}_2\text{O}_3$, $\text{CaCO}_3$). | **#2** |
| 3 | **Rafidah** | `@rafidahsititaqiyyah-maker` | **Penguji QA & Dokumentasi** | Menyusun dokumen pengarah kelompok (`TEAM.md`, `USER_FLOW.md`, `DESAIN_TEKNIS.md`), merancang skenario pengujian alur fungsional (Beranda $\rightarrow$ Modul 1 $\rightarrow$ POE $\rightarrow$ Beranda), memverifikasi persistensi `localStorage`, serta melakukan *peer review* pada Pull Request. | **#3** |

---

## 3. Rincian Distribusi Issue Kerja Kelompok

### Issue #1: Arsitektur Dasar & Navigasi Modul Pembelajaran
* **Penanggung Jawab**: Dhila (`@dhilaa666-cmd`)
* **Branch Kerja**: `main` / `feat/arsitektur-navigasi`
* **Ruang Lingkup**:
  1. Menyiapkan kerangka kerja React + Vite yang stabil dan dapat dijalankan bersama (`npm run dev`).
  2. Mengembangkan komponen dialog modal (`ModuleModal.jsx`) yang menaungi 3 tab: Materi, Simulasi POE, dan Kuis.
  3. Mengatur penyimpanan lokal berbasis `localStorage` dengan penamaan awalan aman `kimiaku-`.
  4. Meninjau dan menyetujui Pull Request dari anggota tim sebelum digabungkan ke `main`.

### Issue #2: Kurasi Konten Kimia & Naskah Interaktif POE
* **Penanggung Jawab**: Aulia (`@wandaaulia55`)
* **Branch Kerja**: `feat/data-laju-reaksi`
* **Ruang Lingkup**:
  1. Menyusun data terstruktur materi 4 faktor laju reaksi pada `src/data/modulesData.js`.
  2. Menuliskan narasi pertanyaan prediksi (*Predict*), parameter simulasi (*Observe*), dan ulasan saintifik mendalam (*Explain*).
  3. Menyusun 5 butir soal pilihan ganda per modul (total 15 soal) lengkap dengan opsi, kunci jawaban, dan umpan balik pembahasannya di `src/data/quizData.js`.
  4. Memvalidasi bahwa seluruh rumus dan persamaan reaksi memenuhi standar penulisan simbol/subskrip kimia.

### Issue #3: Penyusunan Dokumen Pengarah Kelompok & Pengujian Fungsional
* **Penanggung Jawab**: Rafidah (`@rafidahsititaqiyyah-maker`)
* **Branch Kerja**: `docs/dokumen-kelompok`
* **Ruang Lingkup**:
  1. Membuat dokumen pengarah kelompok di root repositori: `TEAM.md`, `USER_FLOW.md`, dan `DESAIN_TEKNIS.md`.
  2. Mendokumentasikan perjalanan pengguna dari Beranda menuju penyelesaian siklus POE Modul 1.
  3. Memetakan arsitektur pemisahan lapisan data (`src/data/`) dan komponen (`src/components/`).
  4. Menguji kestabilan alur navigasi di browser dan melaporkan catatan uji kepada Ketua Tim.

---

## 4. Protokol Kolaborasi & Tata Tertib Repositori

Untuk menjamin kenyamanan kerja tim dan integritas kode sesuai standar lokakarya pembelajaran:

1. **Satu Akun Satu Pengembang**: Setiap anggota berkontribusi melalui akun GitHub pribadi yang terdaftar sebagai kolaborator resmi.
2. **Branch `main` Selalu Stabil**: Tidak diperkenankan melakukan *push* langsung ke branch `main`. Setiap penambahan fitur atau dokumen wajib dikerjakan pada branch masing-masing.
3. **Mekanisme Pull Request (PR) & Tinjauan Sejawat (*Peer Review*)**:
   - Sebelum di-merge, ajukan PR dengan judul deskriptif dan ringkasan perubahan yang jelas.
   - Minimal 1 anggota tim wajib meninjau (*review*) perbedaan kode (*diff*) dan menguji fungsionalitasnya di lingkungan lokal.
4. **Dilarang Force Push & Menjaga Riwayat**: Hindari penggunaan perintah berbahaya seperti `git push --force` di repositori bersama agar riwayat kerja rekan tidak hilang.
5. **Bebas Kredensial & Berkas Sampah**: File konfigurasi lokal, `.env`, dan folder `node_modules` dipastikan tidak masuk ke dalam version control (tercatat di `.gitignore`).
6. **Pencatatan Log AI Agent**: Setiap instruksi dan kontribusi pemanfaatan asisten kecerdasan buatan dicatat secara berkala pada `AI_LOG.md` sebagai bagian dari transparansi pengerjaan.

