# Part 3 — Pecahan dan Desimal

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan arti pecahan, pembilang, dan penyebut;
- mengubah pecahan campuran menjadi pecahan biasa dan sebaliknya;
- membuat pecahan senilai, menyederhanakan, dan membandingkan pecahan;
- menjumlah, mengurang, mengalikan, dan membagi pecahan, lengkap dengan alasannya;
- mengubah pecahan menjadi desimal dan sebaliknya;
- membedakan desimal berhingga dan berulang;
- membulatkan bilangan dan memahami akibatnya pada ketelitian.

## Prasyarat

[Part 1](01-mengenal-bilangan.md) (bilangan rasional dan garis bilangan) dan [Part 2](02-operasi-aritmatika-dasar.md) (empat operasi, sifat identitas, urutan operasi).

## Mengapa Materi Ini Penting?

Hampir semua nilai penting dalam Machine Learning bukan bilangan bulat: peluang 0,25, laju belajar 0,01, akurasi 0,92. Semuanya pecahan atau desimal. Jika kamu tidak nyaman dengan keduanya, rumus akan terasa menakutkan. Sebaliknya, setelah memahami alasan di balik setiap aturan pecahan, rumus aljabar nanti akan terasa wajar.

## Penjelasan Konsep dari Dasar

### 1. Pengertian Pecahan

**Pecahan** (*fraction*) menyatakan sebagian dari sesuatu yang utuh, atau hasil bagi dua bilangan bulat. Jika sebuah pizza dipotong menjadi 4 bagian sama besar dan kamu mengambil 3 potong, kamu mengambil $\frac{3}{4}$ pizza.

Pecahan punya dua tafsir yang sama-sama benar:

- **Bagian dari keseluruhan:** $\frac{3}{4}$ berarti 3 dari 4 bagian yang sama besar.
- **Hasil pembagian:** $\frac{3}{4}$ berarti $3 \div 4$. Jika 3 pizza dibagi rata kepada 4 orang, setiap orang mendapat $\frac{3}{4}$ pizza.

Pada garis bilangan, $\frac{3}{4}$ terletak di antara 0 dan 1, tepat di tiga perempat jarak dari 0 ke 1.

### 2. Pembilang dan Penyebut

Pada pecahan $\frac{a}{b}$:

- $a$ adalah **pembilang** (*numerator*), yaitu banyaknya bagian yang diambil.
- $b$ adalah **penyebut** (*denominator*), yaitu banyaknya bagian sama besar yang membentuk satu keseluruhan.

Penyebut tidak boleh nol, karena "membagi menjadi 0 bagian" tidak punya arti (lihat pembagian dengan nol di Part 2). Semakin besar penyebut (dengan pembilang tetap), semakin kecil tiap potongnya: $\frac{1}{8}$ lebih kecil dari $\frac{1}{2}$.

### 3. Pecahan Biasa dan Pecahan Campuran

- **Pecahan biasa**: pembilang dan penyebut berupa bilangan bulat, seperti $\frac{3}{4}$ atau $\frac{7}{4}$. Jika pembilang lebih kecil dari penyebut, nilainya kurang dari 1 (disebut pecahan murni). Jika pembilang sama atau lebih besar, nilainya 1 atau lebih (pecahan tak murni).
- **Pecahan campuran**: gabungan bilangan bulat dan pecahan murni, seperti $1\frac{3}{4}$ (dibaca "satu tiga per empat").

**Dari biasa ke campuran.** Bagi pembilang dengan penyebut. Hasil bagi menjadi bagian bulat, sisa menjadi pembilang baru.

$\frac{7}{4}$: $7 \div 4 = 1$ sisa $3$, jadi $\frac{7}{4} = 1\frac{3}{4}$.

**Dari campuran ke biasa.** Kalikan bagian bulat dengan penyebut, tambahkan pembilang, simpan penyebutnya.

$2\frac{1}{3}$: $2 \times 3 + 1 = 7$, jadi $2\frac{1}{3} = \frac{7}{3}$. Alasannya: 2 utuh sama dengan $\frac{6}{3}$, ditambah $\frac{1}{3}$ menjadi $\frac{7}{3}$.

### 4. Pecahan Senilai

**Pecahan senilai** adalah pecahan yang tampak berbeda tetapi nilainya sama: $\frac{1}{2} = \frac{2}{4} = \frac{3}{6}$. Bayangkan pizza yang dipotong 2 dan kamu ambil 1, dibandingkan pizza yang dipotong 4 dan kamu ambil 2. Jumlah pizzanya sama.

Cara membuatnya: kalikan (atau bagi) pembilang **dan** penyebut dengan bilangan yang sama, yang bukan nol.

$$
\frac{a}{b} = \frac{a \times k}{b \times k} \qquad (k \neq 0)
$$

Alasannya: $\frac{k}{k} = 1$, dan mengalikan dengan 1 tidak mengubah nilai (identitas perkalian, Part 2):

$$
\frac{1}{2} = \frac{1}{2} \times \frac{3}{3} = \frac{3}{6}
$$

### 5. Menyederhanakan Pecahan

Menyederhanakan pecahan berarti membagi pembilang dan penyebut dengan pembagi yang sama sampai tidak bisa lagi. Hasilnya adalah pecahan dalam **bentuk paling sederhana**: pembilang dan penyebut tidak punya pembagi bersama selain 1.

Cara tercepat: bagi dengan **FPB** (Faktor Persekutuan Terbesar), yaitu pembagi bersama terbesar kedua bilangan.

Menyederhanakan $\frac{18}{24}$:

- Pembagi 18: 1, 2, 3, 6, 9, 18.
- Pembagi 24: 1, 2, 3, 4, 6, 8, 12, 24.
- Pembagi bersama: 1, 2, 3, 6. Yang terbesar: 6, jadi FPB = 6.
- $\frac{18 \div 6}{24 \div 6} = \frac{3}{4}$.

Jika belum menemukan FPB, bagi bertahap dengan bilangan apa pun yang cocok (misalnya 2, lalu 3). Hasil akhirnya sama.

### 6. Membandingkan Pecahan

Dua pecahan hanya mudah dibandingkan jika **potongannya sama besar**, yaitu penyebutnya sama. Jika penyebutnya berbeda, samakan dulu memakai **KPK** (Kelipatan Persekutuan Terkecil), yaitu bilangan terkecil yang merupakan kelipatan kedua penyebut.

Bandingkan $\frac{3}{4}$ dan $\frac{5}{6}$:

- Kelipatan 4: 4, 8, 12, 16, ... Kelipatan 6: 6, 12, 18, ... KPK = 12.
- $\frac{3}{4} = \frac{3 \times 3}{4 \times 3} = \frac{9}{12}$ dan $\frac{5}{6} = \frac{5 \times 2}{6 \times 2} = \frac{10}{12}$.
- $\frac{9}{12} < \frac{10}{12}$, maka $\frac{3}{4} < \frac{5}{6}$.

Dua jalan pintas: jika **penyebut sama**, yang pembilangnya lebih besar lebih besar. Jika **pembilang sama**, yang penyebutnya lebih besar justru lebih kecil ($\frac{1}{3} < \frac{1}{2}$). Atau, ubah keduanya menjadi desimal.

### 7. Penjumlahan dan Pengurangan Pecahan

**Penyebut sama.** Jumlahkan atau kurangi pembilangnya saja, penyebut tetap:

$$
\frac{2}{7} + \frac{3}{7} = \frac{5}{7}
$$

Alasannya: 2 potong ketujuh ditambah 3 potong ketujuh menjadi 5 potong ketujuh. Ukuran potongan tidak berubah.

**Penyebut berbeda.** Samakan dulu penyebutnya, karena hanya potongan yang sama besar yang bisa dihitung bersama. Hitung $\frac{1}{2} + \frac{1}{3}$:

1. KPK dari 2 dan 3 adalah 6.
2. $\frac{1}{2} = \frac{3}{6}$ dan $\frac{1}{3} = \frac{2}{6}$.
3. $\frac{3}{6} + \frac{2}{6} = \frac{5}{6}$.

!!! warning "Kesalahan paling umum"
    $\frac{1}{2} + \frac{1}{3} \neq \frac{2}{5}$. Menjumlahkan pembilang dan penyebut sekaligus tidak punya dasar. Perhatikan juga bahwa $\frac{2}{5}$ lebih kecil dari $\frac{1}{2}$, padahal hasil penjumlahan tidak mungkin lebih kecil dari salah satu sukunya.

Pengurangan sama caranya: $\frac{3}{4} - \frac{1}{6} = \frac{9}{12} - \frac{2}{12} = \frac{7}{12}$.

### 8. Perkalian Pecahan

Kalikan pembilang dengan pembilang, dan penyebut dengan penyebut:

$$
\frac{a}{b} \times \frac{c}{d} = \frac{a \times c}{b \times d}
$$

Contoh: $\frac{2}{3} \times \frac{3}{4} = \frac{6}{12} = \frac{1}{2}$.

Alasannya: "$\frac{2}{3}$ dari $\frac{3}{4}$" berarti mengambil dua per tiga bagian dari sesuatu yang besarnya tiga per empat. Bayangkan persegi dibagi 4 kolom dan 3 baris: ada 12 kotak kecil. Tiga per empat adalah 3 kolom; dua per tiga dari kolom-kolom itu adalah 2 baris dari 3 kolom, yaitu 6 kotak dari 12.

Kamu boleh menyederhanakan sebelum mengalikan: $\frac{2}{3} \times \frac{3}{4}$, angka 3 di atas dan di bawah saling menghapus, tersisa $\frac{2}{4} = \frac{1}{2}$.

Perkalian dengan bilangan bulat: tulis bilangan bulat sebagai pecahan berpenyebut 1. $3 \times \frac{2}{5} = \frac{3}{1} \times \frac{2}{5} = \frac{6}{5}$.

Perkalian dua pecahan murni menghasilkan bilangan yang **lebih kecil** dari keduanya, karena mengambil sebagian dari sebagian.

### 9. Pembagian Pecahan

Membagi dengan sebuah pecahan sama dengan mengalikan dengan **kebalikannya** (*reciprocal*), yaitu pecahan yang pembilang dan penyebutnya ditukar:

$$
\frac{a}{b} \div \frac{c}{d} = \frac{a}{b} \times \frac{d}{c} \qquad (c \neq 0)
$$

Contoh: $\frac{2}{3} \div \frac{4}{5} = \frac{2}{3} \times \frac{5}{4} = \frac{10}{12} = \frac{5}{6}$.

Mengapa boleh ditukar? Mulai dari soal yang gampang: $3 \div \frac{1}{2}$ bertanya "ada berapa setengah di dalam 3?". Setiap satuan berisi 2 setengah, jadi ada $3 \times 2 = 6$. Dan $\frac{1}{2}$ memiliki kebalikan 2. Secara umum, membagi dengan suatu bilangan sama dengan mengalikan dengan kebalikannya, karena bilangan dikali kebalikannya menghasilkan 1 (Part 2): $\frac{c}{d} \times \frac{d}{c} = 1$.

!!! warning "Yang ditukar hanya pecahan pembagi"
    Tukar pembilang dan penyebut dari pecahan **sesudah** tanda $\div$, bukan yang sebelumnya.

### 10. Mengubah Pecahan Menjadi Desimal

Karena pecahan adalah hasil bagi, ubah pecahan menjadi desimal dengan **membagi pembilang oleh penyebut**.

$\frac{3}{4} = 3 \div 4$. Tambahkan koma dan nol agar bisa dibagi:

- $30 \div 4 = 7$ sisa $2$ (karena $4 \times 7 = 28$). Tulis 7 sebagai angka persepuluhan.
- Sisa 2 menjadi $20$. $20 \div 4 = 5$ tanpa sisa. Tulis 5 sebagai angka perseratusan.
- Hasil: $0{,}75$.

Contoh lain: $\frac{1}{8} = 0{,}125$ dan $\frac{2}{3} = 0{,}666\ldots$ (hasil baginya tidak pernah habis).

### 11. Mengubah Desimal Menjadi Pecahan

Setiap angka di kanan koma punya **nilai tempat**:

| Angka ke- | Nama | Nilai |
|---|---|---|
| 1 | persepuluhan | $\frac{1}{10}$ |
| 2 | perseratusan | $\frac{1}{100}$ |
| 3 | perseribuan | $\frac{1}{1000}$ |

Cara mengubah: tulis angka di belakang koma sebagai pembilang, dan penyebutnya 10, 100, 1000, ... sesuai banyaknya angka di belakang koma. Lalu sederhanakan.

- $0{,}75 = \frac{75}{100} = \frac{3}{4}$ (dibagi 25).
- $0{,}125 = \frac{125}{1000} = \frac{1}{8}$ (dibagi 125).
- $2{,}4 = \frac{24}{10} = \frac{12}{5} = 2\frac{2}{5}$.

### 12. Desimal Berhingga dan Desimal Berulang

Bentuk desimal pecahan hanya mungkin salah satu dari dua jenis:

- **Berhingga** (*terminating*): berhenti. Contoh: $\frac{3}{4} = 0{,}75$.
- **Berulang** (*repeating*): pola angka mengulang tanpa akhir. Contoh: $\frac{1}{3} = 0{,}333\ldots$ dan $\frac{1}{7} = 0{,}142857142857\ldots$

Garis di atas angka menandai bagian yang berulang: $0{,}333\ldots = 0{,}\overline{3}$ dan $\frac{1}{7} = 0{,}\overline{142857}$.

**Petunjuk tanpa menghitung.** Ubah pecahan ke bentuk paling sederhana. Jika penyebutnya hanya memiliki faktor prima 2 dan 5 (atau salah satunya), desimalnya berhingga. Jika ada faktor prima lain, desimalnya berulang. Alasannya: 10 sama dengan $2 \times 5$, jadi hanya penyebut yang "cocok" dengan 10, 100, 1000, ... yang bisa dibuat berpenyebut pangkat sepuluh.

- $\frac{7}{20}$: $20 = 2 \times 2 \times 5$, berhingga ($0{,}35$).
- $\frac{5}{12}$: $12 = 2 \times 2 \times 3$, ada faktor 3, berulang ($0{,}41666\ldots$).

**Pengayaan: mengubah desimal berulang menjadi pecahan.** Misalkan $x = 0{,}666\ldots$ (huruf $x$ mewakili bilangan yang kita cari; pemakaian huruf dibahas di Part 7). Maka $10x = 6{,}666\ldots$. Kurangkan: $10x - x = 6{,}666\ldots - 0{,}666\ldots = 6$, sehingga $9x = 6$ dan $x = \frac{6}{9} = \frac{2}{3}$. Bagian berulangnya "hilang" saat dikurangkan.

### 13. Pembulatan dan Ketelitian Angka

**Pembulatan** (*rounding*) mengganti bilangan dengan bilangan terdekat yang lebih pendek. Aturan: lihat angka tepat setelah posisi yang dipertahankan.

- Jika angkanya **0 sampai 4**, angka yang dipertahankan tetap.
- Jika angkanya **5 sampai 9**, angka yang dipertahankan naik 1.

Contoh $3{,}14159$:

- Dua desimal: angka ketiga adalah 1 (kurang dari 5), maka $3{,}14$.
- Empat desimal: angka kelima adalah 9 (5 atau lebih), maka angka keempat (5) naik menjadi 6: $3{,}1416$.
- Bilangan bulat: angka pertama setelah koma adalah 1, maka $3$.

**Ketelitian.** Pembulatan menghasilkan **pendekatan**, jadi muncul **galat** (*error*), yaitu selisih antara pendekatan dan nilai sebenarnya. Semakin banyak desimal dipertahankan, semakin kecil galatnya.

Galat bisa menumpuk. Pendekatan $\frac{1}{3} \approx 0{,}33$ terlihat baik, tetapi $0{,}33 \times 3 = 0{,}99$, padahal $\frac{1}{3} \times 3 = 1$. Karena itu, **bulatkan hanya di akhir perhitungan**, bukan di tengah-tengah.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Pecahan (*fraction*) | bagian dari keseluruhan / hasil bagi | $\frac{3}{4}$ |
| Pembilang (*numerator*) | bilangan di atas garis pecahan | 3 pada $\frac{3}{4}$ |
| Penyebut (*denominator*) | bilangan di bawah garis pecahan | 4 pada $\frac{3}{4}$ |
| Pecahan campuran | bilangan bulat + pecahan murni | $1\frac{3}{4}$ |
| Pecahan senilai | pecahan berbeda tulisan, sama nilai | $\frac{1}{2} = \frac{2}{4}$ |
| FPB | pembagi bersama terbesar | FPB(18, 24) = 6 |
| KPK | kelipatan bersama terkecil | KPK(4, 6) = 12 |
| Kebalikan (*reciprocal*) | pecahan dengan pembilang-penyebut ditukar | kebalikan $\frac{2}{3}$ adalah $\frac{3}{2}$ |
| Desimal berhingga | desimal yang berhenti | 0,75 |
| Desimal berulang | desimal dengan pola berulang tanpa akhir | $0{,}\overline{3}$ |
| Pembulatan | mengganti dengan nilai terdekat yang lebih pendek | 3,14159 menjadi 3,14 |
| Galat (*error*) | selisih pendekatan dan nilai sebenarnya | $3{,}14159 - 3{,}14$ |

## Rumus dan Penjelasan Setiap Simbol

Pada semua rumus, $a$, $b$, $c$, $d$, dan $k$ adalah bilangan bulat sembarang. Syarat pada penyebut wajib dipenuhi.

| Operasi | Rumus | Syarat |
|---|---|---|
| Pecahan senilai | $\dfrac{a}{b} = \dfrac{a \times k}{b \times k}$ | $b \neq 0$, $k \neq 0$ |
| Jumlah | $\dfrac{a}{b} + \dfrac{c}{b} = \dfrac{a + c}{b}$ | $b \neq 0$ (penyebut sama) |
| Jumlah, penyebut beda | $\dfrac{a}{b} + \dfrac{c}{d} = \dfrac{a \times d + c \times b}{b \times d}$ | $b \neq 0$, $d \neq 0$ |
| Kali | $\dfrac{a}{b} \times \dfrac{c}{d} = \dfrac{a \times c}{b \times d}$ | $b \neq 0$, $d \neq 0$ |
| Bagi | $\dfrac{a}{b} \div \dfrac{c}{d} = \dfrac{a \times d}{b \times c}$ | $b, c, d \neq 0$ |

Rumus jumlah dengan penyebut beda adalah bentuk umum dari "samakan penyebut": kedua pecahan dibuat berpenyebut $b \times d$. Untuk bilangan kecil, memakai KPK menghasilkan angka yang lebih kecil dan lebih mudah.

Garis pecahan $\dfrac{a}{b}$ berarti $a \div b$. Pada pecahan campuran $1\frac{3}{4}$, tidak ada tanda plus tertulis, tetapi artinya $1 + \frac{3}{4}$. Hati-hati: ini **berbeda** dengan $1 \times \frac{3}{4}$.

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Menyederhanakan dan membandingkan

Mana yang lebih besar: $\frac{18}{24}$ atau $\frac{5}{6}$?

1. Sederhanakan $\frac{18}{24}$ dengan FPB 6 (dari bagian 5): $\frac{18}{24} = \frac{3}{4}$.
2. Bandingkan $\frac{3}{4}$ dan $\frac{5}{6}$. Dari bagian 6, KPK = 12: $\frac{9}{12}$ dan $\frac{10}{12}$.
3. $\frac{10}{12} > \frac{9}{12}$.

Jawaban: $\frac{5}{6}$ lebih besar.

### Contoh 2 — Menjumlah dan mengurang pecahan campuran

Hitung $1\frac{1}{2} + 2\frac{3}{4}$.

1. Ubah ke pecahan biasa: $1\frac{1}{2} = \frac{3}{2}$ dan $2\frac{3}{4} = \frac{11}{4}$.
2. Samakan penyebut (KPK 2 dan 4 adalah 4): $\frac{3}{2} = \frac{6}{4}$.
3. Jumlahkan: $\frac{6}{4} + \frac{11}{4} = \frac{17}{4}$.
4. Ubah ke campuran: $17 \div 4 = 4$ sisa $1$, jadi $4\frac{1}{4}$.

Periksa dengan desimal: $1{,}5 + 2{,}75 = 4{,}25 = 4\frac{1}{4}$.

### Contoh 3 — Perkalian dan pembagian

Hitung $\frac{2}{3} \times \frac{3}{4}$ lalu $\frac{2}{3} \div \frac{4}{5}$.

1. Perkalian: $\frac{2 \times 3}{3 \times 4} = \frac{6}{12} = \frac{1}{2}$.
2. Pembagian: tukar pecahan pembagi, $\frac{4}{5}$ menjadi $\frac{5}{4}$.
3. Kalikan: $\frac{2}{3} \times \frac{5}{4} = \frac{10}{12} = \frac{5}{6}$.

Periksa dengan desimal: $0{,}666\ldots \div 0{,}8 = 0{,}8333\ldots = \frac{5}{6}$.

### Contoh 4 — Bolak-balik pecahan dan desimal

Ubah $\frac{7}{8}$ menjadi desimal, lalu ubah 0,64 menjadi pecahan.

1. $7 \div 8$: $70 \div 8 = 8$ sisa $6$; $60 \div 8 = 7$ sisa $4$; $40 \div 8 = 5$ sisa $0$. Hasil: $0{,}875$.
2. $0{,}64$ punya dua angka di belakang koma, jadi $\frac{64}{100}$.
3. Sederhanakan dengan FPB 4: $\frac{16}{25}$.

Periksa: $16 \div 25 = 0{,}64$.

### Contoh 5 — Pembulatan dan galat

Hitung $\frac{2}{3} \times 3$ dua cara: dengan $\frac{2}{3}$ asli dan dengan $\frac{2}{3} \approx 0{,}67$.

1. Asli: $\frac{2}{3} \times 3 = \frac{6}{3} = 2$.
2. Pendekatan: $0{,}67 \times 3 = 2{,}01$.
3. Galatnya $2{,}01 - 2 = 0{,}01$.

Pembulatan di tengah jalan membuat jawabannya meleset.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** Ubah $\frac{17}{5}$ menjadi campuran: $17 \div 5 = 3$ sisa $2$, jadi $3\frac{2}{5}$.

**Sedang.** Dari 40 siswa, $\frac{3}{8}$ membawa laptop. Berapa siswa? $\frac{3}{8} \times 40 = \frac{3 \times 40}{8} = \frac{120}{8} = 15$ siswa.

**Sedang.** Tentukan tanpa menghitung apakah $\frac{9}{20}$ berhingga atau berulang. $20 = 2 \times 2 \times 5$, hanya faktor 2 dan 5, maka berhingga. Benar: $\frac{9}{20} = 0{,}45$.

**Menantang.** Hitung $\frac{1}{2} + \frac{1}{3} \times \frac{3}{4}$. Perkalian lebih dulu: $\frac{1}{3} \times \frac{3}{4} = \frac{3}{12} = \frac{1}{4}$. Lalu $\frac{1}{2} + \frac{1}{4} = \frac{2}{4} + \frac{1}{4} = \frac{3}{4}$.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $\frac{1}{2} + \frac{1}{3} = \frac{2}{5}$ | potongan berbeda ukuran tidak bisa dijumlah langsung | $\frac{3}{6} + \frac{2}{6} = \frac{5}{6}$ |
| $\frac{2}{3} + \frac{2}{3} = \frac{4}{6}$ | penyebut ikut dijumlahkan | $\frac{4}{3}$; penyebut tetap |
| Menukar pecahan pertama saat membagi | yang ditukar adalah pecahan pembagi | $\frac{a}{b} \div \frac{c}{d} = \frac{a}{b} \times \frac{d}{c}$ |
| Menganggap $1\frac{3}{4} = 1 \times \frac{3}{4}$ | pecahan campuran berarti dijumlah | $1\frac{3}{4} = \frac{7}{4}$ |
| Mengira $\frac{1}{3}$ lebih besar dari $\frac{1}{2}$ karena 3 > 2 | penyebut besar berarti potongan kecil | $\frac{1}{3} < \frac{1}{2}$ |
| Menulis $\frac{1}{3} = 0{,}33$ lalu memakainya sebagai nilai pasti | itu hanya pendekatan | tulis $0{,}\overline{3}$ atau simpan sebagai pecahan |
| Membulatkan di setiap langkah | galat menumpuk | bulatkan hanya hasil akhir |
| Menyederhanakan dengan mengurangi (mis. $\frac{18}{24} \to \frac{12}{18}$) | pecahan senilai dibentuk dengan kali atau bagi, bukan kurang | bagi pembilang dan penyebut dengan FPB |

## Hubungan dengan Materi Berikutnya

- **Part 4** memakai pecahan dan desimal untuk persentase, rasio, dan proporsi.
- **Part 5** memperluas pangkat ke pangkat negatif dan pecahan, yang bergantung pada kebalikan.
- **Part 7** memakai operasi pecahan untuk menyelesaikan persamaan.
- **Level 3** menyatakan peluang sebagai pecahan dan desimal antara 0 dan 1.

## Hubungan dengan Machine Learning

- **Peluang** dan **akurasi** adalah pecahan: jika 45 dari 60 prediksi benar, akurasinya $\frac{45}{60} = \frac{3}{4} = 0{,}75$.
- **Laju belajar** (*learning rate*), parameter penting dalam pelatihan model, biasanya desimal kecil seperti $0{,}01 = \frac{1}{100}$.
- **Rata-rata** adalah pembagian jumlah nilai oleh banyaknya nilai, jadi hasilnya sering berupa pecahan.
- **Komputer menyimpan desimal sebagai pendekatan**, sehingga pembulatan dan galat selalu ada di balik layar. Pustaka seperti NumPy memakai pendekatan yang sama dengan `float` Python.

### Python: pecahan, desimal, dan pembulatan

Modul `fractions` menyimpan pecahan **tepat**, tanpa pembulatan.

```python
from fractions import Fraction

a = Fraction(1, 2)   # 1/2
b = Fraction(1, 3)   # 1/3
print(a + b)         # 5/6
print(Fraction(2, 3) / Fraction(4, 5))   # 5/6
print(Fraction(18, 24))                  # 3/4, otomatis disederhanakan
print(Fraction(3, 4) < Fraction(5, 6))   # True
print(float(Fraction(3, 4)))             # 0.75
```

Bandingkan dengan `float`, yang berupa desimal pendekatan:

```python
print(1 / 3)          # 0.3333333333333333
print((1 / 3) * 3)    # 1.0
print(round(3.14159, 2))   # 3.14
print(round(3.14159, 4))   # 3.1416
print(round(2.675, 2))     # 2.67
```

Hasil terakhir mungkin mengejutkan: kita mengharapkan 2,68 dengan aturan pembulatan di atas, tetapi komputer menyimpan 2,675 sebagai nilai yang sedikit di bawah 2,675, sehingga dibulatkan ke 2,67. Ini contoh nyata bahwa `float` adalah pendekatan.

## Latihan Mandiri

1. Ubah $\frac{17}{5}$ menjadi pecahan campuran, dan $4\frac{3}{8}$ menjadi pecahan biasa.
2. Sederhanakan: (a) $\frac{36}{48}$ (b) $\frac{45}{60}$
3. Mana yang lebih besar, $\frac{5}{8}$ atau $\frac{3}{5}$?
4. Hitung: (a) $\frac{2}{5} + \frac{3}{10}$ (b) $\frac{5}{6} - \frac{1}{4}$
5. Hitung $\frac{3}{4} \times \frac{2}{9}$.
6. Hitung $\frac{5}{8} \div \frac{3}{4}$.
7. Ubah ke desimal: (a) $\frac{7}{8}$ (b) $\frac{5}{6}$
8. Ubah ke pecahan paling sederhana: (a) $0{,}64$ (b) $3{,}5$
9. Tentukan (tanpa menghitung panjang) apakah desimalnya berhingga atau berulang: (a) $\frac{3}{16}$ (b) $\frac{7}{15}$ (c) $\frac{9}{20}$ (d) $\frac{4}{11}$
10. Bulatkan $4{,}7368$ ke: (a) bilangan bulat (b) satu desimal (c) dua desimal

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $17 \div 5 = 3$ sisa $2$, jadi $3\frac{2}{5}$. Untuk $4\frac{3}{8}$: $4 \times 8 + 3 = 35$, jadi $\frac{35}{8}$.

??? success "Soal 2"
    (a) FPB(36, 48) = 12. $\frac{36 \div 12}{48 \div 12} = \frac{3}{4}$.
    (b) FPB(45, 60) = 15. $\frac{45 \div 15}{60 \div 15} = \frac{3}{4}$.

??? success "Soal 3"
    KPK(8, 5) = 40. $\frac{5}{8} = \frac{25}{40}$ dan $\frac{3}{5} = \frac{24}{40}$. Karena $25 > 24$, $\frac{5}{8}$ lebih besar. Periksa: $0{,}625 > 0{,}6$.

??? success "Soal 4"
    (a) $\frac{2}{5} = \frac{4}{10}$, maka $\frac{4}{10} + \frac{3}{10} = \frac{7}{10}$.
    (b) KPK(6, 4) = 12. $\frac{10}{12} - \frac{3}{12} = \frac{7}{12}$.

??? success "Soal 5"
    $\frac{3 \times 2}{4 \times 9} = \frac{6}{36} = \frac{1}{6}$.

??? success "Soal 6"
    $\frac{5}{8} \times \frac{4}{3} = \frac{20}{24} = \frac{5}{6}$.

??? success "Soal 7"
    (a) $7 \div 8 = 0{,}875$.
    (b) $5 \div 6 = 0{,}8333\ldots$ (angka 3 berulang), ditulis $0{,}8\overline{3}$.

??? success "Soal 8"
    (a) $0{,}64 = \frac{64}{100}$; FPB 4; $\frac{16}{25}$.
    (b) $3{,}5 = \frac{35}{10} = \frac{7}{2} = 3\frac{1}{2}$.

??? success "Soal 9"
    (a) $16 = 2^4$ (hanya faktor 2), **berhingga** ($0{,}1875$).
    (b) $15 = 3 \times 5$, ada faktor 3, **berulang** ($0{,}4666\ldots$).
    (c) $20 = 2 \times 2 \times 5$, **berhingga** ($0{,}45$).
    (d) 11 prima dan bukan 2 atau 5, **berulang** ($0{,}\overline{36}$).

??? success "Soal 10"
    (a) Angka setelah koma adalah 7 (5 atau lebih), jadi 4 naik: **5**.
    (b) Angka kedua setelah koma adalah 3 (kurang dari 5): **4,7**.
    (c) Dua desimal berarti mempertahankan 4,73. Angka berikutnya adalah 6 (5 atau lebih), jadi angka 3 naik menjadi 4. Hasil: **4,74**.

## Ringkasan Materi

- Pecahan $\frac{a}{b}$ adalah bagian dari keseluruhan sekaligus hasil bagi $a \div b$, dengan $b \neq 0$.
- Pecahan senilai dibuat dengan mengalikan atau membagi pembilang **dan** penyebut dengan bilangan yang sama.
- Menjumlah atau mengurang: samakan penyebut dulu. Mengalikan: pembilang dikali pembilang, penyebut dikali penyebut. Membagi: kalikan dengan kebalikan pecahan pembagi.
- Pecahan menjadi desimal dengan pembagian; desimal menjadi pecahan dengan nilai tempat (penyebut 10, 100, ...).
- Desimal pecahan sederhana berhingga jika penyebutnya hanya berfaktor 2 dan 5; selain itu berulang.
- Pembulatan membuat galat; bulatkan hanya di akhir.

## Checklist Penguasaan

- [ ] Menjelaskan arti pembilang dan penyebut serta mengapa penyebut tidak boleh nol.
- [ ] Mengubah pecahan biasa dan campuran dua arah.
- [ ] Menyederhanakan pecahan dengan FPB dan membandingkan pecahan dengan KPK.
- [ ] Menjelaskan mengapa penyebut harus disamakan sebelum dijumlahkan.
- [ ] Mengalikan dan membagi pecahan dengan benar.
- [ ] Mengubah pecahan ke desimal dan desimal ke pecahan.
- [ ] Menebak desimal berhingga atau berulang dari penyebut.
- [ ] Membulatkan bilangan dan menjelaskan mengapa galat bisa menumpuk.
