# Part 5 — Pangkat dan Akar

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan arti pangkat, basis, dan eksponen, serta menghitung pangkat bulat positif;
- memakai aturan perkalian, pembagian, dan pangkat dari pangkat beserta syarat berlakunya;
- menjelaskan pangkat nol dan pangkat negatif beserta alasannya;
- menghitung akar kuadrat dan akar pangkat tiga, dan membedakan nilai eksak dari pendekatan desimal;
- menjelaskan hubungan pangkat dan akar serta pangkat pecahan sebagai pengantar;
- menghindari kesalahan umum pada operasi pangkat dan akar.

## Prasyarat

[Part 2](02-operasi-aritmatika-dasar.md) (perkalian, urutan operasi, aturan tanda) dan [Part 3](03-pecahan-dan-desimal.md) (pecahan, kebalikan, desimal).

## Mengapa Materi Ini Penting?

Pangkat dan akar muncul hampir di setiap rumus Machine Learning: kesalahan dikuadratkan, jarak antartitik memakai akar, laju belajar ditulis seperti $10^{-3}$, dan fungsi eksponensial menjadi dasar banyak model. Kamu harus nyaman membaca dan menyederhanakannya.

## Penjelasan Konsep dari Dasar

### 1. Pengertian Pangkat

**Pangkat** (*power*) adalah perkalian berulang dari bilangan yang sama. Dalam $a^n$:

- $a$ disebut **basis** (*base*), yaitu bilangan yang dikalikan;
- $n$ disebut **eksponen** (*exponent*) atau pangkat, yaitu berapa kali basis ditulis sebagai faktor.

$$
a^n = \underbrace{a \times a \times \cdots \times a}_{n \text{ faktor}}
$$

Contoh: $2^3 = 2 \times 2 \times 2 = 8$. Dibaca "dua pangkat tiga". Perhatikan bahwa $2^3 \neq 2 \times 3 = 6$; eksponen menyatakan **banyaknya faktor**, bukan pengali.

### 2. Pangkat Dua dan Pangkat Tiga

Pangkat dua disebut **kuadrat** (luas persegi dengan sisi $a$ adalah $a^2$). Pangkat tiga disebut **kubik** (volume kubus dengan rusuk $a$ adalah $a^3$).

| $n$ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| $n^2$ | 1 | 4 | 9 | 16 | 25 | 36 | 49 | 64 | 81 | 100 |
| $n^3$ | 1 | 8 | 27 | 64 | 125 | 216 | 343 | 512 | 729 | 1000 |

Dengan basis negatif, tanda hasil bergantung pada eksponen:

- $(-3)^2 = (-3) \times (-3) = 9$ (jumlah faktor negatif genap, hasil positif);
- $(-2)^3 = (-2) \times (-2) \times (-2) = -8$ (jumlah faktor negatif ganjil, hasil negatif).

!!! warning "Kurung menentukan basis"
    $(-3)^2 = 9$, tetapi $-3^2 = -(3^2) = -9$. Tanpa kurung, pangkat dikerjakan lebih dulu daripada tanda minus.

### 3. Aturan Perkalian Pangkat

Jika **basisnya sama**, eksponen dijumlahkan:

$$
a^m \times a^n = a^{m+n}
$$

Alasannya: hitung faktornya. $2^3 \times 2^4 = (2 \times 2 \times 2) \times (2 \times 2 \times 2 \times 2)$ adalah 7 faktor 2, yaitu $2^7 = 128$.

Cek: $2^3 \times 2^4 = 8 \times 16 = 128$.

Aturan ini **hanya** berlaku untuk basis yang sama. $2^3 \times 3^2$ tidak bisa digabung menjadi satu pangkat. Hitung saja: $8 \times 9 = 72$.

### 4. Aturan Pembagian Pangkat

Jika basisnya sama dan tidak nol, eksponen dikurangkan:

$$
\frac{a^m}{a^n} = a^{m-n} \qquad (a \neq 0)
$$

Alasannya: faktor yang sama di pembilang dan penyebut saling menghapus. $\frac{2^5}{2^2} = \frac{2 \times 2 \times 2 \times 2 \times 2}{2 \times 2} = 2 \times 2 \times 2 = 2^3 = 8$.

Cek: $32 \div 4 = 8$.

### 5. Pangkat dari Pangkat

Ketika sebuah pangkat dipangkatkan lagi, eksponen **dikalikan**:

$$
(a^m)^n = a^{m \times n}
$$

Alasannya: $(2^3)^2 = 2^3 \times 2^3 = 2^{3+3} = 2^6 = 64$. Cek: $2^3 = 8$ dan $8^2 = 64$.

Dua aturan terkait untuk perkalian di dalam kurung:

$$
(a \times b)^n = a^n \times b^n, \qquad \left(\frac{a}{b}\right)^n = \frac{a^n}{b^n} \quad (b \neq 0)
$$

Contoh: $(2 \times 3)^2 = 6^2 = 36$ dan $2^2 \times 3^2 = 4 \times 9 = 36$.

!!! warning "Penjumlahan tidak bisa dibagi"
    $(a + b)^2 \neq a^2 + b^2$. Contoh: $(2 + 3)^2 = 25$ sedangkan $2^2 + 3^2 = 13$.

### 6. Pangkat Nol

Setiap bilangan bukan nol yang dipangkatkan nol bernilai 1:

$$
a^0 = 1 \qquad (a \neq 0)
$$

Alasannya: dengan aturan pembagian, $\frac{a^3}{a^3} = a^{3-3} = a^0$. Padahal sesuatu yang dibagi dirinya sendiri sama dengan 1. Maka $a^0 = 1$. Contoh: $5^0 = 1$ dan $(-7)^0 = 1$.

Nilai $0^0$ tidak dibahas di sini; berbagai bidang menggunakan konvensi berbeda, jadi anggap belum terdefinisi.

### 7. Pangkat Negatif

Eksponen negatif berarti **kebalikan**:

$$
a^{-n} = \frac{1}{a^n} \qquad (a \neq 0)
$$

Alasannya: lanjutkan pola aturan pembagian. $\frac{2^0}{2^3} = 2^{0-3} = 2^{-3}$. Di sisi lain, $\frac{2^0}{2^3} = \frac{1}{8}$. Jadi $2^{-3} = \frac{1}{8} = 0{,}125$.

Pola pangkat sepuluh: $10^{-1} = 0{,}1$; $10^{-2} = 0{,}01$; $10^{-3} = 0{,}001$. Eksponen negatif **tidak** membuat hasil negatif, melainkan kecil.

### 8. Pengertian Akar Kuadrat

**Akar kuadrat** (*square root*) dari $x$, ditulis $\sqrt{x}$, adalah bilangan **tidak negatif** yang jika dikuadratkan menghasilkan $x$. Ini adalah kebalikan dari kuadrat.

- $\sqrt{49} = 7$ karena $7^2 = 49$.
- $\sqrt{0} = 0$ dan $\sqrt{1} = 1$.
- $\sqrt{x}$ untuk $x < 0$ tidak ada dalam bilangan real (tidak ada bilangan real yang kuadratnya negatif).

Bilangan yang akarnya bulat disebut **kuadrat sempurna**: 1, 4, 9, 16, 25, ... Akar dari bilangan lain adalah irasional, misalnya $\sqrt{2}$ dan $\sqrt{20}$.

Catatan: $\sqrt{25} = 5$, bukan $\pm 5$. Lambang $\sqrt{\ }$ selalu bernilai tidak negatif. Persamaan yang menanyakan "bilangan mana yang kuadratnya 25" punya dua jawaban (5 dan $-5$), tetapi itu pertanyaan yang berbeda.

**Memperkirakan akar.** Perkirakan $\sqrt{20}$: $4^2 = 16$ dan $5^2 = 25$, jadi nilainya antara 4 dan 5. Coba $4{,}5^2 = 20{,}25$ (terlalu besar) dan $4{,}4^2 = 19{,}36$ (terlalu kecil). Coba $4{,}47^2 = 19{,}9809$ dan $4{,}48^2 = 20{,}0704$. Maka $\sqrt{20} \approx 4{,}47$.

### 9. Akar Pangkat Tiga

**Akar pangkat tiga** (*cube root*) dari $x$, ditulis $\sqrt[3]{x}$, adalah bilangan yang jika dipangkatkan tiga menghasilkan $x$.

- $\sqrt[3]{27} = 3$ karena $3^3 = 27$.
- $\sqrt[3]{125} = 5$.
- $\sqrt[3]{-8} = -2$ karena $(-2)^3 = -8$.

Berbeda dengan akar kuadrat, akar pangkat tiga **ada** untuk bilangan negatif, karena pangkat tiga dari bilangan negatif bisa negatif.

### 10. Hubungan Pangkat dan Akar

Akar adalah kebalikan pangkat, selama syaratnya terpenuhi:

$$
\left(\sqrt{x}\right)^2 = x \quad (x \geq 0), \qquad \sqrt{x^2} = \text{nilai tidak negatif dari } x
$$

Contoh: $\sqrt{(-3)^2} = \sqrt{9} = 3$, bukan $-3$.

Aturan perkalian akar (untuk bilangan tidak negatif):

$$
\sqrt{a \times b} = \sqrt{a} \times \sqrt{b}
$$

Contoh: $\sqrt{4 \times 9} = \sqrt{36} = 6$ dan $\sqrt{4} \times \sqrt{9} = 2 \times 3 = 6$.

Aturan ini dipakai untuk menyederhanakan akar. $\sqrt{50} = \sqrt{25 \times 2} = \sqrt{25} \times \sqrt{2} = 5\sqrt{2}$. Tulisan $5\sqrt{2}$ berarti $5 \times \sqrt{2}$.

!!! warning "Akar dari penjumlahan"
    $\sqrt{a + b} \neq \sqrt{a} + \sqrt{b}$. Contoh: $\sqrt{9 + 16} = \sqrt{25} = 5$, sedangkan $\sqrt{9} + \sqrt{16} = 3 + 4 = 7$.

**Nilai eksak dan pendekatan.** $5\sqrt{2}$ adalah nilai **eksak** (tepat). $7{,}07$ adalah **pendekatan desimal** ($5 \times 1{,}41421\ldots = 7{,}0710\ldots$). Simpan dalam bentuk eksak selama perhitungan dan ubah ke desimal di akhir, untuk menghindari galat yang menumpuk (Part 3).

### 11. Pangkat Pecahan sebagai Pengantar

Eksponen boleh berupa pecahan. Aturan:

$$
a^{1/2} = \sqrt{a}, \qquad a^{1/3} = \sqrt[3]{a}, \qquad a^{m/n} = \left(\sqrt[n]{a}\right)^m
$$

Mengapa $a^{1/2} = \sqrt{a}$? Dengan aturan pangkat dari pangkat, $\left(a^{1/2}\right)^2 = a^{1/2 \times 2} = a^1 = a$. Jadi $a^{1/2}$ adalah bilangan yang jika dikuadratkan menghasilkan $a$, yaitu persis definisi $\sqrt{a}$.

Contoh:

- $9^{1/2} = \sqrt{9} = 3$.
- $8^{1/3} = \sqrt[3]{8} = 2$.
- $4^{3/2} = \left(\sqrt{4}\right)^3 = 2^3 = 8$.

Syarat: basis tidak negatif untuk eksponen pecahan dengan penyebut genap. Topik ini dibahas lebih dalam di Level 1.

### 12. Kesalahan Umum dalam Operasi Pangkat dan Akar

Lihat bagian [Kesalahan Umum](#kesalahan-umum-dan-cara-menghindarinya) di bawah.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Pangkat (*power*) | perkalian berulang | $2^3 = 8$ |
| Basis (*base*) | bilangan yang dikalikan berulang | 2 pada $2^3$ |
| Eksponen (*exponent*) | banyaknya faktor | 3 pada $2^3$ |
| Kuadrat (*square*) | pangkat dua | $5^2 = 25$ |
| Kubik (*cube*) | pangkat tiga | $5^3 = 125$ |
| Akar kuadrat | kebalikan kuadrat, tidak negatif | $\sqrt{25} = 5$ |
| Akar pangkat tiga | kebalikan pangkat tiga | $\sqrt[3]{27} = 3$ |
| Kuadrat sempurna | bilangan yang akar kuadratnya bulat | 36 |
| Nilai eksak | nilai tepat | $\sqrt{2}$ |
| Pendekatan desimal | nilai dekat dalam desimal | $1{,}414$ |

## Rumus dan Penjelasan Setiap Simbol

Pada rumus berikut, $a$ adalah basis, sedangkan $m$ dan $n$ adalah eksponen (bilangan bulat). Syarat basis tidak nol hanya berlaku bila disebutkan.

| Aturan | Rumus | Syarat |
|---|---|---|
| Definisi | $a^n = a \times a \times \cdots \times a$ ($n$ faktor) | $n$ bilangan asli |
| Perkalian | $a^m \times a^n = a^{m+n}$ | basis sama |
| Pembagian | $\dfrac{a^m}{a^n} = a^{m-n}$ | $a \neq 0$ |
| Pangkat dari pangkat | $(a^m)^n = a^{m \times n}$ | |
| Perkalian di dalam kurung | $(ab)^n = a^n b^n$ | |
| Pangkat nol | $a^0 = 1$ | $a \neq 0$ |
| Pangkat negatif | $a^{-n} = \dfrac{1}{a^n}$ | $a \neq 0$ |
| Akar kuadrat | $\sqrt{x}$ | $x \geq 0$ |
| Akar dari hasil kali | $\sqrt{ab} = \sqrt{a}\sqrt{b}$ | $a, b \geq 0$ |
| Pangkat pecahan | $a^{m/n} = \left(\sqrt[n]{a}\right)^m$ | $a \geq 0$ bila $n$ genap |

Cara memakainya: pastikan syarat terpenuhi, ganti huruf dengan bilangan, lalu hitung. Contoh $a^m \times a^n$ dengan $a = 2$, $m = 3$, $n = 4$ memberikan $2^7 = 128$.

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Menggabungkan aturan pangkat

Hitung $\dfrac{2^3 \times 2^4}{2^5}$.

1. Pembilang: basis sama, jumlahkan eksponen. $2^3 \times 2^4 = 2^{3+4} = 2^7$.
2. Bagi: kurangkan eksponen. $\dfrac{2^7}{2^5} = 2^{7-5} = 2^2$.
3. Hitung: $2^2 = 4$.

Cek tanpa aturan: $\dfrac{8 \times 16}{32} = \dfrac{128}{32} = 4$.

### Contoh 2 — Pangkat nol dan negatif

Hitung $5^{-2} \times 5^3$ dan nyatakan $2^{-3}$ sebagai pecahan.

1. $5^{-2} \times 5^3 = 5^{-2+3} = 5^1 = 5$.
2. Cek: $5^{-2} = \frac{1}{25}$ dan $5^3 = 125$, maka $\frac{125}{25} = 5$.
3. $2^{-3} = \dfrac{1}{2^3} = \dfrac{1}{8}$.

### Contoh 3 — Menyederhanakan akar

Sederhanakan $\sqrt{72}$.

1. Cari faktor kuadrat sempurna terbesar dari 72. $72 = 36 \times 2$ dan 36 adalah kuadrat sempurna.
2. $\sqrt{72} = \sqrt{36 \times 2} = \sqrt{36} \times \sqrt{2} = 6\sqrt{2}$.
3. Pendekatan desimal: $6 \times 1{,}41421 \approx 8{,}485$. Cek: $8{,}485^2 \approx 71{,}99$.

Jawaban eksak: $6\sqrt{2}$.

### Contoh 4 — Pangkat pecahan

Hitung $4^{3/2}$ dan $8^{2/3}$.

1. $4^{3/2} = \left(\sqrt{4}\right)^3 = 2^3 = 8$.
2. $8^{2/3} = \left(\sqrt[3]{8}\right)^2 = 2^2 = 4$.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** $(-2)^4 = (-2)(-2)(-2)(-2) = 16$ (empat faktor negatif, genap, hasil positif).

**Sedang.** $(3^2)^3 \div 3^4 = 3^6 \div 3^4 = 3^2 = 9$.

**Sedang.** Jarak titik $(0, 0)$ ke $(3, 4)$ adalah $\sqrt{3^2 + 4^2} = \sqrt{9 + 16} = \sqrt{25} = 5$.

**Menantang.** Apakah $\sqrt{0{,}09}$ rasional? $0{,}3^2 = 0{,}09$, jadi $\sqrt{0{,}09} = 0{,}3 = \frac{3}{10}$, rasional.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $2^3 = 6$ | eksponen bukan pengali | $2^3 = 8$ |
| $2^3 \times 2^2 = 2^6$ | eksponen dijumlahkan, bukan dikalikan | $2^5 = 32$ |
| $2^3 \times 3^2 = 6^5$ | aturan butuh basis sama | $8 \times 9 = 72$ |
| $(2^3)^2 = 2^5$ | pangkat dari pangkat dikalikan | $2^6 = 64$ |
| $(a + b)^2 = a^2 + b^2$ | penjumlahan tidak didistribusi | $(2+3)^2 = 25$ |
| $-3^2 = 9$ | pangkat dikerjakan dulu | $-3^2 = -9$; $(-3)^2 = 9$ |
| $a^0 = 0$ | pangkat nol bernilai 1 | $a^0 = 1$ ($a \neq 0$) |
| $2^{-3} = -8$ | eksponen negatif berarti kebalikan | $\frac{1}{8}$ |
| $\sqrt{9 + 16} = 3 + 4$ | akar tidak mendistribusi ke penjumlahan | $\sqrt{25} = 5$ |
| $\sqrt{25} = \pm 5$ | lambang akar bernilai tidak negatif | $\sqrt{25} = 5$ |
| $\sqrt{-4}$ bernilai $-2$ | tidak ada akar kuadrat real dari bilangan negatif | tidak terdefinisi pada bilangan real |
| Membulatkan $\sqrt{2}$ di awal perhitungan | galat menumpuk | simpan eksak, bulatkan di akhir |

## Hubungan dengan Materi Berikutnya

- **Part 6** memakai pangkat sepuluh untuk notasi ilmiah, dan akar untuk menghubungkan kuadrat dengan nilai absolut.
- **Level 1** memperdalam eksponen, fungsi eksponensial, dan logaritma.
- **Level 2** memakai kuadrat dan akar untuk panjang vektor.
- **Level 3** memakai akar untuk simpangan baku, yaitu akar dari variansi.

## Hubungan dengan Machine Learning

- **Kuadrat kesalahan.** Banyak ukuran kesalahan memakai kuadrat selisih prediksi dan target, misalnya $(\hat{y} - y)^2$. Karena hasilnya selalu tidak negatif, kesalahan positif dan negatif tidak saling menghapus.
- **Akar kuadrat dan jarak.** Jarak antara dua titik data memakai akar jumlah kuadrat selisih koordinat, seperti contoh $\sqrt{3^2 + 4^2} = 5$. Algoritma seperti k-nearest neighbors memakai ini.
- **Pangkat sepuluh negatif.** Laju belajar sering ditulis $10^{-3} = 0{,}001$ atau $10^{-4} = 0{,}0001$.
- **Eksponen dalam fungsi.** Bilangan $e \approx 2{,}718$ dengan pangkat, seperti $e^{-x}$, ada di fungsi sigmoid dan softmax.

### Python: pangkat dan akar

Operator pangkat di Python adalah `**`, bukan `^`.

```python
import math

print(2 ** 3)      # 8
print(2 ** 3 * 2 ** 4)   # 128, sama dengan 2 ** 7
print(2 ** -3)     # 0.125
print(5 ** 0)      # 1
print((-3) ** 2)   # 9
print(-3 ** 2)     # -9, pangkat dikerjakan lebih dulu
```

Akar kuadrat dengan `math.sqrt` dan akar pangkat tiga dengan `math.cbrt` (tersedia di Python 3.11 ke atas):

```python
print(math.sqrt(49))    # 7.0
print(math.sqrt(2))     # 1.4142135623730951, pendekatan irasional
print(math.cbrt(27))    # 3.0
print(math.cbrt(-8))    # -2.0
print(math.sqrt(72))    # 8.48528137423857
print(6 * math.sqrt(2)) # 8.485281374238571, beda di digit terakhir
```

Dua hasil terakhir seharusnya sama secara matematika ($\sqrt{72} = 6\sqrt{2}$), tetapi komputer menghitungnya dengan pendekatan, sehingga digit terakhirnya berbeda.

Pangkat pecahan menghasilkan `float`:

```python
print(4 ** 1.5)   # 8.0
print(9 ** 0.5)   # 3.0
```

Hasil Python selalu desimal pendekatan (kecuali bilangan bulat), sedangkan $6\sqrt{2}$ adalah nilai eksak. Jarak dua titik:

```python
x1, y1 = 0, 0
x2, y2 = 3, 4
jarak = math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
print(jarak)   # 5.0
```

## Latihan Mandiri

1. Hitung: (a) $2^5$ (b) $(-3)^3$ (c) $-3^2$ (d) $(-3)^2$ (e) $10^4$
2. Hitung $3^2 \times 3^4$.
3. Hitung $5^7 \div 5^5$.
4. Hitung $(2^2)^3$.
5. Tulis tanpa eksponen nol atau negatif: (a) $7^0$ (b) $2^{-4}$ (c) $10^{-3}$ (d) $\left(\frac{1}{2}\right)^{-2}$
6. Hitung: (a) $\sqrt{81}$ (b) $\sqrt{144}$ (c) $\sqrt[3]{64}$ (d) $\sqrt[3]{-27}$ (e) $\sqrt{0{,}25}$
7. Tentukan rasional atau irasional: (a) $\sqrt{49}$ (b) $\sqrt{50}$ (c) $\sqrt{\frac{1}{4}}$ (d) $\sqrt[3]{10}$ (e) $\sqrt{0{,}09}$
8. Sederhanakan: (a) $\sqrt{72}$ (b) $\sqrt{48}$
9. Hitung: (a) $16^{1/2}$ (b) $27^{1/3}$ (c) $4^{3/2}$ (d) $8^{2/3}$
10. Temukan kesalahan dan beri jawaban yang benar: (a) $2^3 \times 2^2 = 2^6$ (b) $\sqrt{9 + 16} = 3 + 4 = 7$ (c) $(3 + 4)^2 = 3^2 + 4^2$

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) $2 \times 2 \times 2 \times 2 \times 2 = 32$.
    (b) $(-3)(-3)(-3) = 9 \times (-3) = -27$.
    (c) $-3^2 = -(3 \times 3) = -9$.
    (d) $(-3)^2 = 9$.
    (e) $10^4 = 10.000$.

??? success "Soal 2"
    Basis sama, jumlahkan eksponen: $3^{2+4} = 3^6 = 729$. Cek: $9 \times 81 = 729$.

??? success "Soal 3"
    Kurangkan eksponen: $5^{7-5} = 5^2 = 25$.

??? success "Soal 4"
    Kalikan eksponen: $2^{2 \times 3} = 2^6 = 64$. Cek: $2^2 = 4$ dan $4^3 = 64$.

??? success "Soal 5"
    (a) $7^0 = 1$.
    (b) $2^{-4} = \frac{1}{2^4} = \frac{1}{16}$.
    (c) $10^{-3} = \frac{1}{1.000} = 0{,}001$.
    (d) $\left(\frac{1}{2}\right)^{-2} = \dfrac{1}{\left(\frac{1}{2}\right)^2} = \dfrac{1}{\frac{1}{4}} = 4$ (kebalikan dari $\frac{1}{4}$ adalah 4).

??? success "Soal 6"
    (a) 9, karena $9^2 = 81$. (b) 12, karena $12^2 = 144$. (c) 4, karena $4^3 = 64$. (d) $-3$, karena $(-3)^3 = -27$. (e) $0{,}5$, karena $0{,}5^2 = 0{,}25$.

??? success "Soal 7"
    (a) Rasional ($\sqrt{49} = 7$). (b) Irasional (50 bukan kuadrat sempurna). (c) Rasional ($\sqrt{\frac{1}{4}} = \frac{1}{2}$). (d) Irasional (10 bukan pangkat tiga sempurna: $2^3 = 8$, $3^3 = 27$). (e) Rasional ($0{,}3^2 = 0{,}09$).

??? success "Soal 8"
    (a) $72 = 36 \times 2$, maka $\sqrt{72} = 6\sqrt{2}$.
    (b) $48 = 16 \times 3$, maka $\sqrt{48} = 4\sqrt{3}$.

??? success "Soal 9"
    (a) $\sqrt{16} = 4$. (b) $\sqrt[3]{27} = 3$. (c) $\left(\sqrt{4}\right)^3 = 2^3 = 8$. (d) $\left(\sqrt[3]{8}\right)^2 = 2^2 = 4$.

??? success "Soal 10"
    (a) Eksponen harus dijumlahkan: $2^{3+2} = 2^5 = 32$. Cek: $8 \times 4 = 32$.
    (b) Akar tidak mendistribusi ke penjumlahan: $\sqrt{25} = 5$.
    (c) $(3 + 4)^2 = 7^2 = 49$, sedangkan $3^2 + 4^2 = 25$. Keduanya tidak sama.

## Ringkasan Materi

- $a^n$ adalah $n$ faktor $a$; eksponen menyatakan banyaknya faktor.
- Basis sama: kali berarti jumlahkan eksponen, bagi berarti kurangkan eksponen; pangkat dari pangkat: kalikan eksponen.
- $a^0 = 1$ ($a \neq 0$); $a^{-n} = \frac{1}{a^n}$.
- $\sqrt{x}$ adalah bilangan tidak negatif yang kuadratnya $x$ dan hanya ada untuk $x \geq 0$ pada bilangan real. $\sqrt[3]{x}$ ada untuk semua bilangan real.
- $\sqrt{ab} = \sqrt{a}\sqrt{b}$, tetapi $\sqrt{a+b} \neq \sqrt{a} + \sqrt{b}$ dan $(a+b)^2 \neq a^2 + b^2$.
- $a^{1/2} = \sqrt{a}$ dan $a^{m/n} = \left(\sqrt[n]{a}\right)^m$.
- Simpan nilai eksak selama perhitungan, bulatkan di akhir.

## Checklist Penguasaan

- [ ] Menjelaskan arti basis dan eksponen serta menghitung pangkat dengan basis negatif.
- [ ] Menggunakan tiga aturan eksponen (kali, bagi, pangkat dari pangkat) dan menyebut syaratnya.
- [ ] Menjelaskan mengapa $a^0 = 1$ dan $a^{-n} = \frac{1}{a^n}$.
- [ ] Menghitung akar kuadrat dan akar pangkat tiga, serta memperkirakan akar yang tidak bulat.
- [ ] Menyederhanakan akar dengan $\sqrt{ab} = \sqrt{a}\sqrt{b}$.
- [ ] Menjelaskan beda nilai eksak dan pendekatan desimal.
- [ ] Menghitung pangkat pecahan sederhana.
- [ ] Mengenali kesalahan seperti $(a+b)^2 = a^2 + b^2$.
