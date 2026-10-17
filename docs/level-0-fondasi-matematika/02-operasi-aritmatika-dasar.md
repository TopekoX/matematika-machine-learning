# Part 2 — Operasi Aritmatika Dasar

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- melakukan penjumlahan, pengurangan, perkalian, dan pembagian dengan benar;
- menghitung dengan bilangan negatif, termasuk aturan tandanya;
- menjelaskan sifat komutatif, asosiatif, dan distributif beserta contoh angkanya;
- menjelaskan fungsi identitas penjumlahan dan perkalian;
- menerapkan urutan operasi dan tanda kurung;
- mengenali dan memperbaiki kesalahan aritmatika yang umum.

## Prasyarat

[Part 1 — Mengenal Bilangan](01-mengenal-bilangan.md), terutama bilangan bulat, bilangan negatif, dan garis bilangan.

## Mengapa Materi Ini Penting?

Setiap rumus di Machine Learning tersusun dari empat operasi dasar. Kesalahan urutan atau tanda sering membuat hasil perhitungan salah tanpa disadari. Sifat-sifat operasi (komutatif, asosiatif, distributif) juga menjadi dasar bagi aljabar: kamu akan memakainya setiap kali menyederhanakan rumus.

## Penjelasan Konsep dari Dasar

### 1. Penjumlahan

**Penjumlahan** (*addition*) menggabungkan dua jumlah atau lebih. Lambangnya $+$. Bilangan yang dijumlahkan disebut **suku**, dan hasilnya disebut **jumlah**.

$$
\underbrace{23}_{\text{suku}} + \underbrace{19}_{\text{suku}} = \underbrace{42}_{\text{jumlah}}
$$

Pada garis bilangan, menjumlahkan berarti bergerak ke **kanan**. Menghitung $23 + 19$ dapat dipecah: $23 + 10 = 33$, lalu $33 + 9 = 42$.

### 2. Pengurangan

**Pengurangan** (*subtraction*) mencari sisa atau selisih. Lambangnya $-$. Pada $a - b$, bilangan $a$ disebut **yang dikurangi** dan $b$ **pengurang**. Hasilnya disebut **selisih**.

Pengurangan adalah **kebalikan** (invers) dari penjumlahan: jika $42 - 19 = 23$, maka $23 + 19 = 42$. Kamu bisa memakai ini untuk memeriksa jawaban. Pada garis bilangan, mengurangi berarti bergerak ke **kiri**.

### 3. Perkalian

**Perkalian** (*multiplication*) adalah penjumlahan berulang. $4 \times 6$ berarti 6 dijumlahkan sebanyak 4 kali: $6 + 6 + 6 + 6 = 24$. Lambangnya $\times$, atau titik $\cdot$, atau tanpa lambang bila memakai huruf. Bilangan yang dikalikan disebut **faktor**, dan hasilnya **hasil kali**.

Untuk bilangan besar, pecah salah satu faktornya. Hitung $23 \times 14$:

- $23 \times 10 = 230$
- $23 \times 4 = 92$
- $230 + 92 = 322$

Cara ini sah karena sifat distributif (dibahas di bawah), sebab $14 = 10 + 4$.

### 4. Pembagian

**Pembagian** (*division*) menjawab pertanyaan "berapa kali sebuah bilangan muat di dalam bilangan lain" atau "membagi rata". Lambangnya $\div$ atau garis miring $/$. Pada $a \div b$, $a$ disebut **yang dibagi** dan $b$ **pembagi**. Hasilnya disebut **hasil bagi**.

Pembagian adalah kebalikan perkalian: jika $24 \div 4 = 6$, maka $6 \times 4 = 24$.

Jika tidak habis dibagi, ada **sisa**. Contoh: $17 \div 5 = 3$ sisa $2$, karena $5 \times 3 + 2 = 17$.

!!! warning "Pembagian dengan nol tidak terdefinisi"
    Hasil $a \div 0$ tidak ada. Alasannya: $a \div 0 = c$ berarti $c \times 0 = a$. Padahal bilangan apa pun dikali 0 hasilnya 0. Jika $a \neq 0$, tidak ada $c$ yang cocok. Jika $a = 0$, setiap $c$ cocok, sehingga hasilnya tidak tunggal. Sebaliknya, $0 \div 5 = 0$ benar (karena $0 \times 5 = 0$).

### 5. Operasi Bilangan Negatif

**Menjumlah.** Gerak di garis bilangan: menjumlah bilangan positif ke kanan, bilangan negatif ke kiri.

- $5 + (-3) = 2$ (dari 5 gerak 3 langkah ke kiri)
- $-5 + 3 = -2$ (dari -5 gerak 3 ke kanan)
- $-5 + (-3) = -8$ (dari -5 gerak 3 ke kiri)

**Mengurang.** Mengurangi sebuah bilangan sama dengan menjumlahkan lawannya:

$$
a - b = a + (-b)
$$

Maka $5 - (-3) = 5 + 3 = 8$. Alasan intuitifnya: jika kamu punya utang 3 (yaitu -3) lalu utang itu dihapus ("dikurangi"), kekayaanmu naik 3.

**Mengalikan.** Aturan tanda:

| Perkalian | Hasil | Contoh |
|---|---|---|
| positif × positif | positif | $3 \times 2 = 6$ |
| positif × negatif | negatif | $3 \times (-2) = -6$ |
| negatif × positif | negatif | $(-3) \times 2 = -6$ |
| negatif × negatif | **positif** | $(-3) \times (-2) = 6$ |

Mengapa negatif × negatif positif? Lihat pola ini. Setiap baris, faktor pertama turun 1 dan hasilnya naik 2:

| Perkalian | Hasil |
|---|---|
| $3 \times (-2)$ | $-6$ |
| $2 \times (-2)$ | $-4$ |
| $1 \times (-2)$ | $-2$ |
| $0 \times (-2)$ | $0$ |
| $-1 \times (-2)$ | $2$ |
| $-2 \times (-2)$ | $4$ |

Agar pola tetap konsisten, hasilnya harus positif.

**Membagi.** Aturan tandanya sama dengan perkalian: tanda sama menghasilkan positif, tanda berbeda menghasilkan negatif. Contoh: $(-24) \div 6 = -4$ dan $(-24) \div (-6) = 4$.

### 6. Sifat Komutatif

**Komutatif** (*commutative*) berarti **urutan boleh ditukar** tanpa mengubah hasil. Sifat ini berlaku untuk **penjumlahan** dan **perkalian**:

$$
a + b = b + a, \qquad a \times b = b \times a
$$

Contoh: $3 + 5 = 8 = 5 + 3$ dan $4 \times 6 = 24 = 6 \times 4$.

Sifat ini **tidak** berlaku untuk pengurangan dan pembagian: $8 - 3 = 5$ tetapi $3 - 8 = -5$; $12 \div 4 = 3$ tetapi $4 \div 12 \neq 3$.

### 7. Sifat Asosiatif

**Asosiatif** (*associative*) berarti **cara mengelompokkan** tiga bilangan atau lebih tidak mengubah hasil. Berlaku untuk penjumlahan dan perkalian:

$$
(a + b) + c = a + (b + c), \qquad (a \times b) \times c = a \times (b \times c)
$$

Contoh: $(2 + 3) + 4 = 5 + 4 = 9$ dan $2 + (3 + 4) = 2 + 7 = 9$. Untuk perkalian: $(2 \times 3) \times 4 = 6 \times 4 = 24$ dan $2 \times (3 \times 4) = 2 \times 12 = 24$.

Tidak berlaku untuk pengurangan: $(10 - 4) - 3 = 3$ sedangkan $10 - (4 - 3) = 9$. Tidak berlaku untuk pembagian: $(24 \div 4) \div 2 = 3$ sedangkan $24 \div (4 \div 2) = 12$.

Komutatif dan asosiatif bersama-sama memperbolehkan kita menjumlahkan atau mengalikan dalam urutan apa pun. Misalnya $25 + 37 + 75 = (25 + 75) + 37 = 137$.

### 8. Sifat Distributif

**Distributif** (*distributive*) menghubungkan perkalian dengan penjumlahan (atau pengurangan): perkalian bisa **dibagikan** ke setiap suku di dalam kurung.

$$
a \times (b + c) = a \times b + a \times c
$$

Contoh: $6 \times (10 + 4)$. Cara langsung: $6 \times 14 = 84$. Cara distributif: $6 \times 10 + 6 \times 4 = 60 + 24 = 84$. Hasilnya sama.

Alasannya: $6 \times (10 + 4)$ berarti 6 kelompok yang masing-masing berisi 10 dan 4. Total ada 6 kali sepuluh dan 6 kali empat.

Sifat ini juga berlaku untuk pengurangan: $a \times (b - c) = a \times b - a \times c$. Contoh cepat: $7 \times 98 = 7 \times (100 - 2) = 700 - 14 = 686$.

!!! warning "Batasan"
    Distributif membagikan **perkalian** ke dalam kurung. Pembagian *oleh* sebuah jumlah **tidak** bisa dibagikan: $a \div (b + c)$ **tidak** sama dengan $a \div b + a \div c$. Contoh: $12 \div (4 + 2) = 2$, tetapi $12 \div 4 + 12 \div 2 = 3 + 6 = 9$.

### 9. Identitas Penjumlahan dan Perkalian

**Identitas** adalah bilangan yang jika dioperasikan **tidak mengubah** bilangan lain.

- Identitas penjumlahan adalah **0**: $a + 0 = a$. Contoh: $7 + 0 = 7$.
- Identitas perkalian adalah **1**: $a \times 1 = a$. Contoh: $7 \times 1 = 7$.

Sifat terkait yang perlu diingat:

- $a \times 0 = 0$ untuk bilangan apa pun.
- Setiap bilangan punya **lawan** (invers penjumlahan) yang jika dijumlahkan menghasilkan 0: $5 + (-5) = 0$.
- Setiap bilangan bukan nol punya **kebalikan** (invers perkalian) yang jika dikalikan menghasilkan 1. Contoh: kebalikan 4 adalah $\frac{1}{4}$ karena $4 \times \frac{1}{4} = 1$. Pecahan dibahas di Part 3.

### 10. Urutan Operasi Matematika

Jika sebuah perhitungan memuat beberapa operasi, kita perlu aturan yang sama agar semua orang mendapat hasil yang sama. Urutannya:

1. **Tanda kurung** (dari yang paling dalam).
2. **Pangkat** (dibahas di Part 5; sebagai gambaran, $2^3 = 2 \times 2 \times 2 = 8$).
3. **Perkalian dan pembagian**, dikerjakan dari **kiri ke kanan**.
4. **Penjumlahan dan pengurangan**, dikerjakan dari **kiri ke kanan**.

!!! warning "Perkalian dan pembagian setara"
    Singkatan seperti "KuPaKaBaTaKu" (kurung, pangkat, kali, bagi, tambah, kurang) bisa menyesatkan. Kali dan bagi berada pada **tingkat yang sama**, begitu juga tambah dan kurang. Kerjakan dari kiri ke kanan, bukan "kali dulu baru bagi".

Contoh: $8 + 2 \times 5$. Perkalian lebih dulu: $2 \times 5 = 10$, lalu $8 + 10 = 18$. Jawaban 50 salah, karena menjumlahkan dulu menyalahi aturan.

Contoh dua operasi setingkat: $24 \div 4 \times 2$. Dari kiri: $24 \div 4 = 6$, lalu $6 \times 2 = 12$. Jawaban 3 salah ($4 \times 2$ dikerjakan lebih dulu).

### 11. Penggunaan Tanda Kurung

**Tanda kurung** (*parentheses*) mengubah urutan: isi kurung dikerjakan lebih dulu. Dengan kurung, $(8 + 2) \times 5 = 10 \times 5 = 50$, berbeda dari $8 + 2 \times 5 = 18$.

Kurung yang bersarang dikerjakan dari yang **paling dalam**:

$$
2 \times (3 + (4 - 1)) = 2 \times (3 + 3) = 2 \times 6 = 12
$$

Beberapa aturan yang berguna:

- Angka di depan kurung tanpa lambang berarti dikali: $3(4 + 2) = 3 \times 6 = 18$.
- Tanda minus di depan kurung membalik tanda semua isi kurung: $-(3 + 2) = -5$ dan $-(3 - 5) = -3 + 5 = 2$.
- Kurung mengubah hasil pengurangan: $10 - (3 + 2) = 10 - 5 = 5$, sedangkan $10 - 3 + 2 = 9$.

### 12. Kesalahan Umum dalam Aritmatika

Daftar kesalahan ada di bagian [Kesalahan Umum](#kesalahan-umum-dan-cara-menghindarinya) di bawah.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Suku (*term*) | bilangan yang dijumlahkan | 23 dan 19 pada $23 + 19$ |
| Jumlah (*sum*) | hasil penjumlahan | 42 |
| Selisih (*difference*) | hasil pengurangan | $42 - 19 = 23$ |
| Faktor (*factor*) | bilangan yang dikalikan | 4 dan 6 pada $4 \times 6$ |
| Hasil kali (*product*) | hasil perkalian | 24 |
| Hasil bagi (*quotient*) | hasil pembagian | $24 \div 4 = 6$ |
| Sisa (*remainder*) | bagian yang tidak habis dibagi | $17 \div 5$: sisa 2 |
| Lawan (invers penjumlahan) | bilangan dengan jumlah 0 | lawan 5 adalah -5 |
| Identitas (*identity*) | bilangan yang tidak mengubah hasil | 0 untuk $+$, 1 untuk $\times$ |
| Komutatif | urutan boleh ditukar | $3 + 5 = 5 + 3$ |
| Asosiatif | pengelompokan boleh diubah | $(2 + 3) + 4 = 2 + (3 + 4)$ |
| Distributif | perkalian dibagikan ke suku | $2(3 + 4) = 2 \cdot 3 + 2 \cdot 4$ |

## Rumus dan Penjelasan Setiap Simbol

Pada semua rumus di bawah, huruf $a$, $b$, dan $c$ adalah **pengganti bilangan sembarang**. Artinya, rumus berlaku untuk semua bilangan yang kamu masukkan ke tempat huruf itu. Cara memakainya: ganti huruf dengan bilangan, lalu hitung kedua sisi tanda sama dengan.

| Sifat | Rumus | Berlaku untuk |
|---|---|---|
| Komutatif | $a + b = b + a$ dan $a \times b = b \times a$ | $+$ dan $\times$ |
| Asosiatif | $(a + b) + c = a + (b + c)$ dan $(a \times b) \times c = a \times (b \times c)$ | $+$ dan $\times$ |
| Distributif | $a \times (b + c) = a \times b + a \times c$ | $\times$ atas $+$ dan $-$ |
| Identitas | $a + 0 = a$ dan $a \times 1 = a$ | $+$ dan $\times$ |
| Lawan | $a + (-a) = 0$ | $+$ |
| Pengurangan | $a - b = a + (-b)$ | $-$ |

Simbol perkalian: $\times$ dan $\cdot$ berarti sama. Di aljabar, $ab$ berarti $a \times b$. Simbol pembagian: $a \div b$ sama dengan $\frac{a}{b}$ (dengan $b \neq 0$).

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Operasi bilangan negatif

Hitung $-3 + 8 - (-2)$.

1. Kerjakan dari kiri: $-3 + 8$. Mulai dari -3, gerak 8 langkah ke kanan, hasilnya $5$.
2. Lanjut: $5 - (-2)$. Mengurangi -2 sama dengan menjumlahkan lawannya: $5 + 2 = 7$.

Jawaban: $7$.

### Contoh 2 — Urutan operasi dan kurung

Hitung $12 + 18 \div 3 \times (5 - 2)$.

1. Kurung dulu: $5 - 2 = 3$. Ekspresinya menjadi $12 + 18 \div 3 \times 3$.
2. Perkalian dan pembagian dari kiri: $18 \div 3 = 6$, lalu $6 \times 3 = 18$.
3. Ekspresinya menjadi $12 + 18 = 30$.

Jawaban: $30$.

### Contoh 3 — Sifat distributif untuk hitung cepat

Hitung $7 \times 98$ tanpa kalkulator.

1. Tulis $98 = 100 - 2$, sehingga $7 \times 98 = 7 \times (100 - 2)$.
2. Bagikan: $7 \times 100 - 7 \times 2 = 700 - 14$.
3. Hitung: $700 - 14 = 686$.

Periksa dengan cara langsung: $98 \times 7$: $90 \times 7 = 630$, $8 \times 7 = 56$, $630 + 56 = 686$. Cocok.

### Contoh 4 — Rangkaian perkalian negatif

Hitung $(-2) \times (-3) \times (-4)$.

1. Dua faktor pertama: $(-2) \times (-3) = 6$ (negatif × negatif = positif).
2. Lanjut: $6 \times (-4) = -24$ (positif × negatif = negatif).

Jawaban: $-24$. Pola praktis: jika banyak faktor negatif **ganjil**, hasilnya negatif; jika **genap**, hasilnya positif. Di sini ada tiga faktor negatif, jadi hasilnya negatif.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** $15 + (-20) = -5$. Dari 15 gerak 20 ke kiri melewati nol sampai -5.

**Sedang.** $(-12) \div 4 + 6 \times (-2)$. Pertama $(-12) \div 4 = -3$; $6 \times (-2) = -12$. Lalu $-3 + (-12) = -15$.

**Sedang.** Hitung $25 + 37 + 75$ dengan cepat. Pakai komutatif dan asosiatif: $(25 + 75) + 37 = 100 + 37 = 137$.

**Menantang.** $-(3 - 7) \times 2 + 10 \div (1 + 4)$. Kurung: $3 - 7 = -4$ dan $1 + 4 = 5$. Maka $-(-4) \times 2 + 10 \div 5 = 4 \times 2 + 2 = 8 + 2 = 10$.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $8 + 2 \times 5 = 50$ | penjumlahan dikerjakan sebelum perkalian | $8 + 10 = 18$ |
| $24 \div 4 \times 2 = 3$ | perkalian dikerjakan dulu padahal setingkat dengan bagi | kiri ke kanan: $6 \times 2 = 12$ |
| $10 - (4 + 3) = 10 - 4 + 3 = 9$ | tanda minus tidak membalik isi kurung | $10 - 7 = 3$ atau $10 - 4 - 3 = 3$ |
| $5 - (-3) = 2$ | mengurangi negatif sama dengan menjumlah positif | $5 + 3 = 8$ |
| $(-3) \times (-2) = -6$ | dua tanda negatif menghasilkan positif | $6$ |
| $-3^2$ dianggap $9$ | pangkat dikerjakan sebelum tanda minus (dibahas di Part 5) | $-9$; sedangkan $(-3)^2 = 9$ |
| $a \div (b + c) = a \div b + a \div c$ | distributif tidak berlaku untuk pembagi | hitung kurung dulu |
| $8 - 3 = 3 - 8$ | pengurangan tidak komutatif | $5 \neq -5$ |
| Membagi dengan nol | tidak terdefinisi | periksa pembagi sebelum menghitung |

Cara menghindarinya: tulis tiap langkah satu per satu, kerjakan satu operasi per baris, dan periksa hasil dengan operasi kebalikannya.

## Hubungan dengan Materi Berikutnya

- **Part 3** memakai pembagian untuk membangun pecahan dan desimal.
- **Part 5** memperluas perkalian berulang menjadi pangkat.
- **Part 7** memakai sifat-sifat di atas untuk menyelesaikan persamaan, karena menyelesaikan persamaan berarti memakai operasi kebalikan.
- **Level 2** memakai komutatif, asosiatif, dan distributif untuk perkalian matriks. Di sana perkalian **tidak** komutatif, jadi pahami sifat ini sebagai sifat, bukan keharusan.

## Hubungan dengan Machine Learning

- **Jumlah berbobot.** Banyak model menghitung prediksi dengan pola seperti $y = w_1 x_1 + w_2 x_2 + b$. Rumus ini hanya perkalian dan penjumlahan yang urutannya diatur oleh aturan urutan operasi.
- **Selisih bertanda.** Kesalahan prediksi sering dihitung sebagai prediksi dikurangi nilai sebenarnya. Hasil negatif berarti prediksi terlalu kecil, hasil positif berarti terlalu besar.
- **Kuadrat menghilangkan tanda.** Karena negatif × negatif = positif, mengalikan kesalahan dengan dirinya sendiri membuat nilainya tidak pernah negatif. Contoh: $(-2) \times (-2) = 4$ dan $2 \times 2 = 4$. Ini akan sering kamu temui.
- **Kurung salah, hasil salah.** Satu pasang kurung yang hilang dapat mengubah makna rumus.

### Python: operator aritmatika

Python mengikuti aturan urutan operasi yang sama.

```python
print(23 + 19)     # 42
print(23 * 14)     # 322
print(8 + 2 * 5)   # 18
print((8 + 2) * 5) # 50
```

Operator `/` selalu menghasilkan bilangan desimal (`float`). Operator `//` menghasilkan hasil bagi bulat, dan `%` menghasilkan sisa:

```python
print(17 / 5)    # 3.4
print(17 // 5)   # 3, hasil bagi
print(17 % 5)    # 2, sisa
print(12 + 18 / 3 * (5 - 2))  # 30.0
```

Hasil terakhir `30.0` (bukan `30`) karena ada pembagian dengan `/`. Nilainya sama dengan Contoh 2.

Python juga menolak pembagian dengan nol:

```python
# print(5 / 0)  # ZeroDivisionError: division by zero
```

Hitung kesalahan prediksi (prediksi dikurangi target) lalu kuadratkan:

```python
target = [10, 8, 15]
prediksi = [12, 7, 15]

error = [p - t for p, t in zip(prediksi, target)]
print(error)                   # [2, -1, 0]
print([e * e for e in error])  # [4, 1, 0]
```

Baris pertama berarti: "untuk setiap pasangan `p` (prediksi) dan `t` (target), hitung `p - t`". Perhatikan -1 dikali -1 menghasilkan 1.

## Latihan Mandiri

1. Hitung $346 + 579$.
2. Hitung $502 - 237$.
3. Hitung $36 \times 25$.
4. Hitung $95 \div 8$ dan tulis hasil bagi serta sisanya.
5. Hitung: (a) $-8 + 5$ (b) $-8 - 5$ (c) $8 - (-5)$ (d) $(-4) \times (-6)$ (e) $(-24) \div 6$ (f) $(-3) \times 4 \times (-2)$
6. Sebutkan sifat yang dipakai: (a) $17 + 25 = 25 + 17$ (b) $(4 \times 5) \times 2 = 4 \times (5 \times 2)$ (c) $3 \times (10 + 2) = 3 \times 10 + 3 \times 2$ (d) $9 + 0 = 9$
7. Hitung $12 \times 98$ dengan sifat distributif.
8. Hitung: (a) $7 + 3 \times 4$ (b) $(7 + 3) \times 4$ (c) $36 \div 6 \times 3$ (d) $50 - 4 \times (3 + 2)$ (e) $18 - 6 + 2$
9. Seorang siswa menghitung $10 - (4 + 3)$ dengan menulis $10 - 4 + 3 = 9$. Di mana letak kesalahannya, dan berapa jawaban yang benar?
10. Target: $[10, 8, 15]$ dan prediksi: $[12, 7, 15]$. Hitung kesalahan (prediksi dikurangi target) untuk tiap data, lalu hitung jumlah kuadrat kesalahan.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    Satuan: $6 + 9 = 15$, tulis 5 simpan 1. Puluhan: $4 + 7 + 1 = 12$, tulis 2 simpan 1. Ratusan: $3 + 5 + 1 = 9$. Hasil: **925**. Periksa: $925 - 579 = 346$.

??? success "Soal 2"
    $502 - 237 = 502 - 200 - 37 = 302 - 37 = 265$. Hasil: **265**. Periksa: $265 + 237 = 502$.

??? success "Soal 3"
    $36 \times 25 = 36 \times 100 \div 4 = 3600 \div 4 = 900$. Cara lain: $36 \times 20 = 720$, $36 \times 5 = 180$, $720 + 180 = 900$. Hasil: **900**.

??? success "Soal 4"
    $8 \times 11 = 88$ dan $95 - 88 = 7$ (kurang dari pembagi 8, jadi berhenti). Hasil bagi **11**, sisa **7**. Periksa: $8 \times 11 + 7 = 95$.

??? success "Soal 5"
    - (a) $-8 + 5 = -3$
    - (b) $-8 - 5 = -8 + (-5) = -13$
    - (c) $8 - (-5) = 8 + 5 = 13$
    - (d) $(-4) \times (-6) = 24$
    - (e) $(-24) \div 6 = -4$ (tanda berbeda, hasil negatif)
    - (f) $(-3) \times 4 = -12$, lalu $-12 \times (-2) = 24$

??? success "Soal 6"
    (a) komutatif penjumlahan; (b) asosiatif perkalian; (c) distributif; (d) identitas penjumlahan.

??? success "Soal 7"
    $12 \times 98 = 12 \times (100 - 2) = 1200 - 24 = 1176$. Periksa dengan cara langsung: $98 \times 12 = 980 + 196 = 1176$.

??? success "Soal 8"
    - (a) $7 + 12 = 19$
    - (b) $10 \times 4 = 40$
    - (c) $36 \div 6 = 6$, lalu $6 \times 3 = 18$
    - (d) kurung $3 + 2 = 5$; $4 \times 5 = 20$; $50 - 20 = 30$
    - (e) kiri ke kanan: $18 - 6 = 12$, lalu $12 + 2 = 14$

??? success "Soal 9"
    Tanda minus di depan kurung harus membalik tanda **semua** isi kurung, sehingga $-(4 + 3) = -4 - 3$, bukan $-4 + 3$. Cara benar: $10 - (4 + 3) = 10 - 7 = 3$.

??? success "Soal 10"
    Kesalahan = prediksi − target: $12 - 10 = 2$; $7 - 8 = -1$; $15 - 15 = 0$. Kuadrat: $2 \times 2 = 4$; $(-1) \times (-1) = 1$; $0 \times 0 = 0$. Jumlah: $4 + 1 + 0 = 5$.

## Ringkasan Materi

- Penjumlahan dan perkalian bersifat komutatif dan asosiatif; pengurangan dan pembagian tidak.
- Distributif: $a(b + c) = ab + ac$. Berlaku untuk perkalian atas penjumlahan atau pengurangan, bukan untuk pembagian oleh jumlah.
- Identitas penjumlahan adalah 0, identitas perkalian adalah 1.
- Mengurangi sama dengan menjumlahkan lawan: $a - b = a + (-b)$.
- Perkalian atau pembagian tanda sama menghasilkan positif, tanda berbeda menghasilkan negatif.
- Urutan operasi: kurung, pangkat, kali dan bagi (kiri ke kanan), tambah dan kurang (kiri ke kanan).
- Pembagian dengan nol tidak terdefinisi.

## Checklist Penguasaan

- [ ] Menyebut nama bilangan pada tiap operasi (suku, jumlah, selisih, faktor, hasil kali, hasil bagi).
- [ ] Menghitung penjumlahan, pengurangan, perkalian, dan pembagian bilangan negatif dengan benar.
- [ ] Menjelaskan mengapa negatif × negatif = positif.
- [ ] Memberi contoh angka untuk sifat komutatif, asosiatif, dan distributif, termasuk contoh yang tidak berlaku.
- [ ] Menyelesaikan perhitungan campuran sesuai urutan operasi.
- [ ] Menjelaskan mengapa pembagian dengan nol tidak terdefinisi.
- [ ] Membaca dan mengevaluasi ekspresi dengan tanda kurung bersarang.
