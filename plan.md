# Implementation Plan: Analisis Text Mining dan Coding Interpretatif Juz 1

**Date**: 2026-10-22 | **Spec**: [PRD.md](./PRD.md)

## Summary

Bangun satu notebook Jupyter sebagai alur kerja utama untuk menganalisis 148 ayat Juz 1. Notebook memuat dan memvalidasi data, menyaring ayat, menghitung frekuensi kata, mengirim prompt terpilih ke API OpenAI-compatible, serta mencatat respons dan tinjauan peneliti sampai tahap coding dan tema.

## Technical Context

**Language/Version**: Python 3 (runtime notebook yang tersedia di workspace; versi runtime notebook perlu dikonfirmasi saat eksekusi)  
**Primary Dependencies**: Jupyter Notebook dan pandas; pemanggilan API memakai standard library Python.  
**Storage**: CSV sumber `data/raw/quran_dataset.csv`; notebook `notebooks/analisis_juz1.ipynb`; hasil tabel di `data/outputs/`.  
**Testing**: Validasi terarah di notebook untuk cakupan ayat, jumlah 148 record, rujukan, kolom penting, dan keterlacakan hasil. Eksekusi ulang notebook untuk memastikan alur berjalan.  
**Target Platform**: Lingkungan Jupyter lokal di Windows.  
**Performance Goals**: Pengolahan 148 ayat dan lima tahap API selesai dalam satu sesi notebook; request API dijalankan hanya saat cell tahap LLM dieksekusi.  
**Constraints**: Cakupan hanya Al-Fātiḥah 1–7 dan Al-Baqarah 1–141; API key dibaca dari environment variable dan tidak disimpan di notebook; teks terjemahan adalah korpus utama; preprocessing hanya membersihkan spasi/baris baru dan menyiapkan salinan pencarian tanpa mengubah sumber atau menghapus negasi; setiap code cell memiliki markdown singkat sebelum dan penjelasan hasil sesudahnya; interpretasi tidak boleh diotomatisasi menjadi kesimpulan tanpa tinjauan peneliti.

## Constitution Check

Tidak ditemukan `constitution.md` pada workspace. Karena itu tidak ada prinsip proyek tambahan yang dapat diverifikasi. Plan mengikuti batas dan prinsip yang eksplisit dalam PRD: cakupan terbatas, reproducibility, rujukan ayat, tinjauan manusia, pemisahan hasil Python/AI/peneliti, dan notebook yang langsung dibaca. Kunci API dijaga di luar source code.

## Applied Guidelines

Tidak ada guideline migrasi yang relevan untuk proyek notebook analisis ini. Plan menggunakan struktur notebook yang telah disiapkan dan memakai standard library untuk integrasi API.

## Implementation Steps

### Phase 1 — Setup

#### Step 1: Menyiapkan kerangka notebook

- **Requirements**: REQ-015, REQ-016, REQ-019
- **Design inputs**: PRD §5, §8, §9; struktur workspace yang sudah ada.
- **Description**: Buat `notebooks/analisis_juz1.ipynb` dengan urutan bagian yang mengikuti alur proyek. Gunakan markdown singkat sebelum code cell dan sisipkan penjelasan hasil setelahnya. Sediakan bagian untuk data dan metode, Keyword/Phrase, bantuan AI, tiap tahap coding, evaluasi, dan kesimpulan. Jaga markdown lugas dan spesifik.

### Phase 2 — Foundational

#### Step 2: Memuat dan memeriksa dataset sumber

- **Requirements**: REQ-002, NFR-001, NFR-003
- **Design inputs**: PRD §6; `data/raw/quran_dataset.csv`.
- **Description**: Di notebook, baca CSV dengan parser yang menangani field teks berbaris baru, periksa kolom yang diperlukan, nilai kosong, serta identifikasi surah dan ayat. Tampilkan pemeriksaan penting dan hentikan alur dengan pesan yang jelas jika sumber atau struktur tidak sesuai. Jangan mengubah CSV sumber.

### Phase 3 — User Story 1: Menyiapkan data Juz 1 (P1)

#### Step 3: Menyaring dan memvalidasi 148 ayat

- **Requirements**: REQ-001, REQ-002, NFR-001, NFR-002, NFR-003
- **Design inputs**: PRD §6; definisi rentang Juz 1.
- **Description**: Filter berdasarkan `nomor_surat` dan `nomor_ayat`, bukan nama surah. Pastikan hasil mencakup Al-Fātiḥah 1–7 dan Al-Baqarah 1–141, memiliki 148 record unik, mempertahankan teks Arab/terjemahan, dan menampilkan ringkasan batas data. Sajikan ringkasan validasi dalam notebook.

### Phase 4 — User Story 2: Meninjau kandidat Keyword/Phrase (P1)

#### Step 4: Melakukan preprocessing ringan

- **Requirements**: REQ-002, REQ-017, REQ-020, NFR-007
- **Design inputs**: PRD §6 dan §8; data Juz 1 hasil filter.
- **Description**: Buat salinan kolom pencarian dari terjemahan untuk merapikan whitespace dan baris baru serta menyamakan kapitalisasi untuk pencarian. Pertahankan teks asli, kutipan, urutan kata, tanda negasi, dan rujukan; jangan melakukan stemming atau menghapus kata. Tampilkan contoh sebelum/sesudah dan validasi bahwa jumlah record serta kolom sumber tidak berubah.

#### Step 5: Menghitung kandidat keyword

- **Requirements**: REQ-003, REQ-005, REQ-006, REQ-017
- **Design inputs**: PRD §7 Skenario 2; salinan pencarian terjemahan yang telah dipreproses ringan.
- **Description**: Hitung token/frekuensi dengan aturan yang dijelaskan di notebook. Jika daftar kata umum digunakan untuk tampilan frekuensi, dokumentasikan dan tinjau daftar tersebut; jangan mengecualikan negasi dari konteks atau mengubah teks sumber. Sajikan frekuensi sebagai kandidat untuk dipilih peneliti—bukan ukuran kepentingan makna.

#### Step 6: Menyusun kandidat phrase dan konteks ayat

- **Requirements**: REQ-003, REQ-004, REQ-006, REQ-018, REQ-020
- **Design inputs**: PRD §7 Skenario 2; teks sumber dan salinan pencarian Juz 1.
- **Description**: Pilih phrase yang relevan secara eksplisit dan dokumentasikan dasar pemilihannya. Untuk setiap kandidat keyword/phrase, tampilkan jenis, frekuensi, rujukan surah:ayat, dan kutipan dari teks terjemahan asli. Pertahankan perbedaan antara frekuensi total dan jumlah kutipan contoh yang ditampilkan. Sajikan tabel untuk ditinjau sebelum prompt dikirim ke API.

### Phase 5 — User Story 3: Meminta dan meninjau bantuan AI (P1)

#### Step 7: Mengirim prompt ke API LLM

- **Requirements**: REQ-007, REQ-008, REQ-013, NFR-005, NFR-008
- **Design inputs**: PRD §7 Skenario 3; tabel kandidat dan kutipan kontekstual.
- **Description**: Bentuk prompt dari satu keyword/phrase yang dipilih dan ayat yang cocok, lalu kirim ke endpoint chat completions OpenAI-compatible. Baca key, base URL, dan model dari environment variable. Tampilkan kegagalan request secara eksplisit dan catat prompt, respons, model, serta waktu; jelaskan bahwa request dapat menimbulkan biaya.

#### Step 8: Mencatat respons dan keputusan peneliti

- **Requirements**: REQ-009, REQ-010, REQ-012, REQ-013, REQ-017
- **Design inputs**: PRD §7 Skenario 3 dan 5; prompt serta respons API.
- **Description**: Sediakan tabel/cell untuk mencatat kandidat, rujukan, prompt, respons API, model dan tanggal penggunaan, pemeriksaan bukti, keputusan terima/ubah/tolak, makna final, dan alasan peneliti. Jangan mengisi hasil yang belum tersedia.

### Phase 6 — User Story 4: Menyusun kategori dan tema (P2)

#### Step 9: Melakukan Open dan Axial Coding

- **Requirements**: REQ-009, REQ-011, REQ-012, REQ-017, REQ-018, NFR-002, NFR-004
- **Design inputs**: PRD §7 Skenario 4; kandidat dan makna yang sudah ditinjau.
- **Description**: Dokumentasikan open code sebagai label fenomena yang dekat dengan data, beserta alasan dan rujukan. Susun axial category dengan mencatat kode yang dihubungkan serta alasan hubungannya. Gunakan usulan AI sebagai masukan yang ditinjau, bukan pengelompokan final otomatis.

#### Step 10: Merumuskan Selective Coding, tema, dan interpretasi

- **Requirements**: REQ-009, REQ-011, REQ-012, REQ-017, REQ-018, NFR-002, NFR-004
- **Design inputs**: PRD §7 Skenario 4; hasil Open dan Axial Coding.
- **Description**: Rumuskan core category hanya jika dapat menjelaskan kategori dan didukung bukti. Sintesis kategori menjadi tema yang menjawab fokus analisis; cantumkan ayat pendukung dan batas interpretasinya. Tandai interpretasi sebagai keputusan peneliti dan hindari klaim tafsir otoritatif.

### Phase 7 — User Story 5: Mengevaluasi AI dan menyiapkan hasil (P2)

#### Step 11: Mengevaluasi AI dan mengekspor tabel hasil

- **Requirements**: REQ-012, REQ-014, REQ-018, NFR-001, NFR-002, NFR-006
- **Design inputs**: PRD §7 Skenario 5; seluruh tabel kandidat dan coding yang telah ditinjau.
- **Description**: Tulis evaluasi ringkas tentang kesesuaian konteks, dukungan bukti, kegunaan, dan keterbatasan jawaban AI dengan contoh yang benar-benar dianalisis. Ekspor tabel kandidat ke `data/outputs/keyword_phrase_juz1.csv` dan tabel coding yang telah ditinjau ke `data/outputs/hasil_coding_juz1.csv`. Jalankan ulang notebook dari awal sampai akhir; pastikan validasi menghasilkan 148 ayat, markdown mendahului dan menjelaskan setiap code cell, rujukan tetap terlacak, keputusan AI dan peneliti terpisah, serta CSV hasil dapat dibuka kembali.

### Final Phase — Polish & Validation

#### Step 12: Menjalankan ulang notebook dan memeriksa keterlacakan

- **Requirements**: REQ-001–REQ-020, NFR-001–NFR-007
- **Design inputs**: seluruh bagian notebook dan hasil ekspor.
- **Description**: Jalankan notebook dari awal sampai akhir. Pastikan validasi menghasilkan 148 ayat; preprocessing mempertahankan jumlah record, teks sumber, negasi, dan rujukan; keluaran tabel memiliki konteks; setiap tahap memiliki markdown sebelum/sesudah code cell; keluaran AI dan keputusan manusia terpisah; file ekspor dapat dibuka kembali; dan tidak ada error yang tertutup atau hasil lama yang menyesatkan.

## Project Structure

```text
Interpretasi Text Mining/
├── PRD.md
├── plan.md
├── .gitignore
├── requirements.txt
├── data/
│   ├── raw/
│   │   └── quran_dataset.csv
│   └── outputs/
│       ├── keyword_phrase_juz1.csv
│       └── hasil_coding_juz1.csv
├── notebooks/
│   └── analisis_juz1.ipynb
└── referensi/
    └── Quran_Hadis_Mining_Coding_OK.pdf
```

## Task Breakdown

### Phase 1 — Setup

- [x] T001 [Plan:1.1] Buat kerangka `notebooks/analisis_juz1.ipynb` dengan bagian analisis berurutan, markdown singkat sebelum code cell, dan slot penjelasan hasil setelahnya.

### Phase 2 — Foundational

- [x] T002 [Plan:2.1] Tambahkan cell pemuatan dan pemeriksaan `data/raw/quran_dataset.csv` di `notebooks/analisis_juz1.ipynb`; tampilkan kolom, nilai kosong, dan pesan kesalahan yang eksplisit tanpa mengubah sumber.

### Phase 3 — User Story 1 (P1)

- [x] T003 [US1] [Plan:3.1] Tambahkan filter nomor surah dan ayat serta validasi 148 record Juz 1 di `notebooks/analisis_juz1.ipynb`; tampilkan ringkasan cakupan dan rujukan.

### Phase 4 — User Story 2 (P1)

- [x] T004 [US2] [Plan:4.1] Tambahkan preprocessing ringan pada salinan teks pencarian di `notebooks/analisis_juz1.ipynb`; rapikan whitespace, pertahankan teks sumber dan kata negasi, serta tampilkan validasi sebelum/sesudah.
- [x] T005 [US2] [Plan:4.2] Tambahkan perhitungan frekuensi token dengan aturan normalisasi dan pengecualian kata umum yang terdokumentasi di `notebooks/analisis_juz1.ipynb`.
- [x] T006 [US2] [Plan:4.3] Tambahkan tabel kandidat keyword/phrase terpilih dengan frekuensi, kutipan terjemahan asli, serta rujukan ayat di `notebooks/analisis_juz1.ipynb`.

### Phase 5 — User Story 3 (P1)

- [x] T007 [US3] [Plan:5.1] Kirim prompt untuk satu keyword/phrase beserta frekuensi, konteks ayat, dan rujukan ke API OpenAI-compatible.
- [x] T008 [US3] [Plan:5.2] Catat respons API, model/waktu, pemeriksaan bukti, serta keputusan dan alasan peneliti.

### Phase 6 — User Story 4 (P2)

- [x] T009 [US4] [Plan:6.1] Tambahkan tabel Open dan Axial Coding dengan alasan hubungan serta rujukan ayat di `notebooks/analisis_juz1.ipynb`.
- [x] T010 [US4] [Plan:6.2] Tambahkan Selective Coding, tema, dan interpretasi akhir yang menghubungkan setiap kesimpulan dengan kategori, kode, dan bukti ayat di `notebooks/analisis_juz1.ipynb`.

### Phase 7 — User Story 5 (P2)

- [x] T011 [US5] [Plan:7.1] Tambahkan evaluasi kualitatif AI dan ekspor log AI, tabel kandidat, serta tabel coding yang ditinjau ke file CSV di `data/outputs/`.

### Final Phase — Polish & Validation

- [x] T012 [Plan:7.1] Jalankan ulang `notebooks/analisis_juz1.ipynb`, periksa hasil 148 ayat, preprocessing yang menjaga konteks, markdown tiap code cell, keterlacakan rujukan, pemisahan AI/peneliti, dan pembacaan ulang seluruh CSV keluaran.

## Requirement Mapping

| REQ ID | Description | Plan Items | Implementation Evidence |
|--------|-------------|-------------|-------------------------|
| REQ-001 | Batasi pada Al-Fātiḥah 1–7 dan Al-Baqarah 1–141 | 3.1 | Filter dan validasi cakupan pada `notebooks/analisis_juz1.ipynb` |
| REQ-002 | Pertahankan metadata ayat dan teks | 2.1, 3.1 | Data terpilih dengan kolom sumber pada notebook |
| REQ-003 | Hitung kandidat keyword/phrase | 4.2, 4.3 | Tabel frekuensi dan kandidat pada notebook; `keyword_phrase_juz1.csv` |
| REQ-004 | Sertakan kutipan dan rujukan kandidat | 4.3 | Tabel kandidat dan konteks pada notebook/CSV |
| REQ-005 | Bedakan frekuensi dari makna | 4.2, 5.1 | Penjelasan batas frekuensi dan prompt kontekstual |
| REQ-006 | Pilih kandidat dan catat dasar pemilihan | 4.2, 4.3 | Aturan seleksi dan tabel kandidat di notebook |
| REQ-007 | Minta makna AI berdasarkan bukti/konteks | 5.1 | Template prompt dan input konteks pada notebook |
| REQ-008 | Kirim prompt tahap LLM ke API OpenAI-compatible | 5.1 | Notebook membaca konfigurasi dan API key dari environment variable, mengirim prompt, serta mencatat hasil |
| REQ-009 | Catat hasil seluruh tahap coding | 5.2, 6.1, 6.2 | Tabel respons, Open/Axial/Selective/Thematic Coding |
| REQ-010 | Catat keputusan terima/ubah/tolak | 5.2 | Tabel keputusan manusia di notebook |
| REQ-011 | Telusuri tema/kategori ke kode dan ayat | 6.1, 6.2 | Rujukan dan relasi pada tabel coding |
| REQ-012 | Pisahkan komputasi, AI, keputusan, interpretasi | 5.2, 6.1, 6.2, 7.1 | Bagian dan tabel berlabel terpisah di notebook |
| REQ-013 | Dokumentasikan prompt/respons/model/tanggal | 5.1, 5.2 | Template prompt dan log AI pada notebook |
| REQ-014 | Evaluasi kualitas dan keterbatasan AI | 7.1 | Evaluasi kualitatif AI di notebook |
| REQ-015 | Satu notebook sebagai artefak analisis | 1.1 | `notebooks/analisis_juz1.ipynb` |
| REQ-016 | Markdown singkat sebelum dan sesudah code cell | 1.1, 2.1–7.1 | Struktur dan markdown setiap tahap dalam notebook |
| REQ-017 | Python untuk hitungan; keputusan analitis tampak | 4.1, 5.2, 6.1, 6.2 | Cell komputasi dan tabel tinjauan manusia |
| REQ-018 | Tampilkan tabel/konteks/rujukan yang mudah ditinjau | 4.2, 6.1, 6.2, 7.1 | Tampilan tabel pada notebook dan hasil CSV |
| REQ-019 | Markdown ringkas, spesifik, tanpa pengulangan | 1.1, 7.1 | Standar penulisan markdown dan review akhir |
| REQ-020 | Preprocessing ringan dan mempertahankan konteks | 4.1, 4.3, 7.1 | Validasi salinan pencarian, teks asli, negasi, dan rujukan |

## Open Items and Warnings

- Fokus kerja eksploratif untuk kandidat kata/frasa di seluruh Juz 1 telah dipilih; pertanyaan yang lebih sempit dapat ditambahkan jika diwajibkan tugas.
- Integrasi API memerlukan `OPENAI_API_KEY` dan kuota yang memadai; validasi request riil perlu dilakukan oleh pengguna dengan kredensialnya.
- Sumber, edisi, penerjemah, dan lisensi terjemahan belum terkonfirmasi; verifikasi sebelum publikasi atau interpretasi final.
- Constitution dan artefak arsitektur khusus tidak ditemukan; proyek ini adalah workflow notebook sederhana dan tidak memerlukan desain layanan/aplikasi.
- Runtime Jupyter dan dependensinya harus diverifikasi ketika notebook dijalankan.
