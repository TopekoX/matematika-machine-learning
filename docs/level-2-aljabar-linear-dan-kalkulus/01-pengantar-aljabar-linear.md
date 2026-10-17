# Part 1 — Pengantar Aljabar Linear

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan apa itu aljabar linear dan mengapa ia menjadi bahasa data dalam Machine Learning;
- membedakan skalar, vektor, dan matriks;
- membaca dan menulis vektor beserta elemen dan dimensinya;
- membaca dan menulis matriks beserta ukuran ($m \times n$) dan elemen ($a_{ij}$);
- membedakan dua arti kata "dimensi";
- membaca notasi dasar aljabar linear yang dipakai di seluruh Level 2;
- mengubah sekumpulan data menjadi vektor dan matriks.

## Prasyarat

[Level 1 — Part 6](../level-1-aljabar-dasar-dan-fungsi/06-sistem-persamaan-linear.md) (bentuk matriks sistem persamaan, secara pengantar), [Level 1 — Part 5](../level-1-aljabar-dasar-dan-fungsi/05-koordinat-dan-grafik.md) (pasangan terurut dan koordinat), serta [Level 0 — Part 8](../level-0-fondasi-matematika/08-notasi-matematika-dasar.md) (subskrip, indeks, dan sigma).

## Mengapa Materi Ini Penting?

Data nyata hampir tidak pernah berupa satu angka. Sebuah rumah punya luas, jumlah kamar, dan umur. Sebuah gambar punya ribuan piksel. Sebuah kalimat punya banyak kata. Aljabar linear adalah cara rapi menyimpan dan menghitung **banyak angka sekaligus**. Hampir semua model Machine Learning, dari regresi sederhana sampai jaringan saraf, ditulis dengan vektor dan matriks. Tanpa notasi ini, rumusnya akan panjang dan sulit dibaca.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Apa Itu Aljabar Linear?

**Aljabar linear** (*linear algebra*) adalah cabang matematika yang mempelajari **vektor**, **matriks**, dan operasi **linear** pada keduanya. Operasi linear pada dasarnya hanya penjumlahan dan perkalian dengan bilangan, seperti pada fungsi linear $y = mx + b$ di Level 1, tetapi sekarang dengan banyak angka sekaligus.

Bandingkan dua cara menyimpan data satu rumah:

- Tanpa aljabar linear: tiga variabel terpisah `luas`, `kamar`, dan `umur`.
- Dengan aljabar linear: **satu objek** $\mathbf{x}$ yang berisi ketiganya.

Dengan satu objek, kita bisa menulis "hitung prediksi untuk $\mathbf{x}$" tanpa menyebut ketiga angka satu per satu.

### 2. Mengapa Aljabar Linear Penting untuk Machine Learning?

- **Satu data adalah vektor.** Rumah dengan luas 60, 2 kamar, dan umur 5 tahun menjadi vektor $(60, 2, 5)$.
- **Seluruh data adalah matriks.** Empat rumah menjadi tabel dengan empat baris.
- **Model memakai operasi linear.** Prediksi sebuah model linear adalah jumlah berbobot dari fitur, yang akan kita tulis ringkas di Part 2.
- **Komputer menghitung dengan cepat.** Perkalian matriks dapat dikerjakan secara paralel sehingga pelatihan model besar menjadi mungkin.
- **Data lain pun berbentuk angka.** Gambar adalah matriks piksel, dan kata dapat diubah menjadi vektor.

### 3. Skalar

**Skalar** (*scalar*) adalah **satu bilangan** biasa, seperti $7$, $-2{,}5$, atau $\pi$. Skalar hanya punya besar, tanpa arah dan tanpa urutan.

Notasi: huruf miring biasa, misalnya $a$, $x$, atau $\lambda$. Setiap bilangan yang sudah kamu pakai di Level 0 dan Level 1 adalah skalar. Dalam aljabar linear, skalar sering berperan sebagai pengali (membesarkan atau mengecilkan sebuah vektor), seperti yang dibahas di Part 2.

### 4. Vektor

**Vektor** (*vector*) adalah **daftar bilangan yang berurutan**. Setiap bilangan di dalamnya disebut **elemen** atau **komponen**.

Contoh: data satu rumah (fiktif) dengan luas 60 m², 2 kamar, dan umur 5 tahun:

$$
\mathbf{x} =
\begin{bmatrix} 60 \\ 2 \\ 5 \end{bmatrix}
$$

Hal-hal penting:

- **Urutan penting.** Elemen pertama selalu luas, kedua selalu jumlah kamar, ketiga selalu umur. $(60, 2, 5)$ berbeda dengan $(2, 60, 5)$.
- **Elemen diberi indeks.** $x_1 = 60$, $x_2 = 2$, $x_3 = 5$. Angka kecil di kanan bawah (subskrip) adalah nomor urut elemen, seperti $x_i$ pada notasi sigma di Level 0.
- **Notasi.** Vektor ditulis dengan huruf kecil **tebal**: $\mathbf{x}$, $\mathbf{v}$, $\mathbf{w}$.
- **Vektor kolom dan vektor baris.** Vektor kolom ditulis menurun (seperti di atas). Vektor baris ditulis mendatar: $\begin{bmatrix} 60 & 2 & 5 \end{bmatrix}$. Di buku ini, vektor secara default adalah **vektor kolom**. Dalam teks satu baris, kita sering menulisnya $(60, 2, 5)$ untuk menghemat tempat, dan maksudnya tetap vektor kolom.

**Dua cara memandang vektor.** Pada Level 1 kamu mengenal pasangan terurut $(x, y)$ sebagai titik. Vektor dua elemen $(3, 2)$ dapat dipandang:

1. sebagai **titik** pada bidang: 3 ke kanan dan 2 ke atas dari titik asal; atau
2. sebagai **panah** dari titik asal ke titik itu, yang memiliki **arah** dan **panjang**.

Kedua pandangan itu benar dan saling melengkapi. Pandangan panah dibahas di Part 2.

### 5. Matriks

**Matriks** (*matrix*) adalah **tabel bilangan** yang tersusun dalam baris dan kolom. Contoh: data empat rumah (fiktif) dengan tiga fitur (luas, kamar, umur):

$$
X =
\begin{bmatrix}
60 & 2 & 5 \\
80 & 3 & 10 \\
100 & 4 & 2 \\
45 & 1 & 20
\end{bmatrix}
$$

Hal-hal penting:

- **Baris** (*row*) adalah deretan mendatar; **kolom** (*column*) adalah deretan tegak. Pada $X$, setiap **baris** adalah satu rumah dan setiap **kolom** adalah satu fitur.
- **Ukuran** matriks ditulis **banyak baris $\times$ banyak kolom**, dibaca "m kali n". $X$ memiliki 4 baris dan 3 kolom, jadi ukurannya $4 \times 3$. Urutannya selalu baris dulu, baru kolom.
- **Elemen** di baris ke-$i$ dan kolom ke-$j$ ditulis $a_{ij}$ (dua subskrip: baris dulu, kolom kedua). Pada $X$, elemen $x_{23} = 10$ (baris 2, kolom 3, yaitu umur rumah kedua).
- **Notasi.** Matriks ditulis dengan huruf kapital biasa: $A$, $B$, $X$.
- **Matriks persegi** (*square matrix*) adalah matriks yang jumlah barisnya sama dengan jumlah kolomnya, misalnya $2 \times 2$ atau $3 \times 3$.

Sebuah matriks dapat dipandang sebagai kumpulan **vektor baris** (di sini: empat vektor rumah) atau kumpulan **vektor kolom** (di sini: tiga vektor fitur).

### 6. Dimensi dan Bentuk

Kata **dimensi** dipakai dengan dua arti yang berbeda, dan keduanya muncul di Machine Learning:

1. **Dimensi vektor** adalah **banyaknya elemen**. Vektor $(60, 2, 5)$ berdimensi 3. Dimensi $n$ ditulis sebagai bagian dari himpunan $\mathbb{R}^n$ (dibaca "R pangkat n"), yaitu himpunan semua vektor dengan $n$ bilangan real. Kita tulis $\mathbf{x} \in \mathbb{R}^3$.
2. **Ukuran (bentuk, *shape*) matriks** adalah **baris $\times$ kolom**. $X$ berukuran $4 \times 3$, ditulis $X \in \mathbb{R}^{4 \times 3}$.

Hubungan keduanya: vektor kolom dengan $n$ elemen dapat dianggap matriks berukuran $n \times 1$, dan vektor baris sebagai matriks $1 \times n$. Skalar dapat dianggap matriks $1 \times 1$.

**Dimensi dan visualisasi.**

| Dimensi | Contoh | Bisa digambar? |
|---|---|---|
| 1 | $(5)$ | ya, satu garis bilangan |
| 2 | $(3, 2)$ | ya, bidang |
| 3 | $(3, 2, 5)$ | ya, ruang tiga dimensi |
| 4 atau lebih | $(60, 2, 5, 1)$ | tidak langsung bisa digambar |

Data Machine Learning sering memiliki ratusan atau ribuan fitur, jadi vektornya hidup di ruang berdimensi tinggi. Kita tidak bisa menggambarnya, tetapi aturan hitungnya sama persis dengan dimensi dua atau tiga.

### 7. Notasi Dasar Aljabar Linear

| Notasi | Dibaca | Arti |
|---|---|---|
| $a$ | a | skalar |
| $\mathbf{v}$ | v tebal | vektor |
| $v_i$ | v sub i | elemen ke-$i$ dari $\mathbf{v}$ |
| $A$ | A | matriks |
| $a_{ij}$ | a sub i j | elemen baris ke-$i$, kolom ke-$j$ dari $A$ |
| $\mathbb{R}^n$ | R pangkat n | himpunan vektor dengan $n$ bilangan real |
| $\mathbb{R}^{m \times n}$ | R pangkat m kali n | himpunan matriks berukuran $m \times n$ |
| $\in$ | anggota dari | $\mathbf{x} \in \mathbb{R}^3$: $\mathbf{x}$ adalah vektor 3 elemen |
| $\sum_{i=1}^{n}$ | sigma | jumlahkan untuk $i$ dari 1 sampai $n$ |

Aturan indeks: **baris dulu, kolom kedua**. Matriks $m \times n$ memiliki elemen $a_{ij}$ dengan $1 \leq i \leq m$ dan $1 \leq j \leq n$. Karena itu pada matriks $2 \times 3$, elemen $a_{32}$ **tidak ada** (tidak ada baris ke-3).

!!! warning "Python menomori dari 0"
    Seperti dibahas di Level 0, matematika biasanya memulai nomor dari 1 sedangkan Python dari 0. Elemen $a_{23}$ pada matriks `M` di Python ditulis `M[1][2]`.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Aljabar linear | cabang matematika tentang vektor, matriks, dan operasi linear | |
| Skalar (*scalar*) | satu bilangan | $7$ |
| Vektor (*vector*) | daftar bilangan berurutan | $(60, 2, 5)$ |
| Elemen / komponen | satu bilangan di dalam vektor atau matriks | $x_2 = 2$ |
| Indeks (*index*) | nomor urut elemen | $2$ pada $x_2$ |
| Vektor kolom / baris | vektor yang ditulis menurun / mendatar | |
| Dimensi vektor | banyak elemen vektor | 3 untuk $(60, 2, 5)$ |
| Matriks (*matrix*) | tabel bilangan | $X$ di atas |
| Baris / kolom | deretan mendatar / tegak dalam matriks | |
| Ukuran / bentuk (*shape*) | banyak baris $\times$ banyak kolom | $4 \times 3$ |
| Matriks persegi | matriks dengan baris sama dengan kolom | $2 \times 2$ |
| Fitur (*feature*) | satu sifat yang diukur dari data | luas rumah |
| Sampel / data | satu objek yang diukur | satu rumah |

## Rumus dan Penjelasan Setiap Simbol

**Vektor berdimensi $n$:**

$$
\mathbf{v} =
\begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}
\in \mathbb{R}^n
$$

- $v_i$ adalah elemen ke-$i$, bilangan real, dengan $i = 1, 2, \ldots, n$.
- Tanda $\vdots$ berarti "dan seterusnya" (elemen $v_3$ sampai $v_{n-1}$ tidak ditulis).
- Contoh: $n = 3$ dan $\mathbf{v} = (60, 2, 5)$ memberi $v_1 = 60$.

**Matriks berukuran $m \times n$:**

$$
A =
\begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}
\in \mathbb{R}^{m \times n}
$$

- $m$ adalah banyak baris dan $n$ adalah banyak kolom (keduanya bilangan asli).
- $a_{ij}$ adalah elemen baris ke-$i$, kolom ke-$j$.
- Tanda $\cdots$ berarti "dan seterusnya ke kanan", $\vdots$ berarti "dan seterusnya ke bawah", dan $\ddots$ berarti "dan seterusnya ke kanan bawah".
- **Banyak elemen** $= m \times n$. Matriks $4 \times 3$ memiliki $4 \times 3 = 12$ elemen.

## Penjelasan Langkah demi Langkah

**Menyusun data menjadi matriks:**

1. Tentukan **apa yang menjadi baris** (satu sampel, misalnya satu rumah) dan **apa yang menjadi kolom** (satu fitur).
2. Tulis setiap sampel sebagai satu baris, dengan **urutan fitur yang sama** di semua baris.
3. Hitung ukuran: banyak sampel $\times$ banyak fitur.
4. Periksa: tidak ada sel kosong, dan semua baris berpanjang sama.

## Contoh Soal dan Pembahasan

### Contoh 1 — Mengenali objek dan dimensinya

Tentukan jenis dan dimensi atau ukuran: (a) $7$ (b) $(3, -1)$ (c) $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ (d) $\begin{bmatrix} 5 \\ 0 \\ 2 \\ 9 \end{bmatrix}$.

1. (a) Satu bilangan: **skalar**.
2. (b) Daftar dua bilangan: **vektor**, dimensi 2, anggota $\mathbb{R}^2$.
3. (c) Tabel dengan 2 baris dan 2 kolom: **matriks** berukuran $2 \times 2$, jadi matriks persegi.
4. (d) Satu kolom berisi empat bilangan: **vektor kolom** berdimensi 4, anggota $\mathbb{R}^4$ (atau matriks $4 \times 1$).

### Contoh 2 — Membaca elemen matriks

Diketahui $A = \begin{bmatrix} 4 & 7 & 1 \\ 0 & 5 & 9 \end{bmatrix}$. Tentukan ukuran, $a_{12}$, $a_{21}$, $a_{23}$, baris ke-2, dan kolom ke-3.

1. Ukuran: 2 baris dan 3 kolom, jadi $2 \times 3$.
2. $a_{12}$: baris 1, kolom 2, yaitu $7$.
3. $a_{21}$: baris 2, kolom 1, yaitu $0$.
4. $a_{23}$: baris 2, kolom 3, yaitu $9$.
5. Baris ke-2 adalah $(0, 5, 9)$. Kolom ke-3 adalah $(1, 9)$.
6. Cek jumlah elemen: $2 \times 3 = 6$. Memang ada 6 bilangan.

### Contoh 3 — Menyusun matriks dari tabel

Tiga siswa dengan nilai dua mata pelajaran: Ani (80, 90), Budi (70, 60), Cici (95, 85). Susun matriks nilai, tentukan ukurannya, dan baca $x_{32}$.

1. Misalkan baris adalah siswa dan kolom adalah mata pelajaran.
2. Matriksnya:

$$
X = \begin{bmatrix} 80 & 90 \\ 70 & 60 \\ 95 & 85 \end{bmatrix}
$$

3. Ukuran: 3 siswa $\times$ 2 mata pelajaran $= 3 \times 2$.
4. $x_{32}$: baris 3 (Cici), kolom 2 (mata pelajaran kedua), yaitu $85$.
5. Jika kita tukar peran (baris menjadi mata pelajaran), ukurannya menjadi $2 \times 3$. Kedua susunan sah, tetapi harus konsisten dalam satu perhitungan.

### Contoh 4 — Jumlah berbobot sebagai gambaran

Sebuah model fiktif memprediksi harga rumah dengan bobot $\mathbf{w} = (2, 10, -1)$ untuk (luas, kamar, umur) dan konstanta $b = 20$. Untuk rumah pertama $\mathbf{x} = (60, 2, 5)$, hitung prediksinya.

1. Kalikan setiap fitur dengan bobot pasangannya: $2 \times 60 = 120$, $10 \times 2 = 20$, $(-1) \times 5 = -5$.
2. Jumlahkan: $120 + 20 - 5 = 135$.
3. Tambahkan $b$: $135 + 20 = 155$.
4. Prediksi: $155$ (juta, dalam angka fiktif).

Perhitungan "kalikan pasangan lalu jumlahkan" ini akan kita beri nama dan notasi ringkas di Part 2, yaitu *dot product*: prediksinya menjadi $\mathbf{w} \cdot \mathbf{x} + b$.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** Vektor $(5, 0, 2, 9)$ berdimensi 4. Elemen ketiganya adalah $2$, ditulis $v_3 = 2$.

**Sedang.** Matriks berukuran $5 \times 2$ memiliki 5 baris, 2 kolom, dan $5 \times 2 = 10$ elemen. Vektor baris ke-3 memiliki 2 elemen, dan vektor kolom ke-1 memiliki 5 elemen.

**Sedang.** Dataset 1.000 sampel dengan 20 fitur: matriks fitur berukuran $1000 \times 20$, banyak elemennya $20.000$, dan label (satu nilai per sampel) adalah vektor berdimensi $1000$.

**Menantang.** Mengapa $(3, 2)$ dan $(2, 3)$ berbeda, padahal berisi bilangan yang sama? Karena urutan penting: $(3, 2)$ adalah titik 3 ke kanan dan 2 ke atas, sedangkan $(2, 3)$ adalah 2 ke kanan dan 3 ke atas. Dua titik itu berbeda letaknya.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menyebut matriks $3 \times 4$ punya 3 kolom dan 4 baris | ukuran ditulis baris dulu | 3 baris dan 4 kolom |
| Menulis elemen sebagai $a_{ji}$ saat maksudnya baris $i$, kolom $j$ | urutan subskrip: baris dulu | $a_{ij}$ |
| Menyamakan $(1, 2, 3)$ dengan $(3, 2, 1)$ | urutan elemen vektor penting | berbeda |
| Menyebut skalar dan vektor satu elemen $(5)$ "sama" | secara teknis berbeda jenis (walau mirip) | skalar adalah bilangan; vektor satu elemen adalah daftar |
| Menyebut vektor $(60, 2, 5)$ "berukuran $1 \times 3$" tanpa konteks | bisa vektor kolom ($3 \times 1$) atau baris ($1 \times 3$) | tentukan kolom atau baris |
| Mencampur dua arti "dimensi" | vektor: banyak elemen; matriks: baris $\times$ kolom | bedakan |
| Mengira elemen $a_{32}$ ada di matriks $2 \times 3$ | tidak ada baris ke-3 | indeks harus dalam rentang |
| Mengira `M[2][3]` di Python sama dengan $a_{23}$ | Python mulai dari 0 | `M[1][2]` |
| Menaruh fitur dalam urutan berbeda di tiap baris | kolom tidak lagi berarti fitur yang sama | konsisten |
| Menulis vektor dengan huruf biasa lalu mencampurnya dengan skalar | notasi tebal membedakan | pakai $\mathbf{x}$ untuk vektor |

## Hubungan dengan Machine Learning

- **Matriks fitur.** Dataset dengan $n$ sampel dan $d$ fitur disusun sebagai matriks $X \in \mathbb{R}^{n \times d}$: setiap baris satu sampel, setiap kolom satu fitur.
- **Vektor label.** Nilai yang ingin diprediksi untuk $n$ sampel disusun sebagai vektor $\mathbf{y} \in \mathbb{R}^n$.
- **Parameter model.** Bobot $\mathbf{w} \in \mathbb{R}^d$ adalah vektor dengan satu bobot per fitur.
- **Dimensi tinggi.** Teks, gambar, dan data sensor menghasilkan vektor dengan ribuan elemen. Aturan hitungnya sama dengan dimensi dua.
- **Bentuk (shape) harus cocok.** Banyak kesalahan program Machine Learning adalah salah bentuk, misalnya menjumlahkan vektor yang dimensinya berbeda. Membiasakan diri mengecek ukuran sejak awal sangat membantu.

Catatan: contoh harga rumah di halaman ini memakai angka fiktif untuk latihan notasi, bukan model yang sudah dilatih.

### Python: vektor dan matriks sebagai daftar

Di Python murni, vektor adalah `list` dan matriks adalah `list` yang berisi `list` (satu `list` per baris). Pustaka seperti NumPy menyediakan alat khusus, tetapi buku ini memakai `list` agar setiap langkah terlihat.

```python
skalar = 7
vektor = [60, 2, 5]
matriks = [
    [60, 2, 5],
    [80, 3, 10],
    [100, 4, 2],
    [45, 1, 20],
]

print(len(vektor))                    # 3, dimensi vektor
print(len(matriks), len(matriks[0]))  # 4 3, ukuran 4 x 3
```

`len(matriks)` memberi banyak baris, dan `len(matriks[0])` memberi banyak kolom (panjang baris pertama).

Mengambil elemen. Ingat bahwa Python mulai dari 0, sehingga elemen $x_{23}$ ditulis `matriks[1][2]`:

```python
print(vektor[0])        # 60, elemen pertama (v_1)
print(matriks[1][2])    # 10, elemen baris 2 kolom 3 (x_23)
print(matriks[1])       # [80, 3, 10], baris ke-2
kolom_1 = [baris[0] for baris in matriks]
print(kolom_1)          # [60, 80, 100, 45], kolom ke-1
```

Baris terakhir berarti: "dari setiap baris, ambil elemen pertamanya". Itulah cara mengambil sebuah kolom dari matriks berbentuk `list`.

Jumlah berbobot dari Contoh 4:

```python
w = [2, 10, -1]
b = 20
x = [60, 2, 5]

y_topi = w[0] * x[0] + w[1] * x[1] + w[2] * x[2] + b
print(y_topi)    # 155

# cara yang sama, untuk dimensi berapa pun
y_topi = sum(wi * xi for wi, xi in zip(w, x)) + b
print(y_topi)    # 155
```

`zip(w, x)` memasangkan elemen berurutan dari dua daftar. Baris terakhir akan dipakai lagi sebagai dot product di Part 2.

Memeriksa bentuk sebelum menghitung:

```python
def ukuran(m):
    return len(m), len(m[0])

print(ukuran(matriks))   # (4, 3)
```

## Latihan Mandiri

1. Tentukan jenis (skalar, vektor, atau matriks) dan dimensi atau ukurannya: (a) $7$ (b) $(3, -1)$ (c) $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ (d) $(5, 0, 2, 9)$ (e) $\begin{bmatrix} 1 & 2 & 3 \end{bmatrix}$
2. (a) Vektor $(2, 4, 6, 8, 10)$ anggota $\mathbb{R}^n$ untuk $n$ berapa? (b) Berapa elemen pada matriks $3 \times 4$? (c) Matriks $5 \times 2$ punya berapa baris, kolom, dan elemen?
3. Diketahui $A = \begin{bmatrix} 4 & 7 & 1 \\ 0 & 5 & 9 \end{bmatrix}$. Tentukan ukuran, $a_{12}$, $a_{21}$, $a_{23}$, baris ke-2, dan kolom ke-3.
4. Untuk matriks $A$ pada soal 3, apakah $a_{32}$ dan $a_{24}$ ada? Jelaskan.
5. Tiga siswa: Ani (80, 90), Budi (70, 60), Cici (95, 85) untuk dua mata pelajaran. Susun matriksnya (baris = siswa), tentukan ukurannya, dan baca $x_{32}$. Apa arti $x_{32}$?
6. Untuk `M = [[4, 7, 1], [0, 5, 9]]` di Python, tulis ekspresi untuk $a_{23}$, banyak baris, dan banyak kolom.
7. Vektor $(3, 2)$ digambar sebagai titik di bidang: jelaskan letaknya. Mengapa vektor $(3, 2, 5)$ sulit digambar di bidang? Apakah $(3, 2)$ sama dengan $(2, 3)$?
8. Model fiktif $\hat{y} = w_1 x_1 + w_2 x_2 + w_3 x_3 + b$ dengan $\mathbf{w} = (2, 10, -1)$ dan $b = 20$. Hitung $\hat{y}$ untuk $\mathbf{x} = (80, 3, 10)$.
9. Dataset 1.000 sampel dengan 20 fitur: berapa ukuran matriks fiturnya, berapa banyak elemennya, dan berapa dimensi vektor labelnya?
10. Seorang siswa menulis "matriks $3 \times 4$ punya 3 kolom dan 4 baris". Temukan kesalahannya dan perbaiki.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) skalar. (b) vektor berdimensi 2. (c) matriks $2 \times 2$ (persegi). (d) vektor berdimensi 4. (e) matriks $1 \times 3$, yaitu vektor baris berdimensi 3.

??? success "Soal 2"
    (a) $n = 5$ (ada 5 elemen). (b) $3 \times 4 = 12$ elemen. (c) 5 baris, 2 kolom, $5 \times 2 = 10$ elemen.

??? success "Soal 3"
    Ukuran $2 \times 3$. $a_{12} = 7$ (baris 1, kolom 2). $a_{21} = 0$. $a_{23} = 9$. Baris ke-2: $(0, 5, 9)$. Kolom ke-3: $(1, 9)$.

??? success "Soal 4"
    $a_{32}$ tidak ada karena matriks hanya memiliki 2 baris. $a_{24}$ tidak ada karena matriks hanya memiliki 3 kolom. Indeks harus berada dalam rentang $1 \leq i \leq 2$ dan $1 \leq j \leq 3$.

??? success "Soal 5"
    $X = \begin{bmatrix} 80 & 90 \\ 70 & 60 \\ 95 & 85 \end{bmatrix}$, ukuran $3 \times 2$. $x_{32} = 85$: nilai siswa ketiga (Cici) pada mata pelajaran kedua.

??? success "Soal 6"
    $a_{23}$ adalah `M[1][2]` (bernilai 9). Banyak baris `len(M)` $= 2$. Banyak kolom `len(M[0])` $= 3$.

??? success "Soal 7"
    $(3, 2)$ adalah titik 3 ke kanan dan 2 ke atas dari titik asal. $(3, 2, 5)$ memiliki tiga elemen sehingga membutuhkan tiga sumbu. Bidang hanya punya dua sumbu. $(3, 2) \neq (2, 3)$ karena urutan penting; keduanya titik yang berbeda.

??? success "Soal 8"
    $2(80) + 10(3) + (-1)(10) + 20 = 160 + 30 - 10 + 20 = 200$.

??? success "Soal 9"
    Ukuran $1000 \times 20$. Banyak elemen $1000 \times 20 = 20.000$. Vektor label berdimensi $1000$ (satu nilai per sampel).

??? success "Soal 10"
    Ukuran matriks ditulis baris dulu, jadi $3 \times 4$ berarti **3 baris dan 4 kolom**. Siswa itu menukar urutannya.

## Ringkasan Materi

- Aljabar linear mempelajari vektor, matriks, dan operasi linear; ia adalah bahasa data dan model Machine Learning.
- **Skalar** adalah satu bilangan, ditulis dengan huruf miring biasa.
- **Vektor** adalah daftar bilangan berurutan, ditulis dengan huruf kecil tebal; elemennya $v_i$; urutan penting; secara default vektor kolom.
- **Matriks** adalah tabel bilangan, ditulis dengan huruf kapital; ukurannya baris $\times$ kolom; elemennya $a_{ij}$ (baris dulu, kolom kedua).
- Dimensi vektor adalah banyak elemen; ukuran matriks adalah baris $\times$ kolom; vektor kolom adalah matriks $n \times 1$.
- Dalam ML: satu sampel adalah vektor, seluruh data adalah matriks $n \times d$, labelnya vektor $n$ elemen.
- Python memakai `list` dan menomori dari 0.

## Checklist Penguasaan

- [ ] Membedakan skalar, vektor, dan matriks dengan contoh.
- [ ] Menyebut dimensi sebuah vektor dan ukuran sebuah matriks.
- [ ] Membaca elemen $a_{ij}$ dengan urutan indeks yang benar.
- [ ] Menjelaskan perbedaan dua arti kata "dimensi".
- [ ] Menyusun tabel data menjadi matriks dengan baris dan kolom yang konsisten.
- [ ] Menjelaskan mengapa vektor dapat dipandang sebagai titik dan sebagai panah.
- [ ] Menerjemahkan indeks matematika ke indeks Python.

## Prasyarat untuk Materi Berikutnya

[Part 2 — Vektor](02-vektor.md) membutuhkan: notasi vektor dan elemennya, dimensi, serta pandangan vektor sebagai panah. [Part 3 — Matriks](03-matriks.md) membutuhkan: ukuran matriks dan elemen $a_{ij}$.
