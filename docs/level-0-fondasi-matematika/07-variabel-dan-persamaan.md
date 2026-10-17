# Part 7 — Pengenalan Variabel dan Persamaan

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan variabel, konstanta, dan koefisien, serta membedakannya dalam sebuah ekspresi;
- membaca ekspresi matematika dan menerjemahkan kalimat sederhana ke dalam ekspresi;
- mensubstitusi nilai ke dalam ekspresi dengan benar;
- menjelaskan arti tanda sama dengan dan membedakan ekspresi dari persamaan;
- menyelesaikan persamaan linear satu langkah dan dua langkah serta memeriksa jawabannya;
- mengenali peran variabel dalam Machine Learning.

## Prasyarat

[Part 2](02-operasi-aritmatika-dasar.md) (operasi, bilangan negatif, urutan operasi) dan [Part 3](03-pecahan-dan-desimal.md) (pecahan). Part 5 membantu untuk pangkat dua.

!!! note "Jembatan menuju Level 1"
    Bagian ini hanya pengantar. Aljabar yang lengkap (persamaan lebih rumit, pertidaksamaan, sistem persamaan, dan fungsi) ada di Level 1.

## Mengapa Materi Ini Penting?

Rumus Machine Learning ditulis dengan huruf, bukan angka. Huruf itu memungkinkan satu rumus berlaku untuk banyak nilai. Jika kamu tidak bisa membaca ekspresi seperti $y = 2x + 3$, hampir semua materi berikutnya akan terasa seperti bahasa asing. Selain itu, "mencari nilai yang belum diketahui" (menyelesaikan persamaan) adalah keterampilan yang dipakai di mana-mana.

## Penjelasan Konsep dari Dasar

### 1. Pengertian Variabel

**Variabel** (*variable*) adalah huruf atau simbol yang mewakili bilangan yang **nilainya bisa berubah** atau **belum diketahui**. Variabel seperti kotak kosong bernama yang boleh diisi bilangan.

Contoh: luas persegi dengan panjang sisi $s$ adalah $L = s^2$. Satu rumus ini berlaku untuk semua persegi.

- Jika $s = 3$, maka $L = 9$.
- Jika $s = 10$, maka $L = 100$.

Variabel biasanya huruf kecil seperti $x$, $y$, $n$, atau $t$. Pilihan huruf bebas, tetapi beberapa kebiasaan: $x$ untuk masukan, $y$ untuk keluaran, $n$ untuk banyaknya.

### 2. Pengertian Konstanta

**Konstanta** (*constant*) adalah nilai yang **tetap**. Konstanta bisa berupa angka biasa atau huruf yang nilainya sudah baku.

- Angka: pada $y = 2x + 3$, angka $3$ adalah konstanta.
- Huruf baku: $\pi \approx 3{,}14159$ adalah konstanta, karena nilainya tidak berubah.

!!! note "Catatan"
    Sebuah huruf bisa menjadi variabel pada satu soal dan konstanta pada soal lain. Misalnya, pada "$y = ax + b$", huruf $a$ dan $b$ adalah konstanta selama kita menganalisis satu garis tertentu, tetapi berubah bila kita berpindah ke garis lain.

### 3. Pengertian Koefisien

**Koefisien** (*coefficient*) adalah angka yang **mengalikan** sebuah variabel. Pada $2x$, koefisien $x$ adalah $2$, karena $2x$ berarti $2 \times x$.

- Pada $5y$, koefisiennya $5$.
- Pada $x$, koefisiennya $1$ (karena $x = 1 \times x$).
- Pada $-x$, koefisiennya $-1$.
- Pada $\frac{x}{2}$, koefisiennya $\frac{1}{2}$.

**Suku** (*term*) adalah bagian ekspresi yang dipisahkan oleh tanda $+$ atau $-$. Ekspresi $2x + 3$ punya dua suku: $2x$ (suku berkoefisien 2) dan $3$ (suku konstanta).

### 4. Membaca Ekspresi Matematika

**Ekspresi** (*expression*) adalah rangkaian bilangan, variabel, dan operasi **tanpa tanda sama dengan**. Contoh: $2x + 3$ atau $s^2$. Ekspresi tidak "benar" atau "salah"; ia hanya menyatakan suatu nilai.

Cara membaca penulisan:

- $2x$ berarti $2 \times x$ (tanda kali sering dihilangkan di aljabar).
- $xy$ berarti $x \times y$.
- $x^2$ berarti $x \times x$.
- $\frac{x}{2}$ berarti $x \div 2$.

**Menerjemahkan kalimat ke ekspresi:**

| Kalimat | Ekspresi |
|---|---|
| lima lebih banyak dari $n$ | $n + 5$ |
| tiga kali $m$, dikurangi 2 | $3m - 2$ |
| setengah dari $p$ | $\frac{p}{2}$ |
| kuadrat $q$, ditambah 1 | $q^2 + 1$ |

**Suku sejenis** adalah suku dengan variabel yang sama. Suku sejenis boleh dijumlahkan. $3x + 2x = 5x$, karena menurut sifat distributif, $3x + 2x = (3 + 2)x = 5x$. Suku yang berbeda variabel tidak bisa digabung: $3x + 2y$ tetap seperti itu.

### 5. Substitusi Nilai ke dalam Ekspresi

**Substitusi** berarti mengganti variabel dengan bilangan, lalu menghitung. Gunakan urutan operasi (Part 2).

Ekspresi $2x + 3$ dengan $x = 4$:

$$
2x + 3 = 2 \times 4 + 3 = 8 + 3 = 11
$$

Untuk beberapa nilai $x$:

| $x$ | $2x + 3$ | Hasil |
|---|---|---|
| -2 | $2(-2) + 3 = -4 + 3$ | -1 |
| 0 | $2(0) + 3 = 0 + 3$ | 3 |
| 1 | $2(1) + 3 = 2 + 3$ | 5 |
| 4 | $2(4) + 3 = 8 + 3$ | 11 |

**Pakai kurung saat mensubstitusi bilangan negatif.** Untuk $x^2$ dengan $x = -3$, tulis $(-3)^2 = 9$. Menulis $-3^2$ memberikan $-9$, yang salah (Part 5).

### 6. Persamaan Sederhana

**Persamaan** (*equation*) adalah pernyataan bahwa dua ekspresi **bernilai sama**, dihubungkan dengan tanda sama dengan. Contoh: $x + 3 = 7$.

Persamaan bisa benar atau salah, tergantung nilai variabelnya.

- Jika $x = 4$: $4 + 3 = 7$ **benar**.
- Jika $x = 5$: $5 + 3 = 8 \neq 7$, **salah**.

Nilai variabel yang membuat persamaan benar disebut **penyelesaian** atau **solusi**. **Menyelesaikan persamaan** berarti mencari solusi itu. Solusi $x + 3 = 7$ adalah $x = 4$.

### 7. Menyelesaikan Persamaan Linear Satu Langkah

Persamaan **linear** adalah persamaan yang variabelnya hanya berpangkat satu (tidak ada $x^2$ dan sejenisnya). Persamaan satu langkah hanya butuh **satu operasi kebalikan**.

**Prinsip penting:** persamaan seperti timbangan seimbang. Jika kamu melakukan hal yang **sama** pada **kedua sisi**, timbangan tetap seimbang dan solusinya tidak berubah.

Operasi kebalikan:

- tambah $\leftrightarrow$ kurang
- kali $\leftrightarrow$ bagi

Contoh:

- $x + 5 = 12$. Kurangi 5 di kedua sisi: $x + 5 - 5 = 12 - 5$, maka $x = 7$.
- $x - 4 = 9$. Tambah 4 di kedua sisi: $x = 13$.
- $3x = 15$. Bagi 3 di kedua sisi: $x = 5$.
- $\frac{x}{4} = 6$. Kali 4 di kedua sisi: $x = 24$.

**Periksa selalu** dengan mensubstitusi kembali ke persamaan awal: $3(5) = 15$ benar.

### 8. Menyelesaikan Persamaan Linear Dua Langkah

Jika variabel terkena **dua operasi**, bongkar dengan urutan **terbalik** dari urutan operasi. Variabel dikalikan lebih dulu, baru ditambah, jadi kita kurangi dulu, baru bagi.

Selesaikan $2x + 3 = 11$:

1. Kurangi 3 di kedua sisi: $2x + 3 - 3 = 11 - 3$, jadi $2x = 8$.
2. Bagi 2 di kedua sisi: $x = 4$.
3. Periksa: $2(4) + 3 = 11$ benar.

Contoh lain:

- $5x - 7 = 18$. Tambah 7: $5x = 25$. Bagi 5: $x = 5$.
- $\frac{x}{3} + 2 = 6$. Kurangi 2: $\frac{x}{3} = 4$. Kali 3: $x = 12$.
- $-3x + 1 = 10$. Kurangi 1: $-3x = 9$. Bagi $-3$: $x = -3$.

### 9. Memahami Simbol Sama Dengan

Tanda sama dengan ($=$) berarti **kedua sisi bernilai sama**. Ia bukan perintah "hitung hasilnya". Bayangkan timbangan dua sisi: yang kiri dan kanan berat sama.

Kesalahan umum: menulis rangkaian seperti $3 + 2 = 5 + 4 = 9$. Tanda sama dengan pertama benar ($3 + 2 = 5$), tetapi $5 + 4 = 9$ tidak berarti $3 + 2 = 9$. Setiap tanda sama dengan harus benar. Tulis setiap langkah di baris baru.

!!! warning "Di pemrograman berbeda"
    Dalam Python, `=` berarti **penugasan** (menyimpan nilai ke variabel), dan `==` berarti **membandingkan**. `x = 4` menyimpan 4 ke `x`; `x == 4` menanyakan apakah `x` sama dengan 4.

### 10. Pengantar Variabel dalam Machine Learning

Dalam Machine Learning, huruf-huruf mewakili:

- $x$: **masukan** atau fitur (misalnya jam belajar),
- $y$: **keluaran** atau target (misalnya nilai ujian),
- $w$ dan $b$: **parameter** yang diatur selama model belajar.

Model paling sederhana:

$$
y = wx + b
$$

Jika $w = 2$ dan $b = 3$, kita memperoleh $y = 2x + 3$. Pada contoh fiktif ini, $x$ adalah jam belajar dan $y$ adalah nilai prediksi. Untuk $x = 4$ jam, prediksi $y = 11$. Sebaliknya, jika kita ingin tahu berapa jam belajar yang diperlukan agar nilai prediksi 21, kita menyelesaikan $2x + 3 = 21$.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Variabel (*variable*) | huruf pengganti bilangan yang bisa berubah atau belum diketahui | $x$ |
| Konstanta (*constant*) | nilai tetap | 3 pada $2x + 3$ |
| Koefisien (*coefficient*) | pengali variabel | 2 pada $2x$ |
| Suku (*term*) | bagian ekspresi yang dipisah $+$ atau $-$ | $2x$ dan $3$ |
| Suku sejenis | suku dengan variabel yang sama | $3x$ dan $2x$ |
| Ekspresi (*expression*) | rangkaian bilangan, variabel, operasi tanpa $=$ | $2x + 3$ |
| Persamaan (*equation*) | dua ekspresi yang dinyatakan sama | $2x + 3 = 11$ |
| Substitusi | mengganti variabel dengan bilangan | $x = 4$ |
| Solusi / penyelesaian | nilai yang membuat persamaan benar | $x = 4$ |
| Linear | variabel hanya berpangkat satu | $2x + 3$ |
| Operasi kebalikan | operasi yang membatalkan operasi lain | $+$ dan $-$ |

## Rumus dan Penjelasan Setiap Simbol

**Bentuk umum persamaan linear dua langkah:**

$$
ax + b = c \quad\Longrightarrow\quad x = \frac{c - b}{a} \qquad (a \neq 0)
$$

- $x$ adalah variabel yang dicari.
- $a$ adalah koefisien $x$, dan $a \neq 0$ (kalau $a = 0$, $x$ hilang dari persamaan).
- $b$ adalah konstanta yang ditambahkan pada sisi kiri.
- $c$ adalah nilai di sisi kanan.
- Rumus kanan diperoleh dengan mengurangi $b$ di kedua sisi ($ax = c - b$) lalu membagi dengan $a$.

Contoh: $2x + 3 = 11$ memiliki $a = 2$, $b = 3$, $c = 11$. Maka $x = \frac{11 - 3}{2} = \frac{8}{2} = 4$.

**Model linear dalam Machine Learning:**

$$
\hat{y} = wx + b
$$

- $x$ adalah masukan; $w$ (bobot) dan $b$ (bias) adalah parameter.
- $\hat{y}$ dibaca "y topi", yaitu **prediksi** model, berbeda dari $y$ yang adalah nilai sebenarnya. Tanda topi ($\hat{\ }$) hanya penanda bahwa itu hasil perkiraan.

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Menerjemahkan lalu mensubstitusi

"Tiga kali sebuah bilangan, ditambah 4." Tulis sebagai ekspresi, lalu hitung untuk bilangan 5 dan $-2$.

1. Sebut bilangannya $n$. Ekspresi: $3n + 4$.
2. $n = 5$: $3(5) + 4 = 15 + 4 = 19$.
3. $n = -2$: $3(-2) + 4 = -6 + 4 = -2$.

### Contoh 2 — Persamaan satu langkah

Selesaikan $x - 4 = 9$ dan $\dfrac{x}{4} = 6$.

1. $x - 4 = 9$. Operasi pada $x$ adalah "kurang 4", kebalikannya "tambah 4". Tambah 4 di kedua sisi: $x = 13$. Periksa: $13 - 4 = 9$ benar.
2. $\frac{x}{4} = 6$. Operasi pada $x$ adalah "bagi 4", kebalikannya "kali 4". Kali 4 di kedua sisi: $x = 24$. Periksa: $\frac{24}{4} = 6$ benar.

### Contoh 3 — Persamaan dua langkah

Selesaikan $5x - 7 = 18$.

1. $x$ dikali 5, lalu dikurangi 7. Bongkar terbalik: tambah 7 dulu.
2. Tambah 7 di kedua sisi: $5x = 25$.
3. Bagi 5 di kedua sisi: $x = 5$.
4. Periksa: $5(5) - 7 = 25 - 7 = 18$ benar.

### Contoh 4 — Menyederhanakan, lalu menyelesaikan

Selesaikan $4x + 7 + 2x - 3 = 28$.

1. Gabungkan suku sejenis: $4x + 2x = 6x$ dan $7 - 3 = 4$. Persamaan menjadi $6x + 4 = 28$.
2. Kurangi 4: $6x = 24$.
3. Bagi 6: $x = 4$.
4. Periksa dengan persamaan awal: $4(4) + 7 + 2(4) - 3 = 16 + 7 + 8 - 3 = 28$ benar.

### Contoh 5 — Model linear

Pada model $y = 2x + 3$ (angka fiktif), berapa $x$ jika $y = 21$?

1. Substitusi $y = 21$: $2x + 3 = 21$.
2. Kurangi 3: $2x = 18$.
3. Bagi 2: $x = 9$.
4. Periksa: $2(9) + 3 = 21$ benar. Jadi diperlukan 9 jam.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** Koefisien pada $-7y$ adalah $-7$; pada $y$ saja adalah $1$.

**Sedang.** Sederhanakan $5a + 3a - 2a = (5 + 3 - 2)a = 6a$.

**Sedang.** Hitung $x^2 - 2x + 1$ untuk $x = -1$: $(-1)^2 - 2(-1) + 1 = 1 + 2 + 1 = 4$.

**Menantang.** Selesaikan $-4x + 3 = -9$. Kurangi 3: $-4x = -12$. Bagi $-4$: $x = 3$. Periksa: $-4(3) + 3 = -12 + 3 = -9$ benar.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $2x + 3 = 11 \Rightarrow 2x = 11 + 3$ | operasi kebalikan dari $+3$ adalah $-3$ | $2x = 11 - 3 = 8$ |
| $2x = 8 \Rightarrow x = 8 \times 2$ | kebalikan dari kali adalah bagi | $x = 8 \div 2 = 4$ |
| Melakukan operasi hanya di satu sisi | timbangan jadi tidak seimbang | lakukan di kedua sisi |
| Menggabungkan $3x + 2y = 5xy$ | variabel berbeda bukan suku sejenis | biarkan $3x + 2y$ |
| $3 + 2 = 5 + 4 = 9$ | tiap tanda sama dengan harus benar | tulis di baris terpisah |
| $-3^2$ dianggap $9$ saat $x = -3$ pada $x^2$ | tanpa kurung, minus tidak ikut dikuadratkan | $(-3)^2 = 9$ |
| Lupa memeriksa jawaban | kesalahan tanda tak terdeteksi | substitusi ke persamaan awal |
| Membagi dengan koefisien yang salah, misalnya $-3x = 9$ menjadi $x = 3$ | tanda negatif ikut | $x = -3$ |
| Mengira $=$ di Python sama dengan $=$ di matematika | `=` adalah penugasan | gunakan `==` untuk membandingkan |

## Hubungan dengan Materi Berikutnya

- **Part 8** memperkenalkan notasi sigma, indeks, dan notasi fungsi, yang semuanya memakai variabel.
- **Level 1** memperluas ke persamaan kuadrat, pertidaksamaan, sistem persamaan, dan fungsi.
- **Level 2** memakai variabel untuk vektor dan matriks.
- **Level 4** menyelesaikan masalah "cari $w$ dan $b$ terbaik" untuk regresi linear.

## Hubungan dengan Machine Learning

- **Model sebagai rumus.** Prediksi $\hat{y} = wx + b$ adalah ekspresi dengan variabel masukan $x$ dan parameter $w$, $b$.
- **Belajar = mencari parameter.** Melatih model berarti mencari $w$ dan $b$ yang membuat prediksi dekat dengan kenyataan. Itu mirip dengan menyelesaikan persamaan, hanya saja dengan banyak data dan tanpa solusi yang tepat sempurna.
- **Prediksi dan nilai sebenarnya.** Tanda topi membedakan $\hat{y}$ (prediksi) dari $y$ (target).
- **Substitusi** adalah apa yang dilakukan komputer setiap kali memprediksi: memasukkan nilai $x$ ke dalam rumus.

### Python: variabel, substitusi, dan persamaan

Variabel di Python menyimpan nilai:

```python
x = 4
y = 2 * x + 3
print(y)    # 11
```

Baris `y = 2 * x + 3` berarti: hitung ekspresi di kanan dengan `x = 4`, lalu simpan hasilnya di `y`. Tanda `*` wajib ditulis di Python (tidak boleh menulis `2x`).

Tabel substitusi dengan beberapa nilai `x`:

```python
nilai_x = [-2, 0, 1, 4]
hasil = [2 * x + 3 for x in nilai_x]
print(hasil)   # [-1, 3, 5, 11]
```

Memeriksa apakah suatu nilai merupakan solusi memakai `==`:

```python
print(2 * 4 + 3 == 11)   # True, x = 4 adalah solusi
print(2 * 5 + 3 == 11)   # False
```

Menyelesaikan $2x + 3 = 21$ dengan rumus $x = \frac{c - b}{a}$:

```python
a, b, c = 2, 3, 21
x = (c - b) / a
print(x)                  # 9.0
print(a * x + b == c)     # True
```

Hasil `9.0` (bukan `9`) karena `/` menghasilkan desimal.

## Latihan Mandiri

1. Pada $4x - 7$, sebutkan variabel, koefisien, dan konstanta. Berapa koefisien $x$ pada $-x + 5$?
2. Terjemahkan ke ekspresi: (a) lima lebih banyak dari $n$ (b) tiga kali $m$ dikurangi 2 (c) setengah dari $p$ (d) kuadrat $q$ ditambah 1
3. Sederhanakan: (a) $5a + 3a - 2a$ (b) $4x + 7 + 2x - 3$
4. Untuk $y = 3x - 2$, hitung $y$ saat (a) $x = 0$ (b) $x = 4$ (c) $x = -2$
5. Hitung $x^2 - 2x + 1$ untuk $x = 3$ dan untuk $x = -1$.
6. Selesaikan: (a) $x + 9 = 15$ (b) $x - 7 = -3$ (c) $4x = 28$ (d) $\frac{x}{5} = 3$
7. Selesaikan: (a) $3x + 5 = 20$ (b) $2x - 9 = 7$ (c) $\frac{x}{2} - 1 = 4$ (d) $-4x + 3 = -9$
8. Periksa: apakah $x = 3$ solusi $5x - 4 = 11$? Apakah $x = 2$ solusi $3x + 1 = 8$?
9. Seorang siswa menulis: $2x + 3 = 11$, maka $2x = 11 + 3 = 14$, sehingga $x = 7$. Temukan kesalahannya dan beri jawaban yang benar.
10. Pada model $y = 2x + 3$ (angka fiktif): (a) hitung $y$ saat $x = 5$; (b) cari $x$ saat $y = 21$; (c) hitung $y$ saat $x = -1{,}5$.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    Variabel: $x$. Koefisien: $4$. Konstanta: $-7$. Pada $-x + 5$, koefisien $x$ adalah $-1$.

??? success "Soal 2"
    (a) $n + 5$. (b) $3m - 2$. (c) $\frac{p}{2}$. (d) $q^2 + 1$.

??? success "Soal 3"
    (a) $(5 + 3 - 2)a = 6a$.
    (b) Suku $x$: $4x + 2x = 6x$. Konstanta: $7 - 3 = 4$. Hasil $6x + 4$.

??? success "Soal 4"
    (a) $3(0) - 2 = -2$. (b) $3(4) - 2 = 10$. (c) $3(-2) - 2 = -6 - 2 = -8$.

??? success "Soal 5"
    $x = 3$: $9 - 6 + 1 = 4$. $x = -1$: $(-1)^2 - 2(-1) + 1 = 1 + 2 + 1 = 4$.

??? success "Soal 6"
    (a) Kurangi 9: $x = 6$. (b) Tambah 7: $x = 4$. (c) Bagi 4: $x = 7$. (d) Kali 5: $x = 15$.

??? success "Soal 7"
    (a) $3x = 15$, $x = 5$. (b) $2x = 16$, $x = 8$. (c) $\frac{x}{2} = 5$, $x = 10$. (d) $-4x = -12$, $x = 3$.
    Periksa (d): $-4(3) + 3 = -9$ benar.

??? success "Soal 8"
    $x = 3$: $5(3) - 4 = 11$, benar, jadi solusi. $x = 2$: $3(2) + 1 = 7 \neq 8$, bukan solusi.

??? success "Soal 9"
    Kesalahan: untuk membatalkan "+3", seharusnya dikurangi 3, bukan ditambah. Yang benar: $2x = 11 - 3 = 8$, lalu $x = 4$. Periksa: $2(4) + 3 = 11$.

??? success "Soal 10"
    (a) $2(5) + 3 = 13$.
    (b) $2x + 3 = 21$, maka $2x = 18$, $x = 9$.
    (c) $2(-1{,}5) + 3 = -3 + 3 = 0$.

## Ringkasan Materi

- Variabel adalah huruf pengganti bilangan; konstanta nilai tetap; koefisien pengali variabel.
- Ekspresi tidak memuat tanda sama dengan; persamaan menyatakan dua ekspresi bernilai sama.
- Substitusi: ganti variabel dengan bilangan (pakai kurung untuk bilangan negatif) dan ikuti urutan operasi.
- Menyelesaikan persamaan linear: lakukan operasi kebalikan di **kedua sisi**, membongkar urutan operasi dari belakang. Selalu periksa jawaban.
- Tanda $=$ berarti kedua sisi sama nilainya; di Python, `=` adalah penugasan dan `==` perbandingan.
- Dalam ML, $y = wx + b$ adalah model sederhana dengan masukan $x$ dan parameter $w$, $b$.

## Checklist Penguasaan

- [ ] Membedakan variabel, konstanta, koefisien, dan suku dalam sebuah ekspresi.
- [ ] Menerjemahkan kalimat sederhana ke ekspresi.
- [ ] Mensubstitusi nilai (termasuk negatif) dan menghitung dengan urutan yang benar.
- [ ] Menyelesaikan persamaan satu langkah dan dua langkah serta memeriksa jawaban.
- [ ] Menjelaskan mengapa operasi harus dilakukan di kedua sisi.
- [ ] Menjelaskan perbedaan `=` dan `==` di Python.
- [ ] Menjelaskan peran $x$, $y$, $w$, $b$ dalam $y = wx + b$.
