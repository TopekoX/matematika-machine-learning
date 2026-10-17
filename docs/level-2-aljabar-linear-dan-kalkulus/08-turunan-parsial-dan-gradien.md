# Part 8 — Turunan Parsial dan Gradien

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan fungsi dengan banyak masukan (multivariabel) dan membacanya sebagai tabel dan kontur;
- menghitung turunan parsial dengan menganggap variabel lain sebagai konstanta;
- menyusun turunan parsial menjadi vektor gradien $\nabla f$;
- menjelaskan bahwa gradien menunjuk arah kenaikan tercuram dan panjangnya adalah laju kenaikan itu;
- menghitung turunan berarah dengan dot product;
- menemukan titik kritis ($\nabla f = \mathbf{0}$) dan membedakan minimum dari titik pelana;
- menghitung gradien fungsi kerugian terhadap parameter model, termasuk dalam bentuk matriks.

## Prasyarat

[Part 7 — Turunan](07-turunan.md) (aturan turunan dan aturan rantai), [Part 2 — Vektor](02-vektor.md) (dot product, norma, cosine), dan [Level 1 — Part 7](../level-1-aljabar-dasar-dan-fungsi/07-konsep-fungsi.md) (fungsi dengan lebih dari satu variabel). Bentuk matriks di bagian akhir memakai [Part 3](03-matriks.md) (perkalian matriks dan transpose).

## Mengapa Materi Ini Penting?

Model nyata punya banyak parameter: puluhan, jutaan, bahkan miliaran. Kerugian model adalah fungsi dari **semua** parameter itu sekaligus. Untuk memperbaiki model kita perlu tahu, untuk setiap parameter, bagaimana kerugian berubah bila parameter itu digeser sedikit. Kumpulan jawaban itu adalah **gradien**. Gradien adalah vektor yang menunjuk arah kenaikan tercuram, sehingga arah sebaliknya adalah arah penurunan kerugian tercuram. Inilah inti gradient descent (Part 11).

## Konsep Dasar dengan Bahasa Sederhana

### 1. Fungsi Multivariabel

**Fungsi multivariabel** menerima beberapa masukan dan menghasilkan satu keluaran. Contoh:

- $L(p, l) = p \cdot l$: luas dari panjang dan lebar.
- $f(x, y) = x^2 + y^2$: kuadrat jarak dari titik asal.
- Kerugian model: $L(w, b) = (wx + b - y)^2$ dengan $w$ dan $b$ sebagai masukan (parameter).

Secara umum, $f : \mathbb{R}^n \to \mathbb{R}$ menerima vektor $\mathbf{x} = (x_1, \ldots, x_n)$ dan mengembalikan satu bilangan.

Tabel nilai untuk $f(x, y) = x^2 + y^2$:

| $y \backslash x$ | $-2$ | $-1$ | $0$ | $1$ | $2$ |
|---|---|---|---|---|---|
| $2$ | 8 | 5 | 4 | 5 | 8 |
| $1$ | 5 | 2 | 1 | 2 | 5 |
| $0$ | 4 | 1 | 0 | 1 | 4 |
| $-1$ | 5 | 2 | 1 | 2 | 5 |
| $-2$ | 8 | 5 | 4 | 5 | 8 |

Perhatikan bahwa nilai 5 muncul di empat pasang titik di sekitar $(\pm 1, \pm 2)$ dan $(\pm 2, \pm 1)$. Titik-titik dengan nilai yang sama membentuk **garis kontur** (*level curve*), di sini lingkaran berjari-jari $\sqrt{5}$. Nilai terkecil, 0, ada di titik asal (dasar "mangkuk"). Peta kontur adalah cara menggambar fungsi dua masukan tanpa grafik tiga dimensi, seperti peta ketinggian gunung.

### 2. Turunan Parsial

**Turunan parsial** terhadap sebuah variabel adalah turunan biasa (Part 7) dengan **semua variabel lain dianggap konstanta**. Ia mengukur: jika hanya variabel itu yang digeser sedikit, seberapa cepat $f$ berubah?

Notasi: $\dfrac{\partial f}{\partial x}$ (dibaca "parsial $f$ parsial $x$") atau $f_x$. Simbol $\partial$ dipakai agar berbeda dari $d$ pada turunan biasa. Definisi:

$$
\frac{\partial f}{\partial x}(x, y) = \lim_{h \to 0} \frac{f(x + h, y) - f(x, y)}{h}
$$

Hanya $x$ yang berubah ($y$ tetap). Demikian pula $\dfrac{\partial f}{\partial y}$ menggeser $y$ saja.

**Contoh 1:** $f(x, y) = x^2 + 3xy + y^2$.

- $\dfrac{\partial f}{\partial x}$: anggap $y$ konstanta. $(x^2)' = 2x$; $(3xy)' = 3y$ (karena $3y$ adalah pengali konstanta dari $x$); $(y^2)' = 0$. Hasil: $2x + 3y$.
- $\dfrac{\partial f}{\partial y}$: anggap $x$ konstanta. $(x^2)' = 0$; $(3xy)' = 3x$; $(y^2)' = 2y$. Hasil: $3x + 2y$.
- Di $(1, 2)$: $\dfrac{\partial f}{\partial x} = 2 + 6 = 8$ dan $\dfrac{\partial f}{\partial y} = 3 + 4 = 7$.

**Contoh 2:** $f(x, y) = x^2 y$. $\dfrac{\partial f}{\partial x} = 2xy$ (anggap $y$ pengali) dan $\dfrac{\partial f}{\partial y} = x^2$ (anggap $x^2$ pengali). Di $(2, 3)$: $12$ dan $4$.

**Contoh 3:** $f(x, y) = e^{xy}$. Dengan aturan rantai, $\dfrac{\partial f}{\partial x} = y\,e^{xy}$ dan $\dfrac{\partial f}{\partial y} = x\,e^{xy}$. Di $(1, 0)$: $0$ dan $1$.

**Makna geometris.** Jika grafik $f(x, y)$ dibayangkan sebagai permukaan, memotongnya pada $y$ tetap menghasilkan kurva satu variabel dalam $x$. Turunan parsial terhadap $x$ adalah kemiringan kurva itu.

### 3. Vektor Gradien

**Gradien** $\nabla f$ (dibaca "nabla $f$" atau "gradien $f$") adalah **vektor yang elemennya adalah semua turunan parsial**:

$$
\nabla f(x_1, \ldots, x_n) =
\begin{bmatrix}
\dfrac{\partial f}{\partial x_1} \\[2mm]
\vdots \\[1mm]
\dfrac{\partial f}{\partial x_n}
\end{bmatrix}
$$

Gradien adalah vektor berdimensi $n$, sama dengan banyak masukan, dan nilainya bergantung pada titik tempat ia dihitung.

**Contoh:** $f(x, y) = x^2 + y^2$ memberi $\nabla f = (2x, 2y)$. Di $(1, 2)$: $\nabla f = (2, 4)$. Di $(3, -1)$: $\nabla f = (6, -2)$, dengan norma $\sqrt{36 + 4} = \sqrt{40} \approx 6{,}325$. Untuk fungsi linear $f(x, y) = 3x + 2y$: $\nabla f = (3, 2)$ di mana pun (kemiringan tetap).

### 4. Interpretasi Gradien

Gradien menjawab: **jika ingin menaikkan $f$ secepat mungkin dari titik ini, ke arah mana melangkah?**

**Turunan berarah.** Jika kita melangkah sedikit ke arah vektor satuan $\mathbf{u}$ (norma 1), laju perubahan $f$ adalah

$$
D_{\mathbf{u}} f = \nabla f \cdot \mathbf{u}
$$

yaitu **dot product** gradien dengan arah (Part 2). Karena $\nabla f \cdot \mathbf{u} = \lVert \nabla f \rVert \cdot 1 \cdot \cos\theta$ dengan $\theta$ sudut antara gradien dan arah:

- laju terbesar saat $\cos\theta = 1$, yaitu $\mathbf{u}$ **searah gradien**; nilainya $\lVert \nabla f \rVert$;
- laju terkecil (paling negatif) saat $\mathbf{u}$ **berlawanan** gradien; nilainya $-\lVert \nabla f \rVert$;
- laju nol saat $\mathbf{u}$ **tegak lurus** gradien: itulah arah sepanjang kontur.

**Contoh numerik.** $f(x, y) = x^2 + y^2$ di $(1, 2)$, $\nabla f = (2, 4)$, $\lVert \nabla f \rVert = \sqrt{20} \approx 4{,}472$:

| Arah $\mathbf{u}$ | $\nabla f \cdot \mathbf{u}$ | Arti |
|---|---|---|
| $(1, 0)$ | $2$ | hanya $x$ yang digeser |
| $(0, 1)$ | $4$ | hanya $y$ yang digeser |
| $(0{,}6;\ 0{,}8)$ | $2(0{,}6) + 4(0{,}8) = 4{,}4$ | arah campuran |
| $\dfrac{(2, 4)}{\sqrt{20}} \approx (0{,}447;\ 0{,}894)$ | $\sqrt{20} \approx 4{,}472$ | **searah gradien: tercuram naik** |
| $-\dfrac{(2, 4)}{\sqrt{20}}$ | $-4{,}472$ | **berlawanan: tercuram turun** |
| $\dfrac{(4, -2)}{\sqrt{20}}$ | $0$ | tegak lurus gradien: sepanjang kontur |

Dua baris pertama adalah turunan parsial sendiri. Baris ketiga memberi $4{,}4 < 4{,}472$. Tidak ada arah yang lebih curam daripada arah gradien.

Ringkas:

1. **$\nabla f$ menunjuk arah kenaikan tercuram.**
2. **$\lVert \nabla f \rVert$ adalah laju kenaikan terbesar itu.**
3. **$-\nabla f$ menunjuk arah penurunan tercuram.** Inilah dasar gradient descent.
4. **$\nabla f$ tegak lurus terhadap garis kontur** di titiknya.

### 5. Titik Kritis

Titik tempat **semua** turunan parsial nol, $\nabla f = \mathbf{0}$, disebut **titik kritis**. Di sana tidak ada arah naik atau turun yang tercuram: permukaan "datar" di titik itu.

**Contoh:** $f(x, y) = (x - 1)^2 + (y + 2)^2$. $\dfrac{\partial f}{\partial x} = 2(x - 1)$ dan $\dfrac{\partial f}{\partial y} = 2(y + 2)$. Keduanya nol di $(1, -2)$, dan $f(1, -2) = 0$, nilai terkecil yang mungkin (kuadrat tak negatif). Jadi itu **minimum**.

Titik kritis tidak selalu minimum atau maksimum. Contoh $f(x, y) = x^2 - y^2$: $\nabla f = (2x, -2y) = \mathbf{0}$ di $(0, 0)$. Tetapi $f(0{,}1;\ 0) = 0{,}01 > 0$ dan $f(0;\ 0{,}1) = -0{,}01 < 0$, jadi di sekitar titik itu $f$ naik ke satu arah dan turun ke arah lain, seperti pelana kuda. Itu disebut **titik pelana** (*saddle point*). Membedakan minimum, maksimum, dan pelana pada banyak variabel memakai turunan kedua dalam bentuk matriks (level lanjut); di sini cukup mengetahui bahwa $\nabla f = \mathbf{0}$ adalah syarat perlu, bukan cukup.

### 6. Pendekatan Linear dengan Gradien

Untuk langkah kecil $\mathbf{d}$ (sebuah vektor pergeseran, misalnya $(0{,}1;\ 0{,}1)$) dari titik $\mathbf{x}$:

$$
f(\mathbf{x} + \mathbf{d}) \approx f(\mathbf{x}) + \nabla f(\mathbf{x}) \cdot \mathbf{d}
$$

Ini versi banyak variabel dari "garis singgung" (Part 7). Contoh: $f(x, y) = x^2 + y^2$ di $(1, 2)$, $f = 5$, $\nabla f = (2, 4)$.

- $\mathbf{d} = (0{,}1;\ 0{,}1)$: perkiraan $5 + 2(0{,}1) + 4(0{,}1) = 5{,}6$. Nilai sebenarnya $f(1{,}1;\ 2{,}1) = 1{,}21 + 4{,}41 = 5{,}62$. Dekat.
- $\mathbf{d} = (0{,}1;\ -0{,}1)$: perkiraan $5 + 0{,}2 - 0{,}4 = 4{,}8$. Sebenarnya $f(1{,}1;\ 1{,}9) = 1{,}21 + 3{,}61 = 4{,}82$.

Perkiraan makin baik bila langkah makin kecil. Jika $\mathbf{d} = -\eta\,\nabla f$ dengan $\eta > 0$ kecil, maka $f$ berubah kira-kira $-\eta\,\lVert \nabla f \rVert^2 < 0$: nilainya **turun**. Itulah satu langkah gradient descent (Part 11).

### 7. Gradien dalam Machine Learning

**Gradien kerugian terhadap parameter.** Untuk model $\hat{y} = wx + b$ dan data $(x_i, y_i)$, $i = 1, \ldots, n$, kerugian rata-rata kuadrat (MSE, Level 0 Part 8):

$$
L(w, b) = \frac{1}{n} \sum_{i=1}^{n} (w x_i + b - y_i)^2
$$

Turunan parsial (aturan rantai: luar $(\cdot)^2 \to 2(\cdot)$, dalam terhadap $w$ adalah $x_i$ dan terhadap $b$ adalah 1):

$$
\frac{\partial L}{\partial w} = \frac{2}{n} \sum_{i=1}^{n} (w x_i + b - y_i)\, x_i, \qquad
\frac{\partial L}{\partial b} = \frac{2}{n} \sum_{i=1}^{n} (w x_i + b - y_i)
$$

Gradiennya $\nabla L = \left(\dfrac{\partial L}{\partial w},\ \dfrac{\partial L}{\partial b}\right)$.

**Contoh angka.** Data dari Level 1: $(1, 5)$, $(3, 9)$, $(2, 8)$. Di $(w, b) = (0, 0)$, selisih $\hat{y} - y$ adalah $-5, -9, -8$.

- $L(0, 0) = \dfrac{25 + 81 + 64}{3} = \dfrac{170}{3} \approx 56{,}67$.
- $\dfrac{\partial L}{\partial w} = \dfrac{2}{3}\bigl(-5 \cdot 1 + (-9) \cdot 3 + (-8) \cdot 2\bigr) = \dfrac{2}{3}(-5 - 27 - 16) = \dfrac{2}{3}(-48) = -32$.
- $\dfrac{\partial L}{\partial b} = \dfrac{2}{3}(-5 - 9 - 8) = \dfrac{2}{3}(-22) = -\dfrac{44}{3} \approx -14{,}67$.

Gradien $(-32;\ -14{,}67)$ bernilai negatif di kedua parameter. Jadi menaikkan $w$ dan $b$ **menurunkan** kerugian (kita sedang jauh di bawah data). Di $(w, b) = (2, 3)$: selisih $0, 0, -1$, jadi $L = \tfrac{1}{3} \approx 0{,}33$ dan gradien $\left(-\tfrac{4}{3},\ -\tfrac{2}{3}\right)$ sudah sangat kecil.

**Bentuk matriks.** Susun data sebagai matriks $X$ dengan kolom $x_i$ dan kolom angka 1 (untuk $b$), parameter $\mathbf{w} = (w, b)$, dan label $\mathbf{y}$:

$$
X = \begin{bmatrix} 1 & 1 \\ 3 & 1 \\ 2 & 1 \end{bmatrix}, \qquad
\nabla L = \frac{2}{n} X^{\top}(X\mathbf{w} - \mathbf{y})
$$

Langkahnya: $\mathbf{r} = X\mathbf{w} - \mathbf{y}$ adalah vektor selisih (residu); $X^{\top}\mathbf{r}$ menjumlahkan residu berbobot fitur. Di $\mathbf{w} = (0, 0)$: $\mathbf{r} = (-5, -9, -8)$ dan $X^{\top}\mathbf{r} = (1 \cdot -5 + 3 \cdot -9 + 2 \cdot -8,\ -5 - 9 - 8) = (-48, -22)$. Kalikan $\tfrac{2}{3}$: $(-32;\ -14{,}67)$. Sama dengan hasil per elemen. Satu rumus ini berlaku untuk jutaan data dan banyak fitur.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Fungsi multivariabel | fungsi dengan banyak masukan, satu keluaran | $f(x, y) = x^2 + y^2$ |
| Garis kontur (*level curve*) | titik-titik dengan nilai $f$ sama | lingkaran |
| Turunan parsial | turunan terhadap satu variabel, lainnya konstan | $\frac{\partial f}{\partial x}$ |
| Gradien $\nabla f$ | vektor semua turunan parsial | $(2x, 2y)$ |
| Turunan berarah | laju perubahan ke arah vektor satuan | $\nabla f \cdot \mathbf{u}$ |
| Kenaikan tercuram | arah gradien | |
| Titik kritis | titik dengan $\nabla f = \mathbf{0}$ | $(1, -2)$ |
| Titik pelana (*saddle*) | titik kritis yang bukan minimum atau maksimum | $(0, 0)$ untuk $x^2 - y^2$ |
| Parameter | masukan fungsi kerugian yang diatur model | $w$, $b$ |
| Residu | selisih prediksi dan nilai sebenarnya | $\hat{y}_i - y_i$ |

## Rumus dan Penjelasan Setiap Simbol

**Turunan parsial:**

$$
\frac{\partial f}{\partial x_i}(\mathbf{x}) = \lim_{h \to 0} \frac{f(\ldots, x_i + h, \ldots) - f(\mathbf{x})}{h}
$$

- Hanya $x_i$ yang digeser; $h$ selisih kecil. Praktis: turunkan terhadap $x_i$, anggap variabel lain konstanta.

**Gradien:** $\nabla f = \bigl(\tfrac{\partial f}{\partial x_1}, \ldots, \tfrac{\partial f}{\partial x_n}\bigr)$, vektor $n$ elemen.

**Turunan berarah:** $D_{\mathbf{u}} f = \nabla f \cdot \mathbf{u}$, dengan $\lVert \mathbf{u} \rVert_2 = 1$. Nilai terbesar $\lVert \nabla f \rVert_2$ (searah gradien), terkecil $-\lVert \nabla f \rVert_2$.

**Pendekatan linear:** $f(\mathbf{x} + \mathbf{d}) \approx f(\mathbf{x}) + \nabla f(\mathbf{x}) \cdot \mathbf{d}$ untuk vektor langkah $\mathbf{d}$ yang kecil.

**Gradien MSE linear:** $\nabla L = \dfrac{2}{n} X^{\top}(X\mathbf{w} - \mathbf{y})$ dengan $X \in \mathbb{R}^{n \times d}$, $\mathbf{w} \in \mathbb{R}^d$, $\mathbf{y} \in \mathbb{R}^n$.

## Penjelasan Langkah demi Langkah

**Menghitung gradien $f(x, y) = x^2 + 3xy + y^2$ di $(1, 2)$:**

| Langkah | Perhitungan |
|---|---|
| 1. Turunan parsial terhadap $x$ ($y$ konstan) | $2x + 3y$ |
| 2. Turunan parsial terhadap $y$ ($x$ konstan) | $3x + 2y$ |
| 3. Susun jadi vektor | $\nabla f = (2x + 3y,\ 3x + 2y)$ |
| 4. Substitusi $(1, 2)$ | $(2 + 6,\ 3 + 4) = (8, 7)$ |
| 5. Periksa numerik | $\dfrac{f(1{,}001;\,2) - f(0{,}999;\,2)}{0{,}002} \approx 8$ |
| 6. Arah turun tercuram | $-\nabla f = (-8, -7)$ |

## Contoh Soal dan Pembahasan

### Contoh 1 — Turunan parsial dan gradien

Tentukan $\nabla f$ untuk $f(x, y) = x^2 y + y^3$ dan nilainya di $(2, 1)$.

1. $\dfrac{\partial f}{\partial x} = 2xy$ ($y$ konstan; $y^3$ tidak mengandung $x$ sehingga turunannya 0).
2. $\dfrac{\partial f}{\partial y} = x^2 + 3y^2$.
3. $\nabla f = (2xy,\ x^2 + 3y^2)$. Di $(2, 1)$: $(4,\ 4 + 3) = (4, 7)$.
4. Cek numerik untuk $\dfrac{\partial f}{\partial x}$: $f(2{,}001;\,1) = 5{,}004001$ dan $f(1{,}999;\,1) = 4{,}996001$. Selisihnya $0{,}008$, dibagi $0{,}002$ menghasilkan $4$. Cocok.

### Contoh 2 — Turunan berarah

Berapa laju perubahan $f(x, y) = x^2 + y^2$ di $(1, 2)$ ke arah $\mathbf{v} = (3, 4)$?

1. Vektor satuan: $\lVert \mathbf{v} \rVert = 5$, jadi $\mathbf{u} = (0{,}6;\ 0{,}8)$.
2. $\nabla f(1, 2) = (2, 4)$.
3. $D_{\mathbf{u}} f = 2(0{,}6) + 4(0{,}8) = 1{,}2 + 3{,}2 = 4{,}4$.
4. Bandingkan dengan laju maksimum $\sqrt{20} \approx 4{,}472$. Arah $(3, 4)$ hampir searah gradien $(2, 4)$ sehingga lajunya mendekati maksimum.

### Contoh 3 — Titik kritis

Cari titik kritis $f(x, y) = x^2 + y^2 - 4x + 6y$ dan nilainya.

1. $\dfrac{\partial f}{\partial x} = 2x - 4 = 0$ memberi $x = 2$.
2. $\dfrac{\partial f}{\partial y} = 2y + 6 = 0$ memberi $y = -3$.
3. Titik kritis $(2, -3)$. Nilai: $4 + 9 - 8 - 18 = -13$.
4. Pengecekan: $f = (x - 2)^2 + (y + 3)^2 - 13$ (melengkapi kuadrat), yang minimum bernilai $-13$ di $(2, -3)$. Jadi itu minimum.

### Contoh 4 — Gradien kerugian

Untuk satu data $(x, y) = (2, 5)$ dan $L(w, b) = (wx + b - y)^2$, hitung gradien di $(w, b) = (0, 0)$.

1. Residu: $wx + b - y = 0 + 0 - 5 = -5$.
2. $\dfrac{\partial L}{\partial w} = 2(-5)(2) = -20$.
3. $\dfrac{\partial L}{\partial b} = 2(-5)(1) = -10$.
4. $\nabla L = (-20, -10)$. Kedua negatif: naikkan $w$ dan $b$ untuk menurunkan kerugian.
5. Cek: $L(0, 0) = 25$. Langkah kecil ke arah $-\nabla L$, yaitu $(+0{,}01\cdot 20,\ +0{,}01 \cdot 10) = (0{,}2;\ 0{,}1)$: $L(0{,}2;\ 0{,}1) = (0{,}4 + 0{,}1 - 5)^2 = 20{,}25 < 25$.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** $f(x, y) = 3x + 2y$: $\nabla f = (3, 2)$ di mana pun, karena fungsi linear punya kemiringan tetap.

**Sedang.** $f(x, y) = x^2 + y^2$ di $(3, -1)$: $\nabla f = (6, -2)$. Arah penurunan tercuram $-\nabla f = (-6, 2)$, yaitu menuju titik asal (minimum).

**Sedang.** Pendekatan linear $\sqrt{x^2 + y^2}$ di $(3, 4)$: $f = 5$, $\nabla f = \left(\tfrac{x}{\sqrt{x^2+y^2}},\ \tfrac{y}{\sqrt{x^2+y^2}}\right) = (0{,}6;\ 0{,}8)$. Geser $\mathbf{d} = (0{,}3;\ 0{,}4)$ memberi perkiraan $5 + 0{,}18 + 0{,}32 = 5{,}5$, dan nilai sebenarnya $\sqrt{3{,}3^2 + 4{,}4^2} = \sqrt{30{,}25} = 5{,}5$. Sama persis karena langkahnya searah gradien dari titik asal.

**Menantang.** Gradien tegak lurus kontur: untuk $f = x^2 + y^2$, kontur $f = 5$ di $(1, 2)$ adalah lingkaran, dan arah singgung lingkaran di sana tegak lurus jari-jari $(1, 2)$. Gradien $(2, 4)$ sejajar jari-jari, sehingga tegak lurus kontur. Dot product $(2, 4) \cdot (4, -2) = 0$ membuktikannya.

## Visualisasi atau Interpretasi Grafik

- **Peta kontur:** pada $f = x^2 + y^2$, kontur adalah lingkaran sepusat. Gradien di sebuah titik adalah panah yang menjauhi pusat dan tegak lurus lingkaran di titik itu. Arah $-\nabla f$ menuju pusat.
- **Dasar mangkuk** ($\nabla f = \mathbf{0}$): tidak ada arah turun lagi.
- **Titik pelana**: kontur berbentuk silang. Gradien nol, tetapi ada arah naik dan arah turun.
- **Panjang gradien:** kontur yang rapat berarti $f$ berubah cepat (gradien besar); kontur yang renggang berarti landai.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menurunkan terhadap $x$ sambil ikut menurunkan $y$ | $y$ harus dianggap konstan | $\partial(3xy)/\partial x = 3y$ |
| $\partial(x^2 y)/\partial y = 2xy$ | menurunkan $x$ padahal yang digeser $y$ | $x^2$ |
| Menyebut gradien sebagai bilangan | gradien adalah vektor | $(8, 7)$ |
| Gradien dihitung di satu titik lalu dianggap sama di semua titik | gradien bergantung pada titik | substitusi titiknya |
| Menggunakan vektor arah yang bukan satuan pada turunan berarah | nilainya terlalu besar atau kecil | normalkan $\mathbf{u}$ |
| Menyimpulkan $\nabla f = \mathbf{0}$ pasti minimum | bisa maksimum atau pelana | periksa ($f = x^2 - y^2$) |
| Melangkah searah $+\nabla f$ untuk menurunkan $f$ | itu menaikkan $f$ | $-\nabla f$ |
| Lupa transpose pada $X^{\top}(X\mathbf{w} - \mathbf{y})$ | dimensi tidak cocok | $X^{\top}$ berukuran $d \times n$ |
| Lupa faktor $\tfrac{2}{n}$ | gradien salah skala | sertakan $\tfrac{2}{n}$ (untuk MSE) |
| Menggunakan langkah besar pada pendekatan linear | pendekatan hanya baik untuk langkah kecil | kecilkan langkah |

## Hubungan dengan Machine Learning

- **Gradien adalah arah belajar.** Kerugian $L$ adalah fungsi dari semua parameter. $-\nabla L$ menunjuk arah yang paling cepat menurunkan kerugian pada titik saat ini, dan itu dipakai di gradient descent (Part 11).
- **Satu gradien, banyak parameter.** Model dengan jutaan parameter punya gradien berdimensi jutaan; aturan hitungnya sama dengan dua parameter.
- **Residu menentukan arah.** Pada regresi linear, gradien dibentuk dari residu berbobot fitur: $\tfrac{2}{n}X^{\top}\mathbf{r}$. Jika residu negatif (prediksi terlalu kecil), gradien negatif dan parameter naik.
- **Bentuk matriks memanfaatkan perangkat keras.** Satu perkalian matriks menghitung gradien untuk seluruh data.
- **Gradien nol belum tentu optimum.** Pada jaringan saraf banyak titik pelana; metode pelatihan harus mengatasinya (level lanjut).
- **Backpropagation** adalah cara menghitung gradien ini secara efisien pada jaringan berlapis (Part 9).

### Python: turunan parsial, gradien, dan pemeriksaan

Turunan parsial numerik (selisih tengah) memberi cara memeriksa rumus:

```python
import math

def parsial(f, titik, i, h=1e-6):
    a = list(titik)
    b = list(titik)
    a[i] += h
    b[i] -= h
    return (f(*a) - f(*b)) / (2 * h)

def gradien(f, titik):
    return [round(parsial(f, titik, i), 4) for i in range(len(titik))]

f = lambda x, y: x ** 2 + 3 * x * y + y ** 2
print(gradien(f, (1, 2)))   # [8.0, 7.0]

g = lambda x, y: x ** 2 * y
print(gradien(g, (2, 3)))   # [12.0, 4.0]

k = lambda x, y: x ** 2 * y + y ** 3
print(gradien(k, (2, 1)))   # [4.0, 7.0]

e = lambda x, y: math.exp(x * y)
print(gradien(e, (1, 0)))   # [0.0, 1.0]
```

Gradien $x^2 + y^2$ dan turunan berarah dengan dot product:

```python
q = lambda x, y: x ** 2 + y ** 2

def dot(a, b):
    return sum(ai * bi for ai, bi in zip(a, b))

def norma(v):
    return math.sqrt(sum(vi ** 2 for vi in v))

grad = gradien(q, (1, 2))
print(grad, round(norma(grad), 4))   # [2.0, 4.0] 4.4721

arah = [(1, 0), (0, 1), (0.6, 0.8)]
for u in arah:
    print(u, round(dot(grad, u), 4))
# (1, 0) 2.0
# (0, 1) 4.0
# (0.6, 0.8) 4.4

n = norma(grad)
u_tercuram = [gi / n for gi in grad]
print(round(dot(grad, u_tercuram), 4))          # 4.4721, laju maksimum
print(round(dot(grad, [-gi / n for gi in grad]), 4))   # -4.4721
u_kontur = [4 / n, -2 / n]
print(round(dot(grad, u_kontur), 4))            # 0.0, sepanjang kontur
```

Pendekatan linear dan titik pelana:

```python
perkiraan = q(1, 2) + dot(grad, (0.1, 0.1))
print(round(perkiraan, 4), round(q(1.1, 2.1), 4))   # 5.6 5.62

pelana = lambda x, y: x ** 2 - y ** 2
print(round(pelana(0.1, 0), 4), round(pelana(0, 0.1), 4))   # 0.01 -0.01
```

Gradien kerugian MSE pada data $(1, 5)$, $(3, 9)$, $(2, 8)$, per elemen dan dalam bentuk matriks:

```python
data = [(1, 5), (3, 9), (2, 8)]

def kerugian(w, b):
    return sum((w * x + b - y) ** 2 for x, y in data) / len(data)

def gradien_kerugian(w, b):
    n = len(data)
    r = [w * x + b - y for x, y in data]
    dw = 2 / n * sum(ri * x for ri, (x, y) in zip(r, data))
    db = 2 / n * sum(r)
    return dw, db

print(round(kerugian(0, 0), 4), gradien_kerugian(0, 0))   # 56.6667 (-32.0, -14.666666666666666)
print(round(kerugian(2, 3), 4), gradien_kerugian(2, 3))   # 0.3333 (-1.3333333333333333, -0.6666666666666666)

X = [[1, 1], [3, 1], [2, 1]]
y = [5, 9, 8]
w = [0, 0]
r = [dot(baris, w) - yi for baris, yi in zip(X, y)]
Xt_r = [sum(X[i][j] * r[i] for i in range(3)) for j in range(2)]
print(r, Xt_r)                                # [-5, -9, -8] [-48, -22]
print([2 / 3 * v for v in Xt_r])              # [-32.0, -14.666666666666666]
```

Selisih antara gradien rumus dan gradien numerik sangat kecil, yang menandakan rumusnya benar:

```python
print([round(parsial(kerugian, (0, 0), i), 4) for i in range(2)])   # [-32.0, -14.6667]
```

## Latihan Mandiri

1. Untuk $f(x, y) = x^2 + 3xy + y^2$, hitung $\dfrac{\partial f}{\partial x}$, $\dfrac{\partial f}{\partial y}$, dan nilainya di $(1, 2)$.
2. Hitung turunan parsial $f(x, y) = x^2 y$ dan nilainya di $(2, 3)$.
3. Hitung $\dfrac{\partial}{\partial x}$ dan $\dfrac{\partial}{\partial y}$ dari $e^{xy}$, lalu nilainya di $(1, 0)$.
4. Untuk $L(w, b) = (wx + b - y)^2$ dengan $x = 2$ dan $y = 5$: hitung $\dfrac{\partial L}{\partial w}$ dan $\dfrac{\partial L}{\partial b}$ di $(w, b) = (0, 0)$.
5. Hitung gradien $f(x, y) = x^2 + y^2$ di $(3, -1)$ beserta normanya. Hitung juga gradien $f(x, y) = 3x + 2y$.
6. Untuk $f(x, y) = x^2 + y^2$ di $(1, 2)$: hitung turunan berarah untuk $\mathbf{u} = (1, 0)$, $(0, 1)$, $(0{,}6;\ 0{,}8)$, arah gradien, dan arah $\dfrac{(4, -2)}{\sqrt{20}}$. Arah mana yang tercuram dan arah mana sepanjang kontur?
7. Cari titik kritis $f(x, y) = (x - 1)^2 + (y + 2)^2$. Lalu periksa $f(x, y) = x^2 - y^2$ di titik kritisnya dengan menghitung $f(0{,}1;\ 0)$ dan $f(0;\ 0{,}1)$. Apa kesimpulannya?
8. Pakai pendekatan linear untuk $f(x, y) = x^2 + y^2$ di $(1, 2)$ dengan $\mathbf{d} = (0{,}1;\ -0{,}1)$ dan bandingkan dengan nilai sebenarnya.
9. (ML) Data $(1, 5)$, $(3, 9)$, $(2, 8)$ dan $L(w, b) = \tfrac{1}{3}\sum (wx_i + b - y_i)^2$. Hitung $L$ dan $\nabla L$ di $(0, 0)$ dan di $(2, 3)$. Apa arti tanda gradien di $(0, 0)$?
10. (ML, matriks) Dengan $X = \begin{bmatrix} 1 & 1 \\ 3 & 1 \\ 2 & 1 \end{bmatrix}$, $\mathbf{y} = (5, 9, 8)$, dan $\mathbf{w} = (0, 0)$, hitung $\mathbf{r} = X\mathbf{w} - \mathbf{y}$, $X^{\top}\mathbf{r}$, dan $\nabla L = \tfrac{2}{3}X^{\top}\mathbf{r}$.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $\dfrac{\partial f}{\partial x} = 2x + 3y$ dan $\dfrac{\partial f}{\partial y} = 3x + 2y$. Di $(1, 2)$: $8$ dan $7$.

??? success "Soal 2"
    $\dfrac{\partial f}{\partial x} = 2xy$ dan $\dfrac{\partial f}{\partial y} = x^2$. Di $(2, 3)$: $12$ dan $4$.

??? success "Soal 3"
    $\dfrac{\partial}{\partial x} e^{xy} = y\,e^{xy}$ dan $\dfrac{\partial}{\partial y} e^{xy} = x\,e^{xy}$. Di $(1, 0)$: $0 \cdot e^0 = 0$ dan $1 \cdot e^0 = 1$.

??? success "Soal 4"
    Residu $= 0 + 0 - 5 = -5$. $\dfrac{\partial L}{\partial w} = 2(-5)(2) = -20$ dan $\dfrac{\partial L}{\partial b} = 2(-5)(1) = -10$.

??? success "Soal 5"
    $\nabla f = (2x, 2y) = (6, -2)$, norma $\sqrt{36 + 4} = \sqrt{40} \approx 6{,}325$. Untuk $3x + 2y$: $\nabla f = (3, 2)$ di mana pun.

??? success "Soal 6"
    $\nabla f = (2, 4)$. $(1, 0) \to 2$; $(0, 1) \to 4$; $(0{,}6;\ 0{,}8) \to 1{,}2 + 3{,}2 = 4{,}4$; arah gradien $\to \sqrt{20} \approx 4{,}472$ (tercuram); $\dfrac{(4, -2)}{\sqrt{20}} \to \dfrac{8 - 8}{\sqrt{20}} = 0$ (sepanjang kontur).

??? success "Soal 7"
    $\nabla f = (2(x - 1), 2(y + 2)) = \mathbf{0}$ di $(1, -2)$ (minimum, nilai 0). Untuk $x^2 - y^2$: $\nabla f = (2x, -2y) = \mathbf{0}$ di $(0, 0)$, tetapi $f(0{,}1;\ 0) = 0{,}01 > 0$ dan $f(0;\ 0{,}1) = -0{,}01 < 0$, jadi bukan minimum atau maksimum: **titik pelana**.

??? success "Soal 8"
    $f(1, 2) = 5$, $\nabla f = (2, 4)$. Perkiraan: $5 + 2(0{,}1) + 4(-0{,}1) = 4{,}8$. Sebenarnya $f(1{,}1;\ 1{,}9) = 1{,}21 + 3{,}61 = 4{,}82$.

??? success "Soal 9"
    Di $(0, 0)$: residu $-5, -9, -8$; $L = \dfrac{170}{3} \approx 56{,}67$; $\dfrac{\partial L}{\partial w} = -32$ dan $\dfrac{\partial L}{\partial b} = -\dfrac{44}{3} \approx -14{,}67$. Di $(2, 3)$: residu $0, 0, -1$; $L = \tfrac{1}{3}$; gradien $\left(-\tfrac{4}{3}, -\tfrac{2}{3}\right)$. Gradien negatif di $(0, 0)$ berarti menaikkan $w$ dan $b$ menurunkan kerugian.

??? success "Soal 10"
    $\mathbf{r} = (0 - 5,\ 0 - 9,\ 0 - 8) = (-5, -9, -8)$. $X^{\top}\mathbf{r} = (1(-5) + 3(-9) + 2(-8),\ -5 - 9 - 8) = (-48, -22)$. $\nabla L = \tfrac{2}{3}(-48, -22) = (-32;\ -14{,}67)$, sama dengan hasil per elemen.

## Ringkasan Materi

- Fungsi multivariabel $f(\mathbf{x})$ memiliki banyak masukan dan satu keluaran; garis kontur menghubungkan titik bernilai sama.
- Turunan parsial $\dfrac{\partial f}{\partial x_i}$: turunkan terhadap $x_i$, variabel lain dianggap konstanta.
- Gradien $\nabla f$ adalah vektor semua turunan parsial. Ia menunjuk arah kenaikan tercuram, panjangnya adalah laju kenaikan itu, dan tegak lurus terhadap kontur. $-\nabla f$ adalah arah penurunan tercuram.
- Turunan berarah $D_{\mathbf{u}} f = \nabla f \cdot \mathbf{u}$ untuk vektor satuan $\mathbf{u}$.
- Titik kritis: $\nabla f = \mathbf{0}$; bisa minimum, maksimum, atau pelana.
- Pendekatan linear: $f(\mathbf{x} + \mathbf{d}) \approx f(\mathbf{x}) + \nabla f \cdot \mathbf{d}$.
- Gradien MSE linear: $\dfrac{2}{n}X^{\top}(X\mathbf{w} - \mathbf{y})$. Gradien adalah dasar gradient descent.

## Checklist Penguasaan

- [ ] Menghitung turunan parsial dengan menganggap variabel lain konstan.
- [ ] Menyusun dan menghitung vektor gradien di sebuah titik.
- [ ] Menjelaskan arah dan panjang gradien dan hubungannya dengan kontur.
- [ ] Menghitung turunan berarah dengan dot product dan menemukan arah tercuram.
- [ ] Mencari titik kritis dan mengenali titik pelana.
- [ ] Memakai pendekatan linear untuk memperkirakan perubahan $f$.
- [ ] Menghitung gradien MSE per elemen dan dalam bentuk matriks.

## Prasyarat untuk Materi Berikutnya

Part 9 — Chain Rule dan Backpropagation membutuhkan: turunan parsial (setiap parameter punya turunan sendiri), aturan rantai dari Part 7, dan pemahaman bahwa gradien adalah kumpulan turunan parsial. Part 11 — Optimasi dan Gradient Descent membutuhkan: arah $-\nabla f$, pendekatan linear, dan contoh data $(1, 5)$, $(3, 9)$, $(2, 8)$ yang dipakai di sini.
