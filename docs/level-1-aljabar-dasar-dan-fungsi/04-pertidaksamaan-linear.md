# Part 4 — Pertidaksamaan Linear

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan arti pertidaksamaan dan penyelesaiannya sebagai himpunan nilai;
- menerjemahkan frasa seperti "paling sedikit" dan "tidak lebih dari" menjadi simbol;
- menyelesaikan pertidaksamaan linear satu langkah dan dua langkah;
- menjelaskan, dengan contoh angka, mengapa tanda pertidaksamaan dibalik saat mengalikan atau membagi dengan bilangan negatif;
- menyajikan penyelesaian pada garis bilangan;
- memakai pertidaksamaan untuk batas nilai dalam soal kontekstual dan mengenali constraints sederhana dalam Machine Learning.

## Prasyarat

[Part 3 — Persamaan Linear Satu Variabel](03-persamaan-linear-satu-variabel.md) (menyelesaikan persamaan, prinsip menjaga kesetaraan) dan [Level 0 — Part 1](../level-0-fondasi-matematika/01-mengenal-bilangan.md) (garis bilangan dan perbandingan, termasuk bilangan negatif).

## Mengapa Materi Ini Penting?

Banyak keputusan berbentuk batas, bukan angka pasti: anggaran maksimum, nilai minimum untuk lulus, peluang yang harus berada antara 0 dan 1. Dalam Machine Learning, batas semacam itu disebut **constraints** (kendala). Pertidaksamaan adalah bahasa untuk menuliskannya.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Pengertian Pertidaksamaan

**Pertidaksamaan** (*inequality*) adalah pernyataan yang membandingkan dua ekspresi memakai simbol $<$, $>$, $\leq$, atau $\geq$ (bukan $=$).

Berbeda dengan persamaan yang biasanya punya satu penyelesaian, pertidaksamaan biasanya punya **banyak** penyelesaian. Misalnya $x > 3$ benar untuk $x = 3{,}1$, $x = 4$, $x = 100$, dan seterusnya, tetapi salah untuk $x = 3$ atau $x = 2$.

**Pertidaksamaan linear satu variabel** adalah pertidaksamaan dengan satu variabel berpangkat satu, seperti $2x + 3 > 11$.

**Penyelesaiannya** adalah **himpunan semua nilai** variabel yang membuat pernyataan benar. Ditulis, misalnya, $\{x \mid x > 4\}$, dibaca "himpunan semua $x$ dengan syarat $x$ lebih besar dari 4". Garis tegak "$\mid$" dibaca "dengan syarat".

### 2. Simbol Lebih Besar dan Lebih Kecil

| Simbol | Dibaca | Kata dalam soal |
|---|---|---|
| $x > a$ | $x$ lebih besar dari $a$ | lebih dari |
| $x < a$ | $x$ lebih kecil dari $a$ | kurang dari |
| $x \geq a$ | $x$ lebih besar atau sama dengan $a$ | paling sedikit, minimal, tidak kurang dari |
| $x \leq a$ | $x$ lebih kecil atau sama dengan $a$ | paling banyak, maksimal, tidak lebih dari |

Perbedaan kuncinya: $>$ dan $<$ **tidak** menyertakan batasnya ($x > 3$ tidak mencakup 3), sedangkan $\geq$ dan $\leq$ **menyertakan** batasnya ($x \geq 3$ mencakup 3).

Dua kondisi sekaligus ditulis berantai: $1 < x \leq 5$ berarti $x$ lebih besar dari 1 **dan** paling besar 5.

### 3. Pertidaksamaan Satu Langkah

Langkahnya sama seperti persamaan, dengan satu pengecualian penting (bagian 6): saat mengalikan atau membagi dengan bilangan **negatif**.

| Pertidaksamaan | Langkah | Hasil |
|---|---|---|
| $x + 3 > 7$ | kurangi 3 | $x > 4$ |
| $x - 2 \leq 5$ | tambah 2 | $x \leq 7$ |
| $3x < 12$ | bagi 3 | $x < 4$ |
| $\dfrac{x}{2} \geq 3$ | kali 2 | $x \geq 6$ |

Menambah atau mengurangi bilangan yang sama pada kedua sisi **tidak mengubah arah tanda**. Mengalikan atau membagi dengan bilangan **positif** juga tidak.

### 4. Pertidaksamaan Dua Langkah

Selesaikan $2x + 3 > 11$:

1. Kurangi 3 di kedua sisi: $2x > 8$.
2. Bagi 2 (positif, tanda tetap): $x > 4$.
3. Uji: $x = 5$ memberi $2(5) + 3 = 13 > 11$, benar. $x = 4$ memberi $11 > 11$, salah (karena $>$ tidak menyertakan 4). Jadi 4 bukan penyelesaian, sesuai $x > 4$.

Selesaikan dengan variabel di kedua sisi: $3x - 4 \leq 2x + 1$.

1. Kurangi $2x$: $x - 4 \leq 1$.
2. Tambah 4: $x \leq 5$.

### 5. Menyajikan Hasil pada Garis Bilangan

Penyelesaian digambar sebagai bagian garis bilangan:

- **Lingkaran kosong** (di sini ditulis `o`) di ujung berarti batas **tidak** termasuk ($<$ atau $>$).
- **Lingkaran penuh** (di sini `*`) berarti batas **termasuk** ($\leq$ atau $\geq$).
- Garis `=` menunjukkan seluruh nilai penyelesaian, dan tanda panah menunjukkan lanjut tak berhingga.

$x > 3$ (batas 3 tidak termasuk, lanjut ke kanan):

```text
   -2   -1    0    1    2    3    4    5    6    7    8
────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼───▶
                             o===========================>
```

$x \leq 2$ (batas 2 termasuk, lanjut ke kiri):

```text
   -2   -1    0    1    2    3    4    5    6    7    8
────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼───▶
<=======================*                                 
```

$-1 < x \leq 3$ (dari $-1$ tidak termasuk sampai 3 termasuk):

```text
   -2   -1    0    1    2    3    4    5    6    7    8
────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼───▶
         o===================*                            
```

Cara membaca gambar pertama: titik $o$ berada tepat di bawah angka 3 pada garis bilangan, lalu garis `=` mengarah ke kanan, mencakup 4, 5, 6, dan seterusnya. Titik 3 sendiri tidak ikut.

**Notasi interval** (opsional) menulis hasil yang sama lebih ringkas: $x > 3$ menjadi $(3, \infty)$, dan $x \leq 2$ menjadi $(-\infty, 2]$. Kurung biasa berarti batas tidak termasuk, kurung siku berarti termasuk. Simbol $\infty$ berarti "tak berhingga" dan bukan bilangan.

### 6. Membalik Tanda Pertidaksamaan ketika Mengalikan atau Membagi dengan Bilangan Negatif

**Aturan:** jika kedua sisi dikalikan atau dibagi dengan bilangan **negatif**, arah tanda pertidaksamaan **dibalik**.

Jangan menghafal dulu; lihat dengan angka. Mulai dari pernyataan benar $2 < 5$.

| Operasi pada kedua sisi | Hasil | Pernyataan |
|---|---|---|
| tambah 3 | $5$ dan $8$ | $5 < 8$, benar |
| kali 3 (positif) | $6$ dan $15$ | $6 < 15$, benar |
| kali $-3$ (negatif) | $-6$ dan $-15$ | $-6 < -15$ **salah**; yang benar $-6 > -15$ |

Setelah dikalikan $-3$, urutan angkanya terbalik: $-6$ berada di kanan $-15$ pada garis bilangan, sehingga $-6 > -15$.

**Alasan.** Mengalikan dengan $-1$ memantulkan garis bilangan terhadap titik 0: yang tadinya di kiri menjadi di kanan, sehingga urutan terbalik. Mengalikan dengan bilangan positif hanya menggeser dan meregangkan, tanpa membalik urutan.

Contoh: selesaikan $-2x < 6$.

1. Bagi kedua sisi dengan $-2$ dan balik tanda: $x > -3$.
2. Uji dengan titik: $x = 0$ memberi $-2(0) = 0 < 6$, benar, dan $0 > -3$ benar. $x = -5$ memberi $-2(-5) = 10 < 6$, salah, dan $-5 > -3$ salah. Keduanya konsisten.

Jika kamu **lupa** membalik tanda, hasilnya $x < -3$, dan uji $x = -5$ akan menunjukkan kontradiksi: $-5 < -3$ benar, tetapi $10 < 6$ salah. Kebiasaan menguji dengan satu titik sangat membantu.

### 7. Pertidaksamaan dalam Konteks Batas Nilai

Mengubah soal cerita menjadi pertidaksamaan:

1. Beri nama variabel dan artinya.
2. Terjemahkan frasa batas ("paling banyak", "paling sedikit") menjadi simbol.
3. Selesaikan.
4. **Tafsirkan** jawaban sesuai konteks (misalnya banyak barang harus bilangan bulat tak negatif).

**Anggaran.** Uang Rp50.000. Sebuah buku Rp12.000 dan pulpen Rp3.000 per buah. Berapa pulpen paling banyak jika membeli 1 buku? Misalkan $n$ banyak pulpen:

$$
12.000 + 3.000n \leq 50.000 \quad\Rightarrow\quad 3.000n \leq 38.000 \quad\Rightarrow\quad n \leq 12{,}67\ldots
$$

Karena pulpen berupa bilangan bulat, $n$ paling banyak 12.

**Rata-rata minimal.** Dua nilai 70 dan 80. Berapa nilai ketiga $x$ agar rata-rata paling sedikit 75?

$$
\frac{70 + 80 + x}{3} \geq 75 \quad\Rightarrow\quad 150 + x \geq 225 \quad\Rightarrow\quad x \geq 75
$$

**Pertidaksamaan berantai.** Selesaikan $1 < 2x + 3 \leq 9$ dengan mengoperasikan **ketiga bagian** sekaligus: kurangi 3 menjadi $-2 < 2x \leq 6$, lalu bagi 2 menjadi $-1 < x \leq 3$ (gambar ketiga di atas).

### 8. Pengantar Constraints dalam Machine Learning

**Constraint** adalah batas yang harus dipenuhi sebuah nilai. Contoh yang nyata:

- **Peluang** harus berada di antara 0 dan 1: $0 \leq p \leq 1$.
- **Laju belajar** (*learning rate*) harus positif: $\eta > 0$.
- **Ukuran batch** (jumlah data per langkah) paling sedikit 1: $B \geq 1$.
- **Ambang keputusan.** Banyak model klasifikasi mengubah peluang menjadi kelas: jika $\hat{p} \geq 0{,}5$, prediksi kelas 1; selain itu kelas 0. Itu pertidaksamaan yang dipakai sebagai aturan keputusan.
- **Parameter non-negatif.** Kadang parameter dibatasi $w \geq 0$.

Level lanjutan akan membahas constraint yang lebih kompleks. Di sini cukup memahami bahwa pertidaksamaan adalah bahasa untuk batas semacam itu.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Pertidaksamaan (*inequality*) | perbandingan dengan $<$, $>$, $\leq$, $\geq$ | $2x + 3 > 11$ |
| Himpunan penyelesaian | semua nilai yang membuat pernyataan benar | $\{x \mid x > 4\}$ |
| Batas (*bound*) | nilai di ujung penyelesaian | 4 pada $x > 4$ |
| Tidak termasuk | batas tidak ikut ($<$, $>$) | lingkaran kosong |
| Termasuk | batas ikut ($\leq$, $\geq$) | lingkaran penuh |
| Pertidaksamaan berantai | dua pembatasan sekaligus | $1 < x \leq 5$ |
| Interval | penulisan penyelesaian dengan kurung | $(3, \infty)$ |
| Constraint (kendala) | batas yang harus dipenuhi | $0 \leq p \leq 1$ |

## Rumus dan Penjelasan Setiap Simbol

**Sifat pertidaksamaan.** Misalkan $a < b$, dan $c$ adalah bilangan sembarang.

| Operasi | Hasil | Syarat | Contoh dengan $a = 2$, $b = 5$ |
|---|---|---|---|
| tambah $c$ | $a + c < b + c$ | $c$ sembarang | $c = 3$: $5 < 8$ |
| kurang $c$ | $a - c < b - c$ | $c$ sembarang | $c = 3$: $-1 < 2$ |
| kali $c$ | $ac < bc$ | $c > 0$ | $c = 3$: $6 < 15$ |
| kali $c$ | $ac > bc$ | $c < 0$ | $c = -3$: $-6 > -15$ |
| bagi $c$ | arah mengikuti aturan kali | $c \neq 0$ | |

Simbol: $a$ dan $b$ adalah dua bilangan dengan $a < b$; $c$ adalah bilangan yang dipakai pada kedua sisi. Aturan yang sama berlaku untuk $>$, $\leq$, dan $\geq$ (untuk $\leq$ dan $\geq$, tanda yang dibalik adalah $\leq \leftrightarrow \geq$).

## Penjelasan Langkah demi Langkah

Menyelesaikan $5 - 2x \geq 1$:

| Langkah | Pertidaksamaan | Alasan |
|---|---|---|
| 0 | $5 - 2x \geq 1$ | awal |
| 1 | $-2x \geq -4$ | kurangi 5 di kedua sisi (tanda tetap) |
| 2 | $x \leq 2$ | bagi $-2$ (negatif), **balik tanda** |
| 3 | $x = 2$: $5 - 4 = 1 \geq 1$ benar; $x = 3$: $5 - 6 = -1 \geq 1$ salah | uji batas dan satu titik di luar |

## Contoh Soal dan Pembahasan

### Contoh 1 — Dua langkah dengan tanda tetap

Selesaikan $3x - 5 \leq 10$.

1. Tambah 5: $3x \leq 15$.
2. Bagi 3 (positif): $x \leq 5$.
3. Uji: $x = 5$ memberi $10 \leq 10$, benar. $x = 6$ memberi $13 \leq 10$, salah.

### Contoh 2 — Tanda dibalik

Selesaikan $5 - 2x \geq 1$ dan gambarkan di garis bilangan.

1. Kurangi 5: $-2x \geq -4$.
2. Bagi $-2$ dan balik tanda: $x \leq 2$.
3. Garis bilangan: lingkaran penuh di 2 (karena $\leq$), garis ke kiri (gambar $x \leq 2$ di atas).

### Contoh 3 — Variabel di kedua sisi dan kurung

Selesaikan $3(x - 1) < x + 5$.

1. Jabarkan: $3x - 3 < x + 5$.
2. Kurangi $x$: $2x - 3 < 5$.
3. Tambah 3: $2x < 8$.
4. Bagi 2: $x < 4$.
5. Uji: $x = 3$ memberi $3(2) = 6 < 8$, benar. $x = 4$ memberi $9 < 9$, salah.

### Contoh 4 — Soal cerita

Sebuah kelas membutuhkan rata-rata paling sedikit 75 dari tiga nilai. Dua nilai pertama 70 dan 80. Berapa nilai ketiga minimal?

1. Misalkan nilai ketiga $x$. Syarat: $\dfrac{70 + 80 + x}{3} \geq 75$.
2. Kali 3: $150 + x \geq 225$.
3. Kurangi 150: $x \geq 75$.
4. Jadi nilai ketiga paling sedikit 75. Periksa: $x = 75$ memberi rata-rata $\dfrac{225}{3} = 75$, tepat memenuhi syarat.

### Contoh 5 — Pertidaksamaan berantai

Selesaikan $-4 \leq 3x - 1 < 5$.

1. Tambah 1 pada ketiga bagian: $-3 \leq 3x < 6$.
2. Bagi 3: $-1 \leq x < 2$.
3. Uji: $x = -1$ memberi $3(-1) - 1 = -4$, dan $-4 \leq -4$ benar. $x = 2$ memberi $5 < 5$, salah. Cocok dengan batas kiri termasuk dan batas kanan tidak termasuk.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** $x + 4 > 9$ memberi $x > 5$.

**Sedang.** $-3x < 12$: bagi $-3$ dan balik tanda, $x > -4$. Uji $x = 0$: $0 < 12$ benar.

**Sedang.** $5x + 2 > 8x - 7$. Kurangi $8x$: $-3x + 2 > -7$. Kurangi 2: $-3x > -9$. Bagi $-3$ dan balik: $x < 3$. Uji $x = 0$: $2 > -7$ benar. Uji $x = 4$: $22 > 25$ salah.

**Menantang.** $-\dfrac{x}{2} \leq 3$. Kali $-2$ dan balik tanda: $x \geq -6$. Uji $x = 0$: $0 \leq 3$ benar. Uji $x = -10$: $5 \leq 3$ salah.

## Visualisasi atau Interpretasi Grafik

Garis bilangan di bagian 5 menunjukkan dua hal sekaligus: **batas** (titik) dan **arah** (garis). Pada soal kontekstual, hanya sebagian garis itu yang bermakna. Misalnya, pada soal pulpen, $n \leq 12{,}67$ digambar sebagai garis ke kiri dari 12,67, tetapi yang berlaku hanya bilangan bulat tak negatif: $0, 1, 2, \ldots, 12$.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $-2x < 6 \Rightarrow x < -3$ | lupa membalik tanda saat membagi $-2$ | $x > -3$ |
| Membalik tanda saat menambah atau mengurang | hanya kali/bagi dengan negatif yang membalik | tanda tetap |
| $3x + 2 > 11 \Rightarrow 3x > 13$ | salah kurang: $11 - 2 = 9$ | $3x > 9$, $x > 3$ |
| Lingkaran kosong untuk $\leq$ | $\leq$ menyertakan batas | lingkaran penuh |
| Menyertakan batas pada $x > 4$ | $>$ tidak menyertakan batas | $x = 4$ bukan penyelesaian |
| Mengartikan "paling banyak 5" sebagai $x < 5$ | paling banyak berarti boleh sama | $x \leq 5$ |
| Menjawab $n \leq 12{,}67$ untuk banyak pulpen | banyak pulpen bilangan bulat | $n \leq 12$ |
| Mengoperasikan hanya bagian tengah pada $1 < 2x + 3 \leq 9$ | semua bagian harus diperlakukan sama | operasikan ketiga bagian |
| Tidak menguji hasil | kesalahan tanda tidak terdeteksi | uji satu titik di dalam dan satu di luar |

## Hubungan dengan Machine Learning

- **Ambang keputusan.** Aturan "prediksi kelas 1 jika $\hat{p} \geq 0{,}5$" adalah pertidaksamaan. Mengubah ambang (misalnya menjadi $0{,}7$) mengubah perilaku model.
- **Batas nilai.** Peluang ($0 \leq p \leq 1$), laju belajar ($\eta > 0$), dan ukuran batch ($B \geq 1$) semuanya constraint berbentuk pertidaksamaan.
- **Memeriksa data.** Validasi data sering berbentuk pertidaksamaan, misalnya "umur harus antara 0 dan 120".
- **Constraint pada parameter** (misalnya membatasi besar $w$) muncul dalam teknik regularisasi, yang dibahas di level berikutnya. Pelajari saja ide dasarnya sekarang: batas pada nilai.

### Python: pertidaksamaan, penyaringan, dan pembatasan nilai

Python memakai simbol yang sangat mirip: `<`, `>`, `<=`, `>=`. Hasilnya `True` atau `False`.

```python
print(5 > 3)        # True
print(4 > 4)        # False, > tidak menyertakan batas
print(4 >= 4)       # True
x = 5
print(2 * x + 3 > 11)   # True, x = 5 memenuhi 2x + 3 > 11
```

Pertidaksamaan berantai bisa ditulis seperti di matematika:

```python
p = 0.7
print(0 <= p <= 1)   # True
```

Menyaring semua $x$ yang memenuhi $x > 3$ dari sebuah daftar:

```python
data = [-2, 0, 3, 3.1, 4, 10]
lolos = [x for x in data if x > 3]
print(lolos)   # [3.1, 4, 10]
```

Ambang keputusan: ubah peluang menjadi kelas.

```python
peluang = [0.1, 0.5, 0.49, 0.92]
kelas = [1 if p >= 0.5 else 0 for p in peluang]
print(kelas)   # [0, 1, 0, 1]
```

Membatasi nilai ke rentang $[0, 1]$ (disebut *clipping*):

```python
def batasi(p):
    return max(0, min(p, 1))

print(batasi(-0.2))  # 0
print(batasi(0.4))   # 0.4
print(batasi(1.7))   # 1
```

`min(p, 1)` memastikan nilai tidak melebihi 1 dan `max(0, ...)` memastikan tidak kurang dari 0.

Menyelesaikan soal pulpen dengan mencoba nilai:

```python
n_maks = max(n for n in range(0, 30) if 12000 + 3000 * n <= 50000)
print(n_maks)   # 12
```

## Latihan Mandiri

1. Tulis dengan simbol: (a) $x$ paling sedikit 5 (b) $x$ kurang dari $-2$ (c) $x$ tidak lebih dari 7 (d) $x$ lebih dari 0 dan paling banyak 10
2. Selesaikan: (a) $x + 4 > 9$ (b) $x - 3 \leq 2$ (c) $4x < 20$ (d) $\dfrac{x}{3} \geq 2$
3. Selesaikan: (a) $2x + 3 > 11$ (b) $3x - 5 \leq 10$
4. Selesaikan dan sebutkan kapan tanda dibalik: (a) $-3x < 12$ (b) $5 - 2x \geq 1$ (c) $-\dfrac{x}{2} \leq 3$
5. Selesaikan: (a) $3x - 4 \leq 2x + 1$ (b) $5x + 2 > 8x - 7$
6. Selesaikan: (a) $3(x - 1) < x + 5$ (b) $2(x + 3) \geq 4x - 2$
7. Selesaikan: (a) $1 < 2x + 3 \leq 9$ (b) $-4 \leq 3x - 1 < 5$
8. Apakah $x = 2$ memenuhi $5 - 2x \geq 1$? Apakah $x = 3$? Jelaskan mengapa uji ini membantu memeriksa pembalikan tanda.
9. (a) Uang Rp50.000. Satu buku Rp12.000, pulpen Rp3.000 per buah. Berapa pulpen paling banyak? (b) Dua nilai 70 dan 80. Berapa nilai ketiga minimal agar rata-rata paling sedikit 75?
10. Temukan kesalahan: (a) $-2x < 6 \Rightarrow x < -3$ (b) $3x + 2 > 11 \Rightarrow 3x > 13$ (c) Penyelesaian $x \leq 4$ digambar dengan lingkaran kosong di 4.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) $x \geq 5$. (b) $x < -2$. (c) $x \leq 7$. (d) $0 < x \leq 10$.

??? success "Soal 2"
    (a) $x > 5$. (b) $x \leq 5$. (c) $x < 5$. (d) $x \geq 6$.

??? success "Soal 3"
    (a) $2x > 8$, $x > 4$. (b) $3x \leq 15$, $x \leq 5$.

??? success "Soal 4"
    (a) Bagi $-3$: $x > -4$ (**dibalik**). Uji $x = 0$: $0 < 12$ benar.
    (b) $-2x \geq -4$, bagi $-2$: $x \leq 2$ (**dibalik**).
    (c) Kali $-2$: $x \geq -6$ (**dibalik**).

??? success "Soal 5"
    (a) Kurangi $2x$: $x - 4 \leq 1$, $x \leq 5$ (tanda tetap).
    (b) Kurangi $8x$: $-3x + 2 > -7$; kurangi 2: $-3x > -9$; bagi $-3$ dan balik: $x < 3$.

??? success "Soal 6"
    (a) $3x - 3 < x + 5$, $2x < 8$, $x < 4$.
    (b) $2x + 6 \geq 4x - 2$. Kurangi $2x$: $6 \geq 2x - 2$. Tambah 2: $8 \geq 2x$. Bagi 2: $4 \geq x$, yaitu $x \leq 4$.

??? success "Soal 7"
    (a) Kurangi 3: $-2 < 2x \leq 6$. Bagi 2: $-1 < x \leq 3$.
    (b) Tambah 1: $-3 \leq 3x < 6$. Bagi 3: $-1 \leq x < 2$.

??? success "Soal 8"
    $x = 2$: $5 - 4 = 1 \geq 1$, **ya**. $x = 3$: $5 - 6 = -1 \geq 1$, **tidak**. Hasil $x \leq 2$ menyertakan 2 dan menolak 3, sesuai uji. Jika tanda tidak dibalik (menjadi $x \geq 2$), $x = 3$ akan dianggap benar padahal tidak, sehingga uji ini mendeteksi kesalahan.

??? success "Soal 9"
    (a) $12.000 + 3.000n \leq 50.000$, $3.000n \leq 38.000$, $n \leq 12{,}67$. Banyak pulpen bilangan bulat, jadi paling banyak **12**.
    (b) $\dfrac{150 + x}{3} \geq 75$, $150 + x \geq 225$, $x \geq 75$. Nilai ketiga minimal **75**.

??? success "Soal 10"
    (a) Saat membagi dengan $-2$ tanda harus dibalik: $x > -3$.
    (b) $11 - 2 = 9$, jadi $3x > 9$ dan $x > 3$.
    (c) Karena memakai $\leq$, batas 4 termasuk, jadi lingkaran harus penuh.

## Ringkasan Materi

- Pertidaksamaan membandingkan dua ekspresi dengan $<$, $>$, $\leq$, $\geq$. Penyelesaiannya biasanya berupa himpunan banyak nilai.
- Frasa "paling sedikit" berarti $\geq$, "paling banyak" berarti $\leq$, "lebih dari" berarti $>$, "kurang dari" berarti $<$.
- Tambah, kurang, kali, atau bagi dengan bilangan positif tidak mengubah arah tanda.
- Kali atau bagi dengan bilangan **negatif** membalik arah tanda, karena urutan bilangan terbalik ($2 < 5$ tetapi $-6 > -15$).
- Garis bilangan: lingkaran kosong untuk batas tidak termasuk, lingkaran penuh untuk batas termasuk.
- Uji satu titik di dalam dan satu di luar penyelesaian untuk memeriksa hasil.
- Dalam ML, pertidaksamaan menyatakan constraints seperti $0 \leq p \leq 1$ dan ambang keputusan.

## Checklist Penguasaan

- [ ] Menerjemahkan "paling sedikit", "paling banyak", "kurang dari", dan "lebih dari" ke simbol.
- [ ] Menyelesaikan pertidaksamaan satu dan dua langkah, termasuk variabel di kedua sisi.
- [ ] Menjelaskan dengan contoh angka mengapa tanda dibalik untuk pengali negatif.
- [ ] Menggambar penyelesaian di garis bilangan dengan lingkaran kosong atau penuh yang benar.
- [ ] Menyelesaikan pertidaksamaan berantai.
- [ ] Menafsirkan hasil sesuai konteks (misalnya bilangan bulat).
- [ ] Memberi satu contoh constraint dalam Machine Learning.

## Prasyarat untuk Materi Berikutnya

[Part 5 — Koordinat dan Grafik](05-koordinat-dan-grafik.md) tidak bergantung langsung pada pertidaksamaan, tetapi memakai garis bilangan dan perbandingan nilai. [Part 12](12-integrasi-dan-evaluasi.md) menggabungkan persamaan dan pertidaksamaan dalam soal kontekstual.
