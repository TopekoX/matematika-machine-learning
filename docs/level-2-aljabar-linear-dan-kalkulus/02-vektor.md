# Part 2 — Vektor

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjumlah, mengurang, dan mengalikan vektor dengan skalar, baik secara hitung maupun secara geometri;
- menghitung norma L1 dan L2 serta membuat vektor satuan;
- menghitung jarak Euclidean dan jarak Manhattan antara dua titik data;
- menghitung dot product dan menafsirkannya, termasuk menentukan apakah dua vektor tegak lurus;
- menjelaskan dan menghitung cosine similarity;
- menulis prediksi model linear sebagai $\mathbf{w} \cdot \mathbf{x} + b$.

## Prasyarat

[Part 1 — Pengantar Aljabar Linear](01-pengantar-aljabar-linear.md) (vektor, elemen, dimensi). Juga dibutuhkan: [akar dan kuadrat](../level-0-fondasi-matematika/05-pangkat-dan-akar.md), [nilai absolut](../level-0-fondasi-matematika/06-nilai-absolut-dan-notasi-ilmiah.md), serta notasi $\sum$ dari [Level 0 — Part 8](../level-0-fondasi-matematika/08-notasi-matematika-dasar.md). [Level 1 — Part 11](../level-1-aljabar-dasar-dan-fungsi/11-pengantar-trigonometri.md) membahas kosinus, tetapi ringkasannya diulang di bagian cosine similarity sehingga part itu tidak wajib.

## Mengapa Materi Ini Penting?

Operasi pada vektor adalah "kosakata" yang dipakai terus di Machine Learning: menjumlahkan data, mengukur seberapa jauh dua data, mengukur seberapa mirip dua data, dan menghitung prediksi dari banyak fitur sekaligus. Algoritma seperti k-nearest neighbors, sistem rekomendasi, dan pencarian teks bertumpu pada jarak dan kemiripan. Prediksi regresi linear adalah sebuah dot product.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Vektor sebagai Panah

Dari Part 1: vektor dua elemen $(3, 2)$ dapat dipandang sebagai **panah** dari titik asal ke titik $(3, 2)$. Elemen vektor menyatakan **pergeseran**: 3 ke kanan, lalu 2 ke atas. Panah memiliki **arah** dan **panjang**.

Pandangan panah membantu memahami operasi vektor. Untuk vektor berdimensi 3 atau lebih, kita tidak dapat menggambar panah, tetapi aturan hitungnya sama.

### 2. Penjumlahan dan Pengurangan Vektor

**Penjumlahan** dilakukan **elemen demi elemen**, dengan syarat kedua vektor **berdimensi sama**:

$$
\mathbf{u} + \mathbf{v} =
\begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \end{bmatrix}
$$

Contoh: $\mathbf{u} = (1, 3)$ dan $\mathbf{v} = (3, 2)$.

- $\mathbf{u} + \mathbf{v} = (1 + 3,\ 3 + 2) = (4, 5)$.
- $\mathbf{u} - \mathbf{v} = (1 - 3,\ 3 - 2) = (-2, 1)$.
- $\mathbf{v} - \mathbf{u} = (3 - 1,\ 2 - 3) = (2, -1)$.

Syarat dimensi sama: $(1, 2, 3) + (4, 5)$ **tidak terdefinisi**, sebab elemen ketiga tidak punya pasangan.

<div style="max-width:420px;margin:1rem auto"><svg viewBox="0 0 420 370" width="100%" role="img" aria-labelledby="vp-t vp-d" xmlns="http://www.w3.org/2000/svg" style="color:inherit;font-family:inherit;font-size:13px"><title id="vp-t">Penjumlahan dua vektor</title><desc id="vp-d">Vektor u = (1, 3) berwarna biru, vektor v = (3, 2) berwarna merah, dan jumlahnya u + v = (4, 5) berwarna hijau. Garis putus-putus menunjukkan jajar genjang dari kedua vektor.</desc><defs><marker id="vp-b" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#5c6bc0"/></marker></defs><defs><marker id="vp-r" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#e53935"/></marker></defs><defs><marker id="vp-g" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#43a047"/></marker></defs><defs><marker id="vp-k" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="currentColor"/></marker></defs>
<g stroke="currentColor" stroke-opacity="0.15"><line x1="50" y1="330" x2="50" y2="30"/><line x1="110" y1="330" x2="110" y2="30"/><line x1="170" y1="330" x2="170" y2="30"/><line x1="230" y1="330" x2="230" y2="30"/><line x1="290" y1="330" x2="290" y2="30"/><line x1="350" y1="330" x2="350" y2="30"/><line x1="50" y1="330" x2="350" y2="330"/><line x1="50" y1="270" x2="350" y2="270"/><line x1="50" y1="210" x2="350" y2="210"/><line x1="50" y1="150" x2="350" y2="150"/><line x1="50" y1="90" x2="350" y2="90"/><line x1="50" y1="30" x2="350" y2="30"/></g><g stroke="currentColor" stroke-width="1.5"><line x1="50" y1="330" x2="360" y2="330" marker-end="url(#vp-k)"/><line x1="50" y1="330" x2="50" y2="20" marker-end="url(#vp-k)"/></g><g fill="currentColor"><text x="50" y="346" text-anchor="middle">0</text><text x="110" y="346" text-anchor="middle">1</text><text x="170" y="346" text-anchor="middle">2</text><text x="230" y="346" text-anchor="middle">3</text><text x="290" y="346" text-anchor="middle">4</text><text x="350" y="346" text-anchor="middle">5</text><text x="42" y="274" text-anchor="end">1</text><text x="42" y="214" text-anchor="end">2</text><text x="42" y="154" text-anchor="end">3</text><text x="42" y="94" text-anchor="end">4</text><text x="42" y="34" text-anchor="end">5</text><text x="364" y="346" font-style="italic">x</text><text x="58" y="18" font-style="italic">y</text></g>
<line x1="50" y1="330" x2="110" y2="150" stroke="#5c6bc0" stroke-width="2.5"  marker-end="url(#vp-b)"/>
<line x1="50" y1="330" x2="230" y2="210" stroke="#e53935" stroke-width="2.5"  marker-end="url(#vp-r)"/>
<line x1="50" y1="330" x2="290" y2="30" stroke="#43a047" stroke-width="3"  marker-end="url(#vp-g)"/>
<line x1="110" y1="150" x2="290" y2="30" stroke="#e53935" stroke-width="1.5" stroke-dasharray="5 4" opacity="0.7" marker-end="url(#vp-r)"/>
<line x1="230" y1="210" x2="290" y2="30" stroke="#5c6bc0" stroke-width="1.5" stroke-dasharray="5 4" opacity="0.7" marker-end="url(#vp-b)"/>
<text x="120" y="146" fill="#5c6bc0" font-weight="bold">u = (1, 3)</text>
<text x="238" y="214" fill="#e53935" font-weight="bold">v = (3, 2)</text>
<text x="298" y="26" fill="#43a047" font-weight="bold">u + v = (4, 5)</text>
</svg></div>

**Cara membaca gambar.** Panah biru adalah $\mathbf{u} = (1, 3)$ dan panah merah adalah $\mathbf{v} = (3, 2)$, keduanya dari titik asal. Panah hijau adalah $\mathbf{u} + \mathbf{v} = (4, 5)$. Garis putus-putus menunjukkan cara membentuknya: dari ujung $\mathbf{u}$ bergeraklah sejauh $\mathbf{v}$ (garis merah putus-putus), atau dari ujung $\mathbf{v}$ bergeraklah sejauh $\mathbf{u}$ (garis biru putus-putus). Keduanya berakhir di titik yang sama, $(4, 5)$. Artinya urutan tidak penting: $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$.

Secara geometri, $\mathbf{u} - \mathbf{v}$ adalah panah dari **ujung $\mathbf{v}$ ke ujung $\mathbf{u}$**. Pada gambar, itu panah dari $(3, 2)$ ke $(1, 3)$, yaitu pergeseran $(-2, 1)$.

### 3. Perkalian Vektor dengan Skalar

**Perkalian dengan skalar** $c$ mengalikan **setiap elemen** dengan $c$:

$$
c\,\mathbf{v} = \begin{bmatrix} c\,v_1 \\ c\,v_2 \end{bmatrix}
$$

Contoh dengan $\mathbf{u} = (1, 3)$ dan $\mathbf{v} = (3, 2)$:

- $3\mathbf{u} = (3, 9)$ (panah searah, tiga kali lebih panjang).
- $-2\mathbf{v} = (-6, -4)$ (panah berbalik arah, dua kali lebih panjang).
- $\tfrac{1}{2}(4, 6) = (2, 3)$ (separuh panjang).
- $0 \cdot \mathbf{v} = (0, 0)$, yaitu **vektor nol**.

Aturan: $c > 1$ memperpanjang, $0 < c < 1$ memperpendek, $c < 0$ membalik arah, dan $c = 0$ menghasilkan vektor nol.

**Kombinasi.** Penjumlahan dan perkalian skalar boleh digabung: $3\mathbf{u} - 2\mathbf{v} = (3, 9) - (6, 4) = (-3, 5)$.

**Sifat yang berlaku** (dapat dicek dengan angka): $c(\mathbf{u} + \mathbf{v}) = c\mathbf{u} + c\mathbf{v}$. Untuk $c = 2$: $2(4, 5) = (8, 10)$ dan $2(1, 3) + 2(3, 2) = (2, 6) + (6, 4) = (8, 10)$.

### 4. Norma Vektor

**Norma** (*norm*) adalah ukuran **panjang** sebuah vektor. Ada beberapa jenis; dua yang paling sering dipakai:

**Norma L2 (Euclidean).** Panjang "garis lurus", diperoleh dari teorema Pythagoras (Level 1, Part 5):

$$
\lVert \mathbf{v} \rVert_2 = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2}
$$

Contoh: $\mathbf{v} = (3, 4)$. $\lVert \mathbf{v} \rVert_2 = \sqrt{9 + 16} = \sqrt{25} = 5$.

**Norma L1 (Manhattan).** Jumlah nilai absolut semua elemen:

$$
\lVert \mathbf{v} \rVert_1 = \lvert v_1 \rvert + \lvert v_2 \rvert + \cdots + \lvert v_n \rvert
$$

Contoh: $\lVert (3, 4) \rVert_1 = 3 + 4 = 7$. Namanya "Manhattan" karena sama dengan jarak berjalan di kota berpetak: 3 blok ke satu arah, lalu 4 blok ke arah lain, total 7, bukan lurus 5.

Contoh dengan negatif dan tiga elemen: $\mathbf{c} = (1, -2, 2)$. $\lVert \mathbf{c} \rVert_2 = \sqrt{1 + 4 + 4} = 3$ dan $\lVert \mathbf{c} \rVert_1 = 1 + 2 + 2 = 5$. Nilai absolut membuat elemen negatif tetap menambah panjang.

Sifat penting norma:

- $\lVert \mathbf{v} \rVert \geq 0$, dan $= 0$ hanya jika $\mathbf{v}$ vektor nol.
- $\lVert c\mathbf{v} \rVert = \lvert c \rvert\,\lVert \mathbf{v} \rVert$. Contoh: $\lVert 2(3, 4) \rVert_2 = \lVert (6, 8) \rVert_2 = 10 = 2 \times 5$.
- **Ketaksamaan segitiga:** $\lVert \mathbf{u} + \mathbf{v} \rVert \leq \lVert \mathbf{u} \rVert + \lVert \mathbf{v} \rVert$. Contoh: $\mathbf{u} = (3, 4)$, $\mathbf{v} = (1, 0)$: $\lVert (4, 4) \rVert_2 = \sqrt{32} \approx 5{,}657 \leq 5 + 1 = 6$. Garis lurus tidak pernah lebih panjang daripada jalan memutar.

**Vektor satuan** (*unit vector*) adalah vektor dengan norma L2 sama dengan 1. Untuk mendapatkannya, bagi vektor dengan normanya: $\hat{\mathbf{v}} = \dfrac{\mathbf{v}}{\lVert \mathbf{v} \rVert_2}$. Untuk $(3, 4)$: $\left(\tfrac{3}{5}, \tfrac{4}{5}\right) = (0{,}6;\ 0{,}8)$, yang normanya $\sqrt{0{,}36 + 0{,}64} = 1$. Vektor satuan mempertahankan arah dan menghapus panjang. Vektor nol tidak punya vektor satuan (pembagian dengan nol).

### 5. Jarak Euclidean

**Jarak Euclidean** antara dua titik data $\mathbf{a}$ dan $\mathbf{b}$ adalah **norma L2 dari selisihnya**:

$$
d(\mathbf{a}, \mathbf{b}) = \lVert \mathbf{a} - \mathbf{b} \rVert_2
= \sqrt{(a_1 - b_1)^2 + \cdots + (a_n - b_n)^2}
$$

Ini rumus jarak dua titik di Level 1 yang diperluas ke dimensi berapa pun.

Contoh: $\mathbf{a} = (1, 2, 3)$ dan $\mathbf{b} = (4, 6, 3)$. Selisih: $\mathbf{a} - \mathbf{b} = (-3, -4, 0)$. Jarak: $\sqrt{9 + 16 + 0} = 5$. Jarak Manhattan (norma L1 dari selisih): $3 + 4 + 0 = 7$.

**Jarak mengikuti skala fitur.** Misalkan dua orang dengan fitur (usia dalam tahun, pendapatan dalam ribu rupiah): $\mathbf{a} = (25, 5000)$ dan $\mathbf{b} = (60, 5100)$. Selisih usia 35 kontribusi $35^2 = 1225$, sedangkan selisih pendapatan 100 kontribusi $100^2 = 10.000$. Jaraknya $\sqrt{11.225} \approx 105{,}9$, dan hampir seluruhnya ditentukan oleh pendapatan karena angkanya lebih besar. Karena itu, fitur biasanya diubah ke skala yang sebanding (*scaling*) sebelum menghitung jarak, yang dibahas di level berikutnya.

### 6. Dot Product

**Dot product** (hasil kali titik, *inner product*) dua vektor berdimensi sama adalah **jumlah dari hasil kali elemen-elemen yang berpasangan**:

$$
\mathbf{a} \cdot \mathbf{b} = a_1 b_1 + a_2 b_2 + \cdots + a_n b_n = \sum_{i=1}^{n} a_i b_i
$$

**Hasilnya skalar** (satu bilangan), bukan vektor.

Contoh: $(1, 2, 3) \cdot (4, 5, 6) = 1 \times 4 + 2 \times 5 + 3 \times 6 = 4 + 10 + 18 = 32$.

Syarat: dimensi harus sama. Sifat yang berlaku: $\mathbf{a} \cdot \mathbf{b} = \mathbf{b} \cdot \mathbf{a}$ dan $\mathbf{a} \cdot \mathbf{a} = \lVert \mathbf{a} \rVert_2^2$. Cek: $(3, 4) \cdot (3, 4) = 9 + 16 = 25 = 5^2$.

**Tafsiran geometri.** Untuk sudut $\theta$ di antara kedua vektor:

$$
\mathbf{a} \cdot \mathbf{b} = \lVert \mathbf{a} \rVert_2\,\lVert \mathbf{b} \rVert_2 \cos\theta
$$

Karena panjang selalu tak negatif, tanda dot product ditentukan oleh $\cos\theta$:

| Dot product | $\cos\theta$ | Arti |
|---|---|---|
| positif | positif | sudut lancip: kira-kira searah |
| nol | nol | sudut $90^\circ$: **tegak lurus** (ortogonal) |
| negatif | negatif | sudut tumpul: kira-kira berlawanan |

Cek dengan $\mathbf{a} = (1, 0)$ dan $\mathbf{b} = (1, 1)$ (sudut $45^\circ$): $\mathbf{a} \cdot \mathbf{b} = 1$. Sisi kanan: $1 \times \sqrt{2} \times \cos 45^\circ = \sqrt{2} \times \dfrac{\sqrt{2}}{2} = 1$. Sama.

**Tegak lurus.** $(2, -1) \cdot (1, 2) = 2 - 2 = 0$, jadi kedua vektor tegak lurus. Contoh lain: $(3, 0) \cdot (-2, 5) = -6$ (negatif: sudut tumpul).

**Dot product sebagai jumlah berbobot.** Pada Part 1, prediksi rumah dihitung dengan "kalikan pasangan lalu jumlahkan". Itu persis dot product:

$$
\hat{y} = \mathbf{w} \cdot \mathbf{x} + b
$$

Untuk $\mathbf{w} = (2, 10, -1)$, $\mathbf{x} = (60, 2, 5)$, dan $b = 20$: $\mathbf{w} \cdot \mathbf{x} = 120 + 20 - 5 = 135$, sehingga $\hat{y} = 135 + 20 = 155$. Sama seperti Part 1, kini dengan notasi ringkas yang berlaku untuk dimensi berapa pun.

### 7. Pengantar Cosine Similarity

**Cosine similarity** (kemiripan kosinus) mengukur **seberapa searah** dua vektor, **tanpa memperhatikan panjangnya**. Rumusnya diperoleh dari tafsiran geometri dot product di atas:

$$
\cos\theta = \frac{\mathbf{a} \cdot \mathbf{b}}{\lVert \mathbf{a} \rVert_2\,\lVert \mathbf{b} \rVert_2}
$$

**Ringkasan kosinus** (dari Level 1, Part 11): untuk sudut $\theta$ antara dua vektor, $\cos 0^\circ = 1$ (searah), $\cos 90^\circ = 0$ (tegak lurus), dan $\cos 180^\circ = -1$ (berlawanan). Nilainya selalu berada di antara $-1$ dan $1$. Kamu tidak perlu menghitung sudutnya; cukup memakai rumus dari dot product dan norma.

Syarat: kedua vektor bukan vektor nol (jika tidak, penyebutnya nol).

<div style="max-width:420px;margin:1rem auto"><svg viewBox="0 0 420 300" width="100%" role="img" aria-labelledby="vs-t vs-d" xmlns="http://www.w3.org/2000/svg" style="color:inherit;font-family:inherit;font-size:13px"><title id="vs-t">Sudut antara dua vektor</title><desc id="vs-d">Vektor a = (1, 0) mendatar berwarna biru dan vektor b = (1, 1) berwarna merah membentuk sudut 45 derajat di titik asal.</desc><defs><marker id="vs-b" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#5c6bc0"/></marker></defs><defs><marker id="vs-r" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#e53935"/></marker></defs><defs><marker id="vs-g" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#43a047"/></marker></defs><defs><marker id="vs-k" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="currentColor"/></marker></defs>
<g stroke="currentColor" stroke-opacity="0.15"><line x1="60" y1="260" x2="60" y2="60"/><line x1="160" y1="260" x2="160" y2="60"/><line x1="260" y1="260" x2="260" y2="60"/><line x1="360" y1="260" x2="360" y2="60"/><line x1="60" y1="260" x2="360" y2="260"/><line x1="60" y1="160" x2="360" y2="160"/><line x1="60" y1="60" x2="360" y2="60"/></g><g stroke="currentColor" stroke-width="1.5"><line x1="60" y1="260" x2="370" y2="260" marker-end="url(#vs-k)"/><line x1="60" y1="260" x2="60" y2="50" marker-end="url(#vs-k)"/></g><g fill="currentColor"><text x="60" y="276" text-anchor="middle">0</text><text x="160" y="276" text-anchor="middle">1</text><text x="260" y="276" text-anchor="middle">2</text><text x="360" y="276" text-anchor="middle">3</text><text x="52" y="164" text-anchor="end">1</text><text x="52" y="64" text-anchor="end">2</text><text x="374" y="276" font-style="italic">x</text><text x="68" y="48" font-style="italic">y</text></g>
<line x1="60" y1="260" x2="160" y2="260" stroke="#5c6bc0" stroke-width="2.5" marker-end="url(#vs-b)"/>
<line x1="60" y1="260" x2="160" y2="160" stroke="#e53935" stroke-width="2.5" marker-end="url(#vs-r)"/>
<path d="M110.0,260 A45,45 0 0 0 95.4,224.6" fill="none" stroke="currentColor" stroke-width="1.5"/>
<text x="122.0" y="244.0" fill="currentColor">θ = 45°</text>
<text x="166" y="252" fill="#5c6bc0" font-weight="bold">a = (1, 0)</text>
<text x="166" y="156" fill="#e53935" font-weight="bold">b = (1, 1)</text>
</svg></div>

**Cara membaca gambar.** Vektor $\mathbf{a} = (1, 0)$ (biru) mendatar, dan $\mathbf{b} = (1, 1)$ (merah) miring ke atas. Busur menandai sudut $\theta = 45^\circ$ di antara keduanya.

Hitung: $\mathbf{a} \cdot \mathbf{b} = 1$, $\lVert \mathbf{a} \rVert_2 = 1$, $\lVert \mathbf{b} \rVert_2 = \sqrt{2}$, sehingga $\cos\theta = \dfrac{1}{1 \cdot \sqrt{2}} = \dfrac{\sqrt{2}}{2} \approx 0{,}7071$. Ini cocok dengan $\cos 45^\circ$.

Contoh lain:

| Pasangan | Perhitungan | $\cos\theta$ | Arti |
|---|---|---|---|
| $(2, 0)$ dan $(5, 0)$ | $\dfrac{10}{2 \cdot 5}$ | $1$ | searah penuh (panjang tidak berpengaruh) |
| $(1, 0)$ dan $(0, 5)$ | $\dfrac{0}{1 \cdot 5}$ | $0$ | tegak lurus |
| $(1, 1)$ dan $(-1, -1)$ | $\dfrac{-2}{\sqrt{2}\sqrt{2}}$ | $-1$ | berlawanan arah |
| $(3, 4)$ dan $(6, 8)$ | $\dfrac{50}{5 \cdot 10}$ | $1$ | searah (kelipatan) |

**Mengapa mengabaikan panjang?** Pada teks, dokumen panjang punya hitungan kata yang lebih besar daripada dokumen pendek, padahal topiknya bisa sama. Misalkan hitungan tiga kata untuk tiga dokumen: $\mathbf{d}_1 = (2, 1, 0)$, $\mathbf{d}_2 = (4, 2, 0)$, dan $\mathbf{d}_3 = (0, 1, 3)$.

- $\mathbf{d}_2 = 2\mathbf{d}_1$ (dokumen yang dua kali lebih panjang dengan proporsi kata yang sama): $\cos = 1$, topik sama.
- $\mathbf{d}_1$ dan $\mathbf{d}_3$: $\dfrac{0 + 1 + 0}{\sqrt{5}\sqrt{10}} = \dfrac{1}{\sqrt{50}} \approx 0{,}1414$: sangat berbeda.

Jarak Euclidean $\mathbf{d}_1$ ke $\mathbf{d}_2$ adalah $\sqrt{4 + 1 + 0} \approx 2{,}236$, dan ke $\mathbf{d}_3$ adalah $\sqrt{4 + 0 + 9} \approx 3{,}606$. Di contoh ini keduanya sepakat bahwa $\mathbf{d}_2$ lebih dekat, tetapi jarak Euclidean menganggap $\mathbf{d}_1$ dan $\mathbf{d}_2$ "berbeda" karena panjangnya berbeda, sedangkan cosine similarity menganggapnya sama.

**Jarak kosinus** (*cosine distance*) sering didefinisikan sebagai $1 - \cos\theta$: bernilai 0 untuk searah dan makin besar untuk makin berbeda.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Penjumlahan vektor | jumlahkan elemen yang berpasangan | $(1,3) + (3,2) = (4,5)$ |
| Perkalian skalar | kalikan setiap elemen dengan bilangan | $3(1,3) = (3,9)$ |
| Vektor nol | semua elemen nol | $(0, 0)$ |
| Norma (*norm*) | ukuran panjang vektor | $\lVert (3,4) \rVert_2 = 5$ |
| Norma L2 (Euclidean) | akar jumlah kuadrat elemen | 5 |
| Norma L1 (Manhattan) | jumlah nilai absolut elemen | 7 |
| Vektor satuan | vektor dengan norma 1 | $(0{,}6;\ 0{,}8)$ |
| Jarak Euclidean | norma L2 dari selisih dua vektor | 5 |
| Dot product | jumlah hasil kali elemen berpasangan | $(1,2,3)\cdot(4,5,6) = 32$ |
| Ortogonal | tegak lurus; dot product nol | $(2,-1)\cdot(1,2) = 0$ |
| Cosine similarity | kosinus sudut antara dua vektor | $0{,}7071$ untuk $45^\circ$ |
| Ketaksamaan segitiga | $\lVert \mathbf{u}+\mathbf{v} \rVert \leq \lVert \mathbf{u} \rVert + \lVert \mathbf{v} \rVert$ | |
| Fitur (*feature*) | satu sifat yang diukur | usia |

## Rumus dan Penjelasan Setiap Simbol

Di semua rumus, $\mathbf{a}$, $\mathbf{b}$, $\mathbf{u}$, $\mathbf{v} \in \mathbb{R}^n$ adalah vektor berdimensi sama, dan $c$ adalah skalar.

| Operasi | Rumus | Syarat | Hasil |
|---|---|---|---|
| Penjumlahan | $(\mathbf{u} + \mathbf{v})_i = u_i + v_i$ | dimensi sama | vektor |
| Perkalian skalar | $(c\mathbf{v})_i = c\,v_i$ | | vektor |
| Norma L2 | $\lVert \mathbf{v} \rVert_2 = \sqrt{\sum_{i=1}^{n} v_i^2}$ | | skalar $\geq 0$ |
| Norma L1 | $\lVert \mathbf{v} \rVert_1 = \sum_{i=1}^{n} \lvert v_i \rvert$ | | skalar $\geq 0$ |
| Vektor satuan | $\hat{\mathbf{v}} = \dfrac{\mathbf{v}}{\lVert \mathbf{v} \rVert_2}$ | $\mathbf{v} \neq \mathbf{0}$ | vektor |
| Jarak Euclidean | $d(\mathbf{a}, \mathbf{b}) = \lVert \mathbf{a} - \mathbf{b} \rVert_2$ | dimensi sama | skalar $\geq 0$ |
| Dot product | $\mathbf{a} \cdot \mathbf{b} = \sum_{i=1}^{n} a_i b_i$ | dimensi sama | skalar |
| Cosine similarity | $\cos\theta = \dfrac{\mathbf{a} \cdot \mathbf{b}}{\lVert \mathbf{a} \rVert_2 \lVert \mathbf{b} \rVert_2}$ | $\mathbf{a}, \mathbf{b} \neq \mathbf{0}$ | skalar di $[-1, 1]$ |
| Prediksi linear | $\hat{y} = \mathbf{w} \cdot \mathbf{x} + b$ | $\mathbf{w}, \mathbf{x}$ dimensi sama | skalar |

Penjelasan simbol: $\mathbf{0}$ adalah vektor nol; $\hat{\mathbf{v}}$ (v topi tebal) adalah vektor satuan; angka 2 dan 1 di bawah garis ganda ($\lVert \cdot \rVert_2$) menandai jenis norma; $v_i$ adalah elemen ke-$i$; $\sum_{i=1}^{n}$ menjumlahkan untuk $i = 1$ sampai $n$ (Level 0).

## Penjelasan Langkah demi Langkah

**Menghitung cosine similarity $\mathbf{a} = (1, 2, 3)$ dan $\mathbf{b} = (4, 5, 6)$:**

| Langkah | Perhitungan | Hasil |
|---|---|---|
| 1. Dot product | $1 \cdot 4 + 2 \cdot 5 + 3 \cdot 6 = 4 + 10 + 18$ | $32$ |
| 2. Norma $\mathbf{a}$ | $\sqrt{1 + 4 + 9} = \sqrt{14}$ | $\approx 3{,}7417$ |
| 3. Norma $\mathbf{b}$ | $\sqrt{16 + 25 + 36} = \sqrt{77}$ | $\approx 8{,}7750$ |
| 4. Hasil kali norma | $\sqrt{14} \times \sqrt{77} = \sqrt{1078}$ | $\approx 32{,}8329$ |
| 5. Bagi | $\dfrac{32}{32{,}8329}$ | $\approx 0{,}9746$ |

Nilai $0{,}9746$ dekat dengan 1: kedua vektor hampir searah (sudut sekitar $12{,}9^\circ$).

## Contoh Soal dan Pembahasan

### Contoh 1 — Operasi dasar

Diketahui $\mathbf{u} = (1, 3)$ dan $\mathbf{v} = (3, 2)$. Hitung $\mathbf{u} + \mathbf{v}$, $3\mathbf{u} - 2\mathbf{v}$.

1. $\mathbf{u} + \mathbf{v} = (1 + 3,\ 3 + 2) = (4, 5)$.
2. $3\mathbf{u} = (3, 9)$ dan $2\mathbf{v} = (6, 4)$.
3. $3\mathbf{u} - 2\mathbf{v} = (3 - 6,\ 9 - 4) = (-3, 5)$.
4. Cek elemen pertama dari awal: $3(1) - 2(3) = 3 - 6 = -3$. Cocok.

### Contoh 2 — Norma dan vektor satuan

Hitung norma L1 dan L2 dari $\mathbf{v} = (-5, 12)$, lalu buat vektor satuannya.

1. L2: $\sqrt{(-5)^2 + 12^2} = \sqrt{25 + 144} = \sqrt{169} = 13$.
2. L1: $\lvert -5 \rvert + \lvert 12 \rvert = 5 + 12 = 17$.
3. Vektor satuan: $\dfrac{1}{13}(-5, 12) = \left(-\tfrac{5}{13}, \tfrac{12}{13}\right) \approx (-0{,}385;\ 0{,}923)$.
4. Cek norma: $\tfrac{25}{169} + \tfrac{144}{169} = \tfrac{169}{169} = 1$.

### Contoh 3 — Jarak dan dot product

Hitung jarak Euclidean antara $\mathbf{a} = (1, 2, 3)$ dan $\mathbf{b} = (4, 6, 3)$, serta $\mathbf{a} \cdot \mathbf{b}$.

1. Selisih: $\mathbf{a} - \mathbf{b} = (-3, -4, 0)$.
2. Jarak: $\sqrt{9 + 16 + 0} = 5$.
3. Dot product: $1 \cdot 4 + 2 \cdot 6 + 3 \cdot 3 = 4 + 12 + 9 = 25$.
4. Cek: $\lVert \mathbf{a} \rVert^2 + \lVert \mathbf{b} \rVert^2 - 2\,\mathbf{a} \cdot \mathbf{b} = 14 + 61 - 50 = 25 = d^2$. Hubungan ini juga benar (jarak kuadrat berasal dari dot product).

### Contoh 4 — Menguji tegak lurus

Apakah $(2, -1)$ dan $(1, 2)$ tegak lurus? Bagaimana dengan $(3, 0)$ dan $(-2, 5)$?

1. $(2, -1) \cdot (1, 2) = 2 \cdot 1 + (-1) \cdot 2 = 2 - 2 = 0$. Dot product nol, jadi **tegak lurus**.
2. $(3, 0) \cdot (-2, 5) = 3(-2) + 0 \cdot 5 = -6$. Tidak nol, jadi **tidak tegak lurus**. Karena negatif, sudutnya tumpul.

### Contoh 5 — Cosine similarity

Hitung cosine similarity $\mathbf{d}_1 = (2, 1, 0)$ dan $\mathbf{d}_3 = (0, 1, 3)$.

1. Dot product: $2 \cdot 0 + 1 \cdot 1 + 0 \cdot 3 = 1$.
2. Norma: $\lVert \mathbf{d}_1 \rVert = \sqrt{4 + 1 + 0} = \sqrt{5}$ dan $\lVert \mathbf{d}_3 \rVert = \sqrt{0 + 1 + 9} = \sqrt{10}$.
3. $\cos\theta = \dfrac{1}{\sqrt{5}\sqrt{10}} = \dfrac{1}{\sqrt{50}} \approx \dfrac{1}{7{,}071} \approx 0{,}1414$.
4. Interpretasi: nilainya dekat 0, jadi kedua dokumen berbeda topik.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** $(2, 5) + (1, -3) = (3, 2)$ dan $4(1, -2) = (4, -8)$.

**Sedang.** Jarak $(0, 0)$ ke $(6, 8)$ adalah $\sqrt{36 + 64} = 10$, dan jarak Manhattan-nya $6 + 8 = 14$.

**Sedang.** $(3, 4) \cdot (3, 4) = 25 = \lVert (3, 4) \rVert_2^2$. Dot product sebuah vektor dengan dirinya sendiri adalah norma kuadratnya.

**Menantang.** Prediksi $\hat{y} = \mathbf{w} \cdot \mathbf{x} + b$ dengan $\mathbf{w} = (0{,}5;\ 2)$, $\mathbf{x} = (10, 3)$, $b = 1$: $0{,}5 \times 10 + 2 \times 3 + 1 = 5 + 6 + 1 = 12$.

## Visualisasi atau Interpretasi Grafik

- **Penjumlahan:** panah disambung ujung ke pangkal, atau dibentuk jajar genjang. Hasilnya panah dari titik asal ke ujung gabungan.
- **Perkalian skalar:** memperpanjang, memperpendek, atau membalik panah pada garis yang sama.
- **Norma:** panjang panah (L2, garis lurus); L1 adalah jarak menyusuri sumbu.
- **Dot product dan sudut:** makin kecil sudut, makin besar kosinus dan makin besar dot product (untuk panjang tetap).
- **Cosine similarity:** hanya sudut yang dihitung. Vektor yang sama arah tetapi panjang berbeda mendapat nilai 1.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menjumlahkan vektor berdimensi berbeda | elemen tidak berpasangan | tidak terdefinisi |
| $\lVert (3, 4) \rVert_2 = 3 + 4 = 7$ | itu norma L1 | $\sqrt{9 + 16} = 5$ |
| Lupa nilai absolut pada norma L1 untuk elemen negatif | panjang tidak boleh berkurang | $\lvert -2 \rvert = 2$ |
| Menulis $\sqrt{a^2 + b^2} = a + b$ | akar tidak dipecah pada penjumlahan | $\sqrt{9 + 16} = 5 \neq 7$ |
| Menganggap dot product berupa vektor | hasilnya skalar | $(1,2)\cdot(3,4) = 11$, bukan $(3, 8)$ |
| Menyimpulkan dot product nol berarti salah satu vektor nol | bisa juga tegak lurus | $(2,-1)\cdot(1,2) = 0$ |
| Menghitung cosine similarity dengan vektor nol | penyebutnya nol | tidak terdefinisi |
| Menganggap cosine similarity memakai panjang | panjang dibagi habis | $(1,1)$ dan $(10,10)$ memberi $1$ |
| Membandingkan jarak antar fitur berskala sangat beda tanpa penyesuaian | fitur besar mendominasi | ubah skala fitur dulu |
| Mengira $\mathbf{u} - \mathbf{v} = \mathbf{v} - \mathbf{u}$ | pengurangan membalik arah | $(1,3)-(3,2) = (-2, 1)$, $(3,2)-(1,3) = (2,-1)$ |

## Hubungan dengan Machine Learning

- **Prediksi linear.** $\hat{y} = \mathbf{w} \cdot \mathbf{x} + b$ adalah dot product bobot dan fitur ditambah bias. Bentuk ini dipakai di regresi linear, regresi logistik, dan setiap neuron di jaringan saraf.
- **k-nearest neighbors.** Prediksi untuk data baru memakai jarak Euclidean ke data lama: yang terdekat paling berpengaruh. Skala fitur harus dijaga.
- **Cosine similarity.** Dipakai di pencarian teks, sistem rekomendasi, dan pembandingan *embedding* (vektor yang mewakili kata atau kalimat), karena arah lebih penting daripada panjang.
- **Norma L1 dan L2.** Dipakai pada regularisasi (menghukum bobot yang besar) di level berikutnya: L1 mendorong banyak bobot menjadi tepat nol, L2 mengecilkan semuanya.
- **Vektor satuan** (normalisasi) sering dipakai agar hanya arah yang diperhitungkan.

### Python: operasi vektor dengan `list`

Fungsi berikut hanya memakai `list`, `zip`, dan `math`, sehingga setiap rumus terlihat jelas.

```python
import math

def tambah(u, v):
    return [ui + vi for ui, vi in zip(u, v)]

def kali_skalar(c, v):
    return [c * vi for vi in v]

def dot(a, b):
    return sum(ai * bi for ai, bi in zip(a, b))

def norma2(v):
    return math.sqrt(sum(vi ** 2 for vi in v))

def norma1(v):
    return sum(abs(vi) for vi in v)

def jarak(a, b):
    return norma2([ai - bi for ai, bi in zip(a, b)])

def kosinus(a, b):
    return dot(a, b) / (norma2(a) * norma2(b))
```

Operasi dasar dan norma:

```python
u = [1, 3]
v = [3, 2]
print(tambah(u, v))                    # [4, 5]
print(tambah(kali_skalar(3, u), kali_skalar(-2, v)))   # [-3, 5]

print(norma2([3, 4]))    # 5.0
print(norma1([3, 4]))    # 7
print(norma2([-5, 12]), norma1([-5, 12]))   # 13.0 17
```

Jarak dan dot product:

```python
a = [1, 2, 3]
b = [4, 6, 3]
print(jarak(a, b))       # 5.0
print(dot(a, b))         # 25
print(math.dist(a, b))   # 5.0, fungsi bawaan untuk jarak Euclidean
print(dot([1, 2, 3], [4, 5, 6]))   # 32
print(dot([2, -1], [1, 2]))        # 0, tegak lurus
```

Cosine similarity, termasuk contoh teks:

```python
print(round(kosinus([1, 0], [1, 1]), 4))        # 0.7071
print(round(kosinus([1, 2, 3], [4, 5, 6]), 4))  # 0.9746
print(kosinus([3, 4], [6, 8]))                  # 1.0
print(round(kosinus([2, 1, 0], [4, 2, 0]), 4))  # 1.0
print(round(kosinus([2, 1, 0], [0, 1, 3]), 4))  # 0.1414

sudut = math.degrees(math.acos(kosinus([1, 0], [1, 1])))
print(round(sudut))                              # 45
```

Dalam perhitungan desimal, hasil yang secara matematis tepat 1 kadang tampil sedikit menyimpang (misalnya `0.9999999999999999`) karena pendekatan (Level 0, Part 1), sehingga `round` berguna untuk dibaca.

Prediksi model linear, ditulis sebagai dot product:

```python
w = [2, 10, -1]
x = [60, 2, 5]
b = 20
print(dot(w, x) + b)    # 155
```

Memeriksa syarat dimensi sebelum menjumlah:

```python
# print(tambah([1, 2, 3], [4, 5]))   # tidak memberi galat: zip memotong yang lebih panjang!
print(tambah([1, 2, 3], [4, 5]))     # [5, 7], hasilnya salah dan tanpa peringatan
```

Perhatikan peringatan di atas: `zip` diam-diam memotong vektor yang lebih panjang, sehingga kesalahan dimensi tidak terdeteksi. Dalam kode nyata, periksa `len(u) == len(v)` terlebih dahulu.

## Latihan Mandiri

1. Dengan $\mathbf{u} = (1, 3)$ dan $\mathbf{v} = (3, 2)$, hitung $\mathbf{u} + \mathbf{v}$, $\mathbf{u} - \mathbf{v}$, dan $\mathbf{v} - \mathbf{u}$.
2. Hitung: (a) $3\mathbf{u}$ (b) $-2\mathbf{v}$ (c) $\tfrac{1}{2}(4, 6)$ (d) $3\mathbf{u} - 2\mathbf{v}$
3. Hitung norma L2 dan L1: (a) $(3, 4)$ (b) $(-5, 12)$ (c) $(1, -2, 2)$
4. Buat vektor satuan dari $(3, 4)$ dan dari $(0, -2)$. Buktikan normanya 1.
5. Hitung jarak Euclidean dan jarak Manhattan: (a) $(1, 2, 3)$ dan $(4, 6, 3)$ (b) $(0, 0)$ dan $(6, 8)$
6. Hitung dot product: (a) $(1, 2, 3) \cdot (4, 5, 6)$ (b) $(2, -1) \cdot (1, 2)$ (c) $(3, 0) \cdot (-2, 5)$. Mana yang tegak lurus?
7. Hitung cosine similarity: (a) $(1, 0)$ dan $(1, 1)$ (b) $(3, 4)$ dan $(6, 8)$ (c) $(1, 1)$ dan $(-1, -1)$ (d) $(1, 0)$ dan $(0, 5)$. Tafsirkan masing-masing.
8. Tiga dokumen: $\mathbf{d}_1 = (2, 1, 0)$, $\mathbf{d}_2 = (4, 2, 0)$, $\mathbf{d}_3 = (0, 1, 3)$. Hitung cosine similarity $\mathbf{d}_1$ dengan $\mathbf{d}_2$ dan dengan $\mathbf{d}_3$. Mana yang paling mirip dengan $\mathbf{d}_1$?
9. Model $\hat{y} = \mathbf{w} \cdot \mathbf{x} + b$ dengan $\mathbf{w} = (0{,}5;\ 2)$ dan $b = 1$. Hitung prediksi untuk $\mathbf{x} = (10, 3)$.
10. Temukan kesalahan: (a) "$(1, 2, 3) + (4, 5)$ sama dengan $(5, 7, 3)$." (b) "$(1, 2) \cdot (3, 4) = (3, 8)$." (c) "Norma $(3, 4)$ adalah $3 + 4 = 7$."

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $\mathbf{u} + \mathbf{v} = (4, 5)$. $\mathbf{u} - \mathbf{v} = (1 - 3,\ 3 - 2) = (-2, 1)$. $\mathbf{v} - \mathbf{u} = (3 - 1,\ 2 - 3) = (2, -1)$. Dua pengurangan itu berlawanan tanda.

??? success "Soal 2"
    (a) $(3, 9)$. (b) $(-6, -4)$. (c) $(2, 3)$. (d) $3\mathbf{u} - 2\mathbf{v} = (3, 9) - (6, 4) = (-3, 5)$.

??? success "Soal 3"
    (a) L2 $= \sqrt{9 + 16} = 5$; L1 $= 3 + 4 = 7$. (b) L2 $= \sqrt{25 + 144} = 13$; L1 $= 5 + 12 = 17$. (c) L2 $= \sqrt{1 + 4 + 4} = 3$; L1 $= 1 + 2 + 2 = 5$.

??? success "Soal 4"
    $(3, 4)$ dibagi 5: $(0{,}6;\ 0{,}8)$; norma $\sqrt{0{,}36 + 0{,}64} = 1$. $(0, -2)$: normanya 2, jadi vektor satuan $(0, -1)$; norma $\sqrt{0 + 1} = 1$.

??? success "Soal 5"
    (a) Selisih $(-3, -4, 0)$. Euclidean $\sqrt{9 + 16 + 0} = 5$; Manhattan $3 + 4 + 0 = 7$. (b) Selisih $(-6, -8)$. Euclidean $\sqrt{36 + 64} = 10$; Manhattan $6 + 8 = 14$.

??? success "Soal 6"
    (a) $4 + 10 + 18 = 32$. (b) $2 - 2 = 0$. (c) $-6 + 0 = -6$. Hanya (b) yang tegak lurus (dot product nol).

??? success "Soal 7"
    (a) $\dfrac{1}{1 \cdot \sqrt{2}} \approx 0{,}7071$: searah sebagian (sudut $45^\circ$). (b) $\dfrac{18 + 32}{5 \cdot 10} = 1$: searah penuh. (c) $\dfrac{-2}{\sqrt{2}\sqrt{2}} = -1$: berlawanan arah. (d) $\dfrac{0}{1 \cdot 5} = 0$: tegak lurus.

??? success "Soal 8"
    $\cos(\mathbf{d}_1, \mathbf{d}_2) = \dfrac{8 + 2 + 0}{\sqrt{5}\sqrt{20}} = \dfrac{10}{\sqrt{100}} = 1$. $\cos(\mathbf{d}_1, \mathbf{d}_3) = \dfrac{1}{\sqrt{5}\sqrt{10}} \approx 0{,}1414$. $\mathbf{d}_2$ paling mirip dengan $\mathbf{d}_1$ (nilai 1, karena $\mathbf{d}_2 = 2\mathbf{d}_1$).

??? success "Soal 9"
    $\mathbf{w} \cdot \mathbf{x} = 0{,}5 \times 10 + 2 \times 3 = 5 + 6 = 11$. Prediksi $\hat{y} = 11 + 1 = 12$.

??? success "Soal 10"
    (a) Vektor berdimensi berbeda (3 dan 2) tidak dapat dijumlahkan; penjumlahan tidak terdefinisi. (b) Dot product adalah skalar: $1 \cdot 3 + 2 \cdot 4 = 11$. (c) Itu norma L1. Norma L2 adalah $\sqrt{9 + 16} = 5$.

## Ringkasan Materi

- Penjumlahan dan perkalian skalar dilakukan elemen demi elemen; penjumlahan membutuhkan dimensi sama. Secara geometri: panah disambung, atau diperpanjang, diperpendek, dibalik.
- Norma L2 $= \sqrt{\sum v_i^2}$ (panjang garis lurus), norma L1 $= \sum \lvert v_i \rvert$ (jarak menyusuri sumbu). Vektor satuan $= \mathbf{v} / \lVert \mathbf{v} \rVert_2$.
- Jarak Euclidean $= \lVert \mathbf{a} - \mathbf{b} \rVert_2$; jarak terpengaruh skala fitur.
- Dot product $\mathbf{a} \cdot \mathbf{b} = \sum a_i b_i$ berupa skalar, $= \lVert \mathbf{a} \rVert \lVert \mathbf{b} \rVert \cos\theta$. Nol berarti tegak lurus.
- Cosine similarity $= \dfrac{\mathbf{a} \cdot \mathbf{b}}{\lVert \mathbf{a} \rVert \lVert \mathbf{b} \rVert}$ berada di $[-1, 1]$ dan mengabaikan panjang.
- Prediksi linear adalah $\hat{y} = \mathbf{w} \cdot \mathbf{x} + b$.

## Checklist Penguasaan

- [ ] Menjumlah, mengurang, dan mengalikan vektor dengan skalar, termasuk menyebut syarat dimensi.
- [ ] Menghitung norma L1 dan L2 serta membentuk vektor satuan.
- [ ] Menghitung jarak Euclidean dan menjelaskan pengaruh skala fitur.
- [ ] Menghitung dot product dan menguji ketegaklurusan.
- [ ] Menghitung dan menafsirkan cosine similarity, termasuk nilai 1, 0, dan $-1$.
- [ ] Menulis prediksi model linear sebagai dot product.
- [ ] Menjelaskan mengapa fungsi Python berbasis `zip` perlu pemeriksaan dimensi.

## Prasyarat untuk Materi Berikutnya

[Part 3 — Matriks](03-matriks.md) membutuhkan: operasi vektor dan dot product (perkalian matriks pada dasarnya adalah dot product berulang). [Part 8 — Turunan Parsial dan Gradien](08-turunan-parsial-dan-gradien.md) membutuhkan: vektor, norma, dan arah, karena gradien adalah sebuah vektor.
