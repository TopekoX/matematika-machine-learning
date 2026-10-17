# Part 6 — Nilai Absolut dan Notasi Ilmiah

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan nilai absolut sebagai jarak dari nol dan menghitungnya;
- memakai nilai absolut untuk menghitung jarak antara dua titik pada garis bilangan;
- membedakan nilai absolut dari tanda bilangan;
- menulis dan membaca bilangan sangat besar atau sangat kecil dalam notasi ilmiah;
- menghitung dengan pangkat sepuluh dan notasi ilmiah;
- mengenali notasi ilmiah dalam komputasi dan Machine Learning.

## Prasyarat

[Part 1](01-mengenal-bilangan.md) (garis bilangan dan bilangan negatif), [Part 2](02-operasi-aritmatika-dasar.md) (operasi dasar), dan [Part 5](05-pangkat-dan-akar.md) (pangkat, termasuk pangkat negatif).

## Mengapa Materi Ini Penting?

Dalam Machine Learning, kamu akan sering mengukur **seberapa jauh** dua nilai (tanpa peduli arahnya), dan itulah yang dilakukan nilai absolut. Kamu juga akan bertemu bilangan yang ekstrem: model dengan miliaran parameter, laju belajar seperti $0{,}0001$, atau peluang yang nyaris nol. Notasi ilmiah membuat bilangan semacam itu ringkas dan mudah dibandingkan.

## Penjelasan Konsep dari Dasar

### 1. Pengertian Nilai Absolut

**Nilai absolut** (*absolute value*) dari suatu bilangan adalah **jaraknya dari nol** pada garis bilangan. Jarak tidak pernah negatif, jadi nilai absolut tidak pernah negatif.

Lambangnya dua garis tegak: $|x|$, dibaca "nilai absolut x".

- $|5| = 5$, karena 5 berjarak 5 langkah dari nol.
- $|-5| = 5$, karena -5 juga berjarak 5 langkah dari nol (ke arah kiri).
- $|0| = 0$.

Definisi formalnya memakai dua kasus:

$$
|x| =
\begin{cases}
x & \text{jika } x \geq 0 \\
-x & \text{jika } x < 0
\end{cases}
$$

Untuk bilangan negatif, $-x$ berarti "lawan dari $x$", dan lawan dari bilangan negatif adalah positif. Contoh: $x = -5$ menghasilkan $-x = -(-5) = 5$.

### 2. Nilai Absolut pada Garis Bilangan

Nilai absolut dari **selisih** dua bilangan memberi jarak antara keduanya:

$$
\text{jarak antara } a \text{ dan } b = |a - b|
$$

Contoh: jarak antara $-2$ dan $7$ adalah $|7 - (-2)| = |9| = 9$. Hasilnya sama jika urutan ditukar: $|-2 - 7| = |-9| = 9$. Jarak tidak bergantung pada urutan, sedangkan selisih biasa bergantung pada urutan.

```text
   -4   -3   -2   -1    0    1    2    3    4    5    6    7
────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼───▶
              <---------------- 9 langkah ----------------->
              ^                                            ^
             -2                                           7
```

### 3. Perbedaan Nilai Absolut dan Tanda Bilangan

Sering muncul anggapan bahwa nilai absolut berarti "hapus tanda minus". Itu benar hanya untuk bilangan yang **sudah berupa angka**, tetapi menyesatkan untuk ekspresi.

- $|-3| = 3$ (tanda minus pada bilangan menghilang).
- $-|-3| = -3$. Dihitung dari dalam: $|-3| = 3$, lalu minus di depannya membuat hasilnya $-3$. Nilai absolut tidak pernah negatif, tetapi tanda minus **di luar** garis tegak tetap berlaku.
- $|x - 3|$ tidak sama dengan $x - 3$ kecuali $x \geq 3$. Untuk $x = 1$: $|1 - 3| = |-2| = 2$.
- $|-x| = |x|$ untuk semua $x$.
- $|a + b| \neq |a| + |b|$ pada umumnya. Contoh: $|3 + (-5)| = |-2| = 2$, sedangkan $|3| + |-5| = 3 + 5 = 8$.

Hubungan dengan akar (Part 5): $\sqrt{x^2} = |x|$. Contoh: $\sqrt{(-3)^2} = \sqrt{9} = 3 = |-3|$.

**Pengantar.** Persamaan $|x| = 3$ berarti "x berjarak 3 dari nol". Ada dua titik yang memenuhi: $x = 3$ dan $x = -3$. Sedangkan $|x| < 2$ berarti "x berjarak kurang dari 2 dari nol", yaitu semua bilangan antara $-2$ dan $2$. Penyelesaian lengkapnya dibahas di Level 1.

### 4. Pengertian Notasi Ilmiah

**Notasi ilmiah** (*scientific notation*) adalah cara menulis bilangan sebagai hasil kali bilangan sedang dengan pangkat sepuluh:

$$
a \times 10^n
$$

Syarat bentuk baku:

- $a$ disebut **koefisien** (atau mantissa), dengan $1 \leq |a| < 10$. Artinya hanya ada satu angka bukan nol di depan koma.
- $n$ adalah **eksponen** berupa bilangan bulat. Eksponen menyatakan berapa kali koma digeser.

Contoh: $45.000.000 = 4{,}5 \times 10^7$. Koefisiennya $4{,}5$ (antara 1 dan 10), dan eksponennya 7.

### 5. Notasi Ilmiah untuk Angka Besar

Untuk bilangan besar, geser koma ke **kiri** sampai tersisa satu angka bukan nol di depan koma. Banyaknya pergeseran menjadi eksponen positif.

$45.000.000$: koma ada di ujung kanan (setelah angka nol terakhir). Geser koma 7 tempat ke kiri menjadi $4{,}5$. Maka $4{,}5 \times 10^7$.

Cek: $4{,}5 \times 10^7 = 4{,}5 \times 10.000.000 = 45.000.000$.

Contoh lain: kecepatan cahaya $299.792.458$ m/s kira-kira $3{,}0 \times 10^8$ m/s (pendekatan, dibulatkan menjadi dua angka).

### 6. Notasi Ilmiah untuk Angka Kecil

Untuk bilangan kecil (kurang dari 1), geser koma ke **kanan** sampai ada satu angka bukan nol di depan koma. Banyaknya pergeseran menjadi eksponen **negatif**.

$0{,}00032$: geser koma 4 tempat ke kanan menjadi $3{,}2$. Maka $3{,}2 \times 10^{-4}$.

Cek: $10^{-4} = 0{,}0001$, dan $3{,}2 \times 0{,}0001 = 0{,}00032$.

Eksponen negatif bukan berarti bilangan negatif, melainkan bilangan yang kecil.

### 7. Pangkat Sepuluh

Pangkat sepuluh adalah dasar notasi ilmiah. Setiap kenaikan eksponen 1 membuat nilai 10 kali lebih besar.

| Pangkat | Nilai | Nama |
|---|---|---|
| $10^{-3}$ | $0{,}001$ | seperseribu |
| $10^{-2}$ | $0{,}01$ | seperseratus |
| $10^{-1}$ | $0{,}1$ | sepersepuluh |
| $10^{0}$ | $1$ | satu |
| $10^{1}$ | $10$ | sepuluh |
| $10^{3}$ | $1.000$ | seribu |
| $10^{6}$ | $1.000.000$ | sejuta |
| $10^{9}$ | $1.000.000.000$ | semiliar |
| $10^{12}$ | $1.000.000.000.000$ | setriliun |

**Menghitung dengan notasi ilmiah.**

- **Kali:** kalikan koefisien, jumlahkan eksponen. $(2 \times 10^3) \times (3 \times 10^4) = 6 \times 10^7$.
- **Bagi:** bagi koefisien, kurangkan eksponen. $(8 \times 10^5) \div (2 \times 10^2) = 4 \times 10^3$.
- **Rapikan hasil:** jika koefisien tidak lagi antara 1 dan 10, geser koma. $(6 \times 10^8) \times (5 \times 10^3) = 30 \times 10^{11} = 3 \times 10^{12}$.
- **Tambah dan kurang:** samakan dulu eksponennya. $3 \times 10^4 + 5 \times 10^3 = 3 \times 10^4 + 0{,}5 \times 10^4 = 3{,}5 \times 10^4$.

**Membandingkan.** Untuk bilangan positif berbentuk baku, bandingkan eksponen lebih dulu; jika sama, bandingkan koefisien. $9 \times 10^5 > 2 \times 10^4$, dan $6 \times 10^3 > 4 \times 10^3$.

### 8. Contoh Penggunaan dalam Komputasi dan Machine Learning

Python dan banyak bahasa pemrograman menulis notasi ilmiah dengan huruf `e`:

- `3e8` berarti $3 \times 10^8$.
- `1.5e-3` berarti $1{,}5 \times 10^{-3} = 0{,}0015$.

Huruf `e` di sini berarti "kali sepuluh pangkat" dan **tidak ada hubungannya** dengan bilangan $e \approx 2{,}718$.

Contoh penggunaan:

- Jumlah parameter model bahasa besar sering dalam miliaran, misalnya $7 \times 10^9$.
- Laju belajar umum: $10^{-3}$, $10^{-4}$.
- Selisih kecil antara dua nilai, yang diabaikan jika di bawah $10^{-9}$.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Nilai absolut (*absolute value*) | jarak dari nol | $\lvert -5 \rvert = 5$ |
| Jarak (*distance*) | selisih tanpa arah | $\lvert a - b \rvert$ |
| Lawan | bilangan dengan tanda dibalik | lawan 5 adalah -5 |
| Notasi ilmiah | $a \times 10^n$ dengan $1 \leq \lvert a \rvert < 10$ | $4{,}5 \times 10^7$ |
| Koefisien / mantissa | bilangan sebelum $\times 10^n$ | 4,5 |
| Eksponen | pangkat dari 10 | 7 |
| Pangkat sepuluh | 10 dipangkatkan bilangan bulat | $10^3 = 1.000$ |
| Orde besaran (*order of magnitude*) | perkiraan kasar ukuran, dari eksponen | $10^6$ (jutaan) |

## Rumus dan Penjelasan Setiap Simbol

**Nilai absolut:**

$$
|x| =
\begin{cases}
x & x \geq 0 \\
-x & x < 0
\end{cases}
$$

- $x$ adalah bilangan real apa pun.
- Kurung kurawal besar berarti "pilih salah satu baris sesuai kondisi".
- $\geq$ dibaca "lebih besar atau sama dengan"; $<$ dibaca "lebih kecil dari".

**Jarak dua titik di garis bilangan:**

$$
d(a, b) = |a - b|
$$

- $a$ dan $b$ adalah dua bilangan.
- $d(a, b)$ dibaca "jarak antara $a$ dan $b$".
- Berlaku untuk bilangan real apa pun, dan $d(a, b) = d(b, a)$.

**Notasi ilmiah:**

$$
N = a \times 10^n, \qquad 1 \leq |a| < 10, \quad n \in \mathbb{Z}
$$

- $N$ adalah bilangan yang dinyatakan.
- $a$ adalah koefisien; $n \in \mathbb{Z}$ berarti $n$ adalah bilangan bulat.
- $n > 0$: bilangan besar (koma digeser $n$ tempat ke kanan dari $a$). $n < 0$: bilangan kecil (koma digeser $|n|$ tempat ke kiri). $n = 0$: bilangan antara 1 dan 10.
- Contoh: $N = 3{,}2 \times 10^{-4}$ memiliki $a = 3{,}2$ dan $n = -4$, jadi $N = 0{,}00032$.

**Aturan pangkat sepuluh** (dari Part 5):

$$
10^m \times 10^n = 10^{m+n}, \qquad \frac{10^m}{10^n} = 10^{m-n}
$$

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Nilai absolut sebagai jarak

Hitung $|x - 3|$ untuk $x = 1$, $x = 3$, dan $x = 8$. Tafsirkan sebagai jarak.

1. $x = 1$: $|1 - 3| = |-2| = 2$. Jarak titik 1 ke titik 3 adalah 2.
2. $x = 3$: $|3 - 3| = |0| = 0$. Titik 3 berjarak 0 dari dirinya sendiri.
3. $x = 8$: $|8 - 3| = |5| = 5$. Jarak titik 8 ke titik 3 adalah 5.

Pola: $|x - 3|$ adalah jarak $x$ ke 3.

### Contoh 2 — Mengubah ke notasi ilmiah

Tulis $602.000$ dan $0{,}0000091$ dalam notasi ilmiah.

1. $602.000$: koma ada setelah angka terakhir. Geser ke kiri sampai setelah angka 6: $6{,}02$. Pergeseran 5 tempat. Jadi $6{,}02 \times 10^5$.
2. $0{,}0000091$: geser koma ke kanan sampai setelah angka 9. Pergeseran 6 tempat. Jadi $9{,}1 \times 10^{-6}$.

Cek yang kedua: $10^{-6} = 0{,}000001$, dan $9{,}1 \times 0{,}000001 = 0{,}0000091$.

### Contoh 3 — Perkalian dengan merapikan koefisien

Hitung $(6 \times 10^8) \times (5 \times 10^3)$.

1. Kalikan koefisien: $6 \times 5 = 30$.
2. Jumlahkan eksponen: $8 + 3 = 11$. Hasil sementara $30 \times 10^{11}$.
3. Koefisien 30 tidak berada antara 1 dan 10. Tulis $30 = 3 \times 10^1$.
4. Gabungkan: $3 \times 10^1 \times 10^{11} = 3 \times 10^{12}$.

Cek: $600.000.000 \times 5.000 = 3.000.000.000.000 = 3 \times 10^{12}$.

### Contoh 4 — Penjumlahan dan memori model

Sebuah model punya $7 \times 10^9$ parameter. Jika tiap parameter disimpan dalam 4 byte, berapa byte total? Lalu hitung $3 \times 10^4 + 5 \times 10^3$.

1. Total byte: $7 \times 10^9 \times 4 = 28 \times 10^9$.
2. Rapikan: $28 \times 10^9 = 2{,}8 \times 10^{10}$ byte, atau sekitar 28 miliar byte (28 GB, memakai $1\ \text{GB} = 10^9$ byte).
3. Penjumlahan: eksponen berbeda, samakan dulu. $5 \times 10^3 = 0{,}5 \times 10^4$.
4. $3 \times 10^4 + 0{,}5 \times 10^4 = 3{,}5 \times 10^4$.

Cek: $30.000 + 5.000 = 35.000 = 3{,}5 \times 10^4$.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** $|-12| = 12$ dan $-|12| = -12$.

**Sedang.** Ubah $3{,}7 \times 10^4$ ke bentuk biasa: geser koma 4 tempat ke kanan, hasilnya $37.000$.

**Sedang.** Ubah $8 \times 10^{-3}$ ke bentuk biasa: geser koma 3 tempat ke kiri, hasilnya $0{,}008$.

**Menantang.** Hitung $\dfrac{9 \times 10^{-2}}{3 \times 10^{-5}}$. Bagi koefisien: $9 \div 3 = 3$. Kurangkan eksponen: $-2 - (-5) = -2 + 5 = 3$. Hasil $3 \times 10^3 = 3.000$. Cek: $0{,}09 \div 0{,}00003 = 3.000$.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $\lvert x \rvert$ selalu positif | untuk $x = 0$ hasilnya 0 | nilai absolut tidak negatif |
| $-\lvert -3 \rvert = 3$ | minus di luar tetap berlaku | $-3$ |
| $\lvert a + b \rvert = \lvert a \rvert + \lvert b \rvert$ | hanya benar bila tandanya sama | $\lvert 3 + (-5) \rvert = 2$, bukan 8 |
| $\lvert x - 3 \rvert = x - 3$ | hanya benar bila $x \geq 3$ | $\lvert 1 - 3 \rvert = 2$ |
| $12 \times 10^5$ disebut notasi ilmiah baku | koefisien harus antara 1 dan 10 | $1{,}2 \times 10^6$ |
| $0{,}0004 = 4 \times 10^4$ | bilangan kecil memakai eksponen negatif | $4 \times 10^{-4}$ |
| $10^{-3} = -1.000$ | eksponen negatif berarti kecil, bukan negatif | $0{,}001$ |
| Menambah koefisien tanpa menyamakan eksponen | satuan berbeda | $3 \times 10^4 + 5 \times 10^3 = 3{,}5 \times 10^4$ |
| Mengira `1e-3` di Python memakai bilangan $e$ | `e` berarti "kali sepuluh pangkat" | `1e-3` = $0{,}001$ |
| Mengira $2 \times 10^3 \times 3 \times 10^4 = 6 \times 10^{12}$ | eksponen dijumlah, bukan dikali | $6 \times 10^7$ |

## Hubungan dengan Materi Berikutnya

- **Part 7 dan Part 8** memakai nilai absolut dan notasi dalam penulisan rumus.
- **Level 1** memakai nilai absolut untuk persamaan dan pertidaksamaan, dan eksponen untuk fungsi eksponensial dan logaritma.
- **Level 2** memakai jarak dan nilai absolut untuk norma (ukuran panjang vektor) dan batas toleransi dalam limit.
- **Level 4** memakai nilai absolut pada ukuran kesalahan (misalnya galat absolut) dan regularisasi.

## Hubungan dengan Machine Learning

- **Galat absolut.** Ukuran seperti MAE (*mean absolute error*) memakai $|\text{prediksi} - \text{target}|$ untuk mengukur seberapa jauh prediksi dari kenyataan, tanpa peduli arah.
- **Toleransi.** Perbandingan dua bilangan desimal biasanya memeriksa apakah $|a - b|$ lebih kecil dari ambang kecil seperti $10^{-9}$.
- **Skala bilangan.** Parameter model dalam miliaran ($10^9$), laju belajar kecil ($10^{-4}$), dan peluang yang sangat kecil adalah sehari-hari.
- **Batas komputer.** Bilangan `float` punya batas atas dan bawah; melewatinya menghasilkan `inf` (tak hingga) atau 0.

### Python: nilai absolut dan notasi ilmiah

Fungsi `abs` menghitung nilai absolut:

```python
print(abs(-5))     # 5
print(abs(3 - 8))  # 5, jarak antara 3 dan 8
print(abs(-3.5))   # 3.5
print(-abs(-3))    # -3
```

Notasi ilmiah memakai `e`:

```python
print(3e8)       # 300000000.0
print(1.5e-3)    # 0.0015
print(10 ** 6)   # 1000000
print(10 ** -3)  # 0.001
```

Mengubah bilangan menjadi teks notasi ilmiah dengan format `.2e` (dua angka di belakang koma pada koefisien):

```python
x = 45_000_000
print(f"{x:.2e}")   # 4.50e+07
y = 0.00032
print(f"{y:.1e}")   # 3.2e-04
```

Tanda `f"..."` membuat teks yang memuat nilai dari variabel di dalam kurung kurawal. Garis bawah pada `45_000_000` hanya pemisah agar mudah dibaca.

Perkalian pada contoh 3 dan batas komputer:

```python
print((6e8) * (5e3))   # 3000000000000.0, yaitu 3 x 10^12
print(1e308 * 10)      # inf, terlalu besar
print(1e-400)          # 0.0, terlalu kecil
```

Gunakan nilai absolut untuk membandingkan desimal:

```python
a = 0.1 + 0.2
b = 0.3
print(a == b)                  # False
print(abs(a - b) < 1e-9)       # True
```

## Latihan Mandiri

1. Hitung: (a) $\lvert -12 \rvert$ (b) $\lvert 7 \rvert$ (c) $\lvert 0 \rvert$ (d) $\lvert -3{,}5 \rvert$ (e) $-\lvert -4 \rvert$
2. Hitung jarak antara: (a) $8$ dan $-3$ (b) $-5$ dan $-9$ (c) $2$ dan $10$
3. Benar atau salah (beri alasan atau contoh): (a) $\lvert x \rvert$ selalu positif. (b) $\lvert -x \rvert = \lvert x \rvert$. (c) $\lvert a + b \rvert = \lvert a \rvert + \lvert b \rvert$ untuk semua $a, b$. (d) $\lvert x \rvert = x$ jika $x \geq 0$.
4. Hitung $\lvert 3 - 8 \rvert - \lvert -2 \rvert$.
5. Tulis dalam notasi ilmiah: (a) $45.000.000$ (b) $0{,}00032$ (c) $602.000$ (d) $0{,}0000091$
6. Tulis dalam bentuk biasa: (a) $3{,}7 \times 10^4$ (b) $8 \times 10^{-3}$ (c) $1{,}25 \times 10^2$ (d) $6{,}1 \times 10^{-1}$
7. Hitung dan tulis dalam notasi ilmiah baku: (a) $(2 \times 10^3) \times (3 \times 10^4)$ (b) $(6 \times 10^8) \times (5 \times 10^3)$ (c) $(8 \times 10^5) \div (2 \times 10^2)$
8. Hitung $3 \times 10^4 + 5 \times 10^3$.
9. Mana yang lebih besar: (a) $4{,}2 \times 10^5$ atau $9{,}8 \times 10^4$ (b) $2 \times 10^{-3}$ atau $5 \times 10^{-4}$
10. Sebuah model punya $7 \times 10^9$ parameter, dan tiap parameter disimpan dalam 4 byte. Berapa total byte, dalam notasi ilmiah?

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) 12. (b) 7. (c) 0. (d) 3,5. (e) $-\lvert -4 \rvert = -4$ (nilai absolut dihitung dulu, baru diberi minus).

??? success "Soal 2"
    (a) $\lvert 8 - (-3) \rvert = \lvert 11 \rvert = 11$.
    (b) $\lvert -5 - (-9) \rvert = \lvert 4 \rvert = 4$.
    (c) $\lvert 2 - 10 \rvert = \lvert -8 \rvert = 8$.

??? success "Soal 3"
    (a) **Salah.** $\lvert 0 \rvert = 0$ bukan positif; yang benar, nilai absolut tidak negatif.
    (b) **Benar.** Kedua bilangan berjarak sama dari nol.
    (c) **Salah.** Contoh $a = 3$, $b = -5$: $\lvert -2 \rvert = 2$, sedangkan $3 + 5 = 8$.
    (d) **Benar**, sesuai definisi.

??? success "Soal 4"
    $\lvert 3 - 8 \rvert = \lvert -5 \rvert = 5$. $\lvert -2 \rvert = 2$. Jadi $5 - 2 = 3$.

??? success "Soal 5"
    (a) Geser koma 7 tempat ke kiri: $4{,}5 \times 10^7$.
    (b) Geser koma 4 tempat ke kanan: $3{,}2 \times 10^{-4}$.
    (c) Geser koma 5 tempat ke kiri: $6{,}02 \times 10^5$.
    (d) Geser koma 6 tempat ke kanan: $9{,}1 \times 10^{-6}$.

??? success "Soal 6"
    (a) $37.000$. (b) $0{,}008$. (c) $125$. (d) $0{,}61$.

??? success "Soal 7"
    (a) $2 \times 3 = 6$ dan $3 + 4 = 7$, jadi $6 \times 10^7$.
    (b) $6 \times 5 = 30$ dan $8 + 3 = 11$: $30 \times 10^{11} = 3 \times 10^{12}$.
    (c) $8 \div 2 = 4$ dan $5 - 2 = 3$: $4 \times 10^3$.

??? success "Soal 8"
    $5 \times 10^3 = 0{,}5 \times 10^4$. Maka $3 \times 10^4 + 0{,}5 \times 10^4 = 3{,}5 \times 10^4$ (yaitu $35.000$).

??? success "Soal 9"
    (a) $4{,}2 \times 10^5 = 420.000$ dan $9{,}8 \times 10^4 = 98.000$. Yang pertama lebih besar (eksponen 5 lebih besar dari 4).
    (b) $2 \times 10^{-3} = 0{,}002$ dan $5 \times 10^{-4} = 0{,}0005$. Yang pertama lebih besar (eksponen $-3$ lebih besar dari $-4$).

??? success "Soal 10"
    $7 \times 10^9 \times 4 = 28 \times 10^9 = 2{,}8 \times 10^{10}$ byte.

## Ringkasan Materi

- $\lvert x \rvert$ adalah jarak $x$ dari nol; nilainya tidak pernah negatif. Untuk $x < 0$, $\lvert x \rvert = -x$.
- Jarak antara $a$ dan $b$ adalah $\lvert a - b \rvert$, tidak bergantung pada urutan.
- $\lvert a + b \rvert \neq \lvert a \rvert + \lvert b \rvert$ pada umumnya, dan $\lvert x - 3 \rvert \neq x - 3$ kecuali $x \geq 3$.
- Notasi ilmiah: $a \times 10^n$ dengan $1 \leq \lvert a \rvert < 10$ dan $n$ bilangan bulat. Eksponen positif untuk bilangan besar, negatif untuk bilangan kecil.
- Kali: jumlahkan eksponen; bagi: kurangkan eksponen; tambah/kurang: samakan eksponen dulu; lalu rapikan koefisien.
- Di Python, `1e-3` berarti $10^{-3}$ dan tidak terkait bilangan $e$.

## Checklist Penguasaan

- [ ] Menjelaskan nilai absolut sebagai jarak dan menghitungnya.
- [ ] Menghitung jarak dua titik dengan $\lvert a - b \rvert$.
- [ ] Menjelaskan mengapa $-\lvert -3 \rvert = -3$ dan $\lvert a + b \rvert \neq \lvert a \rvert + \lvert b \rvert$.
- [ ] Mengubah bilangan besar dan kecil ke notasi ilmiah baku, dan sebaliknya.
- [ ] Mengalikan, membagi, dan menjumlahkan bilangan dalam notasi ilmiah.
- [ ] Membandingkan dua bilangan dalam notasi ilmiah.
- [ ] Membaca `3e8` dan `1.5e-3` di Python dengan benar.
