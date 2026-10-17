# Part 3 — Matriks

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- mengenali jenis-jenis matriks: persegi, nol, diagonal, identitas, simetris, dan segitiga;
- menjumlah, mengurang, dan mengalikan matriks dengan skalar;
- menentukan apakah dua matriks dapat dikalikan, ukuran hasilnya, dan menghitung hasil kalinya;
- menjelaskan mengapa perkalian matriks tidak komutatif;
- menghitung transpose matriks;
- menghitung determinan dan invers matriks $2 \times 2$, serta menjelaskan kapan invers tidak ada;
- menghubungkan perkalian matriks dengan prediksi model untuk banyak data sekaligus.

## Prasyarat

[Part 1](01-pengantar-aljabar-linear.md) (ukuran matriks dan elemen $a_{ij}$) dan [Part 2](02-vektor.md) (dot product). Bagian ini juga memakai bentuk matriks dari [Level 1 — Part 6](../level-1-aljabar-dasar-dan-fungsi/06-sistem-persamaan-linear.md) dan bilangan $D = a_1 b_2 - a_2 b_1$ di sana, yang sebenarnya adalah determinan.

## Mengapa Materi Ini Penting?

Matriks adalah cara menyimpan **seluruh dataset** dan **seluruh parameter** sebuah lapisan model. Perkalian matriks menghitung prediksi untuk ribuan data dalam satu operasi. Setiap lapisan jaringan saraf melakukan perkalian matriks. Determinan dan invers menjawab pertanyaan: apakah sebuah sistem persamaan punya jawaban tunggal, dan bagaimana mencarinya.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Jenis-jenis Matriks

| Jenis | Ciri | Contoh |
|---|---|---|
| Persegi | baris = kolom | $2 \times 2$, $3 \times 3$ |
| Matriks baris / kolom | satu baris / satu kolom | vektor baris / vektor kolom |
| Matriks nol | semua elemen $0$ | $O$ |
| Diagonal | elemen di luar diagonal utama semuanya $0$ | $\begin{bmatrix} 2 & 0 \\ 0 & 5 \end{bmatrix}$ |
| Identitas ($I$) | diagonal bernilai $1$, sisanya $0$ | $\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$ |
| Simetris | $a_{ij} = a_{ji}$ | $\begin{bmatrix} 1 & 2 \\ 2 & 3 \end{bmatrix}$ |
| Segitiga atas | semua elemen di bawah diagonal bernilai $0$ | $\begin{bmatrix} 1 & 2 \\ 0 & 4 \end{bmatrix}$ |

**Diagonal utama** adalah elemen $a_{11}, a_{22}, \ldots$ (dari kiri atas ke kanan bawah). **Matriks identitas** $I$ berperan seperti angka 1 pada perkalian: hasil kalinya dengan matriks lain tidak mengubah matriks itu ($AI = IA = A$), seperti $a \times 1 = a$ di Level 0.

### 2. Penjumlahan, Pengurangan, dan Perkalian dengan Skalar

Seperti pada vektor, penjumlahan dan pengurangan dilakukan **elemen demi elemen**, dan hanya untuk matriks **berukuran sama**:

$$
\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}
+
\begin{bmatrix} 6 & 5 & 4 \\ 3 & 2 & 1 \end{bmatrix}
=
\begin{bmatrix} 7 & 7 & 7 \\ 7 & 7 & 7 \end{bmatrix}
$$

Perkalian dengan skalar $c$ mengalikan **setiap elemen** dengan $c$:

$$
3 \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}
=
\begin{bmatrix} 3 & 6 \\ 9 & 12 \end{bmatrix}
$$

Penjumlahan matriks bersifat komutatif ($A + B = B + A$) dan asosiatif, sama seperti bilangan. Matriks $2 \times 3$ tidak dapat dijumlahkan dengan matriks $3 \times 2$: ukurannya berbeda, jadi elemennya tidak berpasangan.

### 3. Perkalian Matriks

Perkalian matriks **bukan** perkalian elemen demi elemen. Ia dibangun dari **dot product** (Part 2): setiap elemen hasil adalah dot product **sebuah baris** dari matriks pertama dengan **sebuah kolom** dari matriks kedua.

**Syarat:** banyak **kolom** matriks pertama harus sama dengan banyak **baris** matriks kedua. Jika $A$ berukuran $m \times n$ dan $B$ berukuran $n \times p$, maka $AB$ **terdefinisi** dan berukuran $m \times p$:

$$
(m \times \underline{n})(\underline{n} \times p) = m \times p
$$

Dua angka di tengah harus sama, dan keduanya "hilang" pada ukuran hasil.

**Rumus elemen:**

$$
c_{ij} = \sum_{k=1}^{n} a_{ik}\, b_{kj}
$$

Artinya: elemen hasil di baris $i$ dan kolom $j$ adalah dot product baris ke-$i$ dari $A$ dengan kolom ke-$j$ dari $B$.

**Contoh $2 \times 2$.** $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ dan $B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$.

- $c_{11}$: baris 1 $(1, 2)$ dot kolom 1 $(5, 7)$ $= 1 \cdot 5 + 2 \cdot 7 = 19$.
- $c_{12}$: baris 1 $(1, 2)$ dot kolom 2 $(6, 8)$ $= 6 + 16 = 22$.
- $c_{21}$: baris 2 $(3, 4)$ dot kolom 1 $(5, 7)$ $= 15 + 28 = 43$.
- $c_{22}$: baris 2 $(3, 4)$ dot kolom 2 $(6, 8)$ $= 18 + 32 = 50$.

$$
AB = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}
$$

**Contoh $2 \times 3$ kali $3 \times 2$.** Kolom pertama (3) sama dengan baris kedua (3), jadi terdefinisi, dan hasilnya $2 \times 2$:

$$
\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}
\begin{bmatrix} 7 & 8 \\ 9 & 10 \\ 11 & 12 \end{bmatrix}
=
\begin{bmatrix} 58 & 64 \\ 139 & 154 \end{bmatrix}
$$

Misalnya $c_{11} = 1 \cdot 7 + 2 \cdot 9 + 3 \cdot 11 = 7 + 18 + 33 = 58$.

**Matriks kali vektor.** Vektor kolom adalah matriks $n \times 1$, sehingga $A\mathbf{x}$ terdefinisi bila $A$ punya $n$ kolom, dan hasilnya vektor kolom:

$$
\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}
\begin{bmatrix} 5 \\ 6 \end{bmatrix}
=
\begin{bmatrix} 1 \cdot 5 + 2 \cdot 6 \\ 3 \cdot 5 + 4 \cdot 6 \end{bmatrix}
=
\begin{bmatrix} 17 \\ 39 \end{bmatrix}
$$

**Tidak komutatif.** Umumnya $AB \neq BA$. Dengan $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ dan $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$:

$$
AB = \begin{bmatrix} 2 & 1 \\ 4 & 3 \end{bmatrix}, \qquad
BA = \begin{bmatrix} 3 & 4 \\ 1 & 2 \end{bmatrix}
$$

Kadang $BA$ bahkan tidak terdefinisi walaupun $AB$ terdefinisi: jika $A$ berukuran $2 \times 3$ dan $B$ berukuran $3 \times 4$, maka $AB$ berukuran $2 \times 4$, tetapi $BA$ membutuhkan kolom $B$ (4) sama dengan baris $A$ (2), dan itu tidak terpenuhi. Ini berbeda dengan perkalian bilangan (Level 0, Part 2), dan perbedaan itu penting. **Sifat yang tetap berlaku:** asosiatif $(AB)C = A(BC)$, distributif $A(B + C) = AB + AC$, dan $AI = IA = A$.

### 4. Transpose

**Transpose** dari matriks $A$, ditulis $A^{\top}$ (dibaca "A transpose"), diperoleh dengan **menukar baris dan kolom**: baris pertama $A$ menjadi kolom pertama $A^{\top}$, dan seterusnya.

$$
A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}
\quad\Rightarrow\quad
A^{\top} = \begin{bmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{bmatrix}
$$

Jika $A$ berukuran $m \times n$, maka $A^{\top}$ berukuran $n \times m$. Elemen: $(A^{\top})_{ij} = a_{ji}$.

Sifat:

- $(A^{\top})^{\top} = A$.
- $(A + B)^{\top} = A^{\top} + B^{\top}$.
- $(AB)^{\top} = B^{\top} A^{\top}$: **urutannya terbalik**.

Cek sifat terakhir dengan $A$ dan $B$ di atas ($AB = \begin{bmatrix} 2 & 1 \\ 4 & 3 \end{bmatrix}$): $(AB)^{\top} = \begin{bmatrix} 2 & 4 \\ 1 & 3 \end{bmatrix}$. Hitung $B^{\top} A^{\top}$ dengan $B^{\top} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$ dan $A^{\top} = \begin{bmatrix} 1 & 3 \\ 2 & 4 \end{bmatrix}$: hasilnya $\begin{bmatrix} 2 & 4 \\ 1 & 3 \end{bmatrix}$. Sama. Sedangkan $A^{\top} B^{\top} = \begin{bmatrix} 3 & 1 \\ 4 & 2 \end{bmatrix}$ berbeda.

**Transpose vektor.** Transpose vektor kolom adalah vektor baris. Dot product dapat ditulis $\mathbf{a} \cdot \mathbf{b} = \mathbf{a}^{\top}\mathbf{b}$ (matriks $1 \times n$ kali $n \times 1$ menghasilkan $1 \times 1$, yaitu skalar). Matriks **simetris** memenuhi $A = A^{\top}$.

### 5. Determinan

**Determinan** adalah satu bilangan yang dihitung dari matriks persegi dan menyimpan informasi penting tentangnya. Untuk matriks $2 \times 2$:

$$
\det\begin{bmatrix} a & b \\ c & d \end{bmatrix} = ad - bc
$$

Yaitu **hasil kali diagonal utama dikurangi hasil kali diagonal lainnya**. Perhatikan kemiripannya dengan $D = a_1 b_2 - a_2 b_1$ pada sistem persamaan di Level 1: itu determinan dari matriks koefisien.

Contoh:

- $\det\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} = 1 \cdot 4 - 2 \cdot 3 = -2$.
- $\det\begin{bmatrix} 3 & 0 \\ 0 & 2 \end{bmatrix} = 6 - 0 = 6$.
- $\det\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix} = 4 - 4 = 0$.

**Arti determinan:**

1. **Bisa dibalik atau tidak.** Jika $\det A \neq 0$, matriks punya invers (bagian berikut). Jika $\det A = 0$, matriks disebut **singular** dan tidak punya invers. Pada contoh terakhir, baris kedua adalah separuh baris pertama, yaitu bilangan yang "berulang" sehingga informasinya hilang.
2. **Faktor perubahan luas** (dibahas di Part 5): determinan menyatakan seberapa besar sebuah transformasi memperbesar atau memperkecil luas. Nilai nol berarti luas runtuh menjadi nol (bidang tertekan menjadi garis).

Sifat: $\det(AB) = \det(A)\det(B)$. Cek dengan $A$ dan $B$ di atas: $\det A = -2$, $\det B = 0 \cdot 0 - 1 \cdot 1 = -1$, dan $\det(AB) = 2 \cdot 3 - 1 \cdot 4 = 2 = (-2)(-1)$.

Matriks yang lebih besar ($3 \times 3$ dan seterusnya) juga punya determinan, tetapi cara menghitungnya panjang dan biasanya diserahkan ke komputer. Di buku ini kita hanya menghitung yang $2 \times 2$ secara manual.

### 6. Invers Matriks $2 \times 2$

**Invers** dari matriks persegi $A$, ditulis $A^{-1}$, adalah matriks yang jika dikalikan dengan $A$ menghasilkan matriks identitas:

$$
A A^{-1} = A^{-1} A = I
$$

Ini sepadan dengan kebalikan pada bilangan: $5 \times \tfrac{1}{5} = 1$. Perhatikan bahwa $A^{-1}$ **bukan** $\tfrac{1}{A}$ (pembagian matriks tidak didefinisikan).

Untuk matriks $2 \times 2$:

$$
A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}
\quad\Rightarrow\quad
A^{-1} = \frac{1}{ad - bc}
\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}
$$

Cara mengingat: **tukar** $a$ dan $d$, **ubah tanda** $b$ dan $c$, lalu **bagi dengan determinan**. Syarat: $ad - bc \neq 0$.

**Contoh.** $A = \begin{bmatrix} 4 & 7 \\ 2 & 6 \end{bmatrix}$.

1. Determinan: $4 \cdot 6 - 7 \cdot 2 = 24 - 14 = 10$ (bukan nol, jadi invers ada).
2. Tukar dan ubah tanda: $\begin{bmatrix} 6 & -7 \\ -2 & 4 \end{bmatrix}$.
3. Bagi 10: $A^{-1} = \begin{bmatrix} 0{,}6 & -0{,}7 \\ -0{,}2 & 0{,}4 \end{bmatrix}$.
4. Periksa $AA^{-1}$: $4(0{,}6) + 7(-0{,}2) = 2{,}4 - 1{,}4 = 1$; $4(-0{,}7) + 7(0{,}4) = -2{,}8 + 2{,}8 = 0$; $2(0{,}6) + 6(-0{,}2) = 1{,}2 - 1{,}2 = 0$; $2(-0{,}7) + 6(0{,}4) = -1{,}4 + 2{,}4 = 1$. Hasilnya $I$.

**Memakai invers untuk menyelesaikan sistem.** Sistem $A\mathbf{x} = \mathbf{b}$ dapat diselesaikan dengan mengalikan kedua sisi dengan $A^{-1}$ dari kiri: $A^{-1}A\mathbf{x} = A^{-1}\mathbf{b}$, sehingga $\mathbf{x} = A^{-1}\mathbf{b}$.

Contoh dari Level 1: $x + y = 5$ dan $x - y = 1$, yaitu $A = \begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$ dan $\mathbf{b} = (5, 1)$.

1. Determinan: $1(-1) - 1(1) = -2$.
2. Invers: $\dfrac{1}{-2}\begin{bmatrix} -1 & -1 \\ -1 & 1 \end{bmatrix} = \begin{bmatrix} 0{,}5 & 0{,}5 \\ 0{,}5 & -0{,}5 \end{bmatrix}$.
3. $\mathbf{x} = A^{-1}\mathbf{b} = (0{,}5 \cdot 5 + 0{,}5 \cdot 1,\ 0{,}5 \cdot 5 - 0{,}5 \cdot 1) = (3, 2)$.
4. Sama dengan solusi $(3, 2)$ di Level 1.

**Kapan invers tidak ada?** Jika $\det A = 0$. Pada sistem persamaan, itu berarti kedua garis sejajar atau berimpit (tidak ada solusi tunggal), sesuai Level 1, Part 6. Matriks yang tidak persegi juga tidak punya invers.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Matriks persegi | baris sama dengan kolom | $2 \times 2$ |
| Diagonal utama | elemen $a_{11}, a_{22}, \ldots$ | $1$ dan $4$ pada $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ |
| Matriks identitas $I$ | diagonal 1, sisanya 0 | $AI = A$ |
| Matriks nol | semua elemen 0 | |
| Matriks simetris | $A = A^{\top}$ | |
| Perkalian matriks | baris dikali kolom lewat dot product | $AB$ |
| Transpose $A^{\top}$ | menukar baris dan kolom | $2 \times 3 \to 3 \times 2$ |
| Determinan | bilangan dari matriks persegi; $ad - bc$ untuk $2 \times 2$ | $-2$ |
| Singular | determinan nol, tidak punya invers | $\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix}$ |
| Invers $A^{-1}$ | matriks dengan $AA^{-1} = I$ | |
| Komutatif | urutan boleh ditukar | tidak berlaku pada $AB$ |

## Rumus dan Penjelasan Setiap Simbol

| Operasi | Rumus | Syarat |
|---|---|---|
| Penjumlahan | $(A + B)_{ij} = a_{ij} + b_{ij}$ | ukuran sama |
| Skalar | $(cA)_{ij} = c\,a_{ij}$ | |
| Perkalian | $(AB)_{ij} = \sum_{k=1}^{n} a_{ik} b_{kj}$ | $A$: $m \times n$, $B$: $n \times p$; hasil $m \times p$ |
| Transpose | $(A^{\top})_{ij} = a_{ji}$ | |
| Transpose perkalian | $(AB)^{\top} = B^{\top}A^{\top}$ | $AB$ terdefinisi |
| Determinan $2 \times 2$ | $\det A = ad - bc$ | $A$ persegi $2 \times 2$ |
| Invers $2 \times 2$ | $A^{-1} = \dfrac{1}{ad - bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$ | $ad - bc \neq 0$ |
| Menyelesaikan sistem | $\mathbf{x} = A^{-1}\mathbf{b}$ | $A$ persegi dan $\det A \neq 0$ |

Simbol: $a_{ik}$ adalah elemen baris $i$ kolom $k$ dari $A$; $k$ adalah indeks penjumlahan yang berjalan sepanjang baris $A$ dan kolom $B$; $\det$ adalah determinan; $^{\top}$ adalah transpose; $^{-1}$ adalah invers (bukan pangkat $-1$ biasa).

## Penjelasan Langkah demi Langkah

**Perkalian matriks (cara praktis):**

1. **Periksa ukuran**: kolom $A$ = baris $B$? Tulis ukuran hasil ($m \times p$).
2. Siapkan tabel hasil kosong berukuran $m \times p$.
3. Untuk setiap sel $(i, j)$: ambil baris ke-$i$ dari $A$ dan kolom ke-$j$ dari $B$, kalikan pasangan berurutan, lalu jumlahkan.
4. Periksa: tidak ada sel kosong, dan ukuran sesuai.

## Contoh Soal dan Pembahasan

### Contoh 1 — Menentukan ukuran hasil

Apakah $AB$ terdefinisi, dan berapa ukurannya? (a) $A$: $2 \times 3$, $B$: $3 \times 4$. (b) $A$: $2 \times 3$, $B$: $2 \times 3$. (c) $A$: $1 \times 3$, $B$: $3 \times 1$.

1. (a) Kolom $A$ = 3 = baris $B$. Terdefinisi, hasil $2 \times 4$.
2. (b) Kolom $A$ = 3, baris $B$ = 2. **Tidak terdefinisi.**
3. (c) Kolom $A$ = 3 = baris $B$. Terdefinisi, hasil $1 \times 1$. Ini dot product dua vektor, hasilnya satu bilangan.

### Contoh 2 — Perkalian dan ketidakkomutatifan

Hitung $AB$ dan $BA$ untuk $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ dan $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$.

1. $AB$: $c_{11} = 1 \cdot 0 + 2 \cdot 1 = 2$; $c_{12} = 1 \cdot 1 + 2 \cdot 0 = 1$; $c_{21} = 3 \cdot 0 + 4 \cdot 1 = 4$; $c_{22} = 3 \cdot 1 + 4 \cdot 0 = 3$. Jadi $AB = \begin{bmatrix} 2 & 1 \\ 4 & 3 \end{bmatrix}$.
2. $BA$: $c_{11} = 0 \cdot 1 + 1 \cdot 3 = 3$; $c_{12} = 0 \cdot 2 + 1 \cdot 4 = 4$; $c_{21} = 1 \cdot 1 + 0 \cdot 3 = 1$; $c_{22} = 1 \cdot 2 + 0 \cdot 4 = 2$. Jadi $BA = \begin{bmatrix} 3 & 4 \\ 1 & 2 \end{bmatrix}$.
3. $AB \neq BA$. Menarik: mengalikan dari kanan dengan $B$ menukar **kolom** $A$, sedangkan dari kiri menukar **baris** $A$.

### Contoh 3 — Determinan dan invers

Tentukan apakah $\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix}$ dan $\begin{bmatrix} 3 & 1 \\ 5 & 2 \end{bmatrix}$ punya invers. Jika ya, carilah.

1. Matriks pertama: $\det = 2 \cdot 2 - 4 \cdot 1 = 0$. **Singular**, tidak punya invers.
2. Matriks kedua: $\det = 3 \cdot 2 - 1 \cdot 5 = 6 - 5 = 1$. Punya invers.
3. $A^{-1} = \dfrac{1}{1}\begin{bmatrix} 2 & -1 \\ -5 & 3 \end{bmatrix} = \begin{bmatrix} 2 & -1 \\ -5 & 3 \end{bmatrix}$.
4. Periksa: $3 \cdot 2 + 1 \cdot (-5) = 1$; $3(-1) + 1 \cdot 3 = 0$; $5 \cdot 2 + 2(-5) = 0$; $5(-1) + 2 \cdot 3 = 1$. Hasilnya $I$.

### Contoh 4 — Prediksi banyak data sekaligus

Dari Part 1: matriks fitur $X$ (4 rumah, 3 fitur) dan bobot $\mathbf{w} = (2, 10, -1)$, $b = 20$. Hitung prediksi semua rumah.

$$
X\mathbf{w} =
\begin{bmatrix} 60 & 2 & 5 \\ 80 & 3 & 10 \\ 100 & 4 & 2 \\ 45 & 1 & 20 \end{bmatrix}
\begin{bmatrix} 2 \\ 10 \\ -1 \end{bmatrix}
=
\begin{bmatrix} 135 \\ 180 \\ 238 \\ 80 \end{bmatrix}
$$

1. Ukuran: $(4 \times 3)(3 \times 1) = 4 \times 1$, satu prediksi per rumah.
2. Baris pertama: $60 \cdot 2 + 2 \cdot 10 + 5 \cdot (-1) = 120 + 20 - 5 = 135$, sama dengan perhitungan di Part 2.
3. Tambahkan $b = 20$ ke setiap elemen: $\hat{\mathbf{y}} = (155, 200, 258, 100)$.

Satu perkalian matriks menggantikan empat dot product terpisah. Untuk 1.000.000 rumah, caranya sama.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** $2\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} - \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} = \begin{bmatrix} 2 & 3 \\ 5 & 8 \end{bmatrix}$.

**Sedang.** Matriks $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ dikali vektor $(5, 6)$ memberi $(17, 39)$ (lihat bagian 3).

**Sedang.** $\det\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} = -2$ dan $\det B = -1$. Maka $\det(AB) = 2$, dan memang $2 \cdot 3 - 1 \cdot 4 = 2$.

**Menantang.** Dengan $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$, $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$, dan $C = \begin{bmatrix} 2 & 0 \\ 1 & 3 \end{bmatrix}$, periksa bahwa $(AB)C = A(BC)$: kedua cara menghasilkan matriks yang sama (dapat diverifikasi dengan kode di bawah).

## Visualisasi atau Interpretasi Grafik

Matriks sendiri adalah tabel, tetapi ia menjadi mudah dipahami ketika dibayangkan sebagai **mesin yang mengubah vektor**: $A\mathbf{x}$ mengubah vektor $\mathbf{x}$ menjadi vektor lain. Pandangan ini, termasuk gambar bagaimana matriks memutar, meregangkan, dan memiringkan bidang, dibahas di Part 5. Determinan memberi tahu seberapa besar luas berubah dalam perubahan itu.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Mengalikan matriks elemen demi elemen | itu bukan perkalian matriks | baris dikali kolom lewat dot product |
| Mengalikan matriks yang kolom-barisnya tidak cocok | tidak terdefinisi | periksa $(m \times n)(n \times p)$ |
| Menganggap $AB = BA$ | perkalian matriks tidak komutatif | hitung keduanya |
| $(AB)^{\top} = A^{\top}B^{\top}$ | urutan harus terbalik | $B^{\top}A^{\top}$ |
| $A^{-1} = \dfrac{1}{A}$ atau $A^{-1}$ berisi $\tfrac{1}{a_{ij}}$ | invers bukan kebalikan tiap elemen | pakai rumus tukar, ubah tanda, bagi determinan |
| Mencari invers saat $\det = 0$ | tidak ada invers | periksa determinan dulu |
| Menukar tanda yang salah: $\begin{bmatrix} -d & b \\ c & -a \end{bmatrix}$ | yang ditukar $a$ dan $d$; yang diubah tanda $b$ dan $c$ | $\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$ |
| $\det\begin{bmatrix} a & b \\ c & d \end{bmatrix} = ac - bd$ | salah pasangan | $ad - bc$ |
| Menjumlahkan matriks berukuran beda | elemen tidak berpasangan | ukuran harus sama |
| Mengira semua matriks persegi punya invers | determinan nol tidak punya | singular |

## Hubungan dengan Machine Learning

- **Lapisan model.** Prediksi untuk $n$ data sekaligus adalah $\hat{\mathbf{y}} = X\mathbf{w} + b$ (matriks fitur kali vektor bobot). Lapisan jaringan saraf melakukan hal serupa dengan banyak bobot sekaligus: $X W + \mathbf{b}$.
- **Efisiensi.** Komputer dan kartu grafis dirancang untuk perkalian matriks yang sangat cepat.
- **Transpose.** Muncul di banyak rumus, misalnya $X^{\top}X$ pada regresi linear (Level 4), yang selalu persegi dan simetris.
- **Invers.** Rumus regresi linear tertutup memakai invers, tetapi dalam praktik invers jarang dihitung langsung karena rawan galat numerik; komputer lebih sering menyelesaikan sistem langsung (Part 4).
- **Singular dan fitur berulang.** Jika dua fitur identik atau satu fitur adalah kelipatan fitur lain, matriks yang terbentuk bisa singular. Itu masalah nyata dalam pemodelan.

### Python: operasi matriks dengan `list`

```python
from fractions import Fraction

def transpose(A):
    return [list(baris) for baris in zip(*A)]

def matmul(A, B):
    if len(A[0]) != len(B):
        raise ValueError("ukuran tidak cocok")
    return [[sum(a * b for a, b in zip(baris, kolom)) for kolom in zip(*B)]
            for baris in A]

def det2(A):
    return A[0][0] * A[1][1] - A[0][1] * A[1][0]

def inv2(A):
    d = det2(A)
    if d == 0:
        raise ValueError("determinan nol, tidak punya invers")
    return [[Fraction(A[1][1], d), Fraction(-A[0][1], d)],
            [Fraction(-A[1][0], d), Fraction(A[0][0], d)]]
```

`zip(*B)` menghasilkan kolom-kolom `B`, dan `zip(baris, kolom)` memasangkan elemen untuk dot product. Fungsi `matmul` adalah rumus $c_{ij} = \sum_k a_{ik} b_{kj}$.

```python
A = [[1, 2], [3, 4]]
B = [[0, 1], [1, 0]]
print(matmul(A, B))                    # [[2, 1], [4, 3]]
print(matmul(B, A))                    # [[3, 4], [1, 2]], berbeda dari AB
print(matmul([[1, 2, 3], [4, 5, 6]], [[7, 8], [9, 10], [11, 12]]))   # [[58, 64], [139, 154]]
print(matmul(A, [[5], [6]]))           # [[17], [39]]
```

Transpose, determinan, dan sifat $(AB)^{\top} = B^{\top}A^{\top}$:

```python
print(transpose([[1, 2, 3], [4, 5, 6]]))     # [[1, 4], [2, 5], [3, 6]]
print(det2(A), det2(B), det2(matmul(A, B)))  # -2 -1 2
print(transpose(matmul(A, B)))                           # [[2, 4], [1, 3]]
print(matmul(transpose(B), transpose(A)))                # [[2, 4], [1, 3]], sama
print(matmul(transpose(A), transpose(B)))                # [[3, 1], [4, 2]], berbeda
```

Invers dan pemakaiannya menyelesaikan sistem. `Fraction` menjaga hasil tetap tepat:

```python
C = [[4, 7], [2, 6]]
inv = inv2(C)
print([[str(x) for x in baris] for baris in inv])   # [['3/5', '-7/10'], ['-1/5', '2/5']]
print(matmul(C, inv))   # [[Fraction(1, 1), Fraction(0, 1)], [Fraction(0, 1), Fraction(1, 1)]]

G = [[1, 1], [1, -1]]
x = matmul(inv2(G), [[5], [1]])
print([str(baris[0]) for baris in x])   # ['3', '2']
```

Prediksi untuk semua rumah (Contoh 4):

```python
X = [[60, 2, 5], [80, 3, 10], [100, 4, 2], [45, 1, 20]]
w = [[2], [10], [-1]]
b = 20
y_topi = [baris[0] + b for baris in matmul(X, w)]
print(y_topi)   # [155, 200, 258, 100]
```

Memeriksa sifat asosiatif dan matriks singular:

```python
C2 = [[2, 0], [1, 3]]
print(matmul(matmul(A, B), C2) == matmul(A, matmul(B, C2)))   # True
print(det2([[2, 4], [1, 2]]))     # 0, singular; inv2 akan menolaknya
```

## Latihan Mandiri

1. Sebutkan jenis tiap matriks: (a) $\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$ (b) $\begin{bmatrix} 2 & 0 \\ 0 & 5 \end{bmatrix}$ (c) $\begin{bmatrix} 1 & 2 \\ 2 & 3 \end{bmatrix}$ (d) $\begin{bmatrix} 1 & 2 & 3 \\ 0 & 4 & 5 \\ 0 & 0 & 6 \end{bmatrix}$
2. Dengan $A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$ dan $B = \begin{bmatrix} 6 & 5 & 4 \\ 3 & 2 & 1 \end{bmatrix}$, hitung $A + B$, $A - B$, dan $2A$.
3. Tentukan apakah $AB$ terdefinisi dan ukurannya: (a) $2 \times 3$ dan $3 \times 2$ (b) $3 \times 2$ dan $2 \times 4$ (c) $2 \times 3$ dan $2 \times 3$ (d) $1 \times 3$ dan $3 \times 1$
4. Hitung $AB$ dan $BA$ untuk $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ dan $B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$. Apakah sama?
5. Hitung $\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}\begin{bmatrix} 7 & 8 \\ 9 & 10 \\ 11 & 12 \end{bmatrix}$.
6. Hitung $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}\begin{bmatrix} 5 \\ 6 \end{bmatrix}$.
7. Tentukan transpose $\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$ dan ukurannya. Periksa $(AB)^{\top} = B^{\top}A^{\top}$ untuk $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ dan $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$.
8. Hitung determinan: (a) $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ (b) $\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix}$ (c) $\begin{bmatrix} 3 & 0 \\ 0 & 2 \end{bmatrix}$. Mana yang singular?
9. Cari invers $\begin{bmatrix} 4 & 7 \\ 2 & 6 \end{bmatrix}$ dan periksa. Lalu selesaikan $x + y = 5$ dan $x - y = 1$ dengan invers.
10. Matriks fitur $X = \begin{bmatrix} 60 & 2 & 5 \\ 80 & 3 & 10 \\ 100 & 4 & 2 \\ 45 & 1 & 20 \end{bmatrix}$, $\mathbf{w} = (2, 10, -1)$, $b = 20$. Hitung $\hat{\mathbf{y}} = X\mathbf{w} + b$ dan sebutkan ukuran setiap bagian.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) matriks identitas. (b) matriks diagonal. (c) matriks simetris ($a_{12} = a_{21} = 2$). (d) matriks segitiga atas (semua elemen di bawah diagonal nol).

??? success "Soal 2"
    $A + B = \begin{bmatrix} 7 & 7 & 7 \\ 7 & 7 & 7 \end{bmatrix}$. $A - B = \begin{bmatrix} -5 & -3 & -1 \\ 1 & 3 & 5 \end{bmatrix}$. $2A = \begin{bmatrix} 2 & 4 & 6 \\ 8 & 10 & 12 \end{bmatrix}$.

??? success "Soal 3"
    (a) kolom 3 = baris 3: terdefinisi, hasil $2 \times 2$. (b) kolom 2 = baris 2: terdefinisi, hasil $3 \times 4$. (c) kolom 3 $\neq$ baris 2: tidak terdefinisi. (d) kolom 3 = baris 3: terdefinisi, hasil $1 \times 1$.

??? success "Soal 4"
    $AB = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}$ (misalnya $1 \cdot 5 + 2 \cdot 7 = 19$). $BA = \begin{bmatrix} 23 & 34 \\ 31 & 46 \end{bmatrix}$ (misalnya $5 \cdot 1 + 6 \cdot 3 = 23$). **Tidak sama.**

??? success "Soal 5"
    $\begin{bmatrix} 58 & 64 \\ 139 & 154 \end{bmatrix}$. Contoh: $4 \cdot 7 + 5 \cdot 9 + 6 \cdot 11 = 28 + 45 + 66 = 139$.

??? success "Soal 6"
    $\begin{bmatrix} 1 \cdot 5 + 2 \cdot 6 \\ 3 \cdot 5 + 4 \cdot 6 \end{bmatrix} = \begin{bmatrix} 17 \\ 39 \end{bmatrix}$.

??? success "Soal 7"
    Transpose: $\begin{bmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{bmatrix}$, ukuran $3 \times 2$. $AB = \begin{bmatrix} 2 & 1 \\ 4 & 3 \end{bmatrix}$, jadi $(AB)^{\top} = \begin{bmatrix} 2 & 4 \\ 1 & 3 \end{bmatrix}$. $B^{\top}A^{\top} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}\begin{bmatrix} 1 & 3 \\ 2 & 4 \end{bmatrix} = \begin{bmatrix} 2 & 4 \\ 1 & 3 \end{bmatrix}$. Sama.

??? success "Soal 8"
    (a) $4 - 6 = -2$. (b) $4 - 4 = 0$ (singular). (c) $6 - 0 = 6$. Hanya (b) yang singular.

??? success "Soal 9"
    $\det = 24 - 14 = 10$, $A^{-1} = \dfrac{1}{10}\begin{bmatrix} 6 & -7 \\ -2 & 4 \end{bmatrix} = \begin{bmatrix} 0{,}6 & -0{,}7 \\ -0{,}2 & 0{,}4 \end{bmatrix}$. Periksa: $AA^{-1} = I$ (lihat bagian 6). Untuk sistem: $A = \begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$, $\det = -2$, $A^{-1} = \begin{bmatrix} 0{,}5 & 0{,}5 \\ 0{,}5 & -0{,}5 \end{bmatrix}$, $\mathbf{x} = A^{-1}(5, 1) = (3, 2)$.

??? success "Soal 10"
    $X\mathbf{w} = (135, 180, 238, 80)$. Tambahkan $b = 20$: $\hat{\mathbf{y}} = (155, 200, 258, 100)$. Ukuran: $X$ adalah $4 \times 3$, $\mathbf{w}$ adalah $3 \times 1$, hasil $X\mathbf{w}$ adalah $4 \times 1$, jadi $\hat{\mathbf{y}}$ adalah vektor berdimensi 4.

## Ringkasan Materi

- Jenis matriks: persegi, nol, diagonal, identitas ($AI = A$), simetris ($A = A^{\top}$), segitiga.
- Penjumlahan dan skalar dilakukan elemen demi elemen (ukuran harus sama).
- Perkalian matriks: $(m \times n)(n \times p) = m \times p$; $c_{ij}$ adalah dot product baris $i$ dengan kolom $j$. Tidak komutatif, tetapi asosiatif dan distributif.
- Transpose menukar baris dan kolom; $(AB)^{\top} = B^{\top}A^{\top}$.
- Determinan $2 \times 2$: $ad - bc$. Nol berarti singular (tanpa invers). $\det(AB) = \det A \det B$.
- Invers $2 \times 2$: tukar $a$ dan $d$, ubah tanda $b$ dan $c$, bagi determinan. $\mathbf{x} = A^{-1}\mathbf{b}$ menyelesaikan $A\mathbf{x} = \mathbf{b}$.
- ML: $X\mathbf{w} + b$ menghitung prediksi untuk semua data sekaligus.

## Checklist Penguasaan

- [ ] Menyebut jenis matriks dan peran matriks identitas.
- [ ] Menentukan apakah $AB$ terdefinisi dan ukuran hasilnya.
- [ ] Menghitung perkalian matriks dua kali dua dan matriks kali vektor.
- [ ] Menjelaskan dengan contoh bahwa $AB \neq BA$.
- [ ] Menghitung transpose dan memakai aturan $(AB)^{\top} = B^{\top}A^{\top}$.
- [ ] Menghitung determinan dan menyatakan arti nolnya.
- [ ] Menghitung invers $2 \times 2$ dan memeriksanya.
- [ ] Menjelaskan bagaimana $X\mathbf{w} + b$ menghitung prediksi banyak data.

## Prasyarat untuk Materi Berikutnya

[Part 4 — Sistem Persamaan Linear (Bentuk Matriks)](04-sistem-persamaan-linear-matriks.md) membutuhkan: perkalian matriks dengan vektor, matriks identitas, serta determinan dan invers $2 \times 2$. [Part 5 — Transformasi Linear](05-transformasi-linear.md) membutuhkan: perkalian matriks dengan vektor dan determinan.
