# PRD — Analisis Text Mining dan Coding Interpretatif Juz 1

> **Status:** Draft  
> **Tanggal:** 22 Oktober 2026  
> **Pemilik proyek:** Mahasiswa / peneliti  
> **Dokumen acuan metode:** `referensi/Quran_Hadis_Mining_Coding_OK.pdf`  
> **Dataset:** `data/raw/quran_dataset.csv`

## 1. Ringkasan

Proyek ini menganalisis terjemahan bahasa Indonesia Al-Qur’an pada Juz 1 dengan menggabungkan text mining dan coding interpretatif. Python digunakan untuk menyaring ayat, menghitung kandidat kata dan frasa, serta menyiapkan prompt beserta konteks dan rujukan ayat. Notebook mengirim prompt ke API OpenAI-compatible yang dikonfigurasi; peneliti meninjau, mengoreksi, dan menetapkan hasil akhir.

Alur analisis mengikuti materi perkuliahan:

**Keyword/Phrase → Open Coding → Axial Coding → Selective Coding → Thematic Coding → Interpretasi**

Frekuensi dan kemiripan komputasional diperlakukan sebagai alat bantu menemukan pola, bukan sebagai bukti tunggal atas kepentingan atau makna suatu ayat.

## 2. Latar Belakang dan Masalah

Text mining dapat menemukan kata yang sering muncul, frasa, dan pola tekstual, tetapi hasil tersebut tidak otomatis menjelaskan fenomena atau tema. Proyek memerlukan alur yang menjaga konteks ayat, menelusuri setiap kode ke sumbernya, dan membedakan usulan AI dari keputusan analitis peneliti.

## 3. Tujuan

- Menemukan kandidat keyword dan phrase dalam terjemahan Juz 1.
- Menafsirkan kandidat berdasarkan konteks ayat, bukan hanya frekuensinya.
- Menyusun hasil secara bertahap dari kode deskriptif ke kategori, konsep inti, dan tema.
- Menilai secara kritis kegunaan dan keterbatasan AI dalam membantu analisis.
- Menyimpan bukti dan keputusan coding agar hasil dapat diperiksa kembali.

## 4. Pengguna dan Kebutuhan

### Pengguna utama

Mahasiswa/peneliti yang mengerjakan praktikum text mining dan interpretasi teks Qur’an.

### Kebutuhan utama

Pengguna perlu dapat:

1. membatasi data ke Juz 1 secara konsisten;
2. melihat kandidat kata/frasa bersama frekuensi, konteks terjemahan, dan rujukan surah:ayat;
3. meminta usulan AI dengan konteks yang cukup;
4. meninjau dan mengubah usulan AI sebelum menetapkan coding;
5. melacak setiap kategori dan tema ke kode serta ayat sumbernya; dan
6. mengevaluasi kemampuan AI berdasarkan hasil yang dapat dibandingkan dengan bukti teks.

## 5. Batas Cakupan

### Termasuk

- Ayat Al-Fātiḥah 1–7 dan Al-Baqarah 1–141.
- Satu Jupyter Notebook (`.ipynb`) sebagai media utama untuk menjalankan dan menjelaskan alur analisis.
- Notebook disusun berurutan mengikuti tahap analisis; setiap code cell didahului markdown singkat tentang tujuan cell dan diikuti markdown ringkas yang menjelaskan hasil aktualnya.
- Python digunakan untuk pekerjaan komputasional yang sesuai, seperti membaca dan menyaring data, menghitung frekuensi, serta menampilkan kutipan dan rujukan.
- Preprocessing ringan pada salinan teks analisis untuk merapikan spasi/baris baru dan menyiapkan pencarian kata tanpa mengubah teks sumber atau menghilangkan konteks.
- Pemilihan kandidat, pembacaan konteks, evaluasi keluaran AI, keputusan coding, dan sintesis interpretasi tetap ditampilkan sebagai langkah analisis peneliti di notebook; tidak seluruh pipeline disembunyikan dalam kode.
- Analisis frekuensi kata dan penemuan kandidat frasa pada teks terjemahan bahasa Indonesia.
- Penyertaan rujukan surah:ayat dan kutipan terjemahan untuk kandidat yang dikirim ke AI.
- Penyiapan dan pengiriman prompt untuk satu keyword atau phrase terpilih beserta seluruh konteks ayat yang cocok ke API LLM.
- Pencatatan prompt, respons AI, platform/model, tanggal, dan keputusan peneliti.
- Usulan makna kontekstual, open code, hubungan/kategori axial, core category, dan tema.
- Validasi manusia, pencatatan prompt dan respons AI, serta dokumentasi alasan menerima, mengubah, atau menolak usulan.
- Penyimpanan hasil analisis dalam tabel yang dapat ditinjau dan digunakan dalam laporan.

### Tidak termasuk

- Ayat di luar Juz 1.
- Dataset hadis atau perbandingan Qur’an dan hadis.
- Penetapan tafsir otoritatif atau fatwa.
- Pengiriman otomatis ke LLM melalui API.
- Penggantian penilaian peneliti dengan hasil AI atau skor frekuensi.

## 6. Baseline Cakupan Data

- **Metode identifikasi:** meninjau struktur dan isi CSV, jumlah record, nomor surah, serta rentang nomor ayat.
- **Jumlah record dataset sumber:** 6.236 ayat.
- **Record dalam cakupan proyek:** 148 ayat.
- **Rincian cakupan:** Al-Fātiḥah (surah 1), ayat 1–7; Al-Baqarah (surah 2), ayat 1–141.
- **Kolom sumber yang relevan:** `nomor_surat`, `nama_surat`, `nomor_ayat`, `teks_arab`, `teks_terjemahan`.
- **Bahasa utama analisis tekstual:** terjemahan bahasa Indonesia.
- **Peran teks Arab:** disertakan sebagai data rujukan; analisis frekuensi utama tidak mengasumsikan normalisasi morfologi Arab.

Penyaringan harus menggunakan nomor surah dan ayat, bukan pencarian nama surah saja. Nomor ayat harus dipertahankan di seluruh keluaran.

## 7. Alur Pengguna dan Pengujian

### Skenario 1 — Menyiapkan data Juz 1 (P1)

Sebagai peneliti, saya ingin menyaring dataset ke Juz 1 agar setiap tahap analisis memakai cakupan yang sama.

**Alasan prioritas:** cakupan data yang benar merupakan prasyarat untuk seluruh analisis.

**Uji mandiri:** membandingkan hasil saringan dengan rentang ayat yang ditetapkan dan memastikan jumlahnya 148.

**Acceptance scenarios**

1. **Given** dataset sumber tersedia, **When** pengguna memilih cakupan Juz 1, **Then** hasil hanya berisi Al-Fātiḥah 1–7 dan Al-Baqarah 1–141.
2. **Given** hasil saringan dibuat, **When** jumlah dan batas ayat diperiksa, **Then** terdapat 148 record unik dengan rujukan surah dan ayat yang benar.

### Skenario 2 — Meninjau kandidat keyword dan phrase (P1)

Sebagai peneliti, saya ingin melihat kandidat kata/frasa beserta frekuensi, kutipan, dan rujukan ayat agar saya dapat memilih masukan AI yang kontekstual.

**Alasan prioritas:** tahap ini menyediakan bahan untuk semua tahap coding berikutnya.

**Uji mandiri:** memeriksa kandidat terpilih terhadap kutipan dan hitungan yang dapat ditelusuri pada hasil saringan Juz 1.

**Acceptance scenarios**

1. **Given** teks terjemahan Juz 1, **When** analisis kandidat dijalankan, **Then** daftar kata/frasa menampilkan frekuensi dan tidak menghilangkan rujukan ayat.
2. **Given** kandidat muncul pada lebih dari satu ayat, **When** kandidat disiapkan untuk ditinjau, **Then** rujukan serta contoh konteksnya dapat dibedakan dan diperiksa.
3. **Given** daftar kandidat memiliki kata umum, **When** kandidat ditinjau, **Then** pengguna dapat mengecualikan kata umum dan mencatat aturan pemilihannya tanpa menganggap frekuensi sebagai makna.

### Skenario 3 — Meminta dan meninjau bantuan AI (P1)

Sebagai peneliti, saya ingin memberikan kandidat beserta konteks ayat kepada AI dan menilai usulannya agar bantuan AI tetap terbatas pada bukti yang diberikan.

**Alasan prioritas:** proyek secara khusus mengevaluasi kemampuan AI dalam analisis makna dan coding.

**Uji mandiri:** memeriksa bahwa setiap usulan makna memiliki kandidat, kutipan, dan rujukan yang menjadi dasar evaluasi manusia.

**Acceptance scenarios**

1. **Given** pengguna telah memilih satu kandidat, **When** prompt dikirim ke API LLM, **Then** prompt mencakup fokus analisis, frekuensi, seluruh teks ayat yang cocok, dan rujukan ayat.
2. **Given** AI mengusulkan makna atau kode, **When** peneliti meninjau jawaban, **Then** hasil dapat diterima, diubah, atau ditolak dengan alasan dan status yang tercatat.
3. **Given** kutipan tidak cukup mendukung suatu kesimpulan, **When** AI atau peneliti menemukan keterbatasan konteks, **Then** keterbatasan tersebut dicatat dan tidak disajikan sebagai kesimpulan yang pasti.
4. **Given** prompt dan konteks dikirim ke API LLM, **When** respons diterima, **Then** notebook mencatat respons dan pengguna dapat meninjau serta memisahkannya dari keputusan peneliti.

### Skenario 4 — Menyusun kategori dan tema yang terlacak (P2)

Sebagai peneliti, saya ingin menghubungkan open code menjadi kategori, konsep inti, dan tema sambil mempertahankan jejak ke ayat sumber.

**Alasan prioritas:** sintesis diperlukan untuk interpretasi, setelah data kandidat dan kode awal diverifikasi.

**Uji mandiri:** memilih sebuah tema akhir dan menelusuri kembali kategori, kode, kutipan, serta rujukan ayat pendukungnya.

**Acceptance scenarios**

1. **Given** open code telah ditinjau, **When** axial coding dilakukan, **Then** setiap kategori mencantumkan kode yang dikelompokkan serta alasan hubungan.
2. **Given** beberapa kategori tersedia, **When** core category dan tema dirumuskan, **Then** keduanya disertai bukti dan hubungan dengan fokus analisis.
3. **Given** sebuah kategori atau tema tidak mempunyai dukungan ayat yang dapat ditelusuri, **When** hasil akhir ditinjau, **Then** hasil tersebut diperbaiki, diberi batasan, atau tidak dimasukkan.

### Skenario 5 — Mengevaluasi penggunaan AI (P2)

Sebagai peneliti, saya ingin mencatat kualitas usulan AI agar laporan membedakan kemampuan AI dari keputusan saya sendiri.

**Alasan prioritas:** evaluasi AI merupakan bagian dari tujuan praktikum dan meningkatkan transparansi.

**Uji mandiri:** meninjau catatan prompt, jawaban, keputusan manusia, dan alasan untuk sampel yang dianalisis.

**Acceptance scenarios**

1. **Given** AI telah memberi jawaban, **When** jawaban dievaluasi, **Then** catatan memuat penilaian relevansi konteks, dukungan bukti, dan ketepatan usulan.
2. **Given** usulan AI diterima, diubah, atau ditolak, **When** catatan analisis dibaca, **Then** alasan keputusan manusia dapat dibedakan dari keluaran AI.

### Kasus tepi

- Jika ayat hilang, kosong, atau rujukannya tidak valid, record tidak boleh diam-diam dianggap sah; masalah harus ditandai dan dilaporkan.
- Jika suatu kandidat tidak ditemukan secara literal, hasil frekuensinya harus dibedakan dari kemungkinan sinonim atau ungkapan dengan makna serupa.
- Jika kandidat muncul terlalu banyak untuk dimasukkan seluruhnya ke prompt, pemilihan kutipan harus dicatat dan tidak boleh mengubah frekuensi total yang dihitung.
- Jika AI memberi jawaban tanpa dukungan kutipan, jawaban ditandai untuk verifikasi dan tidak otomatis masuk ke coding final.
- Jika ejaan atau istilah terjemahan berbeda, pencarian literal dan interpretasi berdasarkan sinonim harus dibedakan.
- Jika keluaran AI tidak tersedia atau tidak konsisten, analisis tetap dapat dilanjutkan dengan peninjauan manusia dan keterbatasannya dicatat.

## 8. Persyaratan

### Persyaratan fungsional

- **REQ-001:** Proyek MUST membatasi analisis pada Al-Fātiḥah ayat 1–7 dan Al-Baqarah ayat 1–141.
- **REQ-002:** Proyek MUST mempertahankan nomor surah, nama surah, nomor ayat, teks Arab, dan terjemahan yang tersedia untuk record dalam cakupan.
- **REQ-003:** Proyek MUST menghasilkan daftar kandidat keyword dan phrase beserta frekuensi kemunculan pada teks terjemahan dalam cakupan.
- **REQ-004:** Proyek MUST menyediakan kutipan terjemahan dan rujukan surah:ayat untuk kandidat yang digunakan dalam analisis atau prompt AI.
- **REQ-005:** Proyek MUST membedakan temuan frekuensi komputasional dari penilaian makna dan kepentingan interpretatif.
- **REQ-006:** Proyek MUST memungkinkan pengguna menyaring kandidat yang tidak relevan dan mencatat dasar pemilihan kandidat.
- **REQ-007:** Prompt AI MUST meminta makna kandidat berdasarkan konteks kutipan, bukti/rujukan, kaitan dengan fokus analisis, dan pengakuan atas keterbatasan konteks.
- **REQ-008:** Notebook MUST mengirim prompt dan konteks kandidat ke API OpenAI-compatible untuk setiap tahap LLM. Base URL dan model MUST dapat dikonfigurasi melalui environment variable; API key MUST dibaca dari `OPENAI_API_KEY` dan MUST NOT disimpan di notebook atau dikomit ke repositori.
- **REQ-009:** Proyek MUST menyediakan ruang pencatatan untuk usulan makna, open code, alasan, kategori axial, core category, tema, dan rujukan ayat pendukung.
- **REQ-010:** Pengguna MUST dapat menerima, mengubah, atau menolak setiap usulan AI, serta mencatat keputusan dan alasannya.
- **REQ-011:** Setiap kategori, konsep inti, dan tema final MUST dapat ditelusuri ke kode dan ayat yang mendukungnya.
- **REQ-012:** Laporan MUST memisahkan hasil komputasi, saran AI, keputusan peneliti, dan interpretasi akhir.
- **REQ-013:** Proyek MUST mendokumentasikan prompt dan respons AI yang digunakan dalam analisis beserta model/platform dan tanggal penggunaan jika diketahui.
- **REQ-014:** Proyek MUST menyediakan evaluasi kualitatif kemampuan AI yang membahas kesesuaian konteks, dukungan bukti, kegunaan usulan, dan keterbatasannya.
- **REQ-015:** Hasil kerja analisis MUST disajikan dalam satu Jupyter Notebook (`.ipynb`) yang mengikuti urutan tahap proyek.
- **REQ-016:** Setiap code cell dalam notebook MUST didahului markdown singkat yang menyatakan apa yang dikerjakan dan diikuti markdown yang menjelaskan keluaran aktualnya secara langsung.
- **REQ-017:** Python MUST digunakan untuk pengolahan dan perhitungan yang dapat diulang, sedangkan pemilihan kandidat dan interpretasi coding MUST tetap tampak sebagai keputusan analitis peneliti, bukan hanya hasil fungsi otomatis.
- **REQ-018:** Notebook MUST menampilkan tabel atau keluaran yang cukup untuk meninjau data, kutipan, rujukan, dan hasil tiap tahap tanpa harus membaca kode untuk memahami kesimpulannya.
- **REQ-019:** Teks markdown notebook MUST ringkas, jelas, dan spesifik pada proses atau hasil yang ditampilkan; hindari penjelasan generik dan pengulangan yang tidak membantu analisis.
- **REQ-020:** Preprocessing MUST hanya merapikan spasi/baris baru dan menyiapkan teks pencarian pada salinan analisis; teks sumber dan kutipan ayat MUST tetap utuh, kata negasi seperti “tidak” dan “jangan” MUST dipertahankan, dan stemming atau normalisasi agresif MUST NOT diterapkan.

### Persyaratan non-fungsional dan kualitas

- **NFR-001:** Analisis MUST dapat diulang pada dataset sumber yang sama dengan aturan penyaringan dan pemrosesan yang terdokumentasi.
- **NFR-002:** Setiap hasil yang masuk ke coding final MUST memiliki rujukan sumber yang dapat diperiksa.
- **NFR-003:** Kesalahan pembacaan dataset, penyaringan, atau data MUST disampaikan secara eksplisit dan tidak boleh menghasilkan keluaran yang tampak berhasil secara keliru.
- **NFR-004:** Bahasa laporan MUST membedakan deskripsi data dan interpretasi, serta tidak menyajikan keluaran AI sebagai tafsir otoritatif.
- **NFR-005:** Prompt AI hanya boleh memuat kandidat terpilih, frekuensi, rujukan, dan konteks ayat yang diperlukan; jangan menambahkan data pribadi yang tidak relevan.
- **NFR-006:** Notebook MUST dapat dibaca berurutan oleh pembaca yang tidak perlu mengeksekusi kode untuk memahami tujuan dan makna keluaran setiap tahap.
- **NFR-007:** Setiap transformasi preprocessing MUST terdokumentasi, tidak mengubah kolom sumber, dan mempertahankan teks ayat serta rujukannya untuk interpretasi.
- **NFR-008:** Kegagalan request API MUST ditampilkan secara eksplisit; prompt dan respons beserta model dan waktu penggunaan MUST dicatat. Pengiriman API dapat menimbulkan biaya dan keluaran AI MUST tetap ditinjau peneliti.

## 9. Kriteria Keberhasilan

- **SC-001:** Hasil penyaringan memuat tepat 148 ayat: 7 ayat dari Al-Fātiḥah dan 141 ayat dari Al-Baqarah.
- **SC-002:** 100% kandidat yang masuk ke tabel analisis memiliki jenis kandidat, frekuensi, kutipan konteks, dan rujukan surah:ayat.
- **SC-003:** 100% kode final, kategori, konsep inti, dan tema memiliki jejak yang dapat ditelusuri ke ayat sumber.
- **SC-004:** 100% saran AI yang digunakan dalam hasil akhir memiliki status tinjauan manusia (diterima, diubah, atau ditolak); tidak ada saran AI yang langsung dianggap sebagai hasil final.
- **SC-005:** Laporan akhir menjelaskan aturan pemilihan kandidat, batasan frekuensi, prompt yang dipakai, serta contoh usulan AI yang diterima dan/atau dikoreksi.
- **SC-006:** Peneliti dapat meninjau kembali satu tema akhir dan menemukan bukti ayat serta alasan coding yang mendasarinya tanpa kehilangan rujukan.
- **SC-007:** Semua code cell analisis dalam notebook memiliki markdown singkat sebelum cell dan penjelasan hasil setelah cell.
- **SC-008:** Pembaca dapat mengikuti urutan dari penyaringan data hingga tema akhir dan membedakan hasil Python, usulan AI, serta keputusan peneliti tanpa menafsirkan kode program.
- **SC-009:** Hasil preprocessing mempertahankan seluruh record dan rujukan ayat, tidak menghapus kata negasi, serta menyimpan teks sumber tanpa perubahan.
- **SC-010:** Notebook mengirim prompt lengkap satu kandidat dengan frekuensi dan semua konteks ayat yang cocok ke API LLM, lalu mencatat respons dan hasil tinjauan peneliti.

## 10. Entitas Data

- **Ayat:** record sumber yang memuat nomor surah, nama surah, nomor ayat, teks Arab, dan terjemahan.
- **Kandidat keyword/phrase:** unit kata atau frasa yang ditemukan, jenis, frekuensi, serta daftar ayat dan kutipan pendukung.
- **Usulan AI:** hasil AI untuk makna atau coding kandidat, dilengkapi konteks prompt dan rujukan yang diberikan.
- **Keputusan peneliti:** status terima/ubah/tolak, hasil revisi jika ada, alasan keputusan, dan catatan evaluasi.
- **Open code:** label fenomena spesifik yang dikaitkan dengan satu atau lebih kandidat dan ayat.
- **Axial category:** kelompok open code yang memiliki hubungan konseptual, disertai penjelasan hubungan.
- **Core category:** konsep inti yang menjelaskan keterkaitan beberapa kategori.
- **Theme:** pola makna yang lebih luas dan didukung kategori, konsep inti, serta bukti ayat.
- **Catatan prompt/AI:** prompt, respons, platform/model dan tanggal penggunaan apabila diketahui.

## 11. Asumsi, Batasan, dan Risiko

### Asumsi

- Dataset yang tersedia adalah sumber data kerja untuk proyek dan kolom `teks_terjemahan` digunakan sebagai korpus utama berbahasa Indonesia.
- Pengguna menjalankan analisis untuk tujuan akademik dan meninjau sendiri seluruh interpretasi.
- AI diakses melalui API OpenAI-compatible; base URL dan model dapat dikonfigurasi, sedangkan API key disimpan di environment variable.
- Fokus kerja yang dipilih adalah eksplorasi kandidat kata/frasa di seluruh Juz 1. Tema akhir tidak ditetapkan sebelumnya dan harus didukung bukti konteks, bukan frekuensi saja.

### Batasan dan risiko

- Dataset sumber berisi seluruh 6.236 ayat, sedangkan proyek hanya menggunakan 148 ayat Juz 1; semua analisis harus menerapkan filter cakupan.
- Dataset tidak memuat hadis, sehingga hasil dan klaim proyek hanya berlaku untuk data Qur’an yang dianalisis.
- Metadata sumber, edisi, penerjemah, dan lisensi terjemahan belum terkonfirmasi dari kolom dataset yang diperiksa. Identitas dan izin penggunaan perlu diverifikasi sebelum publikasi.
- Terjemahan merupakan representasi makna dalam bahasa Indonesia dan dapat memengaruhi frekuensi istilah. Analisis literal tidak mencakup seluruh sinonim, bentuk turunan, atau nuansa teks Arab.
- Frekuensi, kemiripan teks, dan usulan LLM dapat mengabaikan konteks, menghasilkan asosiasi yang keliru, atau menyederhanakan makna. Verifikasi peneliti wajib dilakukan.
- Prompt dan konteks ayat dikirim ke penyedia API pihak ketiga; pengguna perlu meninjau kebijakan layanan terkait data dan biaya. Request yang gagal harus terlihat jelas. Prompt, respons, model, dan waktu penggunaan perlu disimpan agar evaluasi dapat dipahami.

## 12. Pertanyaan Terbuka

- Apakah tugas mata kuliah memerlukan pertanyaan penelitian yang lebih sempit daripada fokus eksploratif kandidat kata/frasa di seluruh Juz 1?
- Apa sumber, edisi, dan penerjemah yang digunakan untuk kolom terjemahan dataset?
- Format akhir tabel dan laporan akan ditetapkan pada tahap perencanaan implementasi berdasarkan kebutuhan mata kuliah.

## 13. Kesiapan untuk Tahap Berikutnya

PRD ini menjadi dasar implementasi workflow notebook dengan API OpenAI-compatible dan tinjauan AI oleh peneliti. Batas data Juz 1 dan fokus eksploratif telah ditetapkan; identitas sumber terjemahan tetap perlu dikonfirmasi sebelum interpretasi final atau publikasi.

## Requirements Quality Checklist

### Cakupan dan ketertelusuran

- [x] Cakupan Juz 1 dinyatakan secara eksplisit.
- [x] Setiap persyaratan memiliki ID REQ-XXX atau NFR-XXX yang unik.
- [x] Kebutuhan API LLM, pengamanan kredensial, verifikasi manusia, dan rujukan ayat tercakup.
- [x] Format notebook dan pembagian peran Python dengan analisis peneliti dinyatakan.

### Keterujian

- [x] Skenario pengguna memiliki prioritas dan acceptance scenarios.
- [x] Persyaratan fungsional dapat diperiksa terhadap keluaran analisis.
- [x] Kriteria keberhasilan terukur dan dapat diverifikasi.

### Kelengkapan dan batasan

- [x] Tujuan, ruang lingkup, luar cakupan, asumsi, risiko, serta entitas data dijelaskan.
- [x] Dataset hadis secara eksplisit berada di luar cakupan.
- [x] Risiko sumber/edisi terjemahan dan keterbatasan AI dinyatakan.
- [ ] Fokus penelitian spesifik dikonfirmasi.
- [ ] Metadata sumber dan edisi terjemahan dikonfirmasi.
