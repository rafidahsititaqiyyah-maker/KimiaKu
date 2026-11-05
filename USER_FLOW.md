# Alur Pengguna (User Flow) – KimiaKu

Dokumen ini memetakan perjalanan belajar siswa (*learner journey*) pada aplikasi web pembelajaran kimia **KimiaKu**, berfokus pada alur navigasi dari Halaman Beranda menuju **Modul 1: Faktor-Faktor Laju Reaksi** yang menggunakan pendekatan pedagogis **POE** (*Predict, Observe, Explain*), hingga kembali ke Beranda.

---

## 1. Diagram Alur Utama (Mermaid Flowchart)

```mermaid
flowchart TD
    A["Halaman Beranda (/)<br>• Hero Banner &amp; Status Progres<br>• Grid 3 Kartu Modul"] -->|Klik Tombol 'Mulai Belajar' / 'Akses Modul'| B["Modal Pembelajaran Terbuka<br>(Modul 1: Faktor Laju Reaksi)"]
    
    subgraph SIKLUS_MODUL_1 ["Modul 1: Siklus Pembelajaran POE"]
        B -->|Pilih Tab 'Simulasi POE'| C["Tahap 1: PREDICT (Dugaan Awal)<br>• Baca fenomena kenaikan konsentrasi &amp; suhu<br>• Pilih 1 dari 3 hipotesis awal"]
        
        C -->|Klik 'Simpan Prediksi &amp; Masuk Observasi'| D["Tahap 2: OBSERVE (Pengamatan &amp; Eksperimen)<br>• Simulator 2D Canvas Partikel<br>• Atur Suhu, Konsentrasi, Luas Sentuh, Katalis<br>• Amati frekuensi &amp; kilatan tumbukan efektif"]
        
        D -->|Klik 'Lanjut ke Tahap 3: Explain'| E["Tahap 3: EXPLAIN (Penjelasan Ilmiah &amp; Refleksi)<br>• Komparasi dugaan vs data observasi<br>• Ulasan teori kinetik &amp; Maxwell-Boltzmann<br>• Refleksi konsep &amp; simpulan"]
        
        E -->|Klik 'Selesaikan Aktivitas POE'| F["Aktivitas Selesai &amp; Catat Rekor<br>• Data tersimpan ke localStorage<br>• Opsi: Baca Materi / Uji Kuis"]
    end
    
    F -->|Klik Tombol Tutup (X) / Kembali| G["Kembali ke Halaman Beranda<br>• Badge Modul 1 bertanda 'Selesai'<br>• Indikator progress bar bertambah"]
```

---

## 2. Rincian Langkah Navigasi & Interaksi Pengguna

### Langkah 1: Beranda Aplikasi (`/`)
* **Tampilan Siswa**:
  * **Navbar**: Memuat identitas *KimiaKu*, menu navigasi cepat, kolom pencarian materi, serta indikator capaian modul selesai (berdasarkan data `localStorage`).
  * **Hero Section**: Judul utama *"Belajar Kimia Jadi Nyata Lewat Simulasi Interaktif"*, deskripsi pendekatan POE, dan tombol aksi utama (*Call to Action*) *"Mulai Belajar Sekarang"*.
  * **Grid Topik Modul (`TopicCards`)**: Tiga kartu materi pembelajaran Fase F:
    1. **Modul 01**: Faktor-Faktor Laju Reaksi (Suhu, Konsentrasi, Luas Permukaan, Katalis).
    2. **Modul 02**: Teori Tumbukan & Energi Aktivasi ($E_a$).
    3. **Modul 03**: Tampilan Makroskopis & Fenomena Kinetika.
* **Tindakan Siswa**:
  * Siswa menekan kartu atau tombol **"Mulai Belajar" / "Akses Modul"** pada kartu **Modul 01: Faktor-Faktor Laju Reaksi**.

---

### Langkah 2: Masuk ke Modul 1 (`ModuleModal.jsx`)
* **Tampilan Siswa**:
  * Layar membuka dialog modal terfokus (*modal dialog backdrop*).
  * Bagian atas menampilkan tajuk: *Modul 01 • Kinetika Kimia*, judul modul, estimasi waktu penyelesaian (20 Menit), dan persamaan reaksi acuan:
    $$2\text{HCl}(aq) + \text{Na}_2\text{S}_2\text{O}_3(aq) \rightarrow 2\text{NaCl}(aq) + \text{SO}_2(g) + \text{S}(s) + \text{H}_2\text{O}(l)$$
  * Terdapat 3 tab navigasi aktivitas:
    1. **Materi Lengkap**: Penjelasan teori runtut 4 faktor laju reaksi.
    2. **Simulasi POE** (*default* atau dipilih siswa): Simulasi interaktif berbasis dugaan ilmiah.
    3. **Uji Pemahaman**: 5 butir kuis formatif dengan umpan balik langsung.
* **Tindakan Siswa**:
  * Siswa beralih ke tab **Simulasi POE** untuk memulai alur belajar induktif.

---

### Langkah 3: Tahap 1 – PREDICT (Dugaan Ilmiah Awal)
* **Tujuan Instruksional**: Mengaktifkan pengetahuan awal (*prior knowledge*) dan merangsang rasa ingin tahu ilmiah siswa sebelum melihat simulasi visual.
* **Stimulus Masalah**:
  > *"Jika konsentrasi asam klorida (HCl) dinaikkan dari 0,5 M ke 2,0 M dan suhunya dinaikkan dari 25°C ke 50°C, apa yang akan terjadi pada tumbukan mikroskopis partikel dan laju reaksinya?"*
* **Tindakan Siswa**:
  1. Siswa membaca fenomena dan menelaah 3 opsi hipotesis yang tersedia.
  2. Siswa memilih 1 kartu prediksi yang dirasa paling tepat.
  3. Siswa menekan tombol **"Simpan Prediksi & Masuk ke Observasi"**.
* **Respons Sistem**:
  * Pilihan prediksi disimpan ke state komponen lokal.
  * Sistem membuka kunci tahap berikutnya dan mengalihkan layar ke antarmuka **Observe**.

---

### Langkah 4: Tahap 2 – OBSERVE (Pengamatan & Eksperimen Partikel)
* **Tujuan Instruksional**: Menyajikan representasi submikroskopis dinamika partikel sehingga konsep kinetika yang abstrak menjadi teramati (*observable*).
* **Tampilan Antarmuka Simulasi**:
  * **Panel Kontrol Eksperimen**:
    * Slider Suhu Reaksi ($20^\circ\text{C}$ hingga $80^\circ\text{C}$).
    * Pilihan Konsentrasi Pereaksi ($0,5\text{ M}$, $1,0\text{ M}$, $2,0\text{ M}$).
    * Pilihan Luas Permukaan Bidang Sentuh (*Bongkahan*, *Butiran*, *Serbuk*).
    * Sakelar Toggle Katalis (*Aktif* / *Non-aktif*).
  * **Viewport Kanvas Partikel 2D**:
    * Menampilkan wadah reaksi tertutup berisi partikel reaktan bergerak acak.
    * Kenaikan suhu mempercepat vektor kecepatan partikel.
    * Kenaikan konsentrasi melipatgandakan kerapatan partikel per volume wadah.
    * Kilatan hijau (*sparkle*) muncul saat terjadi **tumbukan efektif** (energi melampaui $E_a$).
  * **Instrumen Metrik & Statistik**:
    * Total tumbukan vs Jumlah tumbukan efektif yang menghasilkan reaksi.
    * Indikator progres reaksi (0% hingga 100%).
* **Tindakan Siswa**:
  1. Mengubah parameter (misal menaikkan konsentrasi ke 2,0 M dan suhu ke 50°C sesuai prediksi di tahap 1).
  2. Menekan tombol **"Mulai Reaksi & Amati Frekuensi Tumbukan"**.
  3. Mengamati perbedaan frekuensi tumbukan serta laju pembentukan produk.
  4. Menekan tombol **"Lanjut ke Tahap 3: Explain (Penjelasan Ilmiah)"**.

---

### Langkah 5: Tahap 3 – EXPLAIN (Penjelasan Ilmiah & Refleksi)
* **Tujuan Instruksional**: Menghubungkan temuan observasi dengan konsep ilmiah yang sahih serta merekonstruksi pemahaman siswa jika dugaan awalnya keliru.
* **Tampilan & Konten**:
  * **Kartu Refleksi Hasil**:
    * Menampilkan kembali teks prediksi yang dipilih siswa pada Tahap 1.
    * Menampilkan status: *"Prediksi Anda Tepat dan Sesuai Pengamatan!"* atau *"Mari Refleksikan Hasil Pengamatan Anda!"*.
  * **Uraian Mekanisme Kimia**:
    * Penjelasan pengaruh konsentrasi terhadap kerapatan partikel per satuan volume.
    * Penjelasan pengaruh suhu terhadap kurva distribusi energi kinetik Boltzmann.
    * Penjelasan peran katalis dalam menyediakan rute reaksi baru dengan Energi Aktivasi ($E_a$) lebih rendah.
  * **Simpulan Kunci**: Rangkuman relasi antara dunia submikroskopis dan laju reaksi makroskopis.
* **Tindakan Siswa**:
  * Siswa membaca pembahasan reflektif.
  * Siswa menekan tombol **"Selesaikan Aktivitas POE Modul Ini"**.
* **Respons Sistem**:
  * ID modul dicatat ke dalam array `poeRecords` dan disimpan permanen di `localStorage` melalui kunci `kimiaku-poe`.
  * Muncul pesan konfirmasi ketuntasan belajar.

---

### Langkah 6: Kembali ke Halaman Beranda
* **Tindakan Siswa**:
  * Siswa menutup modal melalui tombol silang (**X**) di sudut kanan atas modal atau mengklik area luar modal.
* **Kondisi Beranda Setelah Belajar**:
  1. **Indikator Progres Navbar**: Persentase atau counter modul selesai bertambah secara dinamis.
  2. **Kartu Modul 01**: Menampilkan badge hijau *"Selesai"* beserta tanda centang.
  3. Siswa dapat melanjutkan ke Modul 02, mencoba tab Kuis, atau membaca modul materi lainnya kapan saja tanpa kehilangan progres.

---

## 3. Penanganan Kondisi Khusus (*Edge Cases*)

| Kasus Khusus | Perilaku Sistem | Pengalaman Siswa |
|:---|:---|:---|
| **Siswa me-refresh halaman (F5) saat di tengah simulasi** | Progres kuis dan status modul yang telah selesai tetap tersimpan di `localStorage` (`kimiaku-*`). Modal kembali tertutup secara elegan ke Beranda. | Siswa tidak kehilangan rekam jejak capaian belajarnya. |
| **Siswa belum memilih opsi prediksi pada tahap Predict** | Tombol *"Simpan Prediksi"* dalam kondisi non-aktif (*disabled*) atau menampilkan pemberitahuan pemandu. | Mencegah siswa melompati proses berpikir dugaan awal (*anti-bypass*). |
| **Siswa ingin mengulang observasi simulasi dari tahap Explain** | Tersedia tombol *"Ulangi Observasi Simulasi"* yang membawa siswa kembali ke tab kanvas tanpa menghapus riwayat prediksi. | Memberikan kebebasan eksplorasi eksperimen mandiri berulang kali. |

