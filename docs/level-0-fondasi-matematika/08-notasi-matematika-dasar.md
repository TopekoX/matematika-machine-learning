# Part 8 — Notasi Matematika Dasar

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- membaca simbol operasi, perbandingan, kesamaan, dan tanda kurung dengan benar;
- membaca notasi pecahan, pangkat, dan akar;
- membaca dan menghitung notasi sigma ($\Sigma$) dari daftar bilangan;
- memahami subskrip dan indeks, termasuk perbedaan penomoran di matematika dan Python;
- membaca notasi fungsi $f(x)$ dan menghitung nilainya;
- membaca rumus matematika secara bertahap, termasuk rumus kesalahan rata-rata kuadrat.

## Prasyarat

[Part 1](01-mengenal-bilangan.md) sampai [Part 7](07-variabel-dan-persamaan.md), terutama operasi dasar, urutan operasi, pangkat dan akar, serta variabel.

## Mengapa Materi Ini Penting?

Rumus adalah kalimat yang ditulis dengan simbol. Banyak orang merasa matematika sulit bukan karena konsepnya, melainkan karena simbolnya tidak dikenali. Bagian ini adalah "kamus" untuk membaca rumus Machine Learning yang akan kamu temui di level berikutnya.

## Penjelasan Konsep dari Dasar

### 1. Simbol Penjumlahan, Pengurangan, Perkalian, dan Pembagian

| Operasi | Simbol | Catatan |
|---|---|---|
| Penjumlahan | $+$ | |
| Pengurangan | $-$ | tanda minus juga menandai bilangan negatif |
| Perkalian | $\times$, $\cdot$, atau tanpa simbol | $2x$ berarti $2 \times x$ |
| Pembagian | $\div$, $/$, atau garis pecahan | $\frac{a}{b}$ berarti $a \div b$ |

Dalam aljabar, huruf $x$ (variabel) mudah tertukar dengan tanda kali $\times$, sehingga perkalian sering ditulis dengan titik ($2 \cdot 3$) atau dirapatkan ($ab$). Di bahasa pemrograman, perkalian ditulis `*`, pembagian `/`, dan pangkat `**`.

### 2. Simbol Lebih Besar dan Lebih Kecil

| Simbol | Dibaca | Contoh |
|---|---|---|
| $<$ | lebih kecil dari | $2 < 5$ |
| $>$ | lebih besar dari | $5 > 2$ |
| $\leq$ | lebih kecil atau sama dengan | $x \leq 3$ |
| $\geq$ | lebih besar atau sama dengan | $x \geq 0$ |

Mudah diingat: mulut simbol menghadap bilangan yang **lebih besar**.

Beberapa perbandingan dapat dirangkai. $1 < x \leq 5$ dibaca "$x$ lebih besar dari 1 dan sekaligus kurang dari atau sama dengan 5". Bilangan bulat yang memenuhi: 2, 3, 4, 5. Perhatikan bahwa 1 tidak termasuk (karena $<$) dan 5 termasuk (karena $\leq$).

### 3. Simbol Sama Dengan dan Tidak Sama Dengan

| Simbol | Dibaca | Contoh |
|---|---|---|
| $=$ | sama dengan | $\frac{1}{2} = 0{,}5$ |
| $\neq$ | tidak sama dengan | $3 \neq 5$ |
| $\approx$ | kira-kira sama dengan | $\pi \approx 3{,}14$ |

Gunakan $\approx$ untuk hasil pembulatan atau pendekatan, dan $=$ hanya jika kedua sisi benar-benar sama.

### 4. Tanda Kurung

Ada beberapa bentuk, dan artinya bergantung pada konteks:

- $( \ )$ **kurung biasa**: mengelompokkan operasi ($2(3 + 4)$), menandai argumen fungsi ($f(x)$), dan menulis titik ($(3, 4)$).
- $[ \ ]$ **kurung siku**: pengelompokan tambahan agar kurung bersarang mudah dibaca, misalnya $2[3 + (4 - 1)]$.
- $\{ \ \}$ **kurung kurawal**: menandai himpunan, misalnya $\{1, 2, 3\}$.

Kurung bersarang dikerjakan dari dalam ke luar, seperti di Part 2: $2[3 + (4 - 1)] = 2[3 + 3] = 2 \times 6 = 12$.

### 5. Notasi Pecahan

Pecahan ditulis dengan tiga cara yang berarti sama:

$$
\frac{3}{4}, \qquad 3/4, \qquad 3 \div 4
$$

Garis pecahan juga berfungsi sebagai **tanda kurung tak terlihat**. Pada $\frac{a + b}{c}$, seluruh $a + b$ dibagi $c$. Pada tulisan satu baris, kurung harus ditulis: $(a + b) / c$. Tulisan $a + b / c$ berarti $a + (b / c)$, yang berbeda.

Pecahan boleh bertingkat: $\dfrac{\frac{1}{2}}{3} = \frac{1}{2} \div 3 = \frac{1}{6}$.

### 6. Notasi Pangkat dan Akar

- $x^2$ dibaca "x pangkat dua" atau "x kuadrat". Angka kecil di kanan atas adalah eksponen.
- $x^{n}$ dibaca "x pangkat n". Jika eksponennya lebih dari satu karakter, ditulis dalam kurung kurawal di LaTeX: `x^{10}`.
- $\sqrt{x}$ dibaca "akar x" (akar kuadrat). $\sqrt[3]{x}$ dibaca "akar pangkat tiga dari x".
- $x^{-1} = \frac{1}{x}$ dan $x^{1/2} = \sqrt{x}$ (Part 5).

Garis di atas isi akar menunjukkan seberapa jauh akar berlaku: $\sqrt{9 + 16}$ adalah akar dari $25$, sedangkan $\sqrt{9} + 16$ adalah $3 + 16 = 19$.

### 7. Notasi Sigma

**Notasi sigma** adalah cara singkat menulis penjumlahan banyak suku. Lambangnya huruf Yunani kapital $\Sigma$ (sigma).

**Mulai dari daftar kecil.** Misalkan ada tiga bilangan: $2, 4, 6$. Beri nama menurut urutannya:

- bilangan pertama $x_1 = 2$,
- bilangan kedua $x_2 = 4$,
- bilangan ketiga $x_3 = 6$.

Jumlahnya adalah $x_1 + x_2 + x_3 = 2 + 4 + 6 = 12$. Dengan sigma, kalimat itu ditulis:

$$
\sum_{i=1}^{3} x_i = x_1 + x_2 + x_3 = 12
$$

**Membacanya bagian demi bagian:**

| Bagian | Nama | Arti |
|---|---|---|
| $\Sigma$ | sigma | "jumlahkan semuanya" |
| $i$ | indeks | penghitung yang berganti nilai |
| $i = 1$ (di bawah) | batas bawah | mulai dengan $i = 1$ |
| $3$ (di atas) | batas atas | berhenti setelah $i = 3$ |
| $x_i$ | suku | yang dijumlahkan; ganti $i$ dengan nilainya |

**Bentuk umum.** Untuk $n$ bilangan:

$$
\sum_{i=1}^{n} x_i = x_1 + x_2 + \cdots + x_n
$$

Tanda $\cdots$ berarti "dan seterusnya sampai suku terakhir".

Suku di dalam sigma boleh berupa ekspresi dalam $i$. Misalnya:

$$
\sum_{i=1}^{4} i^2 = 1^2 + 2^2 + 3^2 + 4^2 = 1 + 4 + 9 + 16 = 30
$$

Dua sifat yang berguna:

- Konstanta yang dijumlahkan: $\sum_{i=1}^{n} c = n \cdot c$. Contoh $\sum_{i=1}^{6} 2 = 6 \times 2 = 12$.
- Pengali boleh keluar: $\sum_{i=1}^{n} a\,x_i = a \sum_{i=1}^{n} x_i$. Contoh dengan $x = (2, 4, 6)$ dan $a = 3$: $3 \times 12 = 36$, sama dengan $6 + 12 + 18 = 36$.

**Rata-rata.** Setelah memahami sigma, **rata-rata** (*mean*) dari $n$ bilangan ditulis:

$$
\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

Tanda garis di atas $x$ ($\bar{x}$, dibaca "x bar") menandai rata-rata. Untuk $2, 4, 6$: $\bar{x} = \frac{1}{3} \times 12 = 4$.

### 8. Subskrip dan Indeks

**Subskrip** (*subscript*) adalah angka atau huruf kecil di kanan bawah sebuah simbol, seperti $x_1$ atau $x_i$. Subskrip punya dua fungsi:

- **Indeks (nomor urut).** $x_i$ berarti "bilangan ke-$i$". Indeks menunjuk satu anggota tertentu dari sebuah deretan atau kumpulan data. $x_3$ adalah bilangan ketiga.
- **Label.** Dalam $N_{\text{baru}}$ (Part 4), subskrip hanya nama dan bukan nomor urut.

**Indeks dua dimensi.** Untuk data tabel, $x_{ij}$ berarti data pada baris ke-$i$ (sampel ke-$i$) dan kolom ke-$j$ (fitur ke-$j$).

| | fitur 1 | fitur 2 |
|---|---|---|
| sampel 1 | $x_{11} = 5$ | $x_{12} = 7$ |
| sampel 2 | $x_{21} = 3$ | $x_{22} = 8$ |

!!! warning "Python mulai dari 0"
    Di matematika, penomoran biasanya mulai dari 1 ($x_1$ adalah yang pertama). Di Python, penomoran indeks mulai dari **0**. Jika `x = [2, 4, 6]`, maka `x[0]` bernilai 2 (yang pertama), `x[1]` bernilai 4, dan `x[2]` bernilai 6. Bagi pemula, ini sumber kesalahan yang sangat umum.

### 9. Pengantar Notasi Fungsi

**Fungsi** (*function*) adalah aturan yang mengubah sebuah masukan menjadi tepat satu keluaran. Notasi $f(x)$ dibaca "f dari x":

$$
f(x) = 2x + 3
$$

- $f$ adalah **nama** fungsi (bisa huruf lain, seperti $g$ atau $h$).
- $x$ adalah **masukan** (variabel).
- $2x + 3$ adalah **aturan** yang menghitung keluaran.

Untuk mencari nilai fungsi, substitusi masukan ke dalam aturan: $f(4) = 2(4) + 3 = 11$.

!!! warning "f(x) bukan f dikali x"
    $f(x)$ tidak berarti $f \times x$. Tanda kurung di sini menandai **masukan** fungsi.

Fungsi lain: $g(x) = x^2$ memberi $g(3) = 9$ dan $g(-3) = 9$. Jawaban boleh sama untuk masukan yang berbeda.

Dalam Machine Learning, model ditulis sebagai fungsi: $\hat{y} = f(x)$. Fungsi khusus yang akan sering kamu temui adalah sigmoid, $\sigma(x) = \frac{1}{1 + e^{-x}}$. Untuk $x = 0$: $e^{0} = 1$, sehingga $\sigma(0) = \frac{1}{1 + 1} = \frac{1}{2} = 0{,}5$. Fungsi dibahas lengkap di Level 1.

### 10. Cara Membaca Rumus Matematika

Rumus yang panjang menjadi mudah jika dibaca dengan urutan berikut:

1. **Kenali jenisnya.** Apakah persamaan, ekspresi, atau definisi fungsi?
2. **Daftar simbolnya.** Mana variabel, konstanta, indeks, dan batas? Cari arti setiap simbol yang belum dikenal.
3. **Baca dari dalam ke luar.** Ikuti urutan operasi: kurung, pangkat, kali-bagi, tambah-kurang.
4. **Terjemahkan ke kata-kata.** Ucapkan rumus sebagai kalimat.
5. **Coba angka kecil.** Isi dengan 2 atau 3 data sederhana dan hitung manual.

**Huruf Yunani** yang sering dipakai (arti bisa berbeda antarbuku; periksa keterangan di tiap sumber):

| Huruf | Nama | Penggunaan umum |
|---|---|---|
| $\alpha$, $\beta$, $\gamma$ | alfa, beta, gamma | parameter atau koefisien |
| $\theta$ | teta | parameter model |
| $\eta$ | eta | laju belajar |
| $\lambda$ | lambda | pengatur regularisasi |
| $\mu$ | mu | rata-rata |
| $\sigma$ | sigma kecil | simpangan baku (jangan dikacaukan dengan $\Sigma$ kapital) |
| $\Delta$ | delta kapital | perubahan |
| $\pi$ | pi | 3,14159... |

**Contoh membaca: rata-rata kesalahan kuadrat.**

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} \left( \hat{y}_i - y_i \right)^2
$$

Baca per bagian:

- $\text{MSE}$: nama ukuran (*mean squared error*, rata-rata kuadrat kesalahan).
- $n$: banyaknya data.
- $y_i$: nilai sebenarnya data ke-$i$. $\hat{y}_i$: prediksi untuk data ke-$i$.
- $\hat{y}_i - y_i$: kesalahan data ke-$i$ (Part 2).
- $(\cdot)^2$: dikuadratkan, sehingga tidak negatif (Part 5).
- $\sum_{i=1}^{n}$: jumlahkan untuk semua data.
- $\frac{1}{n}$: bagi dengan banyaknya data, yaitu rata-rata.

Dalam kata: "Untuk setiap data, hitung selisih prediksi dan nilai sebenarnya, kuadratkan, jumlahkan semuanya, lalu bagi dengan banyaknya data."

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Sigma ($\Sigma$) | lambang penjumlahan banyak suku | $\sum_{i=1}^{3} x_i$ |
| Indeks (*index*) | nomor urut anggota deretan | $i$ pada $x_i$ |
| Subskrip (*subscript*) | tulisan kecil di kanan bawah simbol | $x_2$ |
| Batas bawah / atas | nilai awal dan akhir indeks pada sigma | 1 dan $n$ |
| Rata-rata (*mean*) | jumlah dibagi banyaknya data | $\bar{x} = \frac{1}{n}\sum x_i$ |
| Fungsi (*function*) | aturan masukan menjadi satu keluaran | $f(x) = 2x + 3$ |
| Masukan / argumen | nilai di dalam kurung fungsi | 4 pada $f(4)$ |
| Prediksi | nilai perkiraan model | $\hat{y}$ |
| Himpunan (*set*) | kumpulan anggota | $\{1, 2, 3\}$ |

## Rumus dan Penjelasan Setiap Simbol

**Sigma:**

$$
\sum_{i=1}^{n} x_i = x_1 + x_2 + \cdots + x_n
$$

- $n$ adalah banyaknya suku, bilangan bulat positif.
- $x_i$ adalah suku ke-$i$.
- Sigma berlaku bila batas bawah tidak lebih besar dari batas atas. Contoh: $n = 3$ dan $x = (2, 4, 6)$ memberi 12.

**Rata-rata:**

$$
\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

- $\bar{x}$ adalah rata-rata, $n \geq 1$.
- Contoh: $x = (5, 9, 4, 8)$ memberi $\bar{x} = \frac{26}{4} = 6{,}5$.

**Fungsi:**

$$
y = f(x)
$$

- $x$ masukan, $y$ keluaran, $f$ aturan yang menghubungkan keduanya.

**Rata-rata kesalahan kuadrat:**

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} \left( \hat{y}_i - y_i \right)^2
$$

- Simbol sudah dijelaskan di bagian 10. Berlaku bila ada $n \geq 1$ pasangan prediksi dan nilai sebenarnya.

## Contoh Soal dan Pembahasan Langkah demi Langkah

### Contoh 1 — Menghitung sigma dari daftar

Diketahui $x = (5, 9, 4, 8)$. Hitung $\sum_{i=1}^{4} x_i$ dan $x_2 + x_4$.

1. Tulis suku-sukunya: $x_1 = 5$, $x_2 = 9$, $x_3 = 4$, $x_4 = 8$.
2. $\sum_{i=1}^{4} x_i = 5 + 9 + 4 + 8 = 26$.
3. $x_2 + x_4 = 9 + 8 = 17$.

### Contoh 2 — Sigma dengan ekspresi

Hitung $\sum_{i=1}^{3} (2i + 1)$.

1. $i = 1$: $2(1) + 1 = 3$.
2. $i = 2$: $2(2) + 1 = 5$.
3. $i = 3$: $2(3) + 1 = 7$.
4. Jumlahkan: $3 + 5 + 7 = 15$.

### Contoh 3 — Rata-rata

Hitung rata-rata $x = (5, 9, 4, 8)$.

1. Jumlah: $\sum x_i = 26$ (dari Contoh 1).
2. Banyak data: $n = 4$.
3. $\bar{x} = \frac{26}{4} = 6{,}5$.

Interpretasi: jika keempat nilai disamaratakan, masing-masing bernilai 6,5.

### Contoh 4 — Menghitung nilai fungsi

Diketahui $f(x) = 3x - 2$. Hitung $f(0)$, $f(4)$, dan $f(-1)$.

1. $f(0) = 3(0) - 2 = -2$.
2. $f(4) = 3(4) - 2 = 12 - 2 = 10$.
3. $f(-1) = 3(-1) - 2 = -3 - 2 = -5$.

### Contoh 5 — Membaca dan menghitung MSE

Nilai sebenarnya $y = (10, 8, 15)$ dan prediksi $\hat{y} = (12, 7, 15)$. Hitung MSE.

1. Banyak data $n = 3$.
2. Kesalahan $\hat{y}_i - y_i$: $12 - 10 = 2$, $7 - 8 = -1$, $15 - 15 = 0$.
3. Kuadratkan: $2^2 = 4$, $(-1)^2 = 1$, $0^2 = 0$.
4. Jumlahkan: $4 + 1 + 0 = 5$.
5. Bagi dengan $n$: $\frac{5}{3} \approx 1{,}667$.

Jawaban: $\text{MSE} = \frac{5}{3} \approx 1{,}67$. Hasil makin kecil berarti prediksi makin dekat dengan kenyataan.

## Contoh Tambahan dengan Tingkat Kesulitan Bertahap

**Mudah.** Bilangan bulat yang memenuhi $2 < x \leq 6$ adalah 3, 4, 5, 6.

**Sedang.** $\sum_{i=1}^{4} i = 1 + 2 + 3 + 4 = 10$. Jalan pintas: pasangkan ujung-ujungnya, $(1 + 4) + (2 + 3) = 5 + 5 = 10$. Untuk $\sum_{i=1}^{n} i$ rumusnya $\frac{n(n+1)}{2}$; dengan $n = 4$ memberi $\frac{4 \times 5}{2} = 10$.

**Sedang.** Fungsi $g(x) = x^2 + 1$: $g(3) = 10$, $g(-3) = 10$, $g(0) = 1$.

**Menantang.** Apa beda $\sum_{i=1}^{3} x_i^2$ dan $\left(\sum_{i=1}^{3} x_i\right)^2$ untuk $x = (1, 2, 3)$? Yang pertama: $1 + 4 + 9 = 14$. Yang kedua: $(1 + 2 + 3)^2 = 6^2 = 36$. Letak kuadrat (di dalam atau di luar sigma) mengubah hasil.

## Kesalahan Umum dan Cara Menghindarinya

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Membaca $f(x)$ sebagai $f \times x$ | kurung menandai masukan fungsi | $f(x)$ adalah nilai fungsi di $x$ |
| $x_i$ dianggap $x \times i$ | subskrip adalah nomor urut | $x_i$ adalah anggota ke-$i$ |
| $\sum_{i=1}^{n} x_i^2 = \left(\sum x_i\right)^2$ | kuadrat di dalam dan di luar sigma berbeda | $14 \neq 36$ pada contoh di atas |
| Lupa memasukkan batas atas, mis. berhenti di $i = n - 1$ | satu suku hilang | $i$ berjalan dari batas bawah sampai batas atas, keduanya ikut |
| Mengira `x[1]` adalah suku pertama di Python | indeks Python mulai dari 0 | `x[0]` adalah yang pertama |
| $a + b / c = (a + b) / c$ | pembagian dikerjakan lebih dulu | $a + (b / c)$; tulis kurung bila perlu |
| $\sqrt{9 + 16} = \sqrt{9} + 16$ | cakupan akar terlewat | $\sqrt{25} = 5$ |
| Mengacaukan $\Sigma$ (penjumlahan) dengan $\sigma$ (simpangan baku) | huruf besar dan kecil berbeda arti | perhatikan konteks |
| $1 < x \leq 5$ termasuk 1 | $<$ tidak mencakup batas | 1 tidak termasuk, 5 termasuk |

## Hubungan dengan Materi Berikutnya

- **Level 1** mendalami fungsi, grafik, domain, dan fungsi eksponensial.
- **Level 2** memakai subskrip dua dimensi untuk matriks dan sigma untuk perkalian matriks serta turunan.
- **Level 3** memakai sigma, rata-rata, dan simbol $\mu$, $\sigma$ untuk statistika dan peluang.
- **Level 4 dan 5** memakai rumus seperti MSE dalam pelatihan model.

## Hubungan dengan Machine Learning

- **Sigma dan rata-rata** muncul pada hampir semua ukuran kesalahan dan pada penjumlahan berbobot.
- **Indeks** menandai data ke-$i$ dan fitur ke-$j$ pada dataset.
- **Fungsi** adalah bahasa dasar model: model adalah fungsi dari masukan menjadi prediksi.
- **MSE** adalah ukuran kesalahan umum untuk regresi; kamu sudah sanggup membacanya sekarang.

### Python: sigma, rata-rata, indeks, fungsi

Daftar bilangan disimpan dalam `list`. Fungsi `sum` menjumlahkan semuanya, `len` menghitung banyaknya:

```python
x = [5, 9, 4, 8]
print(sum(x))              # 26
print(len(x))              # 4
print(sum(x) / len(x))     # 6.5, rata-rata
```

Indeks Python mulai dari 0:

```python
print(x[0])    # 5, ini x_1 dalam matematika
print(x[1])    # 9, ini x_2
print(x[3])    # 8, ini x_4
```

Sigma dengan ekspresi, $\sum_{i=1}^{3} (2i + 1)$. Fungsi `range(1, 4)` menghasilkan 1, 2, 3 (berhenti **sebelum** 4):

```python
print(sum(2 * i + 1 for i in range(1, 4)))   # 15
print(sum(i ** 2 for i in range(1, 5)))      # 30
```

Fungsi matematika $f(x) = 3x - 2$ ditulis dengan `def`:

```python
def f(x):
    return 3 * x - 2

print(f(0))    # -2
print(f(4))    # 10
print(f(-1))   # -5
```

Menghitung MSE sesuai langkah di Contoh 5:

```python
y = [10, 8, 15]         # nilai sebenarnya
y_topi = [12, 7, 15]    # prediksi

kuadrat = [(p - t) ** 2 for p, t in zip(y_topi, y)]
print(kuadrat)                       # [4, 1, 0]
print(sum(kuadrat) / len(kuadrat))   # 1.6666666666666667
```

Setiap baris sesuai dengan satu bagian rumus: selisih, kuadrat, sigma (`sum`), dan pembagian oleh $n$ (`len`).

## Latihan Mandiri

1. Benar atau salah: (a) $5 \geq 5$ (b) $-2 > -1$ (c) $3 \neq 3{,}0$ (d) $\sqrt{2} \approx 1{,}41$
2. Tuliskan bilangan bulat $x$ yang memenuhi $2 < x \leq 6$.
3. Untuk $x = (2, 4, 6)$, hitung $\sum_{i=1}^{3} x_i$ dan rata-ratanya.
4. Hitung: (a) $\sum_{i=1}^{5} i$ (b) $\sum_{i=1}^{4} i^2$ (c) $\sum_{i=1}^{3} (2i + 1)$ (d) $\sum_{i=1}^{6} 2$
5. Tulis dengan notasi sigma dan hitung: (a) $1 + 2 + 3 + \cdots + 10$ (b) $2 + 4 + 6 + 8$
6. Untuk $x = (5, 9, 4, 8)$: tentukan $x_2$, $x_4$, $x_1 + x_3$, $\sum x_i$, dan rata-ratanya.
7. Pada `x = [5, 9, 4, 8]` di Python, bagaimana mengambil $x_2$ (nilai kedua dalam matematika)?
8. Untuk $f(x) = 3x - 2$, hitung $f(0)$, $f(4)$, dan $f(-1)$.
9. Untuk $g(x) = x^2 + 1$, hitung $g(3)$, $g(-3)$, dan $g(0)$. Apakah $g(3)$ berarti $g \times 3$?
10. Nilai sebenarnya $y = (3, 5, 2)$ dan prediksi $\hat{y} = (2, 5, 4)$. Hitung MSE.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) **Benar** ($\geq$ mencakup sama dengan). (b) **Salah**; $-2$ berada di kiri $-1$, jadi $-2 < -1$. (c) **Salah**; 3 dan 3,0 adalah bilangan yang sama, jadi $3 = 3{,}0$. (d) **Benar**; $1{,}41^2 = 1{,}9881$, dekat dengan 2.

??? success "Soal 2"
    $x$ lebih besar dari 2 dan paling besar 6: **3, 4, 5, 6**.

??? success "Soal 3"
    $\sum x_i = 2 + 4 + 6 = 12$. Rata-rata $= \frac{12}{3} = 4$.

??? success "Soal 4"
    (a) $1 + 2 + 3 + 4 + 5 = 15$.
    (b) $1 + 4 + 9 + 16 = 30$.
    (c) $3 + 5 + 7 = 15$.
    (d) Konstanta 2 dijumlahkan 6 kali: $6 \times 2 = 12$.

??? success "Soal 5"
    (a) $\sum_{i=1}^{10} i = 55$. Cek: pasangan $(1 + 10), (2 + 9), \ldots$ ada 5 pasangan masing-masing 11, jadi $5 \times 11 = 55$.
    (b) $\sum_{i=1}^{4} 2i = 2 + 4 + 6 + 8 = 20$.

??? success "Soal 6"
    $x_2 = 9$; $x_4 = 8$; $x_1 + x_3 = 5 + 4 = 9$; $\sum x_i = 26$; rata-rata $= \frac{26}{4} = 6{,}5$.

??? success "Soal 7"
    Indeks Python mulai dari 0, jadi $x_2$ adalah `x[1]`, yang bernilai 9.

??? success "Soal 8"
    $f(0) = -2$; $f(4) = 12 - 2 = 10$; $f(-1) = -3 - 2 = -5$.

??? success "Soal 9"
    $g(3) = 9 + 1 = 10$; $g(-3) = 9 + 1 = 10$; $g(0) = 0 + 1 = 1$. $g(3)$ **bukan** $g \times 3$; kurung menandai masukan fungsi.

??? success "Soal 10"
    Kesalahan $\hat{y}_i - y_i$: $2 - 3 = -1$; $5 - 5 = 0$; $4 - 2 = 2$. Kuadrat: $1$, $0$, $4$. Jumlah $= 5$. MSE $= \frac{5}{3} \approx 1{,}67$.

## Ringkasan Materi

- Perkalian boleh ditulis $\times$, $\cdot$, atau dirapatkan; garis pecahan berfungsi sebagai kurung tak terlihat.
- $<$, $>$, $\leq$, $\geq$ membandingkan; $=$, $\neq$, $\approx$ menyatakan kesamaan, ketidaksamaan, dan pendekatan.
- $\sum_{i=1}^{n} x_i$ berarti menjumlahkan $x_1$ sampai $x_n$; $i$ adalah indeks yang berjalan dari batas bawah ke batas atas.
- Rata-rata $\bar{x} = \frac{1}{n}\sum x_i$.
- Subskrip $x_i$ menunjuk anggota ke-$i$. Matematika biasanya mulai dari 1, Python mulai dari 0.
- $f(x)$ adalah nilai fungsi $f$ pada masukan $x$, bukan perkalian.
- Baca rumus bertahap: jenis, simbol, dalam ke luar, kata-kata, lalu coba angka kecil.

## Checklist Penguasaan

- [ ] Membaca simbol $<$, $>$, $\leq$, $\geq$, $\neq$, $\approx$ dengan benar.
- [ ] Menjelaskan fungsi garis pecahan sebagai tanda kurung.
- [ ] Menghitung sigma dari daftar kecil dan dari ekspresi dalam $i$.
- [ ] Menghitung rata-rata dengan notasi sigma.
- [ ] Menjelaskan arti $x_i$ dan $x_{ij}$.
- [ ] Menjelaskan beda penomoran indeks di matematika dan Python.
- [ ] Menghitung nilai $f(x)$ untuk beberapa masukan.
- [ ] Membaca rumus MSE dan menghitungnya untuk data kecil.
