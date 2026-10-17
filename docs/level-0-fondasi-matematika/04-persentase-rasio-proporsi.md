# Part 4 — Persentase, Rasio, dan Proporsi

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan arti persentase dan mengubahnya ke desimal dan pecahan (dan sebaliknya);
- menghitung persentase dari suatu nilai, mencari persen, dan menghitung perubahan persentase;
- membedakan persen dan poin persentase;
- menjelaskan dan menyederhanakan rasio;
- menyelesaikan masalah perbandingan senilai dengan proporsi;
- menerapkan semuanya pada data, misalnya distribusi kelas dan pembagian data latih dan uji.

## Prasyarat

[Part 3 — Pecahan dan Desimal](03-pecahan-dan-desimal.md), terutama pecahan senilai, penyederhanaan, dan konversi pecahan-desimal.

## Mengapa Materi Ini Penting?

Persentase dan rasio adalah bahasa sehari-hari untuk membandingkan besaran: "akurasi 92%", "data latih dan uji 80:20", "kelas positif hanya 5% dari data". Tanpa memahaminya, laporan hasil model mudah disalahartikan. Materi ini juga melatih berpikir proporsional yang dipakai di banyak rumus.

## Penjelasan Konsep dari Dasar

### 1. Pengertian Persentase

**Persentase** (*percentage*) adalah pecahan yang penyebutnya 100. Kata *persen* berarti "per seratus", dan lambangnya $\%$.

$$
25\% = \frac{25}{100}
$$

Artinya: jika keseluruhan dibagi menjadi 100 bagian sama besar, kita mengambil 25 bagian. Persentase membuat perbandingan mudah karena semua dinyatakan dengan penyebut yang sama. Misalnya, $\frac{3}{4}$ dan $\frac{18}{25}$ sulit dibandingkan sekilas, tetapi $75\%$ dan $72\%$ langsung jelas.

Persentase bisa lebih dari 100%. $150\%$ berarti satu setengah kali ukuran awal. Persentase juga bisa kurang dari 1%, misalnya $0{,}5\%$ (setengah per seratus).

### 2. Mengubah Persentase Menjadi Desimal dan Pecahan

Persen menjadi desimal: **bagi dengan 100** (geser koma dua tempat ke kiri).

| Persen | Desimal | Pecahan sederhana |
|---|---|---|
| $25\%$ | $0{,}25$ | $\frac{1}{4}$ |
| $5\%$ | $0{,}05$ | $\frac{1}{20}$ |
| $150\%$ | $1{,}5$ | $\frac{3}{2}$ |
| $0{,}5\%$ | $0{,}005$ | $\frac{1}{200}$ |

Sebaliknya, desimal atau pecahan menjadi persen: **kali 100**.

- $0{,}4 \to 0{,}4 \times 100 = 40\%$.
- $\frac{3}{8} = 0{,}375 \to 37{,}5\%$.

Alasannya: $0{,}4$ berarti 4 persepuluhan, yaitu $\frac{40}{100}$, atau 40 per seratus.

### 3. Menghitung Persentase dari Suatu Nilai

Ada tiga pertanyaan yang sering muncul, dan semuanya memakai satu hubungan:

$$
\text{bagian} = \frac{p}{100} \times \text{keseluruhan}
$$

**Mencari bagian.** Berapa $15\%$ dari $240$? Ubah $15\%$ menjadi $0{,}15$, lalu $0{,}15 \times 240 = 36$.

**Mencari persen.** $18$ adalah berapa persen dari $60$? Bagi bagian dengan keseluruhan, lalu kali 100: $\frac{18}{60} = 0{,}3 = 30\%$.

**Mencari keseluruhan.** Jika $20\%$ dari suatu bilangan adalah $30$, berapa bilangannya? Bagian $= 0{,}2 \times$ keseluruhan, maka keseluruhan $= 30 \div 0{,}2 = 150$.

### 4. Menghitung Perubahan Persentase

**Perubahan persentase** mengukur seberapa besar kenaikan atau penurunan dibandingkan nilai **awal**:

$$
\text{perubahan} = \frac{\text{nilai baru} - \text{nilai lama}}{\text{nilai lama}} \times 100\%
$$

Hasil positif berarti kenaikan, negatif berarti penurunan.

- $80 \to 100$: $\frac{100 - 80}{80} \times 100\% = \frac{20}{80} \times 100\% = 25\%$ (naik).
- $100 \to 80$: $\frac{80 - 100}{100} \times 100\% = -20\%$ (turun).

!!! warning "Naik 25% lalu turun 25% tidak kembali ke awal"
    Penurunan dari 100 ke 80 hanya 20%, bukan 25%, karena nilai awalnya berbeda (100, bukan 80). Selalu bandingkan dengan nilai **awal** dari perubahan itu.

**Poin persentase.** Jika akurasi model naik dari $80\%$ menjadi $85\%$, selisihnya **5 poin persentase** (*percentage points*), yaitu pengurangan langsung kedua persentase. Sedangkan kenaikan **relatif** adalah $\frac{85 - 80}{80} \times 100\% = 6{,}25\%$. Kedua pernyataan benar, tetapi berbeda artinya. Jangan menyebut keduanya "5%".

### 5. Pengertian Rasio

**Rasio** (*ratio*) membandingkan dua besaran (atau lebih) yang satuannya sama. Rasio $a$ banding $b$ ditulis $a : b$ dan dibaca "a banding b".

Jika dalam kelas ada 12 siswa laki-laki dan 18 siswa perempuan, rasio laki-laki banding perempuan adalah $12 : 18$.

Dua hal penting:

- **Urutan penting.** $12 : 18$ berbeda dengan $18 : 12$.
- **Satuan harus sama.** Untuk membandingkan 50 cm dan 2 m, ubah dulu menjadi $50 : 200$.

Rasio bisa ditulis sebagai pecahan, tetapi hati-hati dengan maknanya. Rasio $12 : 18$ membandingkan bagian dengan bagian. Dari 30 siswa, siswa laki-laki adalah $\frac{12}{30}$ dari **seluruh** kelas, bukan $\frac{12}{18}$.

### 6. Menyederhanakan Rasio

Seperti pecahan, rasio disederhanakan dengan membagi kedua suku dengan pembagi bersama (FPB). Nilai perbandingannya tidak berubah.

- $12 : 18$: FPB = 6, jadi $2 : 3$.
- Rasio desimal: $0{,}6 : 1{,}5$. Kalikan keduanya dengan 10 untuk menghilangkan koma: $6 : 15$. Lalu bagi dengan 3: $2 : 5$.
- Rasio pecahan: $\frac{1}{2} : \frac{3}{4}$. Kalikan keduanya dengan KPK penyebut (4): $2 : 3$.

### 7. Pengertian Proporsi

**Proporsi** (*proportion*) adalah pernyataan bahwa **dua rasio sama nilainya**:

$$
\frac{a}{b} = \frac{c}{d}
$$

Contoh: $\frac{2}{3} = \frac{4}{6}$. Keduanya sama karena $\frac{4}{6}$ jika disederhanakan menjadi $\frac{2}{3}$. Ini sebenarnya pecahan senilai dari Part 3 yang dipakai untuk membandingkan besaran.

### 8. Menyelesaikan Perbandingan Senilai

Pada **perbandingan senilai**, jika satu besaran menjadi dua kali lipat, besaran lainnya ikut menjadi dua kali lipat. Contoh: harga dan banyak barang yang sama jenisnya.

Soal: 3 kg beras harganya Rp45.000. Berapa harga 5 kg beras?

**Cara satuan.** Cari nilai untuk satu satuan, lalu kalikan.

1. Harga 1 kg: $45.000 \div 3 = 15.000$.
2. Harga 5 kg: $15.000 \times 5 = 75.000$.

**Cara proporsi.** Susun dua rasio yang sama:

$$
\frac{3}{45.000} = \frac{5}{x}
$$

Huruf $x$ mewakili harga yang kita cari. Dengan cara satuan di atas, $x = 75.000$. Periksa: rasio $\frac{3}{45.000} = \frac{1}{15.000}$ dan $\frac{5}{75.000} = \frac{1}{15.000}$. Keduanya sama.

!!! note "Catatan"
    Tidak semua hubungan senilai. Harga 3 kg beras proporsional dengan beratnya, tetapi tinggi badan seseorang tidak proporsional dengan umur. Pastikan hubungan memang berlipat bersama sebelum memakai proporsi.

### 9. Penerapan dalam Data dan Dataset

Contoh berikut hanyalah **ilustrasi matematis**. Angka dan rasio tidak berarti bahwa semua dataset harus seperti itu.

**Distribusi kelas.** Misalkan sebuah dataset berisi 1.000 data dengan 800 berlabel negatif dan 200 berlabel positif.

- Rasio negatif banding positif: $800 : 200 = 4 : 1$.
- Persentase positif: $\frac{200}{1.000} \times 100\% = 20\%$. Persentase negatif: $80\%$.

**Dampaknya pada akurasi.** Model yang selalu menebak "negatif" akan benar pada 800 dari 1.000 data, yaitu akurasi $80\%$, tetapi tidak pernah menemukan satu pun data positif. Maka akurasi tinggi tidak selalu berarti model bagus, apalagi bila kelasnya tidak seimbang.

**Pembagian data latih dan uji (*train-test split*).** Data sering dibagi menjadi data latih (untuk belajar) dan data uji (untuk menguji). Pembagian $80 : 20$ pada 1.250 data berarti:

- latih: $\frac{80}{100} \times 1.250 = 1.000$
- uji: $\frac{20}{100} \times 1.250 = 250$

Perbandingan $80 : 20$ hanya contoh; praktisi memilih rasio sesuai kebutuhan, misalnya $70 : 30$ atau $90 : 10$.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Persentase (*percentage*) | pecahan berpenyebut 100 | $25\% = \frac{25}{100}$ |
| Persen | satu per seratus | $1\% = 0{,}01$ |
| Perubahan persentase | selisih relatif terhadap nilai awal | $80 \to 100$: $+25\%$ |
| Poin persentase | selisih langsung dua persentase | $80\% \to 85\%$: 5 poin |
| Rasio (*ratio*) | perbandingan dua besaran | $12 : 18$ |
| Proporsi (*proportion*) | kesamaan dua rasio | $\frac{2}{3} = \frac{4}{6}$ |
| Perbandingan senilai | satu besaran naik, besaran lain ikut naik berlipat sama | harga dan jumlah barang |
| Data latih (*training set*) | data untuk belajar | 80% data |
| Data uji (*test set*) | data untuk menguji | 20% data |

## Rumus dan Penjelasan Setiap Simbol

**Bagian dari keseluruhan:**

$$
B = \frac{p}{100} \times K
$$

- $B$ adalah bagian yang dicari.
- $p$ adalah persen (bilangan sebelum tanda $\%$).
- $K$ adalah keseluruhan.
- Berlaku bila $p\%$ dihitung dari $K$. Contoh: $p = 15$, $K = 240$, maka $B = 0{,}15 \times 240 = 36$.

**Perubahan persentase:**

$$
\Delta\% = \frac{N_{\text{baru}} - N_{\text{lama}}}{N_{\text{lama}}} \times 100\%
$$

- $\Delta$ (huruf Yunani delta) berarti "perubahan".
- $N_{\text{baru}}$ dan $N_{\text{lama}}$ adalah nilai baru dan nilai lama. Tulisan kecil di bawah ($_{\text{baru}}$) disebut **subskrip** dan hanya berfungsi sebagai label.
- Berlaku bila $N_{\text{lama}} \neq 0$.

**Proporsi dan mencari suku yang hilang:**

$$
\frac{a}{b} = \frac{c}{d} \quad\Longrightarrow\quad d = \frac{b \times c}{a} \qquad (a \neq 0)
$$

Tanda $\Longrightarrow$ dibaca "maka". Rumus kanan diperoleh dengan mengalikan kedua sisi dengan $b \times d$ lalu membaginya dengan $a$. Pada contoh beras, rumus ini dipakai dalam bentuk lain: $x = \frac{45.000 \times 5}{3} = 75.000$.

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Tiga jenis soal persen

Dari 240 siswa, 15% menyukai matematika. Hitung banyak siswanya. Lalu, jika 18 dari 60 peserta lulus, berapa persen yang lulus?

1. Banyak siswa: $15\% = 0{,}15$ dan $0{,}15 \times 240 = 36$ siswa.
2. Persen lulus: $\frac{18}{60} = 0{,}3$, lalu $0{,}3 \times 100 = 30\%$.

Jawaban: 36 siswa; 30%.

### Contoh 2 — Kenaikan dan penurunan tidak simetris

Harga naik dari 80 menjadi 100, lalu turun lagi ke 80. Hitung kedua perubahan persentase.

1. Naik: $\frac{100 - 80}{80} = \frac{20}{80} = 0{,}25$, jadi $+25\%$.
2. Turun: $\frac{80 - 100}{100} = -0{,}2$, jadi $-20\%$.

Kenaikan $25\%$ diimbangi penurunan $20\%$, bukan $25\%$.

### Contoh 3 — Membagi menurut rasio

Dataset berisi 2.400 data dan perbandingan kelas positif banding negatif adalah $1 : 5$. Berapa data tiap kelas, dan berapa persen kelas positif?

1. Jumlah bagian: $1 + 5 = 6$.
2. Satu bagian: $2.400 \div 6 = 400$.
3. Positif: $1 \times 400 = 400$. Negatif: $5 \times 400 = 2.000$.
4. Persen positif: $\frac{400}{2.400} = \frac{1}{6} \approx 0{,}1667$, jadi sekitar $16{,}7\%$.

Perhatikan: rasio $1 : 5$ bukan berarti 1/5 dari seluruh data. Positif adalah $\frac{1}{6}$ dari total.

### Contoh 4 — Pembagian latih dan uji

Dari 1.250 data, berapa data latih dan uji untuk rasio $70 : 30$?

1. Latih: $0{,}70 \times 1.250 = 875$.
2. Uji: $0{,}30 \times 1.250 = 375$.
3. Periksa: $875 + 375 = 1.250$.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** $50\%$ dari 84 sama dengan setengah dari 84, yaitu 42.

**Sedang.** Resep memakai 2 gelas tepung untuk 12 kue. Berapa tepung untuk 30 kue? Per kue $\frac{2}{12} = \frac{1}{6}$ gelas. Untuk 30 kue: $30 \times \frac{1}{6} = 5$ gelas.

**Sedang.** Diskon 20% pada barang seharga Rp250.000. Potongan: $0{,}2 \times 250.000 = 50.000$. Harga akhir: $250.000 - 50.000 = 200.000$. Cara cepat: membayar 80% dari harga, yaitu $0{,}8 \times 250.000 = 200.000$.

**Menantang.** Akurasi turun dari $90\%$ ke $81\%$. Selisih poin: $90 - 81 = 9$ poin persentase. Penurunan relatif: $\frac{81 - 90}{90} = -0{,}1$, yaitu $-10\%$.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menganggap $15\% = 0{,}15\%$ atau $1{,}5$ | pergeseran koma salah | $15\% = 0{,}15$ (bagi 100) |
| Naik 25% lalu turun 25% kembali ke awal | basis berbeda | $100 \to 125 \to 93{,}75$ |
| Menyebut 80% ke 85% "naik 5%" | itu 5 poin persentase | relatif $6{,}25\%$ |
| Rasio $1 : 5$ ditulis $\frac{1}{5}$ dari total | rasio bagian-banding-bagian | positif $= \frac{1}{6}$ dari total |
| Membandingkan rasio dengan satuan berbeda | 50 cm : 2 m bukan 50 : 2 | ubah satuan dulu: $50 : 200$ |
| Memakai proporsi untuk hubungan yang tidak berlipat | tidak semua hubungan senilai | cek dulu apakah berlipat bersama |
| Menukar urutan rasio | $a : b \neq b : a$ | perhatikan urutan di soal |
| Menjumlah persen dari basis berbeda | persen dari basis berbeda tidak bisa dijumlah | hitung nilai asli dulu |

## Hubungan dengan Materi Berikutnya

- **Part 5** memakai perkalian berulang yang terkait dengan pertumbuhan persentase (misalnya bunga majemuk).
- **Part 7** memakai proporsi sebagai contoh persamaan sederhana.
- **Level 3** memakai persentase untuk peluang dan frekuensi relatif.
- **Level 4** memakai rasio pembagian data latih dan uji pada evaluasi model.

## Hubungan dengan Machine Learning

- **Akurasi** adalah persentase prediksi yang benar: $\frac{\text{jumlah benar}}{\text{jumlah semua}} \times 100\%$.
- **Kelas tidak seimbang** (imbalanced) terlihat dari rasio kelas. Ingat contoh akurasi 80% di atas.
- **Pembagian data latih dan uji** dinyatakan sebagai rasio atau persen, misalnya `test_size=0.2`.
- **Perubahan relatif dan poin persentase** dipakai saat melaporkan peningkatan model; pilih istilah yang tepat agar tidak menyesatkan.

### Python: menghitung persentase dan pembagian data

```python
total = 1000
positif = 200
negatif = total - positif

print(positif / total * 100)   # 20.0, persen positif
print(negatif / total * 100)   # 80.0, persen negatif
```

`positif / total` menghasilkan pecahan dalam bentuk desimal (0.2), lalu dikali 100 menjadi persen.

Perubahan persentase:

```python
lama = 80
baru = 100
perubahan = (baru - lama) / lama * 100
print(perubahan)   # 25.0
```

Pembagian latih dan uji. Fungsi `round` dipakai agar hasilnya bilangan bulat yang bersih dari galat desimal:

```python
n = 1250
n_latih = round(n * 0.70)
n_uji = n - n_latih
print(n_latih, n_uji)   # 875 375
```

Baris `n_uji = n - n_latih` memastikan jumlah keduanya tepat sama dengan `n`, meskipun hasil pembulatan berbeda dari harapan.

## Latihan Mandiri

1. Ubah ke desimal dan pecahan sederhana: (a) $35\%$ (b) $125\%$ (c) $0{,}8\%$
2. Ubah ke persen: (a) $0{,}07$ (b) $\frac{3}{5}$ (c) $\frac{7}{8}$
3. Hitung $12\%$ dari $350$.
4. $45$ adalah berapa persen dari $180$?
5. Harga naik dari Rp200.000 menjadi Rp230.000. Berapa persen kenaikannya? Jika harga kemudian turun lagi ke Rp200.000, berapa persen penurunannya (bulatkan dua desimal)?
6. Akurasi model naik dari $82\%$ menjadi $91\%$. Berapa poin persentase kenaikannya, dan berapa persen kenaikan relatifnya (bulatkan dua desimal)?
7. Sederhanakan: (a) $24 : 36$ (b) $0{,}6 : 1{,}5$
8. Resep memakai 2 gelas tepung untuk 12 kue. Berapa gelas untuk 30 kue?
9. Dataset 2.400 data dengan rasio kelas positif banding negatif $1 : 5$. Berapa data tiap kelas, dan berapa persen kelas positif (bulatkan satu desimal)?
10. Pembagian latih dan uji untuk 1.250 data: berapa banyak masing-masing untuk (a) $80 : 20$ dan (b) $70 : 30$?

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) $35\% = 0{,}35 = \frac{35}{100} = \frac{7}{20}$.
    (b) $125\% = 1{,}25 = \frac{125}{100} = \frac{5}{4}$.
    (c) $0{,}8\% = 0{,}008 = \frac{8}{1.000} = \frac{1}{125}$.

??? success "Soal 2"
    (a) $0{,}07 \times 100 = 7\%$.
    (b) $\frac{3}{5} = 0{,}6$, jadi $60\%$.
    (c) $\frac{7}{8} = 0{,}875$, jadi $87{,}5\%$.

??? success "Soal 3"
    $0{,}12 \times 350 = 42$.

??? success "Soal 4"
    $\frac{45}{180} = \frac{1}{4} = 0{,}25$, jadi $25\%$.

??? success "Soal 5"
    Kenaikan: $\frac{230.000 - 200.000}{200.000} = \frac{30.000}{200.000} = 0{,}15$, yaitu $15\%$.
    Penurunan: $\frac{200.000 - 230.000}{230.000} = \frac{-30.000}{230.000} \approx -0{,}1304$, yaitu penurunan sekitar $13{,}04\%$.

??? success "Soal 6"
    Poin persentase: $91 - 82 = 9$ poin. Kenaikan relatif: $\frac{9}{82} \approx 0{,}1098$, yaitu sekitar $10{,}98\%$.

??? success "Soal 7"
    (a) FPB = 12: $24 : 36 = 2 : 3$.
    (b) Kalikan 10: $6 : 15$. Bagi 3: $2 : 5$.

??? success "Soal 8"
    Per kue: $\frac{2}{12} = \frac{1}{6}$ gelas. Untuk 30 kue: $30 \times \frac{1}{6} = 5$ gelas. Periksa: 30 kue adalah $2{,}5$ kali 12 kue, dan $2{,}5 \times 2 = 5$.

??? success "Soal 9"
    Jumlah bagian: $1 + 5 = 6$. Satu bagian: $2.400 \div 6 = 400$. Positif **400**, negatif **2.000**. Persen positif: $\frac{400}{2.400} \approx 0{,}1667$, yaitu sekitar **16,7%**.

??? success "Soal 10"
    (a) $0{,}8 \times 1.250 = 1.000$ latih; $1.250 - 1.000 = 250$ uji.
    (b) $0{,}7 \times 1.250 = 875$ latih; $1.250 - 875 = 375$ uji.

## Ringkasan Materi

- Persentase adalah pecahan berpenyebut 100: $p\% = \frac{p}{100}$. Persen ke desimal: bagi 100; desimal ke persen: kali 100.
- Bagian $= \frac{p}{100} \times$ keseluruhan; persen $= \frac{\text{bagian}}{\text{keseluruhan}} \times 100\%$.
- Perubahan persentase dihitung terhadap nilai **awal**. Poin persentase adalah selisih langsung dua persentase.
- Rasio membandingkan besaran berurutan dan bersatuan sama, dan disederhanakan seperti pecahan.
- Proporsi menyatakan dua rasio sama; dipakai untuk perbandingan senilai.
- Rasio bagian-banding-bagian berbeda dengan bagian-dari-total.

## Checklist Penguasaan

- [ ] Mengubah persen, desimal, dan pecahan ke satu sama lain.
- [ ] Menghitung bagian, persen, dan keseluruhan dari informasi yang diberikan.
- [ ] Menghitung perubahan persentase dan menjelaskan mengapa tidak simetris.
- [ ] Membedakan persen dan poin persentase.
- [ ] Menyederhanakan rasio berbentuk bilangan bulat, desimal, dan pecahan.
- [ ] Membagi jumlah menurut rasio.
- [ ] Menyelesaikan perbandingan senilai dengan cara satuan dan proporsi.
- [ ] Menjelaskan mengapa akurasi tinggi tidak selalu berarti model bagus.
