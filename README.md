# Matematika untuk Machine Learning

Situs dokumentasi berbahasa Indonesia untuk mempelajari matematika yang dibutuhkan dalam **Machine Learning** dan **Deep Learning**. Dibangun dengan [MkDocs](https://www.mkdocs.org/) dan [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

> **Status:** tahap awal. Kerangka situs dan peta belajar sudah ada; **Level 0, 1, 2, 3, 4, dan 5 tersedia**.

Dokumen proyek: `PRD.md` (kebutuhan), `TECHNICAL_DESIGN.md` (desain teknis), `CLAUDE.md` (panduan kerja), `VALIDATION_REPORT.md` (hasil uji awal kerangka situs).

## Tujuan belajar

Setelah menyelesaikan seluruh materi, pembaca mampu memahami dan menurunkan sendiri matematika di balik algoritma Machine Learning klasik dan jaringan saraf tiruan (*neural network*).

## Target pembaca

Pemula yang ingin bergerak bertahap hingga tingkat lanjut.

## Prasyarat

Hanya kemampuan berhitung dasar. Level 0 memulai dari fondasi.

## Peta belajar

Rincian ada di `docs/curriculum/index.md`. Level 0 sampai Level 5 tersedia.

| Level | Judul | Topik representatif | Status |
|---|---|---|---|
| 0 | Fondasi Matematika | Sistem Bilangan, Operasi Aritmatika Dasar, Pecahan | Tersedia (8 part) |
| 1 | Aljabar Dasar dan Fungsi | Aljabar, Fungsi, Eksponen dan Logaritma | Tersedia (12 part) |
| 2 | Aljabar Linear dan Kalkulus | Vektor, Matriks, Turunan, Gradien | Tersedia (12 part) |
| 3 | Probabilitas dan Statistika | Statistik Deskriptif, Peluang, Aturan Bayes, Distribusi, Inferensi | Tersedia (12 part) |
| 4 | Matematika untuk Machine Learning Klasik | Regresi, KNN, Naive Bayes, Pohon, SVM, K-Means, PCA | Tersedia (13 part) |
| 5 | Matematika Lanjutan untuk Deep Learning | Tensor, Backpropagation, Attention, Transformer, BERT | Tersedia (15 part) |
| 5 | Matematika Lanjutan untuk Deep Learning | Backpropagation, Softmax, Adam, Attention | Direncanakan |

## Pendekatan belajar

Intuisi lebih dulu, lalu definisi formal; setiap istilah, simbol, dan rumus dijelaskan; setiap konsep disertai contoh hitungan dan kaitannya dengan Machine Learning.

## Teknologi

MkDocs 1.6, Material for MkDocs 9.x, Python Markdown Extensions, MathJax 3.2.2 (rumus, dimuat dari CDN `cdn.jsdelivr.net`, sehingga **rumus butuh koneksi internet**), dan Mermaid 11.17.2 (diagram, sudah disertakan di `docs/assets/vendor/mermaid/`). Daftar paket ada di `requirements.txt`.

## Instalasi dan pengembangan lokal

Windows (PowerShell):

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
mkdocs serve
```

macOS/Linux: `python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt && mkdocs serve`.

Buka <http://127.0.0.1:8000>. Diuji dengan Python 3.14; rentang versi dimaksudkan untuk Python 3.12+ (belum diuji).

## Build dan validasi

```powershell
mkdocs build --strict
```

Hasil ada di `site/` (diabaikan Git). `--strict` menggagalkan build jika ada tautan atau anchor yang rusak. Material 9.7 menampilkan pemberitahuan "MkDocs 2.0" pada setiap build; itu bukan peringatan proyek dan tidak menggagalkan `--strict`.

## Struktur proyek

```
mkdocs.yml                  konfigurasi situs, tema, nav, ekstensi
requirements.txt
docs/
  index.md                  beranda
  curriculum/index.md       peta belajar
  level-0-fondasi-matematika/  materi Level 0
  level-1-aljabar-dasar-dan-fungsi/  materi Level 1
  level-2-aljabar-linear-dan-kalkulus/  materi Level 2
  level-3-probabilitas-dan-statistika/  materi Level 3
  level-4-matematika-ml-klasik/  materi Level 4
  level-5-matematika-lanjutan-deep-learning/  materi Level 5
  stylesheets/extra.css     penyesuaian tampilan kecil
  javascripts/              konfigurasi MathJax dan Mermaid
  assets/vendor/mermaid/    Mermaid (versi terkunci, dengan LICENSE)
```

## Deploy ke GitHub Pages

`mkdocs gh-deploy --strict` membangun situs dan mendorongnya ke cabang `gh-pages`; atur sumber Pages ke cabang tersebut. `site_url` dan `repo_url` di `mkdocs.yml` masih menunjuk ke repositori `mkdocs-template` dan perlu disesuaikan bila repositori diganti. Belum pernah ada deploy dari repositori ini.

## Kontribusi

Tulis satu halaman atau bagian per perubahan, dalam bahasa Indonesia yang ramah pemula. Jelaskan istilah, simbol, dan rumus; sertakan contoh hitungan; pastikan perhitungan benar dan `mkdocs build --strict` bersih. Tambahkan halaman baru ke `nav:` di `mkdocs.yml`. Lihat `CLAUDE.md`.

## Lisensi

[MIT](LICENSE). Pustaka Mermaid di `docs/assets/vendor/mermaid/` juga berlisensi MIT dan menyertakan `LICENSE`-nya sendiri.
