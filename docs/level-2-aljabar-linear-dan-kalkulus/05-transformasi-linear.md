# Part 5 — Transformasi Linear

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- memandang sebuah matriks sebagai mesin yang mengubah vektor (transformasi linear);
- membaca matriks $2 \times 2$ dari kolom-kolomnya untuk mengetahui ke mana vektor basis berpindah;
- mengenali transformasi umum (peregangan, pencerminan, rotasi, geseran, proyeksi) beserta determinannya;
- menghitung kombinasi linear dan menafsirkan $A\mathbf{x}$ sebagai kombinasi linear kolom-kolom $A$;
- menjelaskan span, independensi linear, basis, dan dimensi;
- menghubungkan independensi linear dengan determinan, rank, dan keberadaan invers;
- (pengayaan) menjelaskan nilai eigen dan vektor eigen secara pengantar.

## Prasyarat

[Part 3 — Matriks](03-matriks.md) (perkalian matriks kali vektor, determinan, invers) dan [Part 4](04-sistem-persamaan-linear-matriks.md) (pivot, rank secara informal, jenis solusi). Gambar di bagian ini memakai vektor sebagai panah seperti di [Part 2](02-vektor.md).

## Mengapa Materi Ini Penting?

Sampai sekarang kita memakai matriks sebagai tabel angka. Tetapi makna sebenarnya sebuah matriks adalah **aksi**: ia mengubah vektor menjadi vektor lain. Itulah yang dilakukan setiap lapisan jaringan saraf. Memahami transformasi linear membantu menjawab pertanyaan nyata: kapan sebuah transformasi dapat dibalik (tanpa kehilangan informasi), apa arti fitur yang saling bergantung, dan bagaimana kombinasi fitur membentuk ruang yang dapat dijangkau model.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Matriks sebagai Transformasi

Sebuah matriks $A$ berukuran $2 \times 2$ mengubah setiap vektor $\mathbf{x} \in \mathbb{R}^2$ menjadi vektor lain $T(\mathbf{x}) = A\mathbf{x} \in \mathbb{R}^2$. Pemetaan seperti ini disebut **transformasi linear** karena memenuhi dua sifat:

1. **Aditif:** $T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})$.
2. **Homogen:** $T(c\,\mathbf{u}) = c\,T(\mathbf{u})$.

Keduanya berarti: "mengubah lalu menjumlahkan sama dengan menjumlahkan lalu mengubah", dan "memperbesar lalu mengubah sama dengan mengubah lalu memperbesar".

Cek dengan angka. Misalkan $A = \begin{bmatrix} 2 & 1 \\ 0 & 1 \end{bmatrix}$, $\mathbf{u} = (1, 2)$, dan $\mathbf{v} = (3, -1)$:

- $A\mathbf{u} = (2 \cdot 1 + 1 \cdot 2,\ 0 + 2) = (4, 2)$ dan $A\mathbf{v} = (6 - 1,\ -1) = (5, -1)$.
- $A(\mathbf{u} + \mathbf{v}) = A(4, 1) = (9, 1)$ dan $A\mathbf{u} + A\mathbf{v} = (9, 1)$. Sama.
- $A(3\mathbf{u}) = A(3, 6) = (12, 6) = 3(4, 2) = 3\,A\mathbf{u}$. Sama.

Konsekuensi penting: **vektor nol selalu tetap nol**, $A\mathbf{0} = \mathbf{0}$.

**Contoh yang bukan linear:** geseran $S(x, y) = (x + 1,\ y)$. Ia menggeser semua titik, termasuk titik asal: $S(0, 0) = (1, 0) \neq (0, 0)$, sehingga bukan transformasi linear. Pemetaan $(x, y) \mapsto (x^2, y)$ juga bukan linear: $T(2, 0) = (4, 0)$ tetapi $2\,T(1, 0) = (2, 0)$.

### 2. Kolom Matriks Menunjukkan ke Mana Vektor Basis Pindah

**Vektor basis standar** pada bidang adalah $\mathbf{e}_1 = (1, 0)$ (satu langkah mendatar) dan $\mathbf{e}_2 = (0, 1)$ (satu langkah tegak). Setiap vektor dapat ditulis dengan keduanya: $(x, y) = x\,\mathbf{e}_1 + y\,\mathbf{e}_2$.

Karena $A$ linear, $A(x, y) = x\,A\mathbf{e}_1 + y\,A\mathbf{e}_2$, jadi **cukup tahu $A\mathbf{e}_1$ dan $A\mathbf{e}_2$**. Dan dari aturan perkalian matriks:

$$
A\mathbf{e}_1 = \text{kolom ke-1 dari } A, \qquad A\mathbf{e}_2 = \text{kolom ke-2 dari } A
$$

Untuk $A = \begin{bmatrix} 2 & 1 \\ 0 & 1 \end{bmatrix}$: $A\mathbf{e}_1 = (2, 0)$ dan $A\mathbf{e}_2 = (1, 1)$. Gambar berikut memperlihatkan **persegi satuan** (dibentuk oleh $\mathbf{e}_1$ dan $\mathbf{e}_2$) sebelum dan sesudah diubah oleh $A$.

<div style="max-width:380px;margin:1rem auto"><svg viewBox="0 0 380 250" width="100%" role="img" aria-labelledby="tl-t tl-d" xmlns="http://www.w3.org/2000/svg" style="color:inherit;font-family:inherit;font-size:13px"><title id="tl-t">Transformasi persegi satuan oleh matriks A</title><desc id="tl-d">Persegi satuan berwarna biru dengan sisi e1 = (1, 0) dan e2 = (0, 1) diubah oleh matriks A = [[2, 1], [0, 1]] menjadi jajar genjang merah dengan sisi A e1 = (2, 0) dan A e2 = (1, 1). Luasnya menjadi 2.</desc><defs><marker id="tl-b" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="10" markerHeight="10" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#5c6bc0"/></marker></defs><defs><marker id="tl-r" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="10" markerHeight="10" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#e53935"/></marker></defs><defs><marker id="tl-k" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="10" markerHeight="10" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="currentColor"/></marker></defs>
<g stroke="currentColor" stroke-opacity="0.15"><line x1="50" y1="50" x2="50" y2="190"/><line x1="120" y1="50" x2="120" y2="190"/><line x1="190" y1="50" x2="190" y2="190"/><line x1="260" y1="50" x2="260" y2="190"/><line x1="330" y1="50" x2="330" y2="190"/><line x1="50" y1="190" x2="330" y2="190"/><line x1="50" y1="120" x2="330" y2="120"/><line x1="50" y1="50" x2="330" y2="50"/></g><g stroke="currentColor" stroke-width="1.5"><line x1="46" y1="190" x2="342" y2="190" marker-end="url(#tl-k)"/><line x1="50" y1="194" x2="50" y2="38" marker-end="url(#tl-k)"/></g>
<text x="50" y="206" text-anchor="middle" fill="currentColor">0</text>
<text x="120" y="206" text-anchor="middle" fill="currentColor">1</text>
<text x="190" y="206" text-anchor="middle" fill="currentColor">2</text>
<text x="260" y="206" text-anchor="middle" fill="currentColor">3</text>
<text x="330" y="206" text-anchor="middle" fill="currentColor">4</text>
<text x="42" y="124" text-anchor="end" fill="currentColor">1</text>
<text x="42" y="54" text-anchor="end" fill="currentColor">2</text>
<polygon points="50,190 190,190 260,120 120,120" fill="#e53935" fill-opacity="0.15" stroke="#e53935" stroke-width="1.5"/>
<polygon points="50,190 120,190 120,120 50,120" fill="#5c6bc0" fill-opacity="0.25" stroke="#5c6bc0" stroke-width="1.5"/>
<line x1="50" y1="190" x2="120" y2="190" stroke="#5c6bc0" stroke-width="2.5"  marker-end="url(#tl-b)"/>
<line x1="50" y1="190" x2="50" y2="120" stroke="#5c6bc0" stroke-width="2.5"  marker-end="url(#tl-b)"/>
<line x1="50" y1="190" x2="190" y2="190" stroke="#e53935" stroke-width="2.5" stroke-dasharray="6 3" marker-end="url(#tl-r)"/>
<line x1="50" y1="190" x2="120" y2="120" stroke="#e53935" stroke-width="2.5"  marker-end="url(#tl-r)"/>
<text x="85.0" y="180.0" fill="#5c6bc0" font-weight="bold" text-anchor="middle">e₁</text>
<text x="60.0" y="132.0" fill="#5c6bc0" font-weight="bold" text-anchor="start">e₂</text>
<text x="190" y="226" fill="#e53935" font-weight="bold" text-anchor="middle">A e₁ = (2, 0)</text>
<text x="128" y="112" fill="#e53935" font-weight="bold" text-anchor="start">A e₂ = (1, 1)</text>
<text x="183.0" y="155.0" fill="#e53935" font-weight="bold" text-anchor="start">luas = 2</text>
</svg></div>

**Cara membaca gambar.** Persegi biru adalah persegi satuan dengan sisi $\mathbf{e}_1 = (1, 0)$ dan $\mathbf{e}_2 = (0, 1)$. Setelah diubah oleh $A$, $\mathbf{e}_1$ pindah ke $A\mathbf{e}_1 = (2, 0)$ (panah merah putus-putus) dan $\mathbf{e}_2$ pindah ke $A\mathbf{e}_2 = (1, 1)$ (panah merah). Persegi berubah menjadi jajar genjang merah dengan titik sudut $(0, 0)$, $(2, 0)$, $(3, 1)$, dan $(1, 1)$. Alasan titik $(3, 1)$: itu $A(1, 1) = A\mathbf{e}_1 + A\mathbf{e}_2 = (2, 0) + (1, 1)$.

Luas persegi satuan adalah 1. Luas jajar genjang: alas 2 dan tinggi 1, yaitu **2**, sama dengan $\det A = 2 \cdot 1 - 1 \cdot 0 = 2$. Jadi **determinan adalah faktor perubahan luas** (menggenapi janji di Part 3).

### 3. Transformasi Umum

Dengan membaca kolom matriks, kita dapat mengenali beberapa transformasi dasar pada bidang:

| Transformasi | Matriks | $A\mathbf{e}_1$ | $A\mathbf{e}_2$ | $\det A$ |
|---|---|---|---|---|
| Peregangan sumbu $x$ dua kali | $\begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix}$ | $(2, 0)$ | $(0, 1)$ | $2$ |
| Pembesaran dua kali | $\begin{bmatrix} 2 & 0 \\ 0 & 2 \end{bmatrix}$ | $(2, 0)$ | $(0, 2)$ | $4$ |
| Pencerminan terhadap sumbu $x$ | $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$ | $(1, 0)$ | $(0, -1)$ | $-1$ |
| Rotasi $90^\circ$ berlawanan jarum jam | $\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ | $(0, 1)$ | $(-1, 0)$ | $1$ |
| Geseran (*shear*) | $\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$ | $(1, 0)$ | $(1, 1)$ | $1$ |
| Proyeksi ke sumbu $x$ | $\begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$ | $(1, 0)$ | $(0, 0)$ | $0$ |

Cara membaca determinan:

- **$\lvert \det \rvert > 1$**: luas membesar; **$< 1$**: luas mengecil; **$= 1$**: luas tetap (rotasi dan geseran).
- **Tanda negatif** ($-1$ pada pencerminan): orientasi terbalik, seperti bayangan di cermin.
- **$\det = 0$** (proyeksi): seluruh bidang runtuh menjadi satu garis. Informasi hilang, sehingga transformasinya **tidak dapat dibalik** dan matriksnya tidak punya invers (Part 3).

Rotasi sebesar sudut $\theta$ ditulis $\begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$. Untuk $\theta = 90^\circ$: $\cos 90^\circ = 0$ dan $\sin 90^\circ = 1$, yang memberi matriks rotasi di tabel. Determinannya $\cos^2\theta + \sin^2\theta = 1$ untuk sudut apa pun, sesuai bahwa rotasi tidak mengubah luas.

### 4. Kombinasi Linear

**Kombinasi linear** dari vektor $\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_k$ adalah vektor yang dibentuk dengan mengalikan setiap vektor dengan skalar lalu menjumlahkannya:

$$
\mathbf{w} = c_1 \mathbf{v}_1 + c_2 \mathbf{v}_2 + \cdots + c_k \mathbf{v}_k
$$

Skalar $c_i$ disebut koefisien. Ini memakai dua operasi dari Part 2 (perkalian skalar dan penjumlahan).

Contoh: $\mathbf{v}_1 = (1, 2)$ dan $\mathbf{v}_2 = (3, 1)$.

- $2\mathbf{v}_1 + \mathbf{v}_2 = (2, 4) + (3, 1) = (5, 5)$.
- $-\mathbf{v}_1 + 2\mathbf{v}_2 = (-1, -2) + (6, 2) = (5, 0)$.

**Matriks kali vektor adalah kombinasi linear kolom.** Misalkan $A$ punya kolom $\mathbf{a}_1$ dan $\mathbf{a}_2$. Maka

$$
A\mathbf{x} = x_1 \mathbf{a}_1 + x_2 \mathbf{a}_2
$$

Contoh: $A = \begin{bmatrix} 1 & 3 \\ 2 & 1 \end{bmatrix}$ (kolom $(1, 2)$ dan $(3, 1)$) dan $\mathbf{x} = (2, 1)$:

- Cara baris dikali kolom (Part 3): $(1 \cdot 2 + 3 \cdot 1,\ 2 \cdot 2 + 1 \cdot 1) = (5, 5)$.
- Cara kombinasi kolom: $2(1, 2) + 1(3, 1) = (5, 5)$.

Keduanya sama. Ini memberi tafsiran baru untuk sistem $A\mathbf{x} = \mathbf{b}$: **adakah kombinasi kolom $A$ yang menghasilkan $\mathbf{b}$?** Solusinya, jika ada, adalah koefisien kombinasi itu.

### 5. Span

**Span** dari sekumpulan vektor adalah **himpunan semua kombinasi linearnya**. Ditulis $\operatorname{span}\{\mathbf{v}_1, \ldots, \mathbf{v}_k\}$.

- Span satu vektor tak nol $\mathbf{v}$: semua kelipatan $c\,\mathbf{v}$, yaitu **sebuah garis** melalui titik asal.
- Span dua vektor yang **tidak sejajar** di bidang: **seluruh bidang**.
- Span dua vektor yang **sejajar**: hanya **sebuah garis**.
- Di ruang tiga dimensi, span dua vektor yang tidak sejajar adalah **sebuah bidang**.

Menguji apakah $\mathbf{b}$ ada di span: selesaikan sistem yang koefisiennya adalah vektor-vektor itu. Contoh: apakah $\mathbf{b} = (5, 5)$ ada di $\operatorname{span}\{(1, 2), (3, 1)\}$? Cari $c_1$ dan $c_2$ dengan $c_1(1, 2) + c_2(3, 1) = (5, 5)$, yaitu $c_1 + 3c_2 = 5$ dan $2c_1 + c_2 = 5$. Dari persamaan kedua $c_2 = 5 - 2c_1$. Substitusi: $c_1 + 15 - 6c_1 = 5$, jadi $-5c_1 = -10$ dan $c_1 = 2$, lalu $c_2 = 1$. **Ya**, dengan koefisien $(2, 1)$ (cocok dengan contoh di atas).

### 6. Independensi Linear

Sekumpulan vektor **bebas linear** (independen) jika **satu-satunya** kombinasi linear yang menghasilkan vektor nol adalah semua koefisien nol:

$$
c_1 \mathbf{v}_1 + \cdots + c_k \mathbf{v}_k = \mathbf{0} \quad\Longrightarrow\quad c_1 = \cdots = c_k = 0
$$

Jika ada kombinasi dengan koefisien tidak semuanya nol yang menghasilkan vektor nol, vektor-vektor itu **bergantung linear** (dependen). Artinya, **setidaknya satu vektor dapat ditulis sebagai kombinasi dari yang lain**, sehingga ia tidak menambah informasi baru.

<div style="max-width:460px;margin:1rem auto"><svg viewBox="0 0 460 230" width="100%" role="img" aria-labelledby="sp-t sp-d" xmlns="http://www.w3.org/2000/svg" style="color:inherit;font-family:inherit;font-size:13px"><title id="sp-t">Independen dan bergantung linear</title><desc id="sp-d">Panel kiri: vektor u = (2, 1) dan v = (-1, 1) tidak sejajar sehingga kombinasinya mengisi seluruh bidang. Panel kanan: u = (2, 1) dan v = (4, 2) sejajar sehingga kombinasinya hanya berupa satu garis.</desc><defs><marker id="sp-b" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="10" markerHeight="10" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#5c6bc0"/></marker></defs><defs><marker id="sp-r" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="10" markerHeight="10" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#e53935"/></marker></defs><defs><marker id="sp-k" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="10" markerHeight="10" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="currentColor"/></marker></defs>
<rect x="18" y="54" width="204" height="170" rx="6" fill="#43a047" fill-opacity="0.08" stroke="currentColor" stroke-opacity="0.25"/>
<g stroke="currentColor" stroke-width="1.3"><line x1="20" y1="150" x2="220" y2="150"/><line x1="80" y1="220" x2="80" y2="56"/></g>
<line x1="80" y1="150" x2="140" y2="120" stroke="#5c6bc0" stroke-width="2.5"  marker-end="url(#sp-b)"/>
<line x1="80" y1="150" x2="50" y2="120" stroke="#e53935" stroke-width="2.5"  marker-end="url(#sp-r)"/>
<text x="148" y="124" fill="#5c6bc0" font-weight="bold">u = (2, 1)</text>
<text x="44" y="112" fill="#e53935" font-weight="bold" text-anchor="middle">v = (-1, 1)</text>
<text x="40" y="72" fill="currentColor">independen</text><text x="120" y="212" text-anchor="middle" fill="currentColor" font-size="12">span = seluruh bidang</text>
<line x1="270" y1="180" x2="456.0" y2="87.0" stroke="#e53935" stroke-width="6" stroke-opacity="0.18"/>
<g stroke="currentColor" stroke-width="1.3"><line x1="270" y1="150" x2="470" y2="150"/><line x1="330" y1="220" x2="330" y2="56"/></g>
<line x1="330" y1="150" x2="390" y2="120" stroke="#5c6bc0" stroke-width="2.5"  marker-end="url(#sp-b)"/>
<line x1="330" y1="150" x2="450" y2="90" stroke="#e53935" stroke-width="2.5" stroke-dasharray="6 3" marker-end="url(#sp-r)"/>
<text x="398" y="142" fill="#5c6bc0" font-weight="bold">u = (2, 1)</text>
<text x="442" y="78" fill="#e53935" font-weight="bold" text-anchor="end">v = (4, 2)</text>
<text x="285" y="72" fill="currentColor">bergantung</text><text x="370" y="212" text-anchor="middle" fill="currentColor" font-size="12">span = satu garis</text>
</svg></div>

**Cara membaca gambar.** Panel kiri: $\mathbf{u} = (2, 1)$ dan $\mathbf{v} = (-1, 1)$ menunjuk ke arah yang berbeda. Kombinasi dari keduanya dapat mencapai titik mana pun di bidang (span = seluruh bidang). Panel kanan: $\mathbf{v} = (4, 2) = 2\mathbf{u}$ berada pada garis yang sama dengan $\mathbf{u}$. Kombinasi keduanya hanya menghasilkan titik-titik pada satu garis (garis merah muda), sehingga $\mathbf{v}$ tidak menambah apa pun.

**Uji cepat untuk dua vektor di bidang:** bentuk matriks dengan kedua vektor sebagai kolom dan hitung determinannya. Determinan **tidak nol** berarti independen; **nol** berarti bergantung.

- $(1, 2)$ dan $(3, 1)$: $\det\begin{bmatrix} 1 & 3 \\ 2 & 1 \end{bmatrix} = 1 - 6 = -5 \neq 0$. **Independen.**
- $(2, 1)$ dan $(4, 2)$: $\det\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix} = 4 - 4 = 0$. **Bergantung** (karena $(4, 2) = 2(2, 1)$).
- $(1, 0)$ dan $(0, 1)$: $\det = 1$. Independen.

**Tiga vektor atau lebih di bidang selalu bergantung** (ada terlalu banyak vektor untuk dua dimensi). Contoh: $(1, 0)$, $(0, 1)$, $(2, 3)$: $(2, 3) = 2(1, 0) + 3(0, 1)$.

**Contoh di ruang tiga dimensi.** $\mathbf{v}_1 = (1, 0, 0)$, $\mathbf{v}_2 = (0, 1, 0)$, $\mathbf{v}_3 = (1, 1, 0)$. Karena $\mathbf{v}_3 = \mathbf{v}_1 + \mathbf{v}_2$, ketiganya bergantung. Semuanya terletak pada bidang $xy$, sehingga spannya hanya sebuah bidang (bukan seluruh ruang).

**Basis dan dimensi.**

- **Basis** adalah sekumpulan vektor yang **independen** dan **span-nya seluruh ruang**. Pada bidang, dua vektor yang tidak sejajar adalah basis, misalnya $\mathbf{e}_1$ dan $\mathbf{e}_2$.
- **Dimensi** ruang adalah banyak vektor dalam basisnya (selalu sama untuk basis mana pun). Bidang berdimensi 2, ruang berdimensi 3.

**Hubungan dengan Part 4 dan Part 3.** Untuk matriks $A$ persegi, hal-hal berikut setara:

- kolom-kolom $A$ **independen**;
- $\det A \neq 0$;
- $A$ punya invers;
- eliminasi menghasilkan **pivot sebanyak kolom** (rank penuh);
- $A\mathbf{x} = \mathbf{b}$ punya tepat satu solusi untuk setiap $\mathbf{b}$.

**Rank** adalah banyak kolom (atau baris) yang independen, sama dengan banyak pivot pada eliminasi. Pada matriks $\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix}$, kolom kedua adalah dua kali kolom pertama, jadi rank 1 (bukan 2), $\det = 0$, dan tidak ada invers.

### 7. Pengantar Nilai Eigen dan Vektor Eigen (Pengayaan)

!!! info "Subbagian pengayaan"
    Bagian ini memperkenalkan gagasannya. Ia dibutuhkan lagi untuk PCA di level berikutnya, tetapi tidak wajib untuk bagian lain di Level 2.

Pada kebanyakan transformasi, sebuah vektor berubah **arah** dan **panjang**. Vektor yang istimewa adalah yang **arahnya tidak berubah**: hanya diperpanjang atau diperpendek. Vektor $\mathbf{v} \neq \mathbf{0}$ disebut **vektor eigen** dari $A$ jika

$$
A\mathbf{v} = \lambda\,\mathbf{v}
$$

dengan skalar $\lambda$ yang disebut **nilai eigen** (*eigenvalue*). Kata *eigen* berarti "milik sendiri" (dari bahasa Jerman).

**Contoh.** $A = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$.

- $A(1, 1) = (2 + 1,\ 1 + 2) = (3, 3) = 3\,(1, 1)$. Arah tetap, panjang menjadi tiga kali: $\lambda = 3$.
- $A(1, -1) = (2 - 1,\ 1 - 2) = (1, -1) = 1\,(1, -1)$. Arah tetap, panjang tetap: $\lambda = 1$.
- Sebaliknya $(1, 0)$ bukan vektor eigen: $A(1, 0) = (2, 1)$ bukan kelipatan $(1, 0)$.

**Cara mencari nilai eigen matriks $2 \times 2$.** Persamaan $A\mathbf{v} = \lambda\mathbf{v}$ setara dengan $(A - \lambda I)\mathbf{v} = \mathbf{0}$. Agar ada $\mathbf{v}$ bukan nol, matriks $A - \lambda I$ harus singular, yaitu determinannya nol:

$$
\det(A - \lambda I) = 0
$$

Untuk contoh di atas:

$$
\det\begin{bmatrix} 2 - \lambda & 1 \\ 1 & 2 - \lambda \end{bmatrix}
= (2 - \lambda)^2 - 1 = \lambda^2 - 4\lambda + 3 = (\lambda - 1)(\lambda - 3) = 0
$$

Jadi $\lambda = 1$ atau $\lambda = 3$. Untuk $\lambda = 3$: $A - 3I = \begin{bmatrix} -1 & 1 \\ 1 & -1 \end{bmatrix}$, dan $(A - 3I)\mathbf{v} = \mathbf{0}$ memberi $-v_1 + v_2 = 0$, yaitu $\mathbf{v} = (1, 1)$ (atau kelipatannya).

**Pemeriksaan cepat.** Jumlah nilai eigen sama dengan **trace** (jumlah diagonal) $= 2 + 2 = 4 = 3 + 1$, dan hasil kalinya sama dengan determinan $= 4 - 1 = 3 = 3 \times 1$.

Tidak semua matriks punya nilai eigen **real**: rotasi $90^\circ$ memutar setiap vektor sehingga tidak ada arah yang tetap. Persamaannya $\lambda^2 + 1 = 0$ tidak punya akar real.

**Mengapa penting di ML (pratinjau).** Arah-arah utama sebaran data (PCA) adalah vektor eigen dari matriks kovarians data, dan nilai eigennya menyatakan seberapa besar sebaran sepanjang arah itu. Level 4 membahasnya.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Transformasi linear | pemetaan vektor yang menjaga penjumlahan dan perkalian skalar | $T(\mathbf{x}) = A\mathbf{x}$ |
| Vektor basis standar | vektor satuan sepanjang sumbu | $\mathbf{e}_1 = (1, 0)$ |
| Persegi satuan | persegi dengan sisi $\mathbf{e}_1$ dan $\mathbf{e}_2$ | luas 1 |
| Geseran (*shear*) | transformasi yang memiringkan bidang | $\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$ |
| Proyeksi | transformasi yang meratakan ke garis | $\det = 0$ |
| Kombinasi linear | jumlah kelipatan vektor | $2\mathbf{v}_1 + \mathbf{v}_2$ |
| Span | himpunan semua kombinasi linear | garis atau bidang |
| Bebas (independen) linear | tidak ada vektor yang kombinasi lainnya | $(1,2)$ dan $(3,1)$ |
| Bergantung (dependen) linear | ada vektor yang kombinasi lainnya | $(2,1)$ dan $(4,2)$ |
| Basis | vektor independen yang span-nya seluruh ruang | $\mathbf{e}_1, \mathbf{e}_2$ |
| Dimensi | banyak vektor dalam basis | 2 untuk bidang |
| Rank | banyak kolom independen (banyak pivot) | 1 atau 2 |
| Nilai eigen $\lambda$ | faktor pengali arah yang tetap | $3$ |
| Vektor eigen | vektor yang arahnya tak berubah | $(1, 1)$ |

## Rumus dan Penjelasan Setiap Simbol

**Transformasi linear oleh matriks:**

$$
T(\mathbf{x}) = A\mathbf{x}, \qquad T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v}), \qquad T(c\mathbf{u}) = c\,T(\mathbf{u})
$$

- $A$ matriks, $\mathbf{x}$ vektor dengan banyak elemen sama dengan banyak kolom $A$, $c$ skalar.

**Kolom sebagai gambar vektor basis:** $A\mathbf{e}_j$ adalah kolom ke-$j$ dari $A$.

**Perkalian sebagai kombinasi kolom:**

$$
A\mathbf{x} = x_1 \mathbf{a}_1 + x_2 \mathbf{a}_2 + \cdots + x_n \mathbf{a}_n
$$

- $\mathbf{a}_j$ adalah kolom ke-$j$ dari $A$ dan $x_j$ adalah elemen ke-$j$ dari $\mathbf{x}$.

**Faktor luas:** luas bayangan $= \lvert \det A \rvert \times$ luas semula (untuk $A$ berukuran $2 \times 2$).

**Uji independen dua vektor di bidang:** $\det[\mathbf{u}\ \mathbf{v}] \neq 0$.

**Nilai eigen:**

$$
A\mathbf{v} = \lambda\mathbf{v}, \qquad \det(A - \lambda I) = 0
$$

- $\lambda$ skalar, $\mathbf{v} \neq \mathbf{0}$, $I$ matriks identitas. Untuk $2 \times 2$: $\lambda^2 - (\operatorname{tr} A)\lambda + \det A = 0$, dengan $\operatorname{tr} A = a + d$.

## Penjelasan Langkah demi Langkah

**Menggambar efek matriks $2 \times 2$ pada persegi satuan:**

1. Baca kolom 1 dan kolom 2 dari $A$: itu $A\mathbf{e}_1$ dan $A\mathbf{e}_2$.
2. Gambar kedua panah dari titik asal.
3. Lengkapi jajar genjang: titik keempatnya adalah jumlah kedua panah, $A\mathbf{e}_1 + A\mathbf{e}_2$.
4. Hitung luas $= \lvert \det A \rvert$ dan periksa dengan gambar.

## Contoh Soal dan Pembahasan

### Contoh 1 — Membaca transformasi dari matriks

Untuk $A = \begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}$: ke mana $\mathbf{e}_1$ dan $\mathbf{e}_2$ pindah, berapa $A(2, 1)$, dan berapa faktor luasnya?

1. Kolom 1: $A\mathbf{e}_1 = (3, 0)$. Kolom 2: $A\mathbf{e}_2 = (1, 2)$.
2. $A(2, 1) = 2\,(3, 0) + 1\,(1, 2) = (6, 0) + (1, 2) = (7, 2)$.
3. Cek dengan baris dikali kolom: $(3 \cdot 2 + 1 \cdot 1,\ 0 \cdot 2 + 2 \cdot 1) = (7, 2)$. Sama.
4. $\det A = 3 \cdot 2 - 1 \cdot 0 = 6$. Luas diperbesar 6 kali.

### Contoh 2 — Kombinasi linear dan span

Apakah $(5, 0)$ ada di span $\{(1, 2), (3, 1)\}$? Bagaimana dengan $(1, 0)$ di span $\{(2, 4)\}$?

1. Pertama: cari $c_1, c_2$ dengan $c_1 + 3c_2 = 5$ dan $2c_1 + c_2 = 0$. Dari persamaan kedua $c_2 = -2c_1$. Substitusi: $c_1 - 6c_1 = 5$, jadi $c_1 = -1$ dan $c_2 = 2$. **Ya**: $-1(1, 2) + 2(3, 1) = (5, 0)$ (cocok dengan contoh sebelumnya).
2. Kedua: span $\{(2, 4)\}$ adalah semua $c(2, 4) = (2c, 4c)$. Untuk $(1, 0)$ butuh $2c = 1$ dan $4c = 0$, yang tidak mungkin bersamaan. **Tidak.**

### Contoh 3 — Independensi linear

Tentukan apakah $(1, 2)$, $(3, 1)$ independen; apakah $(1, 0, 0)$, $(0, 1, 0)$, $(1, 1, 0)$ independen; dan apakah $(1, 0)$, $(0, 1)$, $(2, 3)$ independen.

1. $(1, 2)$ dan $(3, 1)$: $\det = 1 \cdot 1 - 3 \cdot 2 = -5 \neq 0$. **Independen.**
2. Tiga vektor di ruang: $(1, 1, 0) = (1, 0, 0) + (0, 1, 0)$. **Bergantung.** Eliminasi memberi rank 2, bukan 3.
3. Tiga vektor di bidang selalu bergantung: $(2, 3) = 2(1, 0) + 3(0, 1)$. **Bergantung.**

### Contoh 4 — Nilai eigen dan vektor eigen

Cari nilai eigen $A = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$ dan periksa vektor eigennya.

1. Persamaan karakteristik: $\det(A - \lambda I) = (2 - \lambda)^2 - 1 = \lambda^2 - 4\lambda + 3 = 0$.
2. Faktor: $(\lambda - 1)(\lambda - 3) = 0$, jadi $\lambda_1 = 3$ dan $\lambda_2 = 1$.
3. Untuk $\lambda = 3$: $-v_1 + v_2 = 0$ memberi $\mathbf{v} = (1, 1)$. Periksa $A(1, 1) = (3, 3) = 3(1, 1)$.
4. Untuk $\lambda = 1$: $A - I = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}$, jadi $v_1 + v_2 = 0$, $\mathbf{v} = (1, -1)$. Periksa $A(1, -1) = (1, -1) = 1\,(1, -1)$.
5. Cek: jumlah $3 + 1 = 4 = \operatorname{tr} A$ dan hasil kali $3 \cdot 1 = 3 = \det A$.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** Pencerminan terhadap sumbu $x$ mengubah $(x, y)$ menjadi $(x, -y)$: matriksnya $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$. Titik $(3, 2)$ menjadi $(3, -2)$.

**Sedang.** Rotasi $90^\circ$: $\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ mengubah $(2, 1)$ menjadi $(0 \cdot 2 - 1 \cdot 1,\ 1 \cdot 2 + 0) = (-1, 2)$. Panjangnya sama: $\sqrt{4 + 1} = \sqrt{1 + 4}$.

**Sedang.** Proyeksi $\begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$ mengubah $(3, 5)$ dan $(3, -7)$ keduanya menjadi $(3, 0)$. Dua vektor berbeda menjadi sama, sehingga tidak dapat dikembalikan ($\det = 0$).

**Menantang.** Matriks $\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$ punya nilai eigen $2$ dan $3$ dengan vektor eigen $\mathbf{e}_1$ dan $\mathbf{e}_2$: $A\mathbf{e}_1 = 2\mathbf{e}_1$ dan $A\mathbf{e}_2 = 3\mathbf{e}_2$. Matriks diagonal meregangkan setiap sumbu secara terpisah.

## Visualisasi atau Interpretasi Grafik

- **Jajar genjang pada gambar pertama** menunjukkan seluruh isi matriks: kolom sebagai gambar vektor basis, luas sebagai determinan.
- **Panel independen dan bergantung** menunjukkan beda antara span yang mengisi bidang dan span yang hanya garis.
- **Vektor eigen** adalah panah yang tetap berada di garis yang sama setelah transformasi; hanya panjangnya berubah.
- **Matriks singular** ($\det = 0$) selalu meratakan bidang menjadi garis atau titik.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menganggap geseran $(x+1, y)$ transformasi linear | titik asal berpindah | linear harus memetakan $\mathbf{0}$ ke $\mathbf{0}$ |
| Membaca baris matriks sebagai gambar $\mathbf{e}_1$ dan $\mathbf{e}_2$ | gambar vektor basis adalah **kolom** | $A\mathbf{e}_j$ = kolom ke-$j$ |
| Menganggap determinan negatif berarti luas negatif | tanda menandai orientasi terbalik | luas $= \lvert \det \rvert$ |
| Menganggap dua vektor dengan elemen berbeda pasti independen | $(2, 1)$ dan $(4, 2)$ berbeda tetapi sejajar | uji determinan |
| Menganggap tiga vektor di bidang bisa independen | maksimal 2 vektor independen di bidang | pasti bergantung |
| Mengira span dua vektor selalu seluruh bidang | hanya bila tidak sejajar | sejajar memberi garis |
| Menganggap vektor eigen itu unik | setiap kelipatan tak nol dari vektor eigen juga vektor eigen dengan $\lambda$ yang sama | $(1, 1)$, $(2, 2)$, $(-3, -3)$ semuanya vektor eigen dengan $\lambda = 3$ |
| Menyatakan $\mathbf{0}$ sebagai vektor eigen | definisi mensyaratkan $\mathbf{v} \neq \mathbf{0}$ | pilih $\mathbf{v}$ bukan nol |
| Mengira semua matriks punya nilai eigen real | rotasi $90^\circ$ tidak punya | periksa diskriminan |
| Menganggap matriks singular dapat dibalik dengan "pembulatan" | informasi hilang | tidak ada invers |

## Hubungan dengan Machine Learning

- **Lapisan jaringan saraf adalah transformasi linear** (ditambah nonlinearitas). Bobot $W$ memetakan vektor masukan ke vektor keluaran: $W\mathbf{x}$.
- **Fitur yang bergantung linear** (misalnya tinggi dalam cm dan tinggi dalam inci, yang satu adalah kelipatan lainnya) membuat kolom matriks fitur $X$ bergantung. Rank $X$ lebih kecil dari banyak kolom, $X^{\top}X$ singular, dan bobot model tidak unik (Part 4). Pada data nyata, ketergantungan hampir sempurna juga menimbulkan masalah numerik.
- **Span dan kapasitas model.** Model linear dapat membentuk keluaran yang berada di span dari fitur-fiturnya. Fitur baru yang independen memperluas jangkauan model; fitur yang bergantung tidak menambah apa pun.
- **Determinan dan invers.** Transformasi dengan $\det = 0$ menghilangkan informasi, jadi tidak dapat dibalik.
- **Nilai eigen dan vektor eigen (pengayaan).** Dipakai pada PCA, pada analisis kestabilan pelatihan, dan pada algoritma peringkat halaman.

### Python: transformasi, kombinasi linear, independensi, dan nilai eigen

```python
import math

def matvec(A, x):
    return [sum(a * b for a, b in zip(baris, x)) for baris in A]

def det2(A):
    return A[0][0] * A[1][1] - A[0][1] * A[1][0]

A = [[2, 1], [0, 1]]
print(matvec(A, [1, 0]), matvec(A, [0, 1]))   # [2, 0] [1, 1], kolom-kolom A
print(matvec(A, [1, 1]))                       # [3, 1], titik sudut jajar genjang
print(det2(A))                                 # 2, faktor luas
```

Kolom matriks adalah gambar vektor basis. Memeriksa sifat linear dengan angka:

```python
u, v = [1, 2], [3, -1]
jumlah = [a + b for a, b in zip(u, v)]
print(matvec(A, jumlah))                                   # [9, 1]
print([a + b for a, b in zip(matvec(A, u), matvec(A, v))]) # [9, 1], sama
print(matvec(A, [3 * a for a in u]))                       # [12, 6]
print([3 * a for a in matvec(A, u)])                       # [12, 6], sama
```

Matriks kali vektor sebagai kombinasi linear kolom:

```python
def kombinasi(koef, vektor):
    n = len(vektor[0])
    return [sum(c * vk[i] for c, vk in zip(koef, vektor)) for i in range(n)]

B = [[1, 3], [2, 1]]
print(matvec(B, [2, 1]))                    # [5, 5]
print(kombinasi([2, 1], [[1, 2], [3, 1]]))  # [5, 5], 2*(1,2) + 1*(3,1)
print(kombinasi([-1, 2], [[1, 2], [3, 1]])) # [5, 0]
```

Uji independensi dua vektor dengan determinan, dan contoh tiga dimensi:

```python
def independen(u, v):
    return det2([[u[0], v[0]], [u[1], v[1]]]) != 0

print(independen([1, 2], [3, 1]))   # True
print(independen([2, 1], [4, 2]))   # False, karena (4,2) = 2*(2,1)
print(independen([1, 0], [0, 1]))   # True

print(kombinasi([1, 1], [[1, 0, 0], [0, 1, 0]]))   # [1, 1, 0], sama dengan v3
```

Nilai eigen matriks $2 \times 2$ dari rumus $\lambda^2 - (\operatorname{tr} A)\lambda + \det A = 0$, lalu pemeriksaan $A\mathbf{v} = \lambda\mathbf{v}$:

```python
def nilai_eigen(A):
    tr = A[0][0] + A[1][1]
    d = det2(A)
    disc = tr ** 2 - 4 * d
    if disc < 0:
        return "tidak ada nilai eigen real"
    akar = math.sqrt(disc)
    return ((tr + akar) / 2, (tr - akar) / 2)

C = [[2, 1], [1, 2]]
print(nilai_eigen(C))                   # (3.0, 1.0)
print(matvec(C, [1, 1]))                # [3, 3], sama dengan 3 * (1, 1)
print(matvec(C, [1, -1]))               # [1, -1], sama dengan 1 * (1, -1)
print(nilai_eigen([[0, -1], [1, 0]]))   # tidak ada nilai eigen real (rotasi 90 derajat)
```

Fitur yang bergantung membuat prediksi tidak unik:

```python
X = [[1, 2], [2, 4], [3, 6]]   # kolom kedua = 2 * kolom pertama
for w in ([2, 0], [0, 1]):
    print(matvec(X, w))   # [2, 4, 6] untuk keduanya
```

## Latihan Mandiri

1. Apakah linear? (a) $T(x, y) = (2x + y,\ x - y)$ (b) $T(x, y) = (x + 1,\ y)$ (c) $T(x, y) = (x^2,\ y)$. Jika bukan, tunjukkan dengan angka.
2. Untuk $A = \begin{bmatrix} 3 & 1 \\ 0 & 2 \end{bmatrix}$: tentukan $A\mathbf{e}_1$, $A\mathbf{e}_2$, dan $A(2, 1)$ lewat kombinasi kolom. Periksa dengan baris dikali kolom.
3. Kenali transformasi dan determinannya: (a) $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$ (b) $\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ (c) $\begin{bmatrix} 2 & 0 \\ 0 & 2 \end{bmatrix}$ (d) $\begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$
4. Persegi satuan diubah oleh $A = \begin{bmatrix} 2 & 1 \\ 0 & 1 \end{bmatrix}$. Tentukan keempat titik sudut bayangannya dan luasnya.
5. Dengan $\mathbf{v}_1 = (1, 2)$ dan $\mathbf{v}_2 = (3, 1)$, hitung $2\mathbf{v}_1 + \mathbf{v}_2$ dan $-\mathbf{v}_1 + 2\mathbf{v}_2$.
6. Apakah ada di span? (a) $(5, 5)$ di span $\{(1, 2), (3, 1)\}$ (b) $(1, 0)$ di span $\{(2, 4)\}$ (c) $(3, 6)$ di span $\{(1, 2)\}$
7. Tentukan independen atau bergantung: (a) $(1, 2)$ dan $(3, 1)$ (b) $(2, 1)$ dan $(4, 2)$ (c) $(1, 0)$, $(0, 1)$, $(2, 3)$
8. Apakah $(1, 0, 0)$, $(0, 1, 0)$, $(1, 1, 0)$ independen? Berapa dimensi spannya?
9. Untuk $A = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$ (pengayaan): periksa bahwa $(1, 1)$ vektor eigen dengan $\lambda = 3$ dan $(1, -1)$ dengan $\lambda = 1$. Cari nilai eigen dari $\det(A - \lambda I) = 0$ dan periksa dengan trace dan determinan. Apakah $(1, 0)$ vektor eigen?
10. (ML) Matriks fitur $X = \begin{bmatrix} 1 & 2 \\ 2 & 4 \\ 3 & 6 \end{bmatrix}$. Apa hubungan kedua kolom? Hitung $X\mathbf{w}$ untuk $\mathbf{w} = (2, 0)$ dan $\mathbf{w} = (0, 1)$. Apa artinya bagi bobot model?

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) **Linear**: $T(x, y) = \begin{bmatrix} 2 & 1 \\ 1 & -1 \end{bmatrix}\begin{bmatrix} x \\ y \end{bmatrix}$. (b) **Bukan**: $T(0, 0) = (1, 0) \neq (0, 0)$. (c) **Bukan**: $T(2, 0) = (4, 0)$ tetapi $2\,T(1, 0) = 2(1, 0) = (2, 0)$.

??? success "Soal 2"
    $A\mathbf{e}_1 = (3, 0)$ dan $A\mathbf{e}_2 = (1, 2)$. $A(2, 1) = 2(3, 0) + 1(1, 2) = (7, 2)$. Baris dikali kolom: $(3 \cdot 2 + 1 \cdot 1,\ 0 \cdot 2 + 2 \cdot 1) = (7, 2)$.

??? success "Soal 3"
    (a) pencerminan terhadap sumbu $x$, $\det = -1$. (b) rotasi $90^\circ$ berlawanan jarum jam, $\det = 1$. (c) pembesaran dua kali, $\det = 4$. (d) proyeksi ke sumbu $x$, $\det = 0$ (tidak dapat dibalik).

??? success "Soal 4"
    $A(0, 0) = (0, 0)$. $A(1, 0) = (2, 0)$. $A(1, 1) = (3, 1)$. $A(0, 1) = (1, 1)$. Luas $= \lvert \det A \rvert = 2$ (alas 2, tinggi 1).

??? success "Soal 5"
    $2\mathbf{v}_1 + \mathbf{v}_2 = (2, 4) + (3, 1) = (5, 5)$. $-\mathbf{v}_1 + 2\mathbf{v}_2 = (-1, -2) + (6, 2) = (5, 0)$.

??? success "Soal 6"
    (a) **Ya**: $2(1, 2) + 1(3, 1) = (5, 5)$. (b) **Tidak**: butuh $2c = 1$ dan $4c = 0$. (c) **Ya**: $3(1, 2) = (3, 6)$.

??? success "Soal 7"
    (a) $\det = 1 - 6 = -5 \neq 0$: **independen**. (b) $\det = 4 - 4 = 0$ ($(4, 2) = 2(2, 1)$): **bergantung**. (c) Tiga vektor di bidang: **bergantung**, karena $(2, 3) = 2(1, 0) + 3(0, 1)$.

??? success "Soal 8"
    **Bergantung**, karena $(1, 1, 0) = (1, 0, 0) + (0, 1, 0)$. Spannya adalah bidang $xy$, jadi dimensinya **2**.

??? success "Soal 9"
    $A(1, 1) = (3, 3) = 3(1, 1)$ dan $A(1, -1) = (1, -1) = 1(1, -1)$. $\det(A - \lambda I) = (2 - \lambda)^2 - 1 = \lambda^2 - 4\lambda + 3 = 0$, jadi $\lambda = 1$ atau $3$. Trace $= 4 = 1 + 3$, determinan $= 3 = 1 \cdot 3$. $(1, 0)$ **bukan** vektor eigen: $A(1, 0) = (2, 1)$ bukan kelipatan $(1, 0)$.

??? success "Soal 10"
    Kolom kedua adalah dua kali kolom pertama (kolom bergantung, rank 1). $X(2, 0) = 2(1, 2, 3) = (2, 4, 6)$ dan $X(0, 1) = (2, 4, 6)$. Dua bobot berbeda memberi prediksi yang persis sama, sehingga bobot **tidak unik** dan tidak dapat ditentukan dari data.

## Ringkasan Materi

- Matriks adalah transformasi linear: $T(\mathbf{x}) = A\mathbf{x}$ menjaga penjumlahan dan perkalian skalar dan memetakan $\mathbf{0}$ ke $\mathbf{0}$.
- **Kolom ke-$j$ dari $A$** adalah gambar vektor basis $\mathbf{e}_j$. $\lvert \det A \rvert$ adalah faktor perubahan luas; tanda negatif berarti orientasi terbalik; nol berarti bidang runtuh (tanpa invers).
- Kombinasi linear: $\sum c_i \mathbf{v}_i$. $A\mathbf{x}$ adalah kombinasi linear kolom-kolom $A$ dengan koefisien elemen $\mathbf{x}$.
- Span adalah himpunan semua kombinasi linear. Dua vektor tak sejajar di bidang meng-span seluruh bidang.
- Vektor independen jika tak ada yang kombinasi lainnya. Untuk dua vektor di bidang: $\det \neq 0$. Basis = independen + span seluruh ruang; dimensi = banyak vektor basis. Rank = banyak kolom independen.
- (Pengayaan) $A\mathbf{v} = \lambda\mathbf{v}$: vektor eigen mempertahankan arah; nilai eigen dari $\det(A - \lambda I) = 0$; jumlahnya trace, hasil kalinya determinan.
- ML: fitur bergantung membuat parameter tidak unik; lapisan jaringan saraf adalah transformasi linear.

## Checklist Penguasaan

- [ ] Menjelaskan dua sifat transformasi linear dan memeriksanya dengan angka.
- [ ] Membaca kolom matriks sebagai gambar $\mathbf{e}_1$ dan $\mathbf{e}_2$ dan menggambar jajar genjang bayangan.
- [ ] Mengenali pencerminan, rotasi, geseran, proyeksi, dan menyebut determinannya.
- [ ] Menghitung kombinasi linear dan menulis $A\mathbf{x}$ sebagai kombinasi kolom.
- [ ] Menentukan apakah sebuah vektor ada di span sekumpulan vektor.
- [ ] Menguji independensi dua vektor dengan determinan dan menjelaskan basis, dimensi, dan rank.
- [ ] Menghubungkan kolom bergantung dengan determinan nol dan invers yang tidak ada.
- [ ] (Pengayaan) Memeriksa vektor eigen dan mencari nilai eigen matriks $2 \times 2$.

## Prasyarat untuk Materi Berikutnya

[Part 6 — Limit dan Kontinuitas](06-limit-dan-kontinuitas.md) memulai jalur kalkulus dan tidak membutuhkan Part 5. Part 8 — Turunan Parsial dan Gradien kembali memakai vektor, dan Part 12 menggabungkan seluruh aljabar linear (Part 1 sampai 5) dalam evaluasi akhir Level 2. Pengantar nilai eigen akan dipakai lagi di Level 4 untuk PCA.
