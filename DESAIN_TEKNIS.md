# Desain Teknis Arsitektur – KimiaKu

Dokumen ini menjelaskan struktur arsitektur sistem, tumpukan teknologi (*tech stack*), pola pemisahan lapisan data dan komponen antarmuka, serta konvensi rekayasa perangkat lunak yang diterapkan pada aplikasi web pembelajaran kimia **KimiaKu**.

---

## 1. Tumpukan Teknologi (*Tech Stack*) & Lingkungan Pengembangan

| Komponen Sistem | Teknologi yang Digunakan | Justifikasi & Karakteristik |
|:---|:---|:---|
| **Framework Frontend** | **React 18+** | Berbasis *Functional Components* dan *Modern Hooks* (`useState`, `useEffect`, `useRef`). |
| **Build Tool & Bundler** | **Vite** | Kompilasi kilat (*Instant Server Start*) dan pembaruan modul cepat (*Hot Module Replacement* / HMR). |
| **Penyimpanan Data** | **Local-First (`localStorage`)** | Tidak memerlukan server backend atau database awan. Menjaga privasi siswa dan akses bebas gangguan jaringan. |
| **Ikonografi & Visual** | **Lucide React** | Pustaka ikon SVG modern, ringan, dan ramah aksesibilitas (`FlaskConical`, `Flame`, `Sparkles`, dll.). |
| **Styling & Tata Letak** | **Modular Vanilla CSS** | Menggunakan variabel tema CSS terpusat (`App.css`, `index.css`) dengan skema warna ramah mata (*Light Pastel Mode*). |
| **Code Quality & Linter**| **Oxlint** | *High-performance linter* untuk mendeteksi potensi bug dan menjaga konsistensi sintaks JavaScript modern. |

---

## 2. Pola Arsitektur: Pemisahan Lapisan (*Separation of Concerns*)

KimiaKu menerapkan prinsip pemisahan tegas antara **Lapisan Konten Kimia (*Data Layer*)** dan **Lapisan Tampilan (*Presentation / Component Layer*)**:

```
KimiaKu/
├── src/
│   ├── data/                 <-- DATA LAYER (Objek data murni: teks kimia, rumus, soal, opsi)
│   │   ├── modulesData.js    <-- Naskah modul pembelajaran 1-3 & skenario POE
│   │   └── quizData.js       <-- Bank 15 soal kuis, kunci jawaban, dan pembahasan
│   │
│   ├── components/           <-- COMPONENT LAYER (Komponen fungsional React tampilan)
│   │   ├── Navbar.jsx        <-- Navigasi atas, pencarian, dan indikator progres
│   │   ├── HeroSection.jsx   <-- Pengantar beranda, slogan, dan rumus melayang
│   │   ├── TopicCards.jsx    <-- Grid 3 kartu modul dan status ketuntasan
│   │   ├── ModuleModal.jsx   <-- Dialog modal terpusat (Tab Materi, POE, Kuis)
│   │   ├── PoeSimulation.jsx <-- Mesin simulasi interaktif tahap Predict, Observe, Explain
│   │   ├── QuizModal.jsx     <-- Komponen kuis formatif dan penilaian instan
│   │   ├── SearchBar.jsx     <-- Pencarian interaktif materi kinetika
│   │   └── FeaturesSection.jsx<-- Penjelasan pilar keunggulan media KimiaKu
│   │
│   ├── App.jsx               <-- Root Controller: orchestrator state dan sinkronisasi localStorage
│   ├── App.css               <-- Desain antarmuka modular
│   └── main.jsx              <-- Entry point rendering React ke DOM
```

> **Manfaat Didaktis & Rekayasa**:
> Kurator materi kimia (Aulia) dapat menyempurnakan redaksi soal, opsi kuis, atau menambah naskah penjelasan konsep di dalam folder `src/data/` tanpa khawatir merusak layout CSS atau logika komponen React yang dirawat oleh Lead Developer (Dhila).

---

## 3. Rincian Lapisan Data (`src/data/`)

Folder `src/data/` menampung seluruh naskah akademis, struktur materi pembelajaran Fase F, serta instrumen evaluasi kuis dalam format objek JavaScript murni (*JSON-like exports*).

### A. `src/data/modulesData.js`
Mengekspor konstanta array `MODULES_DATA` yang terdiri dari 3 objek modul utama:
* **Identitas Modul**: `id`, `number`, `badge`, `title`, `subtitle`, `category`, `accentColor`, `estimatedTime`.
* **Persamaan Reaksi Terkait (`sampleReaction`)**:
  * Menggunakan struktur array reaktan dan produk terurai: `{ coeff, formula, sub, state }`.
  * Menjamin rendering formula reaksi kimia baku (contoh: $2\text{HCl}(aq) + \text{Na}_2\text{S}_2\text{O}_3(aq) \rightarrow 2\text{NaCl}(aq) + \text{SO}_2(g) + \text{S}(s) + \text{H}_2\text{O}(l)$).
* **Formula Badges (`formulaBadges`)**: Persamaan kinetika per modul (misal: Persamaan Laju $v = k[\text{A}]^m[\text{B}]^n$, rumus kenaikan suhu $\Delta T$, dan persamaan Arrhenius $k = A e^{-E_a/RT}$).
* **Uraian Materi Komprehensif (`comprehensiveContent`)**:
  * Teks pengantar konsep (*introduction*).
  * Array 4 sub-bagian materi faktor laju reaksi: penjelasan ilmiah, notasi kimia, dan aplikasi kontekstual kehidupan sehari-hari (*dailyExample*).
* **Skenario Interaktif POE (`poeScenario`)**:
  * `type`: Menentukan jenis simulasi (`faktor-laju`, `teori-tumbukan`, atau `makroskopis-fenomena`).
  * `question`: Teks pertanyaan prediksi fenomena awal.
  * `predictions`: Tiga butir hipotesis awal dengan penanda `isCorrect: true/false`.
  * `explanationPoe`: Naskah refleksi dan penjelasan saintifik mendalam untuk tahap *Explain*.

### B. `src/data/quizData.js`
Mengekspor objek `QUIZ_DATA` dengan pemetaan ID modul (`faktor-laju`, `teori-tumbukan`, `makroskopis-fenomena`).
* Berisi **5 butir soal pilihan ganda bermutu per modul** (total 15 soal).
* Setiap butir soal memuat:
  * `id`: Penanda unik soal (misal: `q1-1`, `q1-2`, dst.).
  * `question`: Narasi stimulus soal dengan notasi kimia tepat.
  * `options`: Empat pilihan jawaban heterogen yang menguji pemahaman konseptual (bukan sekadar hafalan).
  * `correctIndex`: Indeks opsi jawaban yang benar (0–3).
  * `explanation`: Pembahasan rinci yang menjelaskan prinsip sains mengapa opsi tersebut benar.

---

## 4. Rincian Lapisan Komponen (`src/components/`)

Folder `src/components/` berisi komponen React terenkapsulasi yang menerima properti (*props*) dan menangani interaksi pengguna:

### A. `Navbar.jsx`
* **Fungsi**: Bilah navigasi atas yang selalu terlihat (*sticky header*).
* **Fitur Utama**:
  * Logo merek dan nama media pembelajaran *KimiaKu*.
  * Tautan langsung ke bagian beranda dan modul belajar.
  * Tombol pemicu pencarian cepat (*Quick Search*).
  * Indikator Progres Belajar Anonim: Menghitung persentase modul yang telah dituntaskan siswa berdasarkan `completedTopics`.

### B. `HeroSection.jsx`
* **Fungsi**: Bagian penerima siswa di bagian atas beranda.
* **Fitur Utama**:
  * Teks pemantik kontekstual mengenai pembelajaran kimia berbasis simulasi POE.
  * Tombol CTA (*Call to Action*) *"Mulai Belajar"* yang menggulir halus menuju bagian modul.
  * Dekorasi *Floating Formula Badges* dengan notasi superskrip/subskrip kimia baku ($v = k[\text{A}]^m[\text{B}]^n$, $E_a$, $\Delta T$).

### C. `TopicCards.jsx`
* **Fungsi**: Merender 3 kartu ikhtisar modul pembelajaran di halaman beranda.
* **Fitur Utama**:
  * Mengiterasi data dari `MODULES_DATA`.
  * Menampilkan badge kategori, estimasi durasi, dan persamaan reaksi model.
  * Menampilkan indikator status ketuntasan belajar (tanda centang hijau jika selesai).
  * Tombol akses modul yang memanggil fungsi pembuka modal terpusat.

### D. `ModuleModal.jsx`
* **Fungsi**: Komponen wadah (*container modal dialog*) serbaguna saat salah satu modul dibuka.
* **Fitur Utama**:
  * Menampilkan header modul lengkap dengan tombol tutup ($X$) dan pintasan esc/klik backdrop.
  * Menyediakan 3 tab navigasi internal:
    1. Tab **Materi Lengkap**: Merender teks teori, poin kunci, dan contoh sehari-hari.
    2. Tab **Simulasi POE**: Memuat komponen anak `PoeSimulation.jsx`.
    3. Tab **Uji Pemahaman**: Memuat komponen anak `QuizModal.jsx`.

### E. `PoeSimulation.jsx`
* **Fungsi**: Inti mesin interaktif pedagogis POE (*Predict, Observe, Explain*).
* **Fitur Utama**:
  * **State Mesin POE**: Berpindah teratur antar state `'predict'` $\rightarrow$ `'observe'` $\rightarrow$ `'explain'`.
  * **Kanvas Partikel 2D (Modul 1)**:
    * Menggunakan HTML5 `<canvas>` dan `requestAnimationFrame` untuk merender partikel bola reaktan bergerak bebas.
    * Memodelkan pengaruh suhu (kecepatan partikel), konsentrasi (kerapatan partikel), luas permukaan (ukuran radius dan bidang kontak), serta katalis (ambang batas energi tumbukan efektif).
    * Efek visual percikan (*collision sparks*) hijau untuk tumbukan efektif dan abu-abu untuk lentingan elastis.
  * **Visualisasi Balon & Labu Erlenmeyer (Modul 3)**:
    * Memodelkan reaksi kalsium karbonat dan asam klorida dengan pembentukan gelembung gas dan grafik volume gas vs waktu secara *real-time*.
  * **Tahap Refleksi Ilmiah**: Membandingkan dugaan awal siswa dengan mekanisme partikel nyata.

### F. `QuizModal.jsx`
* **Fungsi**: Antarmuka pengerjaan kuis formatif per modul.
* **Fitur Utama**:
  * Mode interaktif per soal dengan navigasi nomor dan opsi pilihan.
  * Penilaian otomatis: Menghitung persentase skor kelulusan (ambang kelulusan $\ge 60\%$).
  * Umpan balik langsung: Menampilkan kotak penjelasan ilmiah mendalam setelah kuis diselesaikan.
  * Integrasi efek perayaan konfeti visual menggunakan pustaka `canvas-confetti`.

### G. `SearchBar.jsx` & `FeaturesSection.jsx`
* **SearchBar**: Filter data pencarian berbasis teks terhadap judul, kata kunci, dan konsep materi di `MODULES_DATA`.
* **FeaturesSection**: Menjelaskan metodologi edukasi media pembelajaran KimiaKu (Pembelajaran POE, Visualisasi Tumbukan 2D, Penyimpanan Lokal Aman, Notasi Baku).

---

## 5. Kontrak Penyimpanan Data Lokal (*Local-First Storage Contract*)

Sesuai pedoman lokakarya pada `AGENTS.md`, seluruh penyimpanan browser wajib menggunakan satu kunci awalan terstandar (`kimiaku-`):

| Kunci `localStorage` | Tipe Data | Deskripsi Isi Data |
|:---|:---|:---|
| `kimiaku-progres` | `Array<string>` | Daftar ID modul yang telah dituntaskan oleh siswa (contoh: `["faktor-laju"]`). |
| `kimiaku-kuis` | `Record<string, number>` | Nilai terbaik kuis formatif per modul (contoh: `{"faktor-laju": 80, "teori-tumbukan": 100}`). |
| `kimiaku-poe` | `Array<string>` | Rekor ID modul yang siklus aktivitas POE-nya telah diselesaikan siswa secara utuh. |

> **Ketahanan Aplikasi (*Error Handling*)**:
> Seluruh operasi pembacaan dan penulisan `localStorage` pada `App.jsx` dibungkus dengan blok pengaman `try...catch` guna mencegah *crashes* ketika browser siswa mengaktifkan mode penjelajahan privat (*incognito*) yang memblokir akses penyimpanan lokal.

