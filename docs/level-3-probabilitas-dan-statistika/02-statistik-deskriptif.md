# Part 2 — Statistik Deskriptif

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menghitung dan menafsirkan rata-rata (*mean*), median, dan modus, serta memilih yang cocok;
- menghitung nilai minimum, maksimum, dan rentang;
- menghitung kuartil, persentil, dan IQR, serta menyusun ringkasan lima angka;
- menghitung varians dan simpangan baku, dan membedakan rumus populasi ($N$) dari rumus sampel ($n-1$);
- menjelaskan mengapa rumus sampel memakai $n-1$;
- menghitung rata-rata dari tabel frekuensi dan rata-rata berbobot;
- menjelaskan akibat penambahan dan perkalian konstanta pada rata-rata dan simpangan baku;
- menghubungkan ringkasan ini dengan dataset, nilai pengganti data hilang, dan fungsi kerugian.

## Prasyarat

[Part 1 — Pengantar Statistika](01-pengantar-statistika.md) (populasi, sampel, parameter, statistik), [Level 0 — Part 8](../level-0-fondasi-matematika/08-notasi-matematika-dasar.md) (notasi $\sum$ dan rata-rata), dan [Level 0 — Part 5](../level-0-fondasi-matematika/05-pangkat-dan-akar.md) (kuadrat dan akar).

## Mengapa Materi Ini Penting?

Dataset dengan ribuan baris tidak bisa dibaca satu per satu. Kita perlu beberapa angka yang menggambarkan "di mana pusat data" dan "seberapa menyebar data". Dua pertanyaan inilah yang dijawab statistik deskriptif. Dalam Machine Learning, angka-angka ini dipakai untuk memeriksa dataset, mengisi data yang hilang, dan menyiapkan fitur sebelum model dilatih.

## Konsep Dasar dengan Bahasa Sederhana

Seluruh bagian ini memakai data contoh yang sama supaya perhitungannya mudah diikuti.

**Data A:** waktu belajar (jam per minggu) dari $n = 8$ mahasiswa, anggap sebagai sampel:

$$
12,\ 15,\ 11,\ 18,\ 15,\ 14,\ 20,\ 13
$$

Diurutkan dari kecil ke besar: $11,\ 12,\ 13,\ 14,\ 15,\ 15,\ 18,\ 20$.

### 1. Ukuran Pemusatan: Di Mana Pusat Data?

**Rata-rata (mean).** Jumlahkan semua nilai lalu bagi dengan banyak data:

$$
\bar{x} = \frac{1}{n}\sum_{i=1}^{n} x_i
$$

Untuk Data A: jumlah $= 118$, jadi $\bar{x} = \dfrac{118}{8} = 14{,}75$ jam.

**Median.** Nilai tengah setelah data diurutkan.

- Jika $n$ **ganjil**, median adalah nilai di posisi tengah.
- Jika $n$ **genap**, median adalah rata-rata dua nilai tengah.

Data A punya $n = 8$ (genap). Dua nilai tengah adalah posisi ke-4 dan ke-5: $14$ dan $15$. Median $= \dfrac{14 + 15}{2} = 14{,}5$ jam.

**Modus (*mode*).** Nilai yang paling sering muncul. Pada Data A, nilai $15$ muncul dua kali, yang lain sekali, jadi modus $= 15$. Sebuah data bisa tidak punya modus (semua nilai muncul sekali) atau punya lebih dari satu modus.

**Kapan memakai yang mana?**

| Ukuran | Kelebihan | Kekurangan | Cocok untuk |
|---|---|---|---|
| Rata-rata | memakai semua nilai | **sensitif terhadap pencilan** | data numerik yang simetris |
| Median | **tahan terhadap pencilan** | tidak memakai besar nilai ekstrem | data miring atau berpencilan |
| Modus | bisa untuk data kategorik | bisa tidak ada atau banyak | data nominal, nilai yang paling umum |

**Pencilan** (*outlier*) adalah nilai yang sangat jauh dari kebanyakan data. Contoh: gaji (juta rupiah) tujuh orang: $5, 6, 6, 7, 8, 9, 60$. Rata-ratanya $\dfrac{101}{7} \approx 14{,}43$, tetapi mediannya $7$. Pernyataan "rata-rata gaji 14,4 juta" menyesatkan karena enam dari tujuh orang bergaji di bawah 10 juta. Median $7$ lebih mewakili. Jika angka $60$ dibuang, rata-rata turun menjadi $\dfrac{41}{6} \approx 6{,}83$ dan median menjadi $6{,}5$: rata-rata berubah banyak, median berubah sedikit.

### 2. Minimum, Maksimum, dan Rentang

- **Minimum** adalah nilai terkecil dan **maksimum** nilai terbesar. Data A: $11$ dan $20$.
- **Rentang** (*range*) $= \text{maksimum} - \text{minimum}$. Data A: $20 - 11 = 9$.

Rentang mudah dihitung tetapi hanya memakai dua nilai ekstrem, sehingga sangat dipengaruhi pencilan.

### 3. Kuartil, Persentil, dan IQR

**Persentil** ke-$k$ adalah nilai yang kira-kira $k\%$ data berada di bawahnya. **Kuartil** membagi data menjadi empat bagian:

| Kuartil | Persentil | Arti |
|---|---|---|
| $Q_1$ | ke-25 | kira-kira 25% data berada di bawahnya |
| $Q_2$ | ke-50 | median |
| $Q_3$ | ke-75 | kira-kira 75% data berada di bawahnya |

**Cara menghitung (interpolasi linear).** Ada beberapa metode menghitung persentil, dan hasilnya bisa sedikit berbeda (lihat catatan di bawah). Buku ini memakai satu metode saja, yaitu metode interpolasi linear yang juga dipakai oleh NumPy (bawaan) dan fungsi `QUARTILE.INC` di Excel:

1. Urutkan data: $x_{(1)} \leq x_{(2)} \leq \dots \leq x_{(n)}$.
2. Untuk proporsi $p$ (misalnya $p = 0{,}25$ untuk $Q_1$), hitung **posisi** $r = 1 + (n-1)\,p$.
3. Ambil bagian bulat $\lfloor r \rfloor$ dan bagian pecahan $f = r - \lfloor r \rfloor$.
4. Nilainya: $x_{(\lfloor r \rfloor)} + f\,\bigl(x_{(\lfloor r \rfloor + 1)} - x_{(\lfloor r \rfloor)}\bigr)$.

Artinya: letakkan nilai di antara dua data bertetangga sesuai bagian pecahan posisinya.

**IQR** (*interquartile range*) $= Q_3 - Q_1$: lebar bagian tengah data yang memuat sekitar 50% observasi. IQR tahan terhadap pencilan.

**Ringkasan lima angka** (*five-number summary*): minimum, $Q_1$, median, $Q_3$, maksimum.

!!! note "Mengapa metode bisa berbeda?"
    Untuk Data A, metode interpolasi linear memberi $Q_1 = 12{,}75$ dan $Q_3 = 15{,}75$. Metode "median dari separuh bawah dan atas" memberi $12{,}5$ dan $16{,}5$. Fungsi `statistics.quantiles` bawaan Python (metode *exclusive*) memberi $12{,}25$ dan $17{,}25$. Tidak ada yang "salah"; metodenya saja berbeda, dan selisihnya mengecil pada data besar. Yang penting: **tulis metode yang dipakai** dan konsisten.

### 4. Ukuran Penyebaran: Seberapa Menyebar Data?

Dua kelompok bisa punya rata-rata sama tetapi sebaran sangat berbeda. Contoh: nilai $68, 70, 72$ dan nilai $40, 70, 100$ sama-sama memiliki rata-rata $70$, tetapi kelompok kedua jauh lebih menyebar.

**Deviasi** adalah selisih sebuah nilai dari rata-rata: $x_i - \bar{x}$. Untuk Data A ($\bar{x} = 14{,}75$):

| $x_i$ | 12 | 15 | 11 | 18 | 15 | 14 | 20 | 13 |
|---|---|---|---|---|---|---|---|---|
| $x_i - \bar{x}$ | $-2{,}75$ | $0{,}25$ | $-3{,}75$ | $3{,}25$ | $0{,}25$ | $-0{,}75$ | $5{,}25$ | $-1{,}75$ |

Jumlah semua deviasi selalu **nol** (di sini $0$), karena yang positif dan negatif saling meniadakan. Karena itu jumlah deviasi tidak bisa dipakai sebagai ukuran sebaran. Solusinya: **kuadratkan** dulu deviasinya supaya semuanya positif.

**Varians** adalah rata-rata kuadrat deviasi. **Simpangan baku** (*standard deviation*) adalah akar varians, sehingga satuannya sama dengan data.

Untuk **populasi** (semua anggota diamati, $N$ anggota):

$$
\sigma^2 = \frac{1}{N}\sum_{i=1}^{N}(x_i - \mu)^2
\qquad\qquad
\sigma = \sqrt{\sigma^2}
$$

Untuk **sampel** ($n$ anggota):

$$
s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})^2
\qquad\qquad
s = \sqrt{s^2}
$$

Untuk Data A (sebagai sampel): kuadrat deviasinya $7{,}5625,\ 0{,}0625,\ 14{,}0625,\ 10{,}5625,\ 0{,}0625,\ 0{,}5625,\ 27{,}5625,\ 3{,}0625$ dengan jumlah $63{,}5$. Maka $s^2 = \dfrac{63{,}5}{7} \approx 9{,}0714$ dan $s = \sqrt{9{,}0714} \approx 3{,}0119$ jam.

**Membaca hasilnya.** Simpangan baku $\approx 3{,}01$ berarti waktu belajar mahasiswa rata-rata menyimpang sekitar 3 jam dari rata-rata $14{,}75$ jam. Semakin besar $s$, semakin menyebar data.

### 5. Mengapa Rumus Sampel Memakai $n-1$?

Kita biasanya tidak tahu $\mu$, jadi menghitung deviasi dari $\bar{x}$. Padahal $\bar{x}$ adalah titik yang **paling dekat** ke data sampel itu sendiri. Ada identitas yang berlaku untuk bilangan $c$ apa pun:

$$
\sum_{i=1}^{n}(x_i - c)^2 = \sum_{i=1}^{n}(x_i - \bar{x})^2 + n\,(\bar{x} - c)^2
$$

Jika $c = \mu$, suku terakhir tidak negatif. Artinya jumlah kuadrat deviasi dari $\bar{x}$ **selalu lebih kecil atau sama** dibanding dari $\mu$. Jadi jika dibagi $n$, varians sampel cenderung **terlalu kecil**. Membaginya dengan $n-1$ mengoreksi kecenderungan itu.

**Bukti dengan angka.** Populasi kecil: $\{2, 4, 6\}$. Rata-ratanya $\mu = 4$, varians populasinya $\sigma^2 = \dfrac{4 + 0 + 4}{3} = \dfrac{8}{3} \approx 2{,}6667$. Ambil semua 9 sampel berukuran $n = 2$ (dengan pengembalian) dan hitung jumlah kuadrat deviasi dari rata-rata sampelnya:

| Sampel | $(2,2)$ | $(2,4)$ | $(2,6)$ | $(4,2)$ | $(4,4)$ | $(4,6)$ | $(6,2)$ | $(6,4)$ | $(6,6)$ |
|---|---|---|---|---|---|---|---|---|---|
| $\bar{x}$ | 2 | 3 | 4 | 3 | 4 | 5 | 4 | 5 | 6 |
| $\sum(x_i - \bar{x})^2$ | 0 | 2 | 8 | 2 | 0 | 2 | 8 | 2 | 0 |

Jumlah kolom terakhir $= 24$. Rata-ratanya terhadap 9 sampel: $\dfrac{24}{9} = \dfrac{8}{3}$.

- Bagi dengan $n = 2$: rata-rata varians sampel $= \dfrac{8/3}{2} = \dfrac{4}{3} \approx 1{,}3333$. **Terlalu kecil** dibanding $\sigma^2 = 2{,}6667$.
- Bagi dengan $n - 1 = 1$: rata-rata varians sampel $= \dfrac{8}{3} \approx 2{,}6667$. **Tepat sama** dengan $\sigma^2$.

Jadi $s^2$ dengan $n-1$ adalah penaksir varians populasi yang tidak bias. (Akar $s$ masih sedikit meleset sebagai penaksir $\sigma$, tetapi selisihnya kecil dan lazim diabaikan.) Catatan: jika $n = 1$, $s^2$ tidak terdefinisi, dan memang satu data tidak memberi informasi tentang sebaran.

!!! warning "Perhatikan pustaka yang dipakai"
    Python `statistics.stdev` dan `statistics.variance` memakai $n-1$ (sampel), sedangkan `statistics.pstdev` dan `statistics.pvariance` memakai $n$ (populasi). Pustaka lain bisa berbeda: misalnya `np.std` dari NumPy memakai $n$ secara bawaan, sedangkan `Series.std()` pada pandas memakai $n-1$. Selalu periksa dokumentasinya.

### 6. Rata-rata dari Tabel Frekuensi dan Rata-rata Berbobot

Jika data ditulis sebagai nilai $x_i$ beserta **frekuensi**-nya $f_i$ (berapa kali muncul), rata-ratanya:

$$
\bar{x} = \frac{\sum_i f_i\,x_i}{\sum_i f_i}
$$

Pembilang adalah jumlah seluruh nilai, penyebut adalah banyak data. Bentuk yang sama dipakai untuk **rata-rata berbobot**, dengan $f_i$ diganti bobot $w_i$.

### 7. Akibat Menambah atau Mengalikan Konstanta

Misalkan setiap nilai diubah menjadi $y_i = a x_i + b$. Maka:

$$
\bar{y} = a\,\bar{x} + b
\qquad\qquad
s_y = \lvert a \rvert\, s_x
$$

- **Menambah** konstanta $b$ menggeser rata-rata sebesar $b$, tetapi **tidak mengubah** sebaran.
- **Mengalikan** dengan $a$ mengalikan rata-rata dan simpangan baku (dengan $\lvert a \rvert$).

Dengan Data A: tambah $10$ memberi $\bar{y} = 24{,}75$ dan $s$ tetap $3{,}0119$. Kali $2$ memberi $\bar{y} = 29{,}5$ dan $s = 6{,}0238$. Sifat ini menjadi dasar standardisasi di Part 11.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana |
|---|---|
| Rata-rata (*mean*) | jumlah dibagi banyak data |
| Median | nilai tengah setelah diurutkan |
| Modus | nilai yang paling sering muncul |
| Pencilan (*outlier*) | nilai yang sangat jauh dari kebanyakan data |
| Rentang | maksimum dikurangi minimum |
| Kuartil | nilai yang membagi data menjadi empat bagian |
| Persentil | nilai dengan $k\%$ data di bawahnya |
| IQR | $Q_3 - Q_1$ |
| Deviasi | selisih nilai dari rata-rata |
| Varians | rata-rata kuadrat deviasi |
| Simpangan baku | akar varians |
| Frekuensi | berapa kali nilai muncul |

## Rumus dan Penjelasan Setiap Simbol

| Ukuran | Rumus | Syarat |
|---|---|---|
| Rata-rata sampel | $\bar{x} = \dfrac{1}{n}\sum_{i=1}^{n} x_i$ | $n \geq 1$ |
| Rata-rata populasi | $\mu = \dfrac{1}{N}\sum_{i=1}^{N} x_i$ | $N \geq 1$ |
| Median | nilai tengah (atau rata-rata dua nilai tengah) | data diurutkan |
| Rentang | $x_{(n)} - x_{(1)}$ | |
| Posisi persentil | $r = 1 + (n-1)\,p$ | $0 \leq p \leq 1$ |
| IQR | $Q_3 - Q_1$ | |
| Varians populasi | $\sigma^2 = \dfrac{1}{N}\sum (x_i - \mu)^2$ | |
| Varians sampel | $s^2 = \dfrac{1}{n-1}\sum (x_i - \bar{x})^2$ | $n \geq 2$ |
| Simpangan baku | $\sigma = \sqrt{\sigma^2}$ atau $s = \sqrt{s^2}$ | |
| Rata-rata tabel frekuensi | $\bar{x} = \dfrac{\sum f_i x_i}{\sum f_i}$ | $f_i \geq 0$ |
| Transformasi | $\bar{y} = a\bar{x} + b$, $s_y = \lvert a \rvert\, s_x$ | $y_i = a x_i + b$ |

Arti simbol: $x_i$ adalah nilai data ke-$i$; $x_{(i)}$ nilai ke-$i$ setelah diurutkan; $n$ banyak data sampel; $N$ banyak data populasi; $p$ proporsi (antara 0 dan 1); $\lfloor r \rfloor$ bagian bulat dari $r$; $f_i$ frekuensi nilai ke-$i$.

## Penjelasan Langkah demi Langkah

**Meringkas satu variabel numerik:**

1. Urutkan data dari kecil ke besar.
2. Hitung $n$, minimum, maksimum, dan rentang.
3. Hitung rata-rata dan median. Jika keduanya jauh berbeda, curigai pencilan atau distribusi miring.
4. Hitung $Q_1$ dan $Q_3$, lalu IQR.
5. Hitung deviasi, kuadratkan, jumlahkan, bagi dengan $n-1$ (sampel) atau $N$ (populasi), lalu akarkan.
6. Tafsirkan dalam kalimat dengan satuan yang benar.

## Contoh Soal dan Pembahasan

### Contoh 1 — Rata-rata, median, modus

Hitung ketiganya untuk Data A.

1. Diurutkan: $11, 12, 13, 14, 15, 15, 18, 20$.
2. Rata-rata: $\dfrac{11 + 12 + 13 + 14 + 15 + 15 + 18 + 20}{8} = \dfrac{118}{8} = 14{,}75$.
3. Median: $n = 8$ genap, posisi 4 dan 5 adalah $14$ dan $15$, jadi $\dfrac{14 + 15}{2} = 14{,}5$.
4. Modus: $15$ (muncul dua kali).

### Contoh 2 — Pengaruh pencilan

Gaji (juta): $5, 6, 6, 7, 8, 9, 60$. Bandingkan ukuran pemusatan dengan dan tanpa angka $60$.

1. Dengan $60$: rata-rata $\dfrac{101}{7} \approx 14{,}43$; median $= 7$ (nilai ke-4).
2. Tanpa $60$: rata-rata $\dfrac{41}{6} \approx 6{,}83$; median $= \dfrac{6 + 7}{2} = 6{,}5$.
3. Rata-rata berubah $14{,}43 - 6{,}83 \approx 7{,}6$, sedangkan median hanya berubah $0{,}5$.

Kesimpulan: untuk data berpencilan, median lebih mewakili "gaji tipikal".

### Contoh 3 — Kuartil, IQR, dan ringkasan lima angka

Untuk Data A ($n = 8$):

1. $Q_1$: $p = 0{,}25$, posisi $r = 1 + 7(0{,}25) = 2{,}75$. Bagian bulat $2$, pecahan $0{,}75$. Nilai ke-2 $= 12$, ke-3 $= 13$. $Q_1 = 12 + 0{,}75(13 - 12) = 12{,}75$.
2. Median: $r = 1 + 7(0{,}5) = 4{,}5$, antara nilai ke-4 ($14$) dan ke-5 ($15$): $14{,}5$. Sama dengan median di Contoh 1.
3. $Q_3$: $r = 1 + 7(0{,}75) = 6{,}25$. Nilai ke-6 $= 15$, ke-7 $= 18$. $Q_3 = 15 + 0{,}25(18 - 15) = 15{,}75$.
4. IQR $= 15{,}75 - 12{,}75 = 3$.
5. Ringkasan lima angka: $11;\ 12{,}75;\ 14{,}5;\ 15{,}75;\ 20$.
6. Persentil ke-90: $r = 1 + 7(0{,}9) = 7{,}3$. Nilai ke-7 $= 18$, ke-8 $= 20$. $18 + 0{,}3(20 - 18) = 18{,}6$.

Interpretasi: sekitar separuh mahasiswa belajar antara $12{,}75$ dan $15{,}75$ jam per minggu, dan sekitar 90% belajar kurang dari $18{,}6$ jam.

### Contoh 4 — Varians dan simpangan baku

Hitung $s^2$ dan $s$ Data A, serta $\sigma^2$ dan $\sigma$ bila Data A dianggap populasi.

1. $\bar{x} = 14{,}75$.
2. Kuadrat deviasi (dari tabel bagian 4): $7{,}5625;\ 0{,}0625;\ 14{,}0625;\ 10{,}5625;\ 0{,}0625;\ 0{,}5625;\ 27{,}5625;\ 3{,}0625$.
3. Jumlah $= 63{,}5$.
4. Sampel: $s^2 = \dfrac{63{,}5}{8 - 1} = \dfrac{63{,}5}{7} \approx 9{,}0714$, $s = \sqrt{9{,}0714} \approx 3{,}0119$.
5. Populasi: $\sigma^2 = \dfrac{63{,}5}{8} = 7{,}9375$, $\sigma = \sqrt{7{,}9375} \approx 2{,}8174$.

Karena delapan mahasiswa ini sampel dari mahasiswa yang lebih banyak, yang sesuai adalah $s \approx 3{,}01$ jam.

### Contoh 5 — Rata-rata dari tabel frekuensi

Lima puluh pengguna memberi rating 1 sampai 5:

| Rating $x_i$ | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| Frekuensi $f_i$ | 2 | 3 | 10 | 25 | 10 |

1. Banyak data: $\sum f_i = 2 + 3 + 10 + 25 + 10 = 50$.
2. $\sum f_i x_i = 1(2) + 2(3) + 3(10) + 4(25) + 5(10) = 2 + 6 + 30 + 100 + 50 = 188$.
3. Rata-rata $= \dfrac{188}{50} = 3{,}76$.
4. Median: data ke-25 dan ke-26. Frekuensi kumulatif: $2,\ 5,\ 15,\ 40$. Keduanya jatuh pada rating $4$, jadi median $= 4$.
5. Modus: rating $4$ (frekuensi terbesar, $25$).

Rata-rata ($3{,}76$) lebih kecil dari median ($4$), tanda bahwa beberapa rating rendah menarik rata-rata ke bawah.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** Nilai $3, 5, 10$: rata-rata $6$. Deviasi: $-3, -1, 4$, jumlahnya $0$. Jumlah kuadrat deviasi dari rata-rata $= 9 + 1 + 16 = 26$.

**Sedang.** Menguji identitas dengan $c = 5$: $\sum (x_i - 5)^2 = 4 + 0 + 25 = 29$. Rumus: $26 + 3(6 - 5)^2 = 29$. Cocok.

**Sedang.** Rata-rata berbobot: tugas $80$ (bobot $30\%$), UTS $70$ ($30\%$), UAS $90$ ($40\%$). Nilai akhir $= 0{,}3(80) + 0{,}3(70) + 0{,}4(90) = 24 + 21 + 36 = 81$.

**Menantang.** Konversi suhu $F = 1{,}8C + 32$ untuk $C = 20, 25, 30$. Rata-rata $C = 25$, $s_C = 5$. Maka rata-rata $F = 1{,}8(25) + 32 = 77$ dan $s_F = 1{,}8 \times 5 = 9$. Penambah $32$ tidak mengubah sebaran; hanya pengali $1{,}8$ yang melebarkannya.

## Visualisasi atau Interpretasi Grafik

Pada garis bilangan, Data A terlihat seperti ini:

| Nilai | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|---|---|---|---|---|---|---|---|---|---|---|
| Banyak | 1 | 1 | 1 | 1 | 2 | 0 | 0 | 1 | 0 | 1 |

Sebagian besar data berkumpul di $11$ sampai $15$, dengan dua nilai ($18$ dan $20$) menjauh ke kanan. Itulah sebabnya rata-rata ($14{,}75$) sedikit lebih besar dari median ($14{,}5$): nilai besar menarik rata-rata ke kanan. Grafik histogram dan boxplot dibahas di Part 3.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Median data genap diambil dari satu nilai | median adalah nilai di antara dua nilai tengah | rata-ratakan dua nilai tengah |
| Lupa mengurutkan sebelum mencari median atau kuartil | posisi hanya bermakna pada data terurut | urutkan dulu |
| Memakai rata-rata untuk data berpencilan tanpa memeriksa | pencilan menarik rata-rata | bandingkan dengan median |
| Memakai $N$ untuk sampel | varians cenderung terlalu kecil | pakai $n-1$ untuk sampel |
| Menjumlahkan deviasi tanpa dikuadratkan | selalu nol | kuadratkan dulu |
| Melaporkan varians sebagai "simpangan" | satuan varians adalah kuadrat | laporkan simpangan baku untuk satuan asli |
| Lupa mengakarkan | $s^2 \neq s$ | $s = \sqrt{s^2}$ |
| Membandingkan hasil dua pustaka tanpa melihat metodenya | metode kuartil dan $n$ vs $n-1$ berbeda | cek dokumentasi |
| Mengira menambah konstanta mengubah sebaran | hanya menggeser | sebaran hanya berubah oleh perkalian |

## Hubungan dengan Machine Learning

- **Memahami dataset.** Rata-rata, median, simpangan baku, minimum, dan maksimum tiap fitur adalah pemeriksaan pertama sebelum melatih model: apakah skalanya wajar, adakah nilai yang mustahil, adakah pencilan.
- **Mengisi data yang hilang (*imputation*).** Nilai yang hilang sering diisi dengan rata-rata, median, atau modus. Median lebih aman untuk data miring atau berpencilan, dan modus untuk fitur kategorik. Ini hanya salah satu pilihan, dan pengisian ini memengaruhi sebaran fitur.
- **Fungsi kerugian.** Rata-rata kuadrat selisih (MSE) berkaitan dengan rata-rata, sedangkan rata-rata selisih mutlak (MAE) berkaitan dengan median. Rata-rata meminimalkan jumlah kuadrat deviasi (lihat identitas di bagian 5 dengan $c = \bar{x}$). Karena itu MSE lebih sensitif terhadap pencilan daripada MAE.
- **Model dasar (*baseline*).** Model paling sederhana untuk regresi memprediksi rata-rata untuk semua kasus. Untuk klasifikasi, memprediksi kelas yang paling sering muncul (modus). Model yang rumit harus mengalahkan baseline ini agar berguna.
- **Skala fitur.** Fitur dengan simpangan baku sangat berbeda (misalnya umur dengan sebaran puluhan, pendapatan dengan sebaran jutaan) memengaruhi model berbasis jarak dan gradient descent. Menyamakan skala adalah topik Part 11.
- **Rumus sampel vs populasi.** Ringkasan dari data latih adalah statistik sampel. Saat menghitung simpangan baku fitur, cek apakah pustaka memakai $n$ atau $n-1$.

### Python: statistik deskriptif dengan `statistics`

Hanya pustaka standar. Data A:

```python
from statistics import mean, median, multimode, quantiles, variance, pvariance, stdev, pstdev
import math

A = [12, 15, 11, 18, 15, 14, 20, 13]
print(sorted(A))                                # [11, 12, 13, 14, 15, 15, 18, 20]
print(sum(A), mean(A), median(A), multimode(A)) # 118 14.75 14.5 [15]
print(min(A), max(A), max(A) - min(A))          # 11 20 9
```

Persentil dengan interpolasi linear (sama dengan langkah di bagian 3):

```python
def persentil(data, p):
    d = sorted(data)
    r = (len(d) - 1) * p            # posisi berbasis 0
    bawah = math.floor(r)
    atas = min(bawah + 1, len(d) - 1)
    return d[bawah] + (r - bawah) * (d[atas] - d[bawah])

print([persentil(A, p) for p in (0.25, 0.5, 0.75)])  # [12.75, 14.5, 15.75]
print(persentil(A, 0.9), persentil(A, 0.1))          # 18.6 11.7
print(quantiles(A, n=4, method="inclusive"))         # [12.75, 14.5, 15.75]
print(quantiles(A, n=4))                             # [12.25, 14.5, 17.25]
```

Hasil dua baris terakhir menunjukkan bahwa metode `inclusive` sama dengan interpolasi linear di buku ini, sedangkan metode bawaan (`exclusive`) memberi kuartil yang berbeda. Perhatikan juga bahwa `persentil` memakai posisi berbasis 0, yaitu $(n-1)p$, sedangkan di teks posisinya berbasis 1, yaitu $1 + (n-1)p$; hasilnya sama.

Varians dan simpangan baku:

```python
dev = [x - mean(A) for x in A]
print(dev)                      # [-2.75, 0.25, -3.75, 3.25, 0.25, -0.75, 5.25, -1.75]
print(sum(dev))                 # 0.0
print(sum(d * d for d in dev))  # 63.5

print(round(variance(A), 4), round(stdev(A), 4))     # 9.0714 3.0119   (sampel, n-1)
print(round(pvariance(A), 4), round(pstdev(A), 4))   # 7.9375 2.8174   (populasi, n)
```

Pencilan dan transformasi:

```python
gaji = [5, 6, 6, 7, 8, 9, 60]
print(round(mean(gaji), 4), median(gaji))            # 14.4286 7
print(round(mean(gaji[:-1]), 4), median(gaji[:-1]))  # 6.8333 6.5

B = [x + 10 for x in A]
C = [2 * x for x in A]
print(mean(B), round(stdev(B), 4))                   # 24.75 3.0119
print(mean(C), round(stdev(C), 4))                   # 29.5 6.0238
```

Rata-rata dari tabel frekuensi:

```python
nilai = [1, 2, 3, 4, 5]
frek = [2, 3, 10, 25, 10]
n = sum(frek)
print(n, sum(v * f for v, f in zip(nilai, frek)) / n)   # 50 3.76
```

Interpretasi: semua hasil sama dengan hitungan manual di bagian contoh. Kode hanya memeriksa; langkah dan alasannya ada di penjelasan.

## Latihan Mandiri

1. Untuk data $4, 8, 6, 5, 3, 8, 9$ hitung rata-rata, median, modus, dan rentang.
2. Untuk data $10, 20, 30, 40$ hitung rata-rata, median, modus, $Q_1$, $Q_3$, dan IQR (metode interpolasi linear).
3. Untuk sampel $2, 4, 4, 4, 5, 5, 7, 9$ hitung rata-rata, varians dan simpangan baku populasi, serta varians dan simpangan baku sampel.
4. Dua kelompok nilai: A $= 68, 70, 72$ dan B $= 40, 70, 100$. Hitung rata-rata dan simpangan baku sampel masing-masing, lalu bandingkan.
5. Gaji (juta): $5, 6, 6, 7, 8, 9, 60$. Mana yang lebih mewakili gaji tipikal, rata-rata atau median? Jelaskan dengan angka.
6. Untuk data $3, 7, 8, 5, 12, 14, 21, 13, 18$ hitung median, $Q_1$, $Q_3$, IQR, dan persentil ke-90.
7. Jumlah anak dari 20 keluarga: 0 anak (5 keluarga), 1 anak (8), 2 anak (4), 3 anak (3). Hitung rata-rata, median, dan modus.
8. Nilai $60, 70, 80$ mempunyai rata-rata $70$ dan $s = 10$. Berapa rata-rata dan $s$ bila (a) setiap nilai ditambah $5$, (b) setiap nilai dikali $2$?
9. Nilai akhir: tugas $80$ (30%), UTS $70$ (30%), UAS $90$ (40%). Hitung nilai akhir berbobot.
10. Untuk data $3, 5, 10$: tunjukkan bahwa jumlah deviasi dari rata-rata sama dengan $0$, lalu uji identitas $\sum(x_i - c)^2 = \sum(x_i - \bar{x})^2 + n(\bar{x} - c)^2$ dengan $c = 5$.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    Jumlah $= 43$, $n = 7$, rata-rata $= \dfrac{43}{7} \approx 6{,}143$. Diurutkan: $3, 4, 5, 6, 8, 8, 9$; median (nilai ke-4) $= 6$. Modus $= 8$ (muncul dua kali). Rentang $= 9 - 3 = 6$.

??? success "Soal 2"
    Rata-rata $= \dfrac{100}{4} = 25$. Median $= \dfrac{20 + 30}{2} = 25$. Tidak ada modus (semua muncul sekali). $Q_1$: $r = 1 + 3(0{,}25) = 1{,}75$, jadi $10 + 0{,}75(20 - 10) = 17{,}5$. $Q_3$: $r = 1 + 3(0{,}75) = 3{,}25$, jadi $30 + 0{,}25(40 - 30) = 32{,}5$. IQR $= 32{,}5 - 17{,}5 = 15$.

??? success "Soal 3"
    Rata-rata $= \dfrac{40}{8} = 5$. Deviasi: $-3, -1, -1, -1, 0, 0, 2, 4$. Kuadrat: $9, 1, 1, 1, 0, 0, 4, 16$, jumlah $32$. Populasi: $\sigma^2 = \dfrac{32}{8} = 4$, $\sigma = 2$. Sampel: $s^2 = \dfrac{32}{7} \approx 4{,}571$, $s \approx 2{,}138$.

??? success "Soal 4"
    A: rata-rata $70$; deviasi $-2, 0, 2$; jumlah kuadrat $8$; $s^2 = \dfrac{8}{2} = 4$; $s = 2$. B: rata-rata $70$; deviasi $-30, 0, 30$; jumlah kuadrat $1800$; $s^2 = \dfrac{1800}{2} = 900$; $s = 30$. Rata-rata sama, tetapi B jauh lebih menyebar (15 kali lebih besar).

??? success "Soal 5"
    Rata-rata $\approx 14{,}43$ dan median $= 7$. Enam dari tujuh orang bergaji di bawah $10$, sedangkan rata-rata $14{,}43$ ditarik naik oleh satu nilai $60$. Median $7$ lebih mewakili. Tanpa angka $60$, rata-rata menjadi $6{,}83$ dan median $6{,}5$.

??? success "Soal 6"
    Diurutkan: $3, 5, 7, 8, 12, 13, 14, 18, 21$ ($n = 9$). Median (nilai ke-5) $= 12$. $Q_1$: $r = 1 + 8(0{,}25) = 3$, nilai ke-3 $= 7$. $Q_3$: $r = 1 + 8(0{,}75) = 7$, nilai ke-7 $= 14$. IQR $= 14 - 7 = 7$. P90: $r = 1 + 8(0{,}9) = 8{,}2$; nilai ke-8 $= 18$, ke-9 $= 21$; $18 + 0{,}2(21 - 18) = 18{,}6$.

??? success "Soal 7"
    $n = 20$. $\sum f_i x_i = 0(5) + 1(8) + 2(4) + 3(3) = 25$. Rata-rata $= \dfrac{25}{20} = 1{,}25$. Frekuensi kumulatif: $5, 13, 17, 20$. Data ke-10 dan ke-11 keduanya bernilai $1$, jadi median $= 1$. Modus $= 1$ (frekuensi $8$).

??? success "Soal 8"
    (a) Rata-rata $= 70 + 5 = 75$, $s$ tetap $10$. (b) Rata-rata $= 2 \times 70 = 140$, $s = 2 \times 10 = 20$.

??? success "Soal 9"
    $0{,}3(80) + 0{,}3(70) + 0{,}4(90) = 24 + 21 + 36 = 81$.

??? success "Soal 10"
    Rata-rata $= 6$. Deviasi: $-3, -1, 4$; jumlah $= 0$. Jumlah kuadrat deviasi dari rata-rata $= 9 + 1 + 16 = 26$. Dengan $c = 5$: ruas kiri $(3-5)^2 + (5-5)^2 + (10-5)^2 = 4 + 0 + 25 = 29$. Ruas kanan $26 + 3(6 - 5)^2 = 29$. Identitas terbukti pada contoh ini.

## Ringkasan Materi

- **Pemusatan:** rata-rata (memakai semua nilai, sensitif pencilan), median (tahan pencilan), modus (paling sering).
- **Sebaran:** rentang, IQR ($Q_3 - Q_1$), varians, dan simpangan baku.
- Kuartil dan persentil dihitung dari data terurut; metodenya bisa berbeda, jadi sebutkan metode yang dipakai (di sini: interpolasi linear).
- Varians populasi memakai $N$; varians sampel memakai $n-1$ agar tidak bias. Pustaka berbeda bisa memakai bawaan yang berbeda.
- Rata-rata tabel frekuensi: $\dfrac{\sum f_i x_i}{\sum f_i}$. Menambah konstanta menggeser rata-rata tanpa mengubah sebaran; mengalikan konstanta mengalikan simpangan baku dengan $\lvert a \rvert$.
- Dalam ML: pemeriksaan dataset, imputasi, kerugian (MSE terkait rata-rata, MAE terkait median), baseline, dan pengantar standardisasi.

## Checklist Penguasaan

- [ ] Menghitung rata-rata, median, dan modus, dan memilih yang cocok.
- [ ] Menghitung kuartil dengan interpolasi linear, IQR, dan ringkasan lima angka.
- [ ] Menghitung varians dan simpangan baku untuk populasi dan sampel.
- [ ] Menjelaskan mengapa sampel memakai $n-1$.
- [ ] Menghitung rata-rata dari tabel frekuensi dan rata-rata berbobot.
- [ ] Menjelaskan efek $y = ax + b$ pada rata-rata dan simpangan baku.
- [ ] Mengaitkan ringkasan ini dengan pemeriksaan data, imputasi, dan kerugian.

## Prasyarat untuk Materi Berikutnya

[Part 3 — Visualisasi dan Distribusi Data](03-visualisasi-dan-distribusi-data.md) memakai ringkasan lima angka, IQR, rata-rata, dan simpangan baku untuk membuat histogram dan boxplot, serta mendeteksi pencilan dan kemiringan.
