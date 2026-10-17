# Part 6 — Limit dan Kontinuitas

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan limit sebagai nilai yang didekati sebuah fungsi, dan membaca notasinya;
- menghitung limit sederhana dengan tabel nilai, substitusi langsung, dan penyederhanaan bentuk $\frac{0}{0}$;
- menghitung limit kiri dan limit kanan serta menyatakan kapan limit ada;
- menyebut tiga syarat kontinuitas dan mengenali jenis ketidakkontinuan;
- menjelaskan mengapa limit dan kontinuitas dibutuhkan untuk turunan;
- menghubungkan kontinuitas dan limit dengan fungsi yang dipakai di Machine Learning.

## Prasyarat

[Level 1 — Part 7](../level-1-aljabar-dasar-dan-fungsi/07-konsep-fungsi.md) (fungsi, domain), [Level 1 — Part 2](../level-1-aljabar-dasar-dan-fungsi/02-operasi-bentuk-aljabar.md) (memfaktorkan dan menyederhanakan), serta [Level 1 — Part 10](../level-1-aljabar-dasar-dan-fungsi/10-fungsi-eksponensial-dan-logaritma.md) (bilangan $e$ dan sigmoid). Part ini adalah pembuka jalur kalkulus dan tidak membutuhkan Part 1–5 Level 2.

## Mengapa Materi Ini Penting?

Banyak hal yang ingin kita ketahui tidak dapat dihitung langsung. Misalnya, berapa **kecepatan sesaat** sebuah mobil pada detik tertentu? Kecepatan membutuhkan selang waktu, sedangkan "sesaat" berarti selang waktu nol, yang membuat pembagian $\frac{0}{0}$. Limit adalah cara yang rapi untuk menjawab "apa yang terjadi **mendekati** titik itu" tanpa harus berada di titik itu. Dari gagasan inilah turunan lahir (Part 7), dan dari turunan lahir cara model belajar.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Gagasan Limit: Mendekati Tanpa Harus Sampai

Perhatikan fungsi

$$
f(x) = \frac{x^2 - 1}{x - 1}
$$

Pada $x = 1$, fungsi ini **tidak terdefinisi**: penyebutnya $1 - 1 = 0$ (pembagian dengan nol, Level 0). Tetapi bagaimana nilai $f(x)$ ketika $x$ sangat dekat dengan 1?

| $x$ | $0{,}9$ | $0{,}99$ | $0{,}999$ | $1$ | $1{,}001$ | $1{,}01$ | $1{,}1$ |
|---|---|---|---|---|---|---|---|
| $f(x)$ | $1{,}9$ | $1{,}99$ | $1{,}999$ | tidak ada | $2{,}001$ | $2{,}01$ | $2{,}1$ |

Dari kiri maupun kanan, nilai $f(x)$ semakin dekat ke **2**. Kita katakan **limit** $f(x)$ ketika $x$ mendekati 1 adalah 2. Perhatikan: kita tidak peduli berapa nilai $f(1)$ (bahkan tidak ada). Yang penting adalah nilai yang **didekati**.

Mengapa tepatnya 2? Karena untuk $x \neq 1$, pembilang dapat difaktorkan: $x^2 - 1 = (x - 1)(x + 1)$, sehingga $f(x) = x + 1$. Ketika $x$ mendekati 1, $x + 1$ mendekati 2.

### 2. Notasi Limit

$$
\lim_{x \to a} f(x) = L
$$

Dibaca "limit $f(x)$ ketika $x$ mendekati $a$ sama dengan $L$". Artinya: **nilai $f(x)$ dapat dibuat sedekat yang kita mau ke $L$** dengan memilih $x$ cukup dekat ke $a$ (tetapi $x \neq a$).

- $\lim$ adalah singkatan *limit*.
- $x \to a$ dibaca "$x$ mendekati $a$" (tanda panah menandai arah mendekat, bukan "sama dengan").
- $L$ adalah bilangan yang didekati.
- Nilai $f(a)$ **tidak menentukan** limit.

Untuk contoh di atas: $\displaystyle\lim_{x \to 1} \frac{x^2 - 1}{x - 1} = 2$.

### 3. Limit Sederhana

**a. Tabel nilai.** Seperti di atas: pilih $x$ yang mendekati $a$ dari kedua sisi, lihat apakah $f(x)$ mendekati satu bilangan. Cara ini memberi **dugaan**, bukan bukti, tetapi sangat membantu.

**b. Substitusi langsung.** Jika fungsinya "mulus" di $a$ (kontinu, dibahas di bagian 5), limit sama dengan nilai fungsi:

- $\displaystyle\lim_{x \to 3} (2x + 1) = 2(3) + 1 = 7$.
- $\displaystyle\lim_{x \to 2} x^2 = 4$.
- $\displaystyle\lim_{x \to -1} (x^3 + 4) = -1 + 4 = 3$.
- $\displaystyle\lim_{x \to 4} \sqrt{x} = 2$.

**c. Aturan limit.** Jika $\lim_{x \to a} f(x) = F$ dan $\lim_{x \to a} g(x) = G$ (keduanya ada), maka:

| Aturan | Rumus |
|---|---|
| Jumlah dan selisih | $\lim (f \pm g) = F \pm G$ |
| Kelipatan konstanta | $\lim\, c f = c F$ |
| Hasil kali | $\lim (f \cdot g) = F \cdot G$ |
| Hasil bagi | $\lim \dfrac{f}{g} = \dfrac{F}{G}$ (syarat $G \neq 0$) |

Contoh: $\lim_{x \to 2}(x^2 + 3x) = 4 + 6 = 10$, yang sama dengan jumlah limit masing-masing.

**d. Bentuk $\frac{0}{0}$.** Jika substitusi menghasilkan $\frac{0}{0}$, **jangan menyerah**: itu hanya berarti fungsi perlu disederhanakan lebih dulu. Faktorkan pembilang atau penyebut, coret faktor yang sama (yang tidak nol karena $x \neq a$), lalu substitusi.

Contoh 1: $\displaystyle\lim_{x \to 1} \frac{x^2 - 1}{x - 1} = \lim_{x \to 1} \frac{(x - 1)(x + 1)}{x - 1} = \lim_{x \to 1} (x + 1) = 2$.

Contoh 2: $\displaystyle\lim_{x \to 3} \frac{x^2 - x - 6}{x - 3}$. Faktorkan $x^2 - x - 6 = (x - 3)(x + 2)$ (hasil kali $-6$, jumlah $-1$). Hasilnya $\lim_{x \to 3}(x + 2) = 5$.

Contoh 3 (mirip definisi turunan): $\displaystyle\lim_{h \to 0} \frac{(2 + h)^2 - 4}{h} = \lim_{h \to 0} \frac{4h + h^2}{h} = \lim_{h \to 0} (4 + h) = 4$. Bentuk ini akan muncul lagi di Part 7.

Contoh 4 (akar): $\displaystyle\lim_{x \to 4} \frac{\sqrt{x} - 2}{x - 4}$. Karena $x - 4 = (\sqrt{x} - 2)(\sqrt{x} + 2)$, bentuknya menjadi $\dfrac{1}{\sqrt{x} + 2}$, dan limitnya $\dfrac{1}{4}$.

**e. Limit di tak hingga (pengantar).** Notasi $x \to \infty$ berarti $x$ membesar tanpa batas. Misalnya $\dfrac{1}{x}$ semakin kecil:

| $x$ | $10$ | $100$ | $1.000$ | $1.000.000$ |
|---|---|---|---|---|
| $\dfrac{1}{x}$ | $0{,}1$ | $0{,}01$ | $0{,}001$ | $0{,}000001$ |

Jadi $\displaystyle\lim_{x \to \infty} \frac{1}{x} = 0$. Akibatnya $\displaystyle\lim_{x \to \infty} \frac{2x + 1}{x} = \lim_{x \to \infty} \left(2 + \frac{1}{x}\right) = 2$. Dua limit penting dalam Machine Learning: $\lim_{x \to \infty} e^{-x} = 0$ dan karena itu sigmoid $\sigma(x) = \dfrac{1}{1 + e^{-x}}$ mendekati $1$ untuk $x$ besar dan mendekati $0$ untuk $x$ sangat negatif (Level 1, Part 10). Dan $\lim_{n \to \infty}\left(1 + \dfrac{1}{n}\right)^n = e$ adalah cara kita mengenal bilangan $e$.

### 4. Limit Kiri dan Limit Kanan

Kadang nilai yang didekati dari kiri berbeda dari yang didekati dari kanan.

- **Limit kiri**: $\displaystyle\lim_{x \to a^-} f(x)$, mendekati $a$ dari nilai yang **lebih kecil** (tanda minus kecil di atas $a$).
- **Limit kanan**: $\displaystyle\lim_{x \to a^+} f(x)$, mendekati $a$ dari nilai yang **lebih besar**.

**Aturan:** limit $\lim_{x \to a} f(x)$ **ada** jika dan hanya jika limit kiri dan limit kanan **sama-sama ada dan sama nilainya**.

Contoh 1: $f(x) = \dfrac{\lvert x \rvert}{x}$ di $a = 0$. Untuk $x > 0$, $\lvert x \rvert = x$, jadi $f(x) = 1$. Untuk $x < 0$, $\lvert x \rvert = -x$, jadi $f(x) = -1$.

| $x$ | $-0{,}1$ | $-0{,}001$ | $0{,}001$ | $0{,}1$ |
|---|---|---|---|---|
| $f(x)$ | $-1$ | $-1$ | $1$ | $1$ |

Limit kiri $= -1$, limit kanan $= 1$. Karena berbeda, $\lim_{x \to 0} \dfrac{\lvert x \rvert}{x}$ **tidak ada**.

Contoh 2 (fungsi bercabang): $p(x) = x + 1$ untuk $x < 2$ dan $p(x) = x + 2$ untuk $x \geq 2$. Limit kiri di 2: $2 + 1 = 3$. Limit kanan di 2: $2 + 2 = 4$. Tidak sama, jadi limit di 2 tidak ada, walaupun $p(2) = 4$ terdefinisi.

Contoh 3 (ReLU, dipakai di jaringan saraf): $r(x) = \max(0, x)$, yaitu $0$ untuk $x \leq 0$ dan $x$ untuk $x > 0$. Di $a = 0$: limit kiri $= 0$ dan limit kanan $= 0$. Sama, jadi $\lim_{x \to 0} r(x) = 0$, yang juga sama dengan $r(0) = 0$.

### 5. Kontinuitas

Secara intuitif, fungsi **kontinu** adalah fungsi yang grafiknya dapat digambar **tanpa mengangkat pena**: tidak ada lubang, loncatan, atau putus.

**Definisi.** Fungsi $f$ **kontinu di titik $a$** jika memenuhi **tiga syarat** sekaligus:

1. $f(a)$ **terdefinisi**.
2. $\lim_{x \to a} f(x)$ **ada**.
3. $\lim_{x \to a} f(x) = f(a)$.

Jika salah satu gagal, $f$ **tidak kontinu** (diskontinu) di $a$. Tiga jenis ketidakkontinuan yang umum:

| Jenis | Syarat yang gagal | Contoh |
|---|---|---|
| **Lubang** (dapat dihilangkan) | $f(a)$ tidak ada atau berbeda dari limit | $\dfrac{x^2 - 1}{x - 1}$ di $x = 1$ |
| **Loncatan** | limit kiri $\neq$ limit kanan | $\dfrac{\lvert x \rvert}{x}$ di $x = 0$, fungsi tangga |
| **Tak hingga** | limit tidak berhingga | $\dfrac{1}{x}$ di $x = 0$ |

**Menutup lubang.** Pada $f(x) = \dfrac{x^2 - 1}{x - 1}$ yang limitnya 2 di $x = 1$:

- Jika kita **mendefinisikan** $f(1) = 2$, maka ketiga syarat terpenuhi dan $f$ menjadi **kontinu** di 1.
- Jika kita mendefinisikan $f(1) = 5$, syarat 3 gagal ($2 \neq 5$): **diskontinu**.

**Fungsi yang kontinu di seluruh domainnya:** polinomial, $e^x$, $\sin x$ dan $\cos x$, $\sqrt{x}$ (untuk $x \geq 0$), $\ln x$ (untuk $x > 0$), dan fungsi pecahan di semua titik yang penyebutnya tidak nol. Jumlah, selisih, hasil kali, hasil bagi (penyebut tak nol), dan komposisi fungsi kontinu juga kontinu. Karena itu, sigmoid $\sigma(x) = \dfrac{1}{1 + e^{-x}}$ kontinu di semua titik (penyebutnya $1 + e^{-x} > 1$, tidak pernah nol).

**Teorema nilai antara (tanpa bukti).** Jika $f$ kontinu pada $[a, b]$ dan $f(a)$ dan $f(b)$ berlawanan tanda, ada setidaknya satu titik di antaranya tempat $f = 0$. Contoh: $f(x) = x^2 - 2$ pada $[1, 2]$: $f(1) = -1 < 0$ dan $f(2) = 2 > 0$, jadi ada akar di antara 1 dan 2 (yaitu $\sqrt{2} \approx 1{,}414$). Ide ini dipakai komputer untuk mencari akar secara berulang.

### 6. Mengapa Limit dan Kontinuitas Dibutuhkan untuk Turunan

Turunan, yang dibahas di Part 7, didefinisikan sebagai limit:

$$
\lim_{h \to 0} \frac{f(a + h) - f(a)}{h}
$$

Penyebut $h \to 0$ membuat bentuk $\frac{0}{0}$, dan penyelesaiannya persis seperti Contoh 3 di bagian 3. Fungsi yang **tidak kontinu** di sebuah titik **tidak punya turunan** di titik itu, sehingga kontinuitas adalah prasyarat bagi turunan. (Kebalikannya tidak selalu benar: ReLU kontinu di 0 tetapi tidak punya turunan di sana, yang dibahas di Part 7.)

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Limit (*limit*) | nilai yang didekati fungsi saat masukan mendekati suatu titik | $\lim_{x \to 1}\frac{x^2-1}{x-1} = 2$ |
| $x \to a$ | $x$ mendekati $a$ | |
| Limit kiri $x \to a^-$ | mendekati dari nilai yang lebih kecil | |
| Limit kanan $x \to a^+$ | mendekati dari nilai yang lebih besar | |
| Bentuk $\frac{0}{0}$ | hasil substitusi yang belum menentukan limit | perlu disederhanakan |
| Kontinu (*continuous*) | grafik tanpa lubang atau loncatan | polinomial |
| Diskontinu | tidak kontinu | $\frac{1}{x}$ di 0 |
| Lubang (*removable*) | limit ada tetapi nilai fungsi hilang atau salah | $\frac{x^2-1}{x-1}$ di 1 |
| Loncatan (*jump*) | limit kiri dan kanan berbeda | fungsi tangga |
| Tak hingga | nilai membesar tanpa batas | $\frac{1}{x}$ di 0 |
| Fungsi tangga (*step function*) | nilainya berubah mendadak di satu titik | ambang keputusan |

## Rumus dan Penjelasan Setiap Simbol

**Limit:**

$$
\lim_{x \to a} f(x) = L
$$

- $x$ variabel masukan; $a$ titik yang didekati; $L$ bilangan yang didekati. $x \neq a$ selama mendekat.

**Limit ada:**

$$
\lim_{x \to a} f(x) = L \iff \lim_{x \to a^-} f(x) = L \ \text{dan} \ \lim_{x \to a^+} f(x) = L
$$

- Tanda $\iff$ dibaca "jika dan hanya jika".

**Kontinuitas di $a$:** $\lim_{x \to a} f(x) = f(a)$ dengan $f(a)$ terdefinisi dan limitnya ada.

**Aturan limit:** jumlah, selisih, kelipatan, hasil kali, hasil bagi (penyebut limit tidak nol) mengikuti nilai limit masing-masing.

## Penjelasan Langkah demi Langkah

**Menghitung limit $\lim_{x \to a} f(x)$:**

1. **Substitusi langsung** $x = a$. Jika hasilnya bilangan biasa, itulah limit (untuk fungsi kontinu).
2. Jika hasilnya $\frac{0}{0}$: **faktorkan** (atau sederhanakan) lalu coret faktor yang sama.
3. Substitusi lagi.
4. Jika ada kecurigaan fungsi berubah di $a$ (nilai mutlak, fungsi bercabang), **hitung limit kiri dan kanan** terpisah.
5. **Periksa** dengan tabel nilai.

## Contoh Soal dan Pembahasan

### Contoh 1 — Limit dengan tabel

Duga $\lim_{x \to 2}(3x - 1)$.

1. Hitung dari kiri: $x = 1{,}9 \to 4{,}7$; $x = 1{,}99 \to 4{,}97$; $x = 1{,}999 \to 4{,}997$.
2. Hitung dari kanan: $x = 2{,}01 \to 5{,}03$; $x = 2{,}1 \to 5{,}3$.
3. Kedua sisi mendekati $5$.
4. Cek substitusi: $3(2) - 1 = 5$. Cocok.

### Contoh 2 — Bentuk $\frac{0}{0}$

Hitung $\displaystyle\lim_{x \to 2} \frac{x^2 - 4}{x - 2}$.

1. Substitusi: $\frac{4 - 4}{2 - 2} = \frac{0}{0}$. Belum diketahui.
2. Faktorkan: $x^2 - 4 = (x - 2)(x + 2)$.
3. Coret $(x - 2)$ (valid karena $x \neq 2$ ketika mendekati): $\dfrac{(x - 2)(x + 2)}{x - 2} = x + 2$.
4. Substitusi: $2 + 2 = 4$.
5. Cek tabel: $x = 1{,}99$ memberi $3{,}99$ dan $x = 2{,}01$ memberi $4{,}01$. Mendekati 4.

### Contoh 3 — Limit kiri dan kanan

Tentukan limit kiri, limit kanan, dan apakah limit ada di $x = 2$ untuk $p(x) = x + 1$ ($x < 2$) dan $p(x) = x + 2$ ($x \geq 2$).

1. Limit kiri: pakai rumus untuk $x < 2$: $\lim_{x \to 2^-}(x + 1) = 3$.
2. Limit kanan: pakai rumus untuk $x \geq 2$: $\lim_{x \to 2^+}(x + 2) = 4$.
3. $3 \neq 4$, jadi limit di 2 **tidak ada**.
4. Jenis ketidakkontinuan: **loncatan** sebesar $4 - 3 = 1$.

### Contoh 4 — Memeriksa kontinuitas

Apakah $f(x) = \dfrac{x^2 - 1}{x - 1}$ untuk $x \neq 1$ dengan $f(1) = 5$ kontinu di $x = 1$?

1. Syarat 1: $f(1) = 5$ terdefinisi. Terpenuhi.
2. Syarat 2: $\lim_{x \to 1} f(x) = 2$ ada. Terpenuhi.
3. Syarat 3: $\lim = 2 \neq 5 = f(1)$. **Gagal.**
4. Jadi $f$ diskontinu di 1 (lubang). Jika diubah menjadi $f(1) = 2$, $f$ kontinu.

### Contoh 5 — ReLU dan fungsi tangga

Periksa kontinuitas $r(x) = \max(0, x)$ dan fungsi tangga $s(x) = 0$ untuk $x < 0$, $s(x) = 1$ untuk $x \geq 0$ di $x = 0$.

1. ReLU: $r(0) = 0$; limit kiri $0$; limit kanan $0$. Ketiga syarat terpenuhi: **kontinu**.
2. Tangga: $s(0) = 1$; limit kiri $0$; limit kanan $1$. Limit kiri $\neq$ limit kanan: **diskontinu (loncatan)**.
3. ReLU hanya "patah" di 0, sedangkan fungsi tangga **meloncat**.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** $\lim_{x \to 5} 7 = 7$ (limit konstanta adalah konstanta itu sendiri) dan $\lim_{x \to 5} x = 5$.

**Sedang.** $\lim_{x \to -2} (x^2 + 3x - 1) = 4 - 6 - 1 = -3$ (substitusi langsung, polinomial kontinu).

**Sedang.** $\lim_{x \to \infty} \dfrac{5}{x^2} = 0$ karena penyebut membesar tanpa batas.

**Menantang.** $\displaystyle\lim_{h \to 0} \frac{(x + h)^2 - x^2}{h} = \lim_{h \to 0} \frac{2xh + h^2}{h} = \lim_{h \to 0}(2x + h) = 2x$. Inilah turunan $x^2$, dibahas di Part 7.

## Visualisasi atau Interpretasi Grafik

- **Limit** adalah "tinggi yang didekati": telusuri grafik dari kiri dan kanan menuju $x = a$, lalu lihat ketinggian yang dituju.
- **Lubang**: grafik terlihat mulus tetapi satu titik hilang (lingkaran kosong), seperti $\frac{x^2-1}{x-1}$ yang mirip garis $y = x + 1$ dengan satu titik hilang di $(1, 2)$.
- **Loncatan**: grafik putus; ujung kiri dan ujung kanan berada pada ketinggian berbeda.
- **Tak hingga**: grafik menukik ke atas atau ke bawah di dekat $a$ (seperti $\frac{1}{x}$ di dekat 0).
- **Kontinu**: satu goresan tanpa mengangkat pena.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menganggap limit sama dengan $f(a)$ | limit tidak peduli nilai $f(a)$ | kontinuitas yang mensyaratkan keduanya sama |
| Menyatakan limit tidak ada karena $f(a)$ tidak terdefinisi | limit hanya melihat sekitar $a$ | $\lim_{x \to 1}\frac{x^2-1}{x-1} = 2$ |
| Menganggap $\frac{0}{0} = 0$ atau $= 1$ | $\frac{0}{0}$ tak tentu | sederhanakan dulu |
| Mencoret suku tanpa memfaktorkan: $\frac{x^2 - 1}{x - 1} = x^2$ | hanya faktor yang boleh dicoret | faktorkan $(x - 1)(x + 1)$ |
| Menyimpulkan limit ada dari satu sisi saja | harus kiri dan kanan sama | periksa dua sisi |
| Menyimpulkan limit ada karena tabel hanya sampai $0{,}01$ | tabel hanya dugaan | verifikasi aljabar |
| Menganggap $\infty$ sebagai bilangan | $\infty$ berarti tak terbatas | $x \to \infty$ artinya membesar terus |
| Menganggap fungsi kontinu pasti punya turunan | ReLU di 0 kontinu tetapi tidak punya turunan | kontinu adalah syarat perlu, bukan cukup |
| Menyamakan "diskontinu" dengan "tidak terdefinisi di mana-mana" | diskontinu hanya di titik tertentu | periksa per titik |

## Hubungan dengan Machine Learning

- **Fungsi yang mulus dibutuhkan untuk belajar.** Model dilatih memakai turunan fungsi kerugian terhadap parameter (Part 7 dan seterusnya). Fungsi kerugian dan aktivasi yang kontinu (bahkan mulus) memungkinkan turunan itu dihitung.
- **Fungsi tangga bermasalah.** Fungsi tangga (misalnya "prediksi 1 jika skor $\geq 0$, selain itu 0") meloncat di ambang, sehingga turunannya nol di semua tempat kecuali di loncatan yang tidak punya turunan. Karena itu model dilatih dengan pengganti yang mulus seperti sigmoid.
- **ReLU** kontinu tetapi patah di 0. Dalam praktik, nilai turunan di 0 ditetapkan dengan konvensi (misalnya 0).
- **Limit di tak hingga.** Sigmoid mendekati 0 atau 1 untuk masukan yang sangat besar: kurvanya mendatar, dan turunannya mendekati 0 di sana. Ini penyebab *vanishing gradient* (Part 9).
- **Toleransi numerik.** Komputer tidak dapat mengambil $h$ sama dengan 0 atau terlalu kecil; kita memakai $h$ kecil sebagai pendekatan limit, yang dibahas di Part 7.

### Python: mengamati limit lewat tabel

Fungsi yang tidak terdefinisi di satu titik tetap dapat diamati di sekitarnya:

```python
def f(x):
    return (x * x - 1) / (x - 1)

for h in [0.1, 0.01, 0.001, 0.0001]:
    print(h, round(f(1 - h), 6), round(f(1 + h), 6))
# 0.1 1.9 2.1
# 0.01 1.99 2.01
# 0.001 1.999 2.001
# 0.0001 1.9999 2.0001
```

Baris `round(...)` hanya merapikan tampilan. Pada $x = 1$ sendiri, Python menolak:

```python
# print(f(1))   # ZeroDivisionError: division by zero
```

Limit kiri dan kanan fungsi $\frac{\lvert x \rvert}{x}$:

```python
def g(x):
    return abs(x) / x

print([g(x) for x in (-0.1, -0.001, 0.001, 0.1)])   # [-1.0, -1.0, 1.0, 1.0]
```

Sisi kiri $-1$, sisi kanan $1$: limit di 0 tidak ada.

Limit di tak hingga ($\frac{1}{x}$, $\frac{2x+1}{x}$, dan sigmoid):

```python
import math

print([1 / x for x in (10, 100, 1000, 10 ** 6)])            # [0.1, 0.01, 0.001, 1e-06]
print([(2 * x + 1) / x for x in (10, 100, 1000)])           # [2.1, 2.01, 2.001]

def sigmoid(x):
    return 1 / (1 + math.exp(-x))

print([round(sigmoid(x), 6) for x in (5, 10, 20)])          # [0.993307, 0.999955, 1.0]
```

Fungsi ReLU dan fungsi tangga, serta perbandingan nilai di sekitar 0:

```python
def relu(x):
    return max(0, x)

def tangga(x):
    return 1 if x >= 0 else 0

for x in (-0.001, 0, 0.001):
    print(x, relu(x), tangga(x))
# -0.001 0 0
# 0 0 1
# 0.001 0.001 1
```

ReLU berubah mulus menuju $0$ dari kedua sisi (nilainya $0$ dan $0{,}001$), sedangkan fungsi tangga meloncat dari $0$ ke $1$.

Tabel hanya memberi dugaan, bukan bukti, jadi hasil seperti ini harus selalu dikonfirmasi lewat aljabar (misalnya memfaktorkan).

## Latihan Mandiri

1. Duga $\lim_{x \to 2}(3x - 1)$ dengan tabel nilai untuk $x = 1{,}9;\ 1{,}99;\ 2{,}01;\ 2{,}1$. Cocokkan dengan substitusi.
2. Hitung dengan substitusi: (a) $\lim_{x \to 3}(x^2 + 2x)$ (b) $\lim_{x \to -1}(x^3 + 4)$ (c) $\lim_{x \to 4}\sqrt{x}$
3. Hitung: (a) $\lim_{x \to 1}\dfrac{x^2 - 1}{x - 1}$ (b) $\lim_{x \to 2}\dfrac{x^2 - 4}{x - 2}$ (c) $\lim_{x \to 3}\dfrac{x^2 - x - 6}{x - 3}$
4. Hitung $\displaystyle\lim_{x \to 0}\frac{(x + 3)^2 - 9}{x}$.
5. Hitung: (a) $\lim_{x \to \infty}\dfrac{1}{x}$ (b) $\lim_{x \to \infty}\dfrac{5}{x^2}$ (c) $\lim_{x \to \infty}\dfrac{2x + 1}{x}$
6. Tentukan limit kiri, limit kanan, dan apakah limit ada di $x = 0$ untuk $\dfrac{\lvert x \rvert}{x}$, dan di $x = 2$ untuk $p(x) = x + 1$ ($x < 2$), $p(x) = x + 2$ ($x \geq 2$).
7. Apakah $f(x) = \dfrac{x^2 - 1}{x - 1}$ kontinu di $x = 1$ jika: (a) $f(1)$ tidak didefinisikan (b) $f(1) = 2$ (c) $f(1) = 5$?
8. Sebutkan jenis ketidakkontinuan: (a) $\dfrac{1}{x}$ di $x = 0$ (b) $\dfrac{\lvert x \rvert}{x}$ di $x = 0$ (c) $\dfrac{x^2 - 1}{x - 1}$ di $x = 1$
9. Apakah ReLU $r(x) = \max(0, x)$ kontinu di $x = 0$? Jelaskan dengan tiga syarat.
10. (ML) Fungsi tangga $s(x)$ ($0$ untuk $x < 0$, $1$ untuk $x \geq 0$) dan sigmoid $\sigma(x)$: mana yang kontinu di $0$? Mengapa sifat itu penting untuk melatih model? Gunakan juga teorema nilai antara untuk $x^2 - 2$ pada $[1, 2]$.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $x = 1{,}9 \to 4{,}7$; $1{,}99 \to 4{,}97$; $2{,}01 \to 5{,}03$; $2{,}1 \to 5{,}3$. Mendekati $5$ dari kedua sisi. Substitusi: $3(2) - 1 = 5$. Cocok.

??? success "Soal 2"
    (a) $9 + 6 = 15$. (b) $-1 + 4 = 3$. (c) $2$.

??? success "Soal 3"
    (a) $\dfrac{(x-1)(x+1)}{x-1} = x + 1 \to 2$. (b) $\dfrac{(x-2)(x+2)}{x-2} = x + 2 \to 4$. (c) $x^2 - x - 6 = (x-3)(x+2)$, jadi $x + 2 \to 5$.

??? success "Soal 4"
    $\dfrac{x^2 + 6x + 9 - 9}{x} = \dfrac{x^2 + 6x}{x} = x + 6 \to 6$.

??? success "Soal 5"
    (a) $0$. (b) $0$ (penyebut $x^2$ membesar). (c) $\dfrac{2x + 1}{x} = 2 + \dfrac{1}{x} \to 2$.

??? success "Soal 6"
    $\dfrac{\lvert x \rvert}{x}$ di 0: limit kiri $-1$, kanan $1$, **limit tidak ada**. $p(x)$ di 2: limit kiri $3$, kanan $4$, **limit tidak ada**.

??? success "Soal 7"
    Limit di 1 adalah $2$. (a) $f(1)$ tidak terdefinisi: **diskontinu** (lubang). (b) $f(1) = 2$: **kontinu**. (c) $f(1) = 5 \neq 2$: **diskontinu**.

??? success "Soal 8"
    (a) tak hingga. (b) loncatan. (c) lubang (dapat dihilangkan).

??? success "Soal 9"
    $r(0) = 0$ terdefinisi (syarat 1). Limit kiri $= 0$ dan limit kanan $= 0$, jadi limit ada dan sama dengan $0$ (syarat 2). Limit $= 0 = r(0)$ (syarat 3). **Kontinu.**

??? success "Soal 10"
    Sigmoid kontinu di semua titik, sedangkan fungsi tangga diskontinu di 0 (limit kiri $0 \neq$ limit kanan $1$). Pelatihan memakai turunan, sehingga fungsi harus mulus; fungsi tangga tidak memberi petunjuk arah perbaikan. Untuk $x^2 - 2$: $f(1) = -1 < 0$ dan $f(2) = 2 > 0$, kontinu pada $[1, 2]$, jadi ada akar di antaranya (yaitu $\sqrt{2} \approx 1{,}414$).

## Ringkasan Materi

- $\lim_{x \to a} f(x) = L$: nilai $f(x)$ mendekati $L$ ketika $x$ mendekati $a$; nilai $f(a)$ tidak menentukan limit.
- Cara menghitung: tabel (dugaan), substitusi langsung, dan untuk $\frac{0}{0}$ faktorkan lalu coret. Aturan limit: jumlah, kelipatan, hasil kali, hasil bagi.
- Limit ada $\iff$ limit kiri dan limit kanan ada dan sama. Contoh limit tak ada: $\frac{\lvert x \rvert}{x}$ di 0.
- Kontinu di $a$: $f(a)$ terdefinisi, limit ada, dan limit $= f(a)$. Tiga jenis diskontinuitas: lubang, loncatan, tak hingga.
- Polinomial, $e^x$, sigmoid, dan ReLU kontinu; fungsi tangga meloncat.
- Limit dibutuhkan karena turunan didefinisikan sebagai limit; fungsi harus kontinu agar punya turunan.

## Checklist Penguasaan

- [ ] Menjelaskan limit sebagai nilai yang didekati, bukan nilai di titik itu.
- [ ] Menghitung limit dengan tabel, substitusi, dan penyederhanaan $\frac{0}{0}$.
- [ ] Menghitung limit kiri dan kanan serta menyimpulkan ada atau tidaknya limit.
- [ ] Menyebut tiga syarat kontinuitas dan memeriksanya pada contoh.
- [ ] Mengenali lubang, loncatan, dan ketakhinggaan.
- [ ] Menjelaskan mengapa fungsi tangga bermasalah dan ReLU atau sigmoid lebih bersahabat.
- [ ] Menjelaskan mengapa limit dibutuhkan untuk mendefinisikan turunan.

## Prasyarat untuk Materi Berikutnya

[Part 7 — Turunan](07-turunan.md) membutuhkan: limit (terutama bentuk $\frac{0}{0}$ dan penyederhanaan aljabar), limit kiri-kanan (untuk memahami titik tempat turunan tidak ada), dan kontinuitas.
