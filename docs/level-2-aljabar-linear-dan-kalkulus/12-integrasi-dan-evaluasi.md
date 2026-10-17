# Part 12 — Integrasi dan Evaluasi Level 2

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- merangkum konsep dan rumus utama aljabar linear dan kalkulus yang dipelajari di Level 2;
- menghubungkan vektor, matriks, turunan, gradien, dan gradient descent dalam satu alur kerja regresi;
- menyelesaikan latihan bertingkat (dasar, menengah, terpadu) yang menggabungkan beberapa part;
- menjelaskan alasan di balik konsep, bukan hanya menghitung;
- menilai kesiapan untuk Level 3 dan mengetahui bagian mana yang perlu diulang.

## Prasyarat

[Part 1](01-pengantar-aljabar-linear.md) sampai [Part 11](11-optimasi-dan-gradient-descent.md). Eigen (di Part 5) dan integral (Part 10) boleh dikuasai pada tingkat pengantar.

## Mengapa Materi Ini Penting?

Konsep yang dipelajari terpisah baru benar-benar berguna saat disatukan. Dalam Machine Learning, satu iterasi pelatihan memakai hampir semuanya sekaligus: data disimpan sebagai matriks, prediksi dihitung dengan perkalian matriks, kerugian diukur, gradiennya dihitung dengan turunan parsial dan aturan rantai, lalu parameter diperbarui dengan gradient descent. Part ini mengikat semuanya dan memeriksa apakah kamu memahami alasannya.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Peta Level 2

**Jalur aljabar linear (Part 1 sampai 5):**

| Part | Konsep inti | Rumus atau gagasan kunci |
|---|---|---|
| 1 | skalar, vektor, matriks, dimensi | $\mathbf{x} \in \mathbb{R}^n$, $A \in \mathbb{R}^{m \times n}$, elemen $a_{ij}$ |
| 2 | operasi vektor, norma, jarak, dot product | $\mathbf{a}\cdot\mathbf{b} = \sum a_i b_i$, $\cos\theta = \dfrac{\mathbf{a}\cdot\mathbf{b}}{\lVert\mathbf{a}\rVert\lVert\mathbf{b}\rVert}$ |
| 3 | operasi matriks | $(AB)_{ij} = \sum_k a_{ik}b_{kj}$, $\det$, $A^{-1}$ ($2 \times 2$) |
| 4 | sistem linear | $A\mathbf{x} = \mathbf{b}$, eliminasi Gauss, tiga jenis solusi |
| 5 | transformasi, span, independensi | kolom $A$ = gambar basis; $\lvert\det A\rvert$ faktor luas; rank |

**Jalur kalkulus (Part 6 sampai 11):**

| Part | Konsep inti | Rumus atau gagasan kunci |
|---|---|---|
| 6 | limit dan kontinuitas | $\lim_{x\to a}f(x)$, kontinu: limit $= f(a)$ |
| 7 | turunan | $f'(a) = \lim_{h\to 0}\frac{f(a+h)-f(a)}{h}$, aturan rantai, $\sigma' = \sigma(1-\sigma)$ |
| 8 | turunan parsial dan gradien | $\nabla f$ menunjuk kenaikan tercuram; $\frac{2}{n}X^{\top}(X\mathbf{w}-\mathbf{y})$ |
| 9 | backpropagation | aturan rantai pada graf: forward, lalu backward |
| 10 | integral | $\int_a^b f\,dx = F(b)-F(a)$, luas bertanda |
| 11 | gradient descent | $\theta \leftarrow \theta - \eta\nabla L(\theta)$, $0<\eta<2/L''$ |

### 2. Satu Alur Terpadu: Regresi Linear dari Awal sampai Akhir

Kita pakai data dan model yang sama sejak Level 1: titik $(1, 5)$, $(3, 9)$, $(2, 8)$ dan model $\hat{y} = wx + b$. Berikut setiap langkah beserta part asalnya.

**Langkah 1: susun data sebagai matriks (Part 1).** Kolom pertama fitur $x$, kolom kedua angka 1 untuk bias:

$$
X = \begin{bmatrix} 1 & 1 \\ 3 & 1 \\ 2 & 1 \end{bmatrix}, \qquad
\mathbf{y} = \begin{bmatrix} 5 \\ 9 \\ 8 \end{bmatrix}, \qquad
\mathbf{w} = \begin{bmatrix} w \\ b \end{bmatrix}
$$

**Langkah 2: prediksi dengan perkalian matriks (Part 3).** $\hat{\mathbf{y}} = X\mathbf{w}$. Untuk $\mathbf{w} = (0, 0)$: $\hat{\mathbf{y}} = (0, 0, 0)$.

**Langkah 3: ukur kerugian (Part 11).** Residu $\mathbf{r} = X\mathbf{w} - \mathbf{y} = (-5, -9, -8)$ dan $L = \dfrac{1}{3}\lVert \mathbf{r} \rVert_2^2 = \dfrac{25 + 81 + 64}{3} \approx 56{,}67$ (norma dari Part 2).

**Langkah 4: gradien (Part 8, memakai turunan dari Part 7).** $\nabla L = \dfrac{2}{3}X^{\top}\mathbf{r} = \dfrac{2}{3}(-48, -22) = (-32;\ -14{,}67)$ (transpose dari Part 3).

**Langkah 5: pembaruan (Part 11).** Dengan $\eta = 0{,}05$: $\mathbf{w} \leftarrow \mathbf{w} - \eta\nabla L = (1{,}6;\ 0{,}7333)$. Kerugian turun menjadi $11{,}89$. Ulangi sampai gradien hampir nol: hasil akhirnya $(w, b) = (2;\ 3{,}3333)$ dengan $L = 0{,}2222$.

**Langkah 6: periksa dengan solusi tepat (Part 3 dan 4).** Persamaan normal $(X^{\top}X)\mathbf{w} = X^{\top}\mathbf{y}$ menghasilkan sistem $14w + 6b = 48$ dan $6w + 3b = 22$. Eliminasi Gauss: $R_2 \leftarrow 7R_2 - 3R_1$ memberi $0 \cdot w + 3b = 10$ (karena $7 \cdot 3 - 3 \cdot 6 = 3$ dan $7 \cdot 22 - 3 \cdot 48 = 10$), jadi $b = \dfrac{10}{3}$. Substitusi balik: $14w + 20 = 48$, sehingga $w = 2$. Sama dengan hasil gradient descent.

**Langkah 7: garis hasil.** $\hat{y} = 2x + 3{,}333$. Pada $x = 1$: $5{,}333$ (data 5), $x = 3$: $9{,}333$ (data 9), $x = 2$: $7{,}333$ (data 8). Tidak ada garis yang melewati ketiga titik, tetapi garis ini adalah yang paling dekat menurut kerugian kuadrat.

Untuk jaringan saraf, hanya langkah 2 dan 4 yang berubah: prediksi memakai banyak lapisan, dan gradien dihitung dengan backpropagation (Part 9). Langkah lain sama.

### 3. Di Mana Setiap Konsep Muncul dalam ML

| Konsep Level 2 | Muncul di Machine Learning |
|---|---|
| vektor dan dot product | prediksi $\hat{y} = \mathbf{w}\cdot\mathbf{x} + b$, kemiripan kosinus |
| norma dan jarak | k-nearest neighbors, regularisasi, hentikan iterasi |
| perkalian matriks | prediksi seluruh data, lapisan jaringan |
| sistem linear, eliminasi | persamaan normal, penentuan parameter |
| rank dan independensi | fitur duplikat membuat parameter tidak unik |
| nilai eigen (pengayaan) | PCA (Level 4) |
| limit dan kontinuitas | mulus tidaknya fungsi kerugian dan aktivasi |
| turunan | arah dan laju perbaikan parameter |
| gradien | arah penurunan tercuram pada banyak parameter |
| aturan rantai | backpropagation |
| integral | peluang kontinu, nilai harapan (Level 3), AUC |
| gradient descent | pelatihan hampir semua model |

### 4. Kebiasaan yang Perlu Dibawa ke Level Berikutnya

- **Periksa dimensi dan ukuran** sebelum menjumlah atau mengalikan vektor dan matriks.
- **Periksa hasil hitungan dengan cara lain**: substitusi, turunan numerik, atau kasus kecil yang dapat dihitung manual.
- **Pantau kerugian** saat melatih. Jika naik, curigai laju belajar atau gradien yang salah.
- **Pisahkan "konsep" dari "kode":** pahami rumus dulu, baru implementasi.
- **Curiga pada kesimpulan terlalu cepat**: akurasi tinggi belum berarti model baik, gradien nol belum berarti minimum, kerugian kecil pada data latih belum berarti baik pada data baru.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Dari part |
|---|---|---|
| Matriks fitur $X$ | baris = data, kolom = fitur | 1, 3 |
| Residu | prediksi dikurangi nilai sebenarnya | 8, 11 |
| Persamaan normal | $(X^{\top}X)\mathbf{w} = X^{\top}\mathbf{y}$ | 4, 11 |
| Gradien | vektor turunan parsial | 8 |
| Backpropagation | aturan rantai dari belakang ke depan | 9 |
| Laju belajar | besar langkah gradient descent | 11 |
| Konvergen | mendekati minimum | 11 |
| Singular | determinan nol, tidak punya invers | 3, 4, 5 |
| Rank | banyak kolom independen | 4, 5 |

## Rumus dan Penjelasan Setiap Simbol

Rumus-rumus utama sudah dijelaskan per part (lihat tabel di bagian 1). Untuk alur terpadu, empat rumus yang dipakai bersama:

| Langkah | Rumus | Syarat dan arti simbol |
|---|---|---|
| Prediksi | $\hat{\mathbf{y}} = X\mathbf{w}$ | $X \in \mathbb{R}^{n \times d}$, $\mathbf{w} \in \mathbb{R}^d$ |
| Kerugian | $L = \dfrac{1}{n}\lVert X\mathbf{w} - \mathbf{y} \rVert_2^2$ | rata-rata kuadrat residu |
| Gradien | $\nabla L = \dfrac{2}{n}X^{\top}(X\mathbf{w} - \mathbf{y})$ | vektor $d$ elemen |
| Pembaruan | $\mathbf{w} \leftarrow \mathbf{w} - \eta\,\nabla L$ | $\eta > 0$, aman bila $\eta < 2/\lambda_{\max}$ untuk turunan kedua terbesar $\lambda_{\max}$ |
| Solusi tepat | $(X^{\top}X)\mathbf{w} = X^{\top}\mathbf{y}$ | $X^{\top}X$ punya invers |

## Penjelasan Langkah demi Langkah

**Menyelesaikan soal terpadu (strategi umum):**

1. **Baca soal dan petakan** ke part: apakah ini vektor, matriks, sistem, turunan, gradien, integral, atau optimasi?
2. **Tulis yang diketahui** (data, parameter, dimensi) dan yang ditanyakan.
3. **Pilih rumus** dan periksa syaratnya (dimensi cocok, penyebut tidak nol, $\det \neq 0$, dan sebagainya).
4. **Hitung langkah demi langkah** dengan satu operasi per baris.
5. **Periksa dengan cara lain** (substitusi, tabel, numerik, atau kasus khusus).
6. **Tafsirkan hasil** dalam kalimat dan sebut satuannya.

## Contoh Soal dan Pembahasan

### Contoh 1 — Terpadu: matriks, gradien, dan satu langkah

Dengan data $(1, 5)$, $(3, 9)$, $(2, 8)$ dan $\mathbf{w} = (1, 1)$: hitung prediksi, kerugian, gradien, dan satu langkah $\eta = 0{,}05$.

1. $\hat{\mathbf{y}} = X\mathbf{w} = (1 + 1,\ 3 + 1,\ 2 + 1) = (2, 4, 3)$.
2. Residu $= (2 - 5,\ 4 - 9,\ 3 - 8) = (-3, -5, -5)$. $L = \dfrac{9 + 25 + 25}{3} = 19{,}667$.
3. $X^{\top}\mathbf{r} = (1(-3) + 3(-5) + 2(-5),\ -3 - 5 - 5) = (-28, -13)$. $\nabla L = \dfrac{2}{3}(-28, -13) = (-18{,}667;\ -8{,}667)$.
4. $\mathbf{w}_{\text{baru}} = (1 + 0{,}9333,\ 1 + 0{,}4333) = (1{,}9333;\ 1{,}4333)$.
5. Kerugian baru $\approx 4{,}36$. Turun dari $19{,}67$.

### Contoh 2 — Menyelesaikan sistem dan mengaitkannya dengan independensi

Selesaikan $x + y + z = 6$, $x - y + z = 2$, $2x + y - z = 1$. Apakah kolom-kolom matriks koefisien independen?

1. Matriks diperluas: $\left[\begin{array}{ccc|c} 1 & 1 & 1 & 6 \\ 1 & -1 & 1 & 2 \\ 2 & 1 & -1 & 1 \end{array}\right]$.
2. $R_2 \leftarrow R_2 - R_1$: $\begin{bmatrix} 0 & -2 & 0 & -4 \end{bmatrix}$. $R_3 \leftarrow R_3 - 2R_1$: $\begin{bmatrix} 0 & -1 & -3 & -11 \end{bmatrix}$.
3. Dari baris 2: $y = 2$. Baris 3: $-2 - 3z = -11$, jadi $z = 3$. Baris 1: $x = 6 - 2 - 3 = 1$. Solusi $(1, 2, 3)$.
4. Pivot ada tiga (lengkap), jadi kolom-kolomnya independen, matriksnya punya invers, dan solusinya tunggal untuk setiap ruas kanan.

### Contoh 3 — Turunan dan gradient descent satu parameter

Untuk $L(w) = 5(w - 2)^2$, tentukan $\eta$ yang aman dan $\eta$ yang tepat sasaran, lalu jalankan satu langkah dari $w_0 = 7$.

1. $L'(w) = 10(w - 2)$, $L'' = 10$.
2. Aman: $0 < \eta < \dfrac{2}{10} = 0{,}2$. Tepat sasaran: $\eta = \dfrac{1}{10} = 0{,}1$.
3. Dengan $\eta = 0{,}1$: $w_1 = 7 - 0{,}1 \cdot 50 = 2$. Tepat di minimum.

### Contoh 4 — Integral dan peluang

Fungsi $f(x) = 3x^2$ pada $[0, 1]$: periksa sebagai kepadatan, hitung $P(X \leq 0{,}5)$ dan rata-rata $\int x f(x)\,dx$.

1. $\int_0^1 3x^2\,dx = \bigl[x^3\bigr]_0^1 = 1$. Sah.
2. $P(X \leq 0{,}5) = \bigl[x^3\bigr]_0^{0{,}5} = 0{,}125$.
3. Rata-rata: $\int_0^1 x \cdot 3x^2\,dx = \left[\dfrac{3x^4}{4}\right]_0^1 = \dfrac{3}{4}$.
4. Masuk akal: kepadatan lebih besar di dekat 1, jadi rata-rata lebih dari $0{,}5$.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** Prediksi $\hat{y} = \mathbf{w}\cdot\mathbf{x} + b$ dengan $\mathbf{w} = (2, 1)$, $\mathbf{x} = (3, 4)$, $b = 1$: $6 + 4 + 1 = 11$.

**Sedang.** Gradien $f(x, y) = x^2 + y^2$ di $(3, -1)$ adalah $(6, -2)$. Satu langkah gradient descent dengan $\eta = 0{,}1$: $(3 - 0{,}6,\ -1 + 0{,}2) = (2{,}4;\ -0{,}8)$, yang lebih dekat ke titik asal.

**Sedang.** Neuron sigmoid: $x = 2$, $w = 0{,}5$, $b = -1$, $y = 1$: gradien $\dfrac{\partial L}{\partial w} = -0{,}5$, $\dfrac{\partial L}{\partial b} = -0{,}25$, dan satu langkah $\eta = 0{,}1$ memberi $w = 0{,}55$, $b = -0{,}975$ (Part 9).

**Menantang.** Fitur duplikat: jika kolom kedua $X$ adalah dua kali kolom pertama, $X^{\top}X = \begin{bmatrix} 14 & 28 \\ 28 & 56 \end{bmatrix}$ untuk $x = (1, 2, 3)$, dengan $\det = 14 \cdot 56 - 28^2 = 0$. Singular: persamaan normal tidak punya solusi tunggal. Gradient descent tetap berjalan, tetapi memberi salah satu dari banyak parameter yang sama baiknya.

## Visualisasi atau Interpretasi Grafik

Visualisasi kunci Level 2, dan di part mana masing-masing dapat dibaca:

- **Panah vektor dan jajar genjang** (Part 2, Part 5): penjumlahan, sudut, dan faktor luas.
- **Garis singgung** (Part 7): turunan sebagai kemiringan.
- **Peta kontur dan gradien** (Part 8): gradien tegak lurus kontur.
- **Graf komputasi** (Part 9): forward dan backward pass.
- **Batang Riemann** (Part 10): integral sebagai luas.
- **Kurva kerugian per iterasi** (Part 11): turun mulus, lambat, atau naik.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Mengalikan matriks tanpa memeriksa ukuran | kolom pertama harus sama dengan baris kedua | periksa $(m \times n)(n \times p)$ |
| Menyimpulkan $AB = BA$ | tidak komutatif | hitung keduanya |
| Mencari invers saat $\det = 0$ | tidak ada | periksa determinan |
| Menulis $\lim$ sama dengan $f(a)$ selalu | hanya bila kontinu | periksa tiga syarat |
| Lupa aturan rantai pada fungsi bersusun | turunan salah faktor | turunan luar kali turunan dalam |
| Menaikkan parameter searah gradien untuk menurunkan kerugian | itu menaikkan | $-\nabla L$ |
| Menganggap gradien nol pasti minimum | bisa pelana | periksa turunan kedua atau bentuk fungsi |
| Laju belajar terlalu besar | divergen | $0 < \eta < 2/L''$ pada kuadrat |
| Menulis $+ C$ pada integral tentu | konstanta saling menghapus | $F(b) - F(a)$ |
| Mengira kerugian harus nol | minimum bisa positif | bandingkan dengan nilai minimum |
| Mengabaikan fitur yang bergantung linear | parameter tidak unik | periksa rank |

## Hubungan dengan Machine Learning

- **Seluruh pelatihan model linear adalah Level 2.** Data (matriks), prediksi (perkalian matriks), kerugian (norma), gradien (turunan parsial), dan pembaruan (gradient descent), dengan persamaan normal (eliminasi Gauss) sebagai pembanding.
- **Jaringan saraf menambahkan dua hal:** banyak lapisan (aturan rantai dan backpropagation) dan fungsi aktivasi nonlinear (sigmoid dan ReLU, dengan turunan lokalnya).
- **Level 3** menambahkan ketidakpastian: peluang, distribusi, nilai harapan. Integral dan logaritma dari Level 1 dan 2 dipakai terus.
- **Level 4** menerapkan semuanya pada algoritma klasik (regresi, regresi logistik, PCA). **Level 5** memperluasnya ke bentuk matriks lengkap untuk deep learning.

### Python: alur terpadu regresi linear

```python
def dot(a, b):
    return sum(ai * bi for ai, bi in zip(a, b))

X = [[1, 1], [3, 1], [2, 1]]
y = [5, 9, 8]
n = len(y)

def prediksi(w):
    return [dot(baris, w) for baris in X]

def kerugian(w):
    return sum((p - yi) ** 2 for p, yi in zip(prediksi(w), y)) / n

def gradien(w):
    r = [p - yi for p, yi in zip(prediksi(w), y)]
    return [2 / n * sum(X[i][j] * r[i] for i in range(n)) for j in range(2)]
```

Satu iterasi dari $\mathbf{w} = (0, 0)$:

```python
w = [0.0, 0.0]
print(prediksi(w), round(kerugian(w), 4))      # [0.0, 0.0, 0.0] 56.6667
g = gradien(w)
print([round(v, 4) for v in g])                # [-32.0, -14.6667]
w = [wj - 0.05 * gj for wj, gj in zip(w, g)]
print([round(v, 4) for v in w], round(kerugian(w), 4))   # [1.6, 0.7333] 11.8889
```

Iterasi sampai konvergen, lalu membandingkan dengan solusi tepat:

```python
w = [0.0, 0.0]
for _ in range(2000):
    g = gradien(w)
    w = [wj - 0.05 * gj for wj, gj in zip(w, g)]
print([round(v, 4) for v in w], round(kerugian(w), 4))   # [2.0, 3.3333] 0.2222

# persamaan normal dengan eliminasi Gauss pada sistem 2 x 2
from fractions import Fraction
A = [[Fraction(14), Fraction(6), Fraction(48)],
     [Fraction(6), Fraction(3), Fraction(22)]]
A[1] = [7 * a - 3 * b for a, b in zip(A[1], A[0])]    # R2 <- 7*R2 - 3*R1
b_tepat = A[1][2] / A[1][1]
w_tepat = (A[0][2] - A[0][1] * b_tepat) / A[0][0]
print(w_tepat, b_tepat)                                # 2 10/3
```

Memeriksa gradien rumus terhadap gradien numerik (kebiasaan baik):

```python
def gradien_numerik(w, h=1e-6):
    hasil = []
    for j in range(2):
        a, b = list(w), list(w)
        a[j] += h
        b[j] -= h
        hasil.append((kerugian(a) - kerugian(b)) / (2 * h))
    return hasil

titik = [1.0, 1.0]
print([round(v, 4) for v in gradien(titik)])          # [-18.6667, -8.6667]
print([round(v, 4) for v in gradien_numerik(titik)])  # [-18.6667, -8.6667]
```

Fitur duplikat membuat prediksi tidak berubah walaupun bobot berbeda:

```python
Xd = [[1, 2], [2, 4], [3, 6]]
for w in ([2, 0], [0, 1]):
    print([dot(baris, w) for baris in Xd])            # [2, 4, 6] untuk keduanya
```

## Latihan Campuran Bertingkat

### Tingkat 1 — Dasar

1. Untuk $\mathbf{u} = (2, -1, 3)$ dan $\mathbf{v} = (1, 4, -2)$ hitung $\mathbf{u} + \mathbf{v}$, $2\mathbf{u} - \mathbf{v}$, $\mathbf{u}\cdot\mathbf{v}$, $\lVert\mathbf{u}\rVert_2$, dan $\lVert\mathbf{v}\rVert_1$.
2. Untuk $A = \begin{bmatrix} 2 & 1 \\ 0 & 3 \end{bmatrix}$ dan $B = \begin{bmatrix} 1 & 0 \\ 4 & 1 \end{bmatrix}$ hitung $AB$, $\det A$, dan $A^{-1}$ (periksa $AA^{-1} = I$).
3. Selesaikan $2x + y = 7$ dan $x - y = 2$ dengan eliminasi Gauss.
4. Hitung: (a) $\lim_{x \to 2}\dfrac{x^2 - 4}{x - 2}$ (b) $\lim_{x \to \infty}\dfrac{3x + 1}{x}$
5. Turunkan: (a) $x^3 - 4x^2 + x$ (b) $e^{2x}$ (c) $\ln(3x)$
6. Hitung $\int (6x^2 + 2)\,dx$ dan $\int_0^3 2x\,dx$.
7. Hitung gradien $f(x, y) = x^2 y + y$ di $(2, 3)$.
8. Satu langkah gradient descent untuk $L(w) = (w - 5)^2$ dari $w = 1$ dengan $\eta = 0{,}25$.

### Tingkat 2 — Menengah

1. Hitung cosine similarity $\mathbf{a} = (1, 2, 2)$ dan $\mathbf{b} = (3, 0, 4)$.
2. Matriks rotasi $90^\circ$ $\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ diterapkan pada $(3, 1)$: hasil dan determinannya? Berapa nilai eigen $\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$?
3. Selesaikan $x + y + z = 6$, $x - y + z = 2$, $2x + y - z = 1$.
4. Untuk $A = \begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix}$: tentukan determinan, rank, dan jenis solusi $A\mathbf{x} = \mathbf{b}$ untuk $\mathbf{b} = (3, 6)$ dan $\mathbf{b} = (3, 7)$.
5. Turunkan $\sqrt{1 + x^2}$ di $x = 1$, dan tunjukkan bahwa $\dfrac{d}{dx}\ln(1 + e^x) = \sigma(x)$; hitung nilainya di $x = 0$.
6. Untuk $f(x, y) = x^2 + 2y^2$ di $(1, 1)$: gradien, turunan berarah ke $(1, 0)$, dan laju kenaikan terbesar. Di mana titik kritisnya, dan jenisnya?
7. Hitung $\int_0^2 x^2\,dx$ dengan aturan trapesium $n = 4$ dan bandingkan dengan nilai eksak.
8. Neuron sigmoid dengan $x = 1$, $w = 0$, $b = 0$, target $y = 1$: hitung $\dfrac{\partial L}{\partial w}$ dan $\dfrac{\partial L}{\partial b}$ dengan backpropagation, lalu perbarui dengan $\eta = 1$.

### Tingkat 3 — Terpadu

1. Pada data $(1, 5)$, $(3, 9)$, $(2, 8)$, dari $\mathbf{w} = (1, 1)$ hitung gradien dan satu langkah $\eta = 0{,}05$ serta kerugian sebelum dan sesudahnya.
2. Selesaikan persamaan normal $14w + 6b = 48$, $6w + 3b = 22$ dengan eliminasi Gauss ($R_2 \leftarrow 7R_2 - 3R_1$).
3. Untuk $L(w) = (w - 3)^2$: untuk $\eta = 1{,}2$ hitung dua langkah dari $w_0 = 0$ dan tentukan apakah konvergen.
4. Tunjukkan bahwa matriks fitur dengan kolom $x = (1, 2, 3)$ dan $2x$ menghasilkan $X^{\top}X$ singular, dan jelaskan akibatnya bagi parameter.
5. $f(x) = 3x^2$ pada $[0, 1]$: periksa sebagai kepadatan, hitung $P(X \leq 0{,}5)$ dan rata-rata $\int x f(x)\,dx$.
6. Neuron $x = 2$, $w = 0{,}5$, $b = -1$, $y = 1$: lakukan satu siklus lengkap (forward, backward, update dengan $\eta = 0{,}1$) dan buktikan kerugian turun.

## Kunci Jawaban dan Pembahasan

??? success "Tingkat 1"
    1. $\mathbf{u} + \mathbf{v} = (3, 3, 1)$. $2\mathbf{u} - \mathbf{v} = (4 - 1,\ -2 - 4,\ 6 + 2) = (3, -6, 8)$. $\mathbf{u}\cdot\mathbf{v} = 2 - 4 - 6 = -8$. $\lVert\mathbf{u}\rVert_2 = \sqrt{14} \approx 3{,}742$. $\lVert\mathbf{v}\rVert_1 = 1 + 4 + 2 = 7$.
    2. $AB = \begin{bmatrix} 6 & 1 \\ 12 & 3 \end{bmatrix}$. $\det A = 6$. $A^{-1} = \dfrac{1}{6}\begin{bmatrix} 3 & -1 \\ 0 & 2 \end{bmatrix}$. Periksa: baris 1 $\times$ kolom 1: $2 \cdot \tfrac{3}{6} + 0 = 1$; baris 1 $\times$ kolom 2: $2 \cdot (-\tfrac{1}{6}) + 1 \cdot \tfrac{2}{6} = 0$; hasil $I$.
    3. $R_2 \leftarrow 2R_2 - R_1$: $\begin{bmatrix} 0 & -3 & -3 \end{bmatrix}$, jadi $y = 1$ dan $x = 3$.
    4. (a) $\dfrac{(x-2)(x+2)}{x-2} = x + 2 \to 4$. (b) $3 + \dfrac{1}{x} \to 3$.
    5. (a) $3x^2 - 8x + 1$. (b) $2e^{2x}$. (c) $\dfrac{1}{3x} \cdot 3 = \dfrac{1}{x}$.
    6. $2x^3 + 2x + C$; $\bigl[x^2\bigr]_0^3 = 9$.
    7. $\nabla f = (2xy,\ x^2 + 1) = (12, 5)$.
    8. $L'(1) = 2(1 - 5) = -8$, $w = 1 - 0{,}25(-8) = 3$.

??? success "Tingkat 2"
    1. $\mathbf{a}\cdot\mathbf{b} = 3 + 0 + 8 = 11$, $\lVert\mathbf{a}\rVert = 3$, $\lVert\mathbf{b}\rVert = 5$, jadi $\cos\theta = \dfrac{11}{15} \approx 0{,}7333$.
    2. $A(3, 1) = (-1, 3)$, $\det = 1$. Nilai eigen matriks diagonal: $2$ dan $3$ (vektor eigen $\mathbf{e}_1$, $\mathbf{e}_2$).
    3. Solusi $(1, 2, 3)$ (lihat Contoh 2).
    4. $\det = 4 - 4 = 0$, rank $1$. Untuk $\mathbf{b} = (3, 6)$: $R_2 - 2R_1 = (0, 0, 0)$, banyak solusi. Untuk $\mathbf{b} = (3, 7)$: $(0, 0, 1)$, tidak ada solusi.
    5. $\dfrac{x}{\sqrt{1 + x^2}}$, di $x = 1$: $\dfrac{1}{\sqrt{2}} \approx 0{,}7071$. $\bigl(\ln(1 + e^x)\bigr)' = \dfrac{e^x}{1 + e^x} = \sigma(x)$; di $x = 0$: $0{,}5$.
    6. $\nabla f = (2x, 4y) = (2, 4)$. Turunan berarah ke $(1, 0)$: $2$. Laju terbesar $\sqrt{20} \approx 4{,}472$. $\nabla f = \mathbf{0}$ di $(0, 0)$, minimum (nilai $0$, karena $f \geq 0$).
    7. Trapesium: $\dfrac{1{,}75 + 3{,}75}{2} = 2{,}75$. Eksak $\tfrac{8}{3} \approx 2{,}667$.
    8. $z = 0$, $a = 0{,}5$, $L = 0{,}25$. $\dfrac{\partial L}{\partial w} = 2(a - y)\,a(1 - a)\,x = 2(-0{,}5)(0{,}25)(1) = -0{,}25$ dan $\dfrac{\partial L}{\partial b} = -0{,}25$. Dengan $\eta = 1$: $w = 0{,}25$, $b = 0{,}25$.

??? success "Tingkat 3"
    1. Lihat Contoh 1: gradien $(-18{,}667;\ -8{,}667)$, $\mathbf{w} = (1{,}9333;\ 1{,}4333)$, kerugian $19{,}667 \to 4{,}36$.
    2. $R_2 \leftarrow 7R_2 - 3R_1$: $0 \cdot w + 3b = 10$, jadi $b = \tfrac{10}{3}$. Lalu $14w + 6 \cdot \tfrac{10}{3} = 48$, $14w = 28$, $w = 2$.
    3. $L' = 2(w - 3)$. $w_1 = 0 - 1{,}2(-6) = 7{,}2$. $w_2 = 7{,}2 - 1{,}2 \cdot 2 \cdot 4{,}2 = 7{,}2 - 10{,}08 = -2{,}88$. Jarak ke minimum: $3, 4{,}2, 5{,}88$, membesar $1{,}4$ kali. **Divergen**, karena $\eta = 1{,}2 > 1 = 2/L''$.
    4. $X^{\top}X = \begin{bmatrix} 14 & 28 \\ 28 & 56 \end{bmatrix}$, $\det = 784 - 784 = 0$: singular. Akibatnya persamaan normal tidak punya solusi tunggal; banyak bobot memberi prediksi sama (kolom bergantung linear).
    5. $\int_0^1 3x^2\,dx = 1$, sah. $P(X \leq 0{,}5) = 0{,}125$. Rata-rata $\dfrac{3}{4}$.
    6. Forward $z = 0$, $a = 0{,}5$, $L = 0{,}25$. Backward $\dfrac{\partial L}{\partial w} = -0{,}5$, $\dfrac{\partial L}{\partial b} = -0{,}25$. Update $w = 0{,}55$, $b = -0{,}975$. Forward baru: $z = 0{,}125$, $a \approx 0{,}5312$, $L \approx 0{,}2198 < 0{,}25$.

## Evaluasi Akhir Level 2

Evaluasi ini menilai **pemahaman alasan**. Jawab dengan kalimatmu sendiri lalu bandingkan dengan jawaban contoh.

1. Mengapa $A\mathbf{x}$ dapat dipandang sebagai kombinasi linear kolom-kolom $A$, dan apa artinya untuk sistem $A\mathbf{x} = \mathbf{b}$?
2. Apa arti determinan nol, secara geometri dan untuk sistem persamaan?
3. Apa beda vektor independen dan bergantung linear, dan mengapa fitur yang bergantung bermasalah dalam Machine Learning?
4. Apa beda limit dari nilai fungsi di sebuah titik? Mengapa limit dibutuhkan untuk mendefinisikan turunan?
5. Apa arti turunan pertama dan turunan kedua, dan bagaimana keduanya dipakai mencari minimum?
6. Mengapa gradien menunjuk arah kenaikan tercuram? (Petunjuk: dot product.)
7. Mengapa backpropagation menghitung dari belakang ke depan, dan apa yang disimpan saat forward pass?
8. Apa hubungan integral dan turunan, dan mengapa integral muncul dalam peluang?
9. Jelaskan gradient descent dan pengaruh laju belajar. Mengapa mangkuk yang lebih sempit membutuhkan $\eta$ lebih kecil?
10. Bandingkan persamaan normal dan gradient descent: kapan masing-masing lebih cocok?

??? success "Jawaban contoh evaluasi"
    1. Baris dikali kolom memberi $A\mathbf{x} = x_1\mathbf{a}_1 + x_2\mathbf{a}_2 + \cdots$ (kolom $A$ dengan koefisien elemen $\mathbf{x}$). Jadi $A\mathbf{x} = \mathbf{b}$ menanyakan apakah $\mathbf{b}$ dapat dibentuk dari kolom-kolom $A$, yaitu apakah $\mathbf{b}$ berada di span kolom; koefisiennya adalah solusi.
    2. Geometri: transformasi meratakan bidang (luas menjadi nol), sehingga informasi hilang dan tidak dapat dibalik. Sistem: tidak ada invers; solusinya tidak ada atau tak tunggal (banyak).
    3. Independen: tidak ada vektor yang merupakan kombinasi lainnya; bergantung: ada. Fitur bergantung membuat kolom $X$ bergantung, $X^{\top}X$ singular, dan banyak bobot memberi prediksi sama, sehingga parameter tidak unik dan interpretasi bobot tidak berarti.
    4. Limit adalah nilai yang didekati fungsi di sekitar titik, tidak peduli nilai di titik itu. Turunan adalah limit kemiringan garis potong ($\frac{0}{0}$ jika disubstitusi langsung), sehingga butuh limit.
    5. Turunan pertama: laju perubahan sesaat (kemiringan garis singgung); nol di titik kritis. Turunan kedua: kelengkungan; positif berarti cekung ke atas (minimum), negatif berarti cekung ke bawah (maksimum).
    6. Turunan berarah $D_{\mathbf{u}}f = \nabla f\cdot\mathbf{u} = \lVert\nabla f\rVert\cos\theta$; nilai terbesar saat $\cos\theta = 1$, yaitu $\mathbf{u}$ searah gradien, dengan laju $\lVert\nabla f\rVert$.
    7. Gradien bagian dalam bergantung pada gradien bagian luar (aturan rantai: turunan luar kali turunan dalam), jadi mulai dari kerugian dan mundur sambil memakai ulang hasil sebelumnya. Yang disimpan: nilai antara dari forward pass (misalnya $z$ dan $a$) karena turunan lokal membutuhkannya.
    8. Teorema dasar kalkulus: $\int_a^b f = F(b) - F(a)$ dengan $F' = f$ (integral dan turunan saling berkebalikan). Peluang peubah kontinu adalah luas di bawah fungsi kepadatan, yaitu integral.
    9. Ulangi $\theta \leftarrow \theta - \eta\nabla L$. Kecil: lambat. Besar: osilasi atau divergen. Mangkuk sempit berarti $L''$ besar sehingga pembaruan mudah melompati dasar; pada kuadrat konvergen bila $\eta < 2/L''$.
    10. Persamaan normal: solusi tepat, cocok untuk model linear kecil dengan $X^{\top}X$ punya invers. Gradient descent: bertahap, cocok untuk data atau model besar, dan untuk model tanpa rumus langsung (misalnya jaringan saraf).

**Cara menilai diri.** Jawaban bernilai penuh jika memuat alasan dan contoh angka, bukan hanya definisi. Targetkan lancar pada minimal 8 dari 10 butir sebelum melanjutkan.

## Persiapan Menuju Level 3

[Level 3 — Probabilitas dan Statistika](../level-3-probabilitas-dan-statistika/index.md) sekarang tersedia. Pemetaan kesiapan:

| Dibutuhkan Level 3 | Berasal dari | Ulangi jika |
|---|---|---|
| Notasi $\sum$, indeks, rata-rata | Level 0 Part 8 | belum lancar menghitung sigma |
| $e^x$ dan $\ln x$ | Level 1 Part 10 | belum paham logaritma sebagai kebalikan pangkat |
| Persentase, rasio, pecahan | Level 0 Part 3 dan 4 | masih salah memakai pecahan |
| Integral dan luas di bawah kurva | Part 10 | belum bisa menghitung integral tentu sederhana |
| Turunan dan titik kritis (untuk penaksiran) | Part 7 | belum bisa mencari titik kritis |
| Vektor dan matriks (distribusi banyak peubah) | Part 1 sampai 3 | belum nyaman dengan dot product dan perkalian matriks |
| Aturan rantai (log-likelihood) | Part 7 dan 9 | belum lancar menurunkan fungsi bersusun |

**Kriteria minimal:** kuasai Part 1 sampai 4 dan Part 6 sampai 8, serta ide dasar Part 9 sampai 11. Eigen (Part 5) dan backpropagation lengkap (Part 9) boleh dipahami secara pengantar. Jika evaluasi akhir belum 8 dari 10, ulangi part terkait terlebih dahulu.

!!! note "Yang belum dibahas di Level 2"
    Turunan matriks dan Jacobian, bentuk matriks backpropagation, optimizer lanjutan (Momentum, Adam), uji turunan kedua dalam bentuk matriks (Hessian), dan integral lipat belum termasuk. Semuanya dibahas di level lanjutan ketika dibutuhkan.

## Ringkasan Materi

- Level 2 menyatukan dua alat: **aljabar linear** (vektor, matriks, sistem, transformasi) dan **kalkulus** (limit, turunan, gradien, integral, optimasi).
- Satu alur terpadu: data $\to$ matriks $X$ $\to$ prediksi $X\mathbf{w}$ $\to$ kerugian $\to$ gradien $\frac{2}{n}X^{\top}(X\mathbf{w}-\mathbf{y})$ $\to$ pembaruan $\mathbf{w} \leftarrow \mathbf{w} - \eta\nabla L$, dengan persamaan normal (eliminasi Gauss) sebagai pembanding.
- Untuk jaringan saraf, gradien dihitung oleh backpropagation (aturan rantai), dan aktivasi menambah nonlinearitas.
- Laju belajar harus sesuai kelengkungan: $0 < \eta < 2/L''$ pada fungsi kuadrat.
- Integral membawa kita ke peluang kontinu dan nilai harapan (Level 3).
- Kebiasaan: periksa dimensi, periksa hasil dengan cara lain, pantau kerugian, dan jelaskan alasan.

## Checklist Penguasaan

- [ ] Menjelaskan dengan kalimat sendiri alur data hingga pembaruan parameter pada regresi linear.
- [ ] Menghitung satu iterasi gradient descent dua parameter dengan matriks.
- [ ] Menyelesaikan persamaan normal dengan eliminasi Gauss dan membandingkannya dengan gradient descent.
- [ ] Mengerjakan Tingkat 1 dan 2 dengan benar, dan sebagian besar Tingkat 3.
- [ ] Menjawab minimal 8 dari 10 butir evaluasi konseptual dengan alasan.
- [ ] Mengetahui bagian Level 2 yang perlu diulang sebelum Level 3.

## Prasyarat untuk Materi Berikutnya

[Level 3 — Probabilitas dan Statistika](../level-3-probabilitas-dan-statistika/index.md) sekarang tersedia. Jika kamu memenuhi kriteria kesiapan di atas, kamu siap melanjutkannya.
