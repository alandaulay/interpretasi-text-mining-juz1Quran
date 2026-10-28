# Analisis Text Mining dan Coding Interpretatif Juz 1

Notebook utama `coding_quran_mining.ipynb` mendemonstrasikan alur lima tahap untuk menganalisis terjemahan teks. Notebook eksplorasi sebelumnya berada di `notebooks/analisis_juz1.ipynb`.

1. Keyword dan phrase
2. Open coding
3. Axial coding
4. Selective coding
5. Thematic coding

Python menghitung frekuensi, TF-IDF, dan kemiripan kode serta menautkan kandidat ke contoh ayat. Gemini menyediakan usulan label dan kategori melalui API. Hasilnya bersifat eksploratif; model AI bukan sumber tafsir otoritatif.

## Persiapan

- Python 3.10 atau lebih baru
- Dataset yang boleh Anda gunakan dan bagikan, disimpan sebagai `data/raw/quran_dataset.csv`
- Kolom minimum dataset: `nomor_surat`, `nomor_ayat`, dan `teks_terjemahan`. Notebook juga memakai `nama_surat` untuk menampilkan metadata.
- Gemini API key yang valid

Dataset tidak disertakan dalam paket publik ini. Pastikan Anda memiliki izin untuk menggunakan dan mendistribusikan dataset sebelum menambahkannya ke repository.

## Menjalankan

1. Buat dan aktifkan virtual environment Python.
2. Instal dependensi:

   ```powershell
   python -m pip install -r requirements.txt
   ```

3. Salin `.env.example` sebagai `.env`, lalu isi `GEMINI_API_KEY` secara lokal. Jangan commit atau bagikan `.env`.
4. Letakkan dataset berlisensi Anda di `data/raw/quran_dataset.csv`.
5. Buka `coding_quran_mining.ipynb` di Jupyter atau VS Code dan jalankan cell secara berurutan.

Permintaan LLM dapat memakai kuota atau menimbulkan biaya. Cell API mengirim prompt dan contoh teks ke penyedia model. Jangan jalankan ulang semua cell jika tidak ingin mengirim permintaan baru.

## Keluaran

Hasil CSV disimpan ke `data/outputs/`. Notebook dan hasil LLM membantu eksplorasi, tetapi perlu diperiksa terhadap ayat dan tidak boleh diperlakukan sebagai tafsir final. Validasi oleh ahli tetap diperlukan untuk klaim keagamaan.

## Keamanan dan privasi

- Jangan pernah commit API key, `.env`, token, atau kredensial.
- Jangan mengunggah dataset atau materi kuliah berhak cipta tanpa izin.
- Notebook publik sebaiknya disimpan tanpa output cell karena output dapat memuat cuplikan dataset.
- Sebelum membagikan hasil, tinjau ulang lisensi dataset, keluaran, dan log prompt/respons.
