# Part 2 — Tensor dan Operasi Multidimensi

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- membedakan skalar, vektor, matriks, dan tensor, serta **rank** (jumlah sumbu) dan **ukuran** tiap dimensi;
- membaca dan menulis **shape** tensor serta menghitung jumlah elemennya;
- mengambil elemen dan irisan tensor lewat **indexing**;
- melakukan *reshape* dan membedakannya dari transpose;
- menerapkan aturan **broadcasting** dan memperkirakan shape hasil;
- menjumlahkan atau merata-ratakan tensor sepanjang satu sumbu;
- menghitung perkalian matriks dan perkalian *batch* beserta shape hasilnya;
- memakai pemeriksaan shape untuk menemukan kesalahan sebelum menghitung.

## Prasyarat

[Level 2 Part 2 — Vektor](../level-2-aljabar-linear-dan-kalkulus/02-vektor.md) (dot product) dan [Level 2 Part 3 — Matriks](../level-2-aljabar-linear-dan-kalkulus/03-matriks.md) (ukuran matriks dan perkaliannya). Part ini memperluas keduanya ke lebih dari dua sumbu.

## Masalah yang Ingin Diselesaikan

Satu pesan teks terdiri dari beberapa token, tiap token diwakili sebuah vektor. Satu *batch* terdiri dari beberapa pesan. Jadi data yang masuk ke model bukan lagi sekadar vektor atau matriks, tetapi **kotak angka dengan tiga sumbu atau lebih**: (pesan, token, nilai vektor). Di sisi lain, parameter model juga berupa kotak angka. Kita membutuhkan bahasa yang rapi untuk menyatakan:

- bentuk setiap kotak angka (shape),
- cara mengambil bagian tertentu darinya,
- cara menggabungkan dua kotak yang bentuknya berbeda (broadcasting),
- cara mengalikannya (perkalian matriks dan *batch*).

Hampir semua kesalahan praktis di Deep Learning adalah kesalahan shape, sehingga keterampilan "membaca shape" sama pentingnya dengan menghitung angka.

## Intuisi dan Istilah Penting

### 1. Tensor sebagai kotak angka

**Tensor** (*tensor*) adalah susunan bilangan dalam sejumlah sumbu (*axis*). Di Deep Learning, kata tensor dipakai untuk semua jenis susunan itu, dari satu bilangan sampai susunan berpuluh-puluh sumbu.

| Nama | Jumlah sumbu (**rank**) | Shape | Contoh | Contoh penggunaan |
|---|---|---|---|---|
| Skalar | 0 | $()$ | $7$ | loss, *learning rate* |
| Vektor | 1 | $(3)$ | $[1,\,2,\,3]$ | bias, vektor fitur satu contoh |
| Matriks | 2 | $(2, 3)$ | matriks $A$ pada contoh di bawah | bobot satu lapisan, data $X$ |
| Tensor rank 3 | 3 | $(2, 2, 3)$ | lihat di bawah | *batch* pesan × token × vektor |
| Tensor rank 4 | 4 | $(8, 3, 32, 32)$ | gambar | *batch* citra × kanal × tinggi × lebar |

### 2. Rank, dimensi, dan ukuran: tiga kata yang sering tertukar

- **Rank** (atau *order*, *ndim*): jumlah sumbu. Matriks punya rank 2.
- **Ukuran sumbu**: banyaknya elemen sepanjang satu sumbu. Matriks $(2,3)$ punya sumbu pertama berukuran 2 dan sumbu kedua berukuran 3.
- **Dimensi** dipakai untuk dua arti. "Vektor berdimensi 768" berarti panjangnya 768. "Tensor tiga dimensi" berarti rank-nya 3. Agar jelas, bagian ini memakai **rank** untuk jumlah sumbu dan **ukuran** untuk panjang sumbu.

!!! warning "Rank di Deep Learning bukan rank matriks di aljabar linear"
    Pada aljabar linear, "rank matriks" berarti jumlah baris (atau kolom) yang bebas linear. Di Deep Learning, "rank tensor" hanya berarti jumlah sumbu. Konteks menentukan artinya; Part ini selalu memakai arti kedua.

### 3. Contoh tensor yang dipakai berulang

Kita pakai tensor kecil $T$ berukuran $(2, 2, 3)$ yang melambangkan **2 pesan**, masing-masing **2 token**, dan tiap token diwakili **vektor berpanjang 3**. Angkanya hanya contoh agar mudah dihitung (bukan embedding sungguhan):

$$
T_{1} = \begin{bmatrix}1 & 2 & 3\\ 4 & 5 & 6\end{bmatrix},\qquad
T_{2} = \begin{bmatrix}7 & 8 & 9\\ 10 & 11 & 12\end{bmatrix}
$$

$T_1$ adalah pesan pertama (token pertama $[1,2,3]$, token kedua $[4,5,6]$), dan $T_2$ pesan kedua. Sumbu pertama adalah pesan, sumbu kedua token, sumbu ketiga elemen vektor.

## Rumus dan Penjelasan Setiap Simbol

### Shape dan jumlah elemen

Tensor dengan shape $(s_1, s_2, \dots, s_r)$ memiliki rank $r$ dan jumlah elemen

$$
N = s_1\times s_2\times\cdots\times s_r
$$

$s_k$ adalah ukuran sumbu ke-$k$ (bilangan bulat positif). Untuk $T$: $2\times 2\times 3 = 12$ elemen.

### Indexing

Elemen ditulis dengan satu indeks per sumbu: $T_{n,i,j}$ berarti pesan ke-$n$, token ke-$i$, elemen ke-$j$. Di tulisan matematika ini indeks dimulai dari **1**. Pustaka seperti NumPy memulai dari **0**, jadi $T_{n,i,j}$ di sini sama dengan `T[n-1, i-1, j-1]` di kode.

- $T_{2,1,2}$ = pesan kedua, token pertama, elemen kedua $=8$.
- Tanda `:` (semua indeks pada satu sumbu) memberi **irisan** (*slice*): $T_{:,1,:}$ adalah token pertama dari **setiap** pesan, berbentuk $(2,3)$, yaitu $\begin{bmatrix}1&2&3\\7&8&9\end{bmatrix}$.

Satu indeks yang dipilih menghilangkan satu sumbu: mengambil satu pesan dari $(2,2,3)$ menghasilkan $(2,3)$.

### Reshape dan transpose

**Reshape** mengubah shape tanpa mengubah urutan elemen (dibaca baris demi baris, sumbu terakhir paling cepat berubah) dan jumlah elemen harus tetap. Dari $(2,3)$ ke $(3,2)$:

$$
\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}
\ \xrightarrow{\text{reshape}}\
\begin{bmatrix}1&2\\3&4\\5&6\end{bmatrix}
$$

**Transpose** menukar sumbu dan **mengubah letak elemen**:

$$
\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}^{\!\top}
=
\begin{bmatrix}1&4\\2&5\\3&6\end{bmatrix}
$$

Keduanya menghasilkan shape $(3,2)$, tetapi isinya berbeda. Menukar keduanya adalah kesalahan umum.

### Operasi elemen

Dua tensor yang **shape-nya sama** dapat dijumlahkan, dikurangkan, atau dikalikan elemen demi elemen (**perkalian Hadamard**, ditulis $A\odot B$). Hasilnya berbentuk sama. Skalar dapat dioperasikan dengan tensor apa pun (diterapkan ke setiap elemen). Fungsi seperti sigmoid atau ReLU juga diterapkan elemen demi elemen.

### Broadcasting

**Broadcasting** adalah aturan yang memperluas tensor yang lebih kecil secara virtual agar cocok dengan yang lebih besar, tanpa menyalin data. Aturannya:

1. Sejajarkan kedua shape dari **kanan**. Shape yang lebih pendek dianggap diawali ukuran 1.
2. Untuk tiap pasangan ukuran: sama, **atau** salah satunya 1. Jika tidak, operasi **tidak sah** (error).
3. Ukuran hasil pada tiap sumbu adalah yang lebih besar dari pasangan itu.

| Shape A | Shape B | Sah? | Shape hasil |
|---|---|---|---|
| $(2,3)$ | $(3)$ | ya | $(2,3)$ |
| $(2,3)$ | $(2,1)$ | ya | $(2,3)$ |
| $(2,1)$ | $(1,3)$ | ya | $(2,3)$ |
| $(2,2,3)$ | $(3)$ | ya | $(2,2,3)$ |
| $(5,1,4)$ | $(3,1)$ | ya | $(5,3,4)$ |
| $(2,3)$ | $(2)$ | **tidak** ($3\ne 2$) | — |
| $(2,3,4)$ | $(3)$ | **tidak** ($4\ne 3$) | — |

### Reduksi sepanjang sumbu

Menjumlahkan (atau merata-ratakan) sepanjang satu sumbu **menghilangkan** sumbu itu. Untuk $A=\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}$ berbentuk $(2,3)$:

- jumlah sepanjang sumbu 1 (antar-baris, atau "tumpuk baris"): $[5,\,7,\,9]$, shape $(3)$;
- jumlah sepanjang sumbu 2 (di dalam tiap baris): $[6,\,15]$, shape $(2)$.

Dengan notasi sumbu 1 = indeks baris dan sumbu 2 = indeks kolom. Pustaka kode menomori dari 0, sehingga "sumbu 1" di sini sama dengan `axis=0` di kode.

### Perkalian matriks dan perkalian batch

Untuk $A\in\mathbb{R}^{m\times n}$ dan $B\in\mathbb{R}^{n\times p}$:

$$
(AB)_{ij}=\sum_{k=1}^{n} a_{ik}\,b_{kj},\qquad AB\in\mathbb{R}^{m\times p}
$$

Ukuran dalam $n$ harus sama dan **hilang** pada hasil. Setiap elemen hasil adalah dot product baris ke-$i$ dari $A$ dengan kolom ke-$j$ dari $B$.

Pada tensor rank 3, perkalian matriks diterapkan **pada dua sumbu terakhir**, dan sumbu di depannya diperlakukan sebagai *batch*:

$$
(N, m, n)\times(n, p)\to(N, m, p),\qquad
(N, m, n)\times(N, n, p)\to(N, m, p)
$$

Bentuk pertama berarti matriks yang sama $(n,p)$ dikalikan ke setiap anggota *batch* (cara satu lapisan jaringan memproses seluruh token). Bentuk kedua berarti anggota *batch* ke-$k$ dikalikan dengan pasangannya ke-$k$ (dipakai pada attention).

## Contoh Numerik Langkah demi Langkah

### Contoh 1 — Shape, indexing, dan jumlah elemen

$T$ berbentuk $(2,2,3)$, rank 3, jumlah elemen $2\cdot2\cdot3=12$.

- $T_{1,2,3}$: pesan 1, token 2, elemen 3 $\to 6$.
- $T_{2,\,:,\,:}=\begin{bmatrix}7&8&9\\10&11&12\end{bmatrix}$, shape $(2,3)$.
- $T_{:,\,2,\,3}=[6,\,12]$: elemen ketiga token kedua dari setiap pesan, shape $(2)$.

### Contoh 2 — Broadcasting penambahan bias

$A=\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}$ berbentuk $(2,3)$ dan $\mathbf{b}=[10,20,30]$ berbentuk $(3)$.

1. Sejajarkan dari kanan: $(2,3)$ dan $(3)$. Pasangan terakhir $3=3$, sumbu pertama hanya ada di $A$. Sah.
2. $\mathbf{b}$ dipakai ulang pada setiap baris $A$:

$$
A+\mathbf{b}=\begin{bmatrix}1+10&2+20&3+30\\4+10&5+20&6+30\end{bmatrix}=\begin{bmatrix}11&22&33\\14&25&36\end{bmatrix}
$$

Persis inilah yang terjadi saat bias ditambahkan ke setiap contoh dalam $Z=XW+\mathbf{b}$.

### Contoh 3 — Kolom + baris

$\mathbf{c}=\begin{bmatrix}1\\2\end{bmatrix}$ berbentuk $(2,1)$ dan $\mathbf{r}=[10,20,30]$ berbentuk $(1,3)$. Pasangan sumbu: $2$ dengan $1$ dan $1$ dengan $3$; hasilnya $(2,3)$ dengan elemen $c_i+r_j$:

$$
\mathbf{c}+\mathbf{r}=\begin{bmatrix}1+10&1+20&1+30\\2+10&2+20&2+30\end{bmatrix}=\begin{bmatrix}11&21&31\\12&22&32\end{bmatrix}
$$

### Contoh 4 — Shape yang tidak cocok

$A$ berbentuk $(2,3)$ ditambah $[1,2]$ berbentuk $(2)$. Sejajarkan dari kanan: $3$ dengan $2$, tidak sama dan tidak ada yang 1. **Tidak sah.** Perbaikan, jika maksudnya menambahkan satu bilangan per baris: ubah $[1,2]$ menjadi kolom $(2,1)$, yaitu $\begin{bmatrix}1\\2\end{bmatrix}$:

$$
A+\begin{bmatrix}1\\2\end{bmatrix}=\begin{bmatrix}2&3&4\\6&7&8\end{bmatrix}
$$

### Contoh 5 — Perkalian matriks dan shape-nya

$A$ berbentuk $(2,3)$ dikali $B=\begin{bmatrix}1&0\\0&1\\1&1\end{bmatrix}$ berbentuk $(3,2)$. Ukuran dalam $3=3$ dan hasil $(2,2)$.

- $(AB)_{11}=1\cdot1+2\cdot0+3\cdot1=4$
- $(AB)_{12}=1\cdot0+2\cdot1+3\cdot1=5$
- $(AB)_{21}=4\cdot1+5\cdot0+6\cdot1=10$
- $(AB)_{22}=4\cdot0+5\cdot1+6\cdot1=11$

$$
AB=\begin{bmatrix}4&5\\10&11\end{bmatrix}
$$

### Contoh 6 — Perkalian batch

$T$ berbentuk $(2,2,3)$ dikali $B$ berbentuk $(3,2)$ memberikan shape $(2,2,2)$: setiap pesan $T_n$ (berbentuk $(2,3)$) dikali $B$.

- Pesan 1: $\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}B=\begin{bmatrix}4&5\\10&11\end{bmatrix}$ (Contoh 5).
- Pesan 2: $\begin{bmatrix}7&8&9\\10&11&12\end{bmatrix}B=\begin{bmatrix}7+9&8+9\\10+12&11+12\end{bmatrix}=\begin{bmatrix}16&17\\22&23\end{bmatrix}$.

Artinya "ubah vektor berpanjang 3 setiap token menjadi vektor berpanjang 2 memakai matriks yang sama". Inilah cara satu lapisan jaringan atau satu proyeksi di Transformer bekerja pada seluruh token sekaligus.

### Contoh 7 — Rata-rata antar-token

Rata-rata $T$ sepanjang sumbu token (sumbu 2) menghilangkan sumbu itu, sehingga hasilnya $(2,3)$:

$$
\begin{bmatrix}\tfrac{1+4}{2}&\tfrac{2+5}{2}&\tfrac{3+6}{2}\\[2pt]\tfrac{7+10}{2}&\tfrac{8+11}{2}&\tfrac{9+12}{2}\end{bmatrix}=\begin{bmatrix}2{,}5&3{,}5&4{,}5\\8{,}5&9{,}5&10{,}5\end{bmatrix}
$$

Satu vektor per pesan: representasi sederhana sebuah pesan sebagai rata-rata vektor token-tokennya.

## Interpretasi, Asumsi, dan Keterbatasan

- **Broadcasting tidak pernah menebak niat.** Ia hanya mengikuti aturan sumbu. Shape $(3)$ dan $(3,1)$ berbeda dan menghasilkan operasi berbeda (lihat Kesalahan Umum).
- **Reshape mengasumsikan urutan baris-demi-baris.** Untuk mengubah arti sumbu (misalnya menukar token dan elemen), gunakan transpose, bukan reshape.
- **Perkalian matriks tidak komutatif.** $AB\neq BA$ pada umumnya, dan salah satunya bahkan bisa tidak terdefinisi.
- Contoh di sini memakai bilangan bulat kecil. Pada komputer, tensor berisi bilangan pecahan berpresisi terbatas.

## Hubungan dengan Deep Learning dan NLP

| Operasi | Muncul sebagai |
|---|---|
| Penambahan dengan broadcasting | bias $\mathbf{b}$ ditambahkan ke setiap baris $XW$ |
| Hadamard | gerbang, masker *dropout*, penskalaan ([Part 8](08-regularisasi-dan-generalisasi.md)) |
| Perkalian matriks | lapisan jaringan ([Part 3](03-matematika-neural-network.md)); proyeksi Q, K, V ([Part 12](12-attention-dan-self-attention.md)) |
| Perkalian batch | memproses banyak pesan sekaligus |
| Reduksi sepanjang sumbu | softmax (jumlah sepanjang kelas), rata-rata antar-token, normalisasi |
| Transpose | $QK^\top$ untuk skor attention |

Pada attention misalnya, $Q$ berbentuk $(m, d)$ dan $K$ berbentuk $(m, d)$ memberikan $QK^\top$ berbentuk $(m, m)$: satu skor untuk setiap pasangan token. Memahami shape sekarang membuat bagian itu jauh lebih mudah.

## Python

Pustaka standar saja: kita tulis alat kecil untuk membaca shape dan mengalikan matriks pada daftar bersarang.

```python
def bentuk(t):
    """Shape dari list bersarang (semua cabang dianggap sama panjang)."""
    ukuran = []
    while isinstance(t, list):
        ukuran.append(len(t))
        t = t[0]
    return tuple(ukuran)

def matmul(P, Q):
    return [[sum(P[i][k] * Q[k][j] for k in range(len(Q)))
             for j in range(len(Q[0]))] for i in range(len(P))]

A = [[1, 2, 3], [4, 5, 6]]          # shape (2, 3)
b = [10, 20, 30]                    # shape (3,)
B = [[1, 0], [0, 1], [1, 1]]        # shape (3, 2)
T = [[[1, 2, 3], [4, 5, 6]],
     [[7, 8, 9], [10, 11, 12]]]     # shape (2, 2, 3)

print(bentuk(A), bentuk(b), bentuk(B), bentuk(T))
# broadcasting (2,3) + (3,): b dipakai ulang pada tiap baris
print([[A[i][j] + b[j] for j in range(3)] for i in range(2)])
# kolom (2,1) + baris (1,3) -> (2,3)
kol, bar = [1, 2], [10, 20, 30]
print([[kol[i] + bar[j] for j in range(3)] for i in range(2)])
# jumlah sepanjang sumbu 0 dan sumbu 1
print([sum(A[i][j] for i in range(2)) for j in range(3)], [sum(r) for r in A])
# perkalian matriks (2,3) @ (3,2) -> (2,2)
print(matmul(A, B))
# batch: tiap T[n] (2,3) dikali B (3,2) -> (2,2,2)
hasil = [matmul(Tn, B) for Tn in T]
print(bentuk(hasil), hasil)
# reshape (2,3) -> (3,2): urutan elemen (row-major) dipertahankan
datar = [v for baris in A for v in baris]
print(datar, [datar[i * 2:(i + 1) * 2] for i in range(3)])
# rata-rata sepanjang sumbu 1 (antar token): (2,2,3) -> (2,3)
print([[sum(Tn[i][j] for i in range(2)) / 2 for j in range(3)] for Tn in T])
```

Keluaran:

```text
(2, 3) (3,) (3, 2) (2, 2, 3)
[[11, 22, 33], [14, 25, 36]]
[[11, 21, 31], [12, 22, 32]]
[5, 7, 9] [6, 15]
[[4, 5], [10, 11]]
(2, 2, 2) [[[4, 5], [10, 11]], [[16, 17], [22, 23]]]
[1, 2, 3, 4, 5, 6] [[1, 2], [3, 4], [5, 6]]
[[2.5, 3.5, 4.5], [8.5, 9.5, 10.5]]
```

Semua angka sama dengan Contoh 2, 3, 5, 6, 7 dan jumlah per sumbu di atas. Perhatikan bahwa `bar` dan `kol` di sini adalah daftar biasa; kita menggambar "kolom" dan "baris" lewat indeks `i` dan `j`.

### Opsional: pembanding dengan NumPy

!!! note "Blok opsional"
    Blok ini membutuhkan **NumPy**, yang **tidak dipasang oleh proyek ini** dan tidak dijalankan oleh situs. Keluaran di bawah dijalankan sungguhan dengan Python 3.14 dan NumPy 2.5.2. NumPy menomori sumbu dari 0.

```python
import numpy as np

A = np.array([[1, 2, 3], [4, 5, 6]])
b = np.array([10, 20, 30])
B = np.array([[1, 0], [0, 1], [1, 1]])
T = np.arange(1, 13).reshape(2, 2, 3)

print(A.shape, T.shape, T.ndim, T.size)
print(A + b)
print(A @ B)
print((T @ B).shape)
print(T[:, 0, :])
print(A.reshape(3, 2))
print(A.T)
print(np.array([1, 2, 3]) + np.array([[1], [2], [3]]))
print(T.mean(axis=1))
try:
    A + np.array([1, 2])
except ValueError as e:
    print(e)
```

```text
(2, 3) (2, 2, 3) 3 12
[[11 22 33]
 [14 25 36]]
[[ 4  5]
 [10 11]]
(2, 2, 2)
[[1 2 3]
 [7 8 9]]
[[1 2]
 [3 4]
 [5 6]]
[[1 4]
 [2 5]
 [3 6]]
[[2 3 4]
 [3 4 5]
 [4 5 6]]
[[ 2.5  3.5  4.5]
 [ 8.5  9.5 10.5]]
operands could not be broadcast together with shapes (2,3) (2,)
```

`T[:, 0, :]` adalah $T_{:,1,:}$ pada notasi bagian ini, dan baris terakhir adalah pesan galat untuk Contoh 4.

## Kesalahan Umum

1. **Menyamakan reshape dengan transpose.** Keduanya menghasilkan $(3,2)$ dari $(2,3)$, tetapi isinya berbeda.
2. **Mencampur vektor $(n)$ dengan kolom $(n,1)$.** Menambahkan $[1,2,3]$ berbentuk $(3)$ dengan kolom $\begin{bmatrix}1\\2\\3\end{bmatrix}$ berbentuk $(3,1)$ menghasilkan matriks $(3,3)$:

    $$
    \begin{bmatrix}2&3&4\\3&4&5\\4&5&6\end{bmatrix}
    $$

    bukan vektor berpanjang 3. Ini penyebab klasik loss yang tiba-tiba "aneh" karena operasi diam-diam tersebar ke matriks.
3. **Salah menghitung ukuran dalam pada perkalian matriks.** $(m,n)(p,q)$ hanya sah jika $n=p$.
4. **Menganggap hasil broadcasting menyalin data.** Secara konsep tensor kecil "dipakai ulang"; ini tidak mengubah nilai tensor aslinya.
5. **Mengacaukan indeks 1-based (matematika) dengan 0-based (kode).** Selalu cek konvensi sebelum mengambil elemen.
6. **Menjumlahkan sepanjang sumbu yang salah.** Jumlah sepanjang token dan jumlah sepanjang elemen vektor memberi shape dan arti berbeda.

## Latihan Mandiri

1. Tentukan rank dan jumlah elemen untuk shape $(4,5,3)$, $(7)$, $()$, dan $(32,128,768)$.
2. Pada $T$ di atas, berapa $T_{2,1,2}$, $T_{1,2,:}$, dan $T_{:,2,3}$? Tuliskan juga shape masing-masing.
3. Ubah $A=\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}$ menjadi shape $(3,2)$ dan $(6)$ dengan reshape, lalu tuliskan $A^\top$. Apakah reshape $(3,2)$ sama dengan $A^\top$?
4. Apakah pasangan shape berikut sah untuk broadcasting? Jika ya, tuliskan shape hasil: (a) $(5,1,4)$ dan $(3,1)$; (b) $(3,4)$ dan $(4,3)$; (c) $(8)$ dan $(1)$; (d) $(2,3,4)$ dan $(3)$.
5. Hitung $\begin{bmatrix}1&2\\3&4\end{bmatrix}+[10,20]$ dan $\begin{bmatrix}1&2\\3&4\end{bmatrix}+\begin{bmatrix}10\\20\end{bmatrix}$.
6. Untuk $A=\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}$, hitung jumlah semua elemen, rata-rata tiap kolom, dan rata-rata tiap baris, beserta shape tiap hasil.
7. Hitung $\begin{bmatrix}1&2\\3&4\end{bmatrix}\begin{bmatrix}0&1\\1&0\end{bmatrix}$ dan $\begin{bmatrix}0&1\\1&0\end{bmatrix}\begin{bmatrix}1&2\\3&4\end{bmatrix}$. Apa yang dilakukan masing-masing perkalian?
8. Sebuah lapisan memproses *batch* $(32,10,64)$ dengan $W$ berbentuk $(64,128)$ dan $\mathbf{b}$ berbentuk $(128)$. Berapa shape $XW+\mathbf{b}$ dan berapa jumlah parameter lapisan?
9. Hitung $[1,2]+\begin{bmatrix}10\\20\end{bmatrix}$ dan jelaskan mengapa hasilnya matriks.
10. $Q=\begin{bmatrix}1&0\\0&1\end{bmatrix}$ dan $K=\begin{bmatrix}1&1\\2&0\end{bmatrix}$ (masing-masing $(2,2)$). Hitung $QK^\top$ dan tafsirkan elemen-elemennya sebagai dot product. Berapa shape $QK^\top$ jika $Q$ dan $K$ berbentuk $(5,16)$?

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $(4,5,3)$: rank 3, $4\cdot5\cdot3=60$ elemen. $(7)$: rank 1, 7 elemen. $()$: rank 0 (skalar), 1 elemen. $(32,128,768)$: rank 3, $32\cdot128\cdot768=3{.}145{.}728$ elemen.

??? success "Soal 2"
    $T_{2,1,2}$: pesan 2, token 1, elemen 2 $=8$ (skalar, shape $()$). $T_{1,2,:}=[4,5,6]$, shape $(3)$. $T_{:,2,3}=[6,12]$, shape $(2)$.

??? success "Soal 3"
    Reshape $(3,2)$: $\begin{bmatrix}1&2\\3&4\\5&6\end{bmatrix}$. Reshape $(6)$: $[1,2,3,4,5,6]$. $A^\top=\begin{bmatrix}1&4\\2&5\\3&6\end{bmatrix}$. **Tidak sama**: shape sama $(3,2)$, tetapi susunan elemennya berbeda.

??? success "Soal 4"
    (a) Sah: $(3,1)$ dianggap $(1,3,1)$. Dari kanan: $4$ dengan $1$ menjadi $4$; $1$ dengan $3$ menjadi $3$; $5$ dengan $1$ menjadi $5$. Shape $(5,3,4)$. (b) Tidak sah: $4\ne 3$ dan $3\ne 4$ tanpa 1. (c) Sah: $(8)$. (d) Tidak sah: $4\ne 3$ pada sumbu terakhir.

??? success "Soal 5"
    Baris $[10,20]$: $\begin{bmatrix}11&22\\13&24\end{bmatrix}$ (ditambahkan ke setiap baris). Kolom $\begin{bmatrix}10\\20\end{bmatrix}$: $\begin{bmatrix}11&12\\23&24\end{bmatrix}$ (ditambahkan ke setiap kolom).

??? success "Soal 6"
    Jumlah semua: $1+2+3+4+5+6=21$ (skalar). Rata-rata tiap kolom: $[2{,}5,\,3{,}5,\,4{,}5]$, shape $(3)$. Rata-rata tiap baris: $[2,\,5]$, shape $(2)$.

??? success "Soal 7"
    Pertama: baris $[1,2]$ kali kolom pertama $[0,1]^\top$ $=2$, kali kolom kedua $[1,0]^\top=1$; hasil $\begin{bmatrix}2&1\\4&3\end{bmatrix}$: **kolom saling bertukar**. Kedua: $\begin{bmatrix}3&4\\1&2\end{bmatrix}$: **baris saling bertukar**. Hasil berbeda, jadi perkalian matriks tidak komutatif.

??? success "Soal 8"
    $XW$ berbentuk $(32,10,128)$; $\mathbf{b}$ berbentuk $(128)$ disejajarkan dari kanan dan dibroadcast, sehingga hasil $(32,10,128)$. Parameter: $64\times128+128=8192+128=8320$. Jumlah parameter tidak bergantung pada ukuran *batch* atau jumlah token.

??? success "Soal 9"
    $[1,2]$ berbentuk $(2)$ dan kolom berbentuk $(2,1)$. Hasil berbentuk $(2,2)$: $\begin{bmatrix}1+10&2+10\\1+20&2+20\end{bmatrix}=\begin{bmatrix}11&12\\21&22\end{bmatrix}$. Setiap elemen kolom dipasangkan dengan setiap elemen baris, sehingga hasilnya matriks, bukan vektor.

??? success "Soal 10"
    $K^\top=\begin{bmatrix}1&2\\1&0\end{bmatrix}$. $QK^\top=\begin{bmatrix}1&2\\1&0\end{bmatrix}$. Elemen $(i,j)$ adalah dot product baris ke-$i$ dari $Q$ dengan baris ke-$j$ dari $K$: $(1,0)\cdot(1,1)=1$; $(1,0)\cdot(2,0)=2$; $(0,1)\cdot(1,1)=1$; $(0,1)\cdot(2,0)=0$. Untuk $(5,16)$ dan $(5,16)$: $K^\top$ berbentuk $(16,5)$ dan hasilnya $(5,5)$.

## Ringkasan Materi

- **Tensor** adalah kotak angka; **rank** adalah jumlah sumbu, **shape** adalah daftar ukuran sumbu, jumlah elemen $=$ hasil kali shape.
- Indexing memilih elemen atau irisan; satu indeks menghilangkan satu sumbu. Matematika memakai indeks 1-based, kode 0-based.
- **Reshape** mempertahankan urutan elemen; **transpose** menukar sumbu dan mengubah letak elemen.
- **Broadcasting**: sejajarkan shape dari kanan; tiap pasangan harus sama atau salah satunya 1.
- Reduksi sepanjang sumbu menghilangkan sumbu itu.
- Perkalian matriks $(m,n)(n,p)\to(m,p)$; pada tensor rank 3 diterapkan pada dua sumbu terakhir sebagai *batch*.
- Memeriksa shape dulu mencegah sebagian besar kesalahan.

## Checklist Penguasaan

- [ ] Membaca rank, shape, dan jumlah elemen suatu tensor.
- [ ] Mengambil elemen dan irisan dengan indeks.
- [ ] Membedakan reshape dan transpose dengan contoh.
- [ ] Menerapkan aturan broadcasting dan menentukan shape hasil atau galat.
- [ ] Menghitung jumlah dan rata-rata sepanjang sumbu tertentu.
- [ ] Menghitung perkalian matriks dan perkalian batch beserta shape-nya.
- [ ] Menjelaskan bagaimana bias ditambahkan ke setiap contoh dalam satu batch.

## Prasyarat untuk Materi Berikutnya

[Part 3 — Matematika Neural Network](03-matematika-neural-network.md) memakai semua operasi ini: $XW+\mathbf{b}$ untuk satu lapisan, aktivasi elemen demi elemen, dan susunan beberapa lapisan. Pastikan kamu bisa menentukan shape hasil perkalian matriks dan penambahan bias.
