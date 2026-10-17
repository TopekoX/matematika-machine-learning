# Part 3 — Matematika Regresi Linear

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menulis model regresi linear dalam bentuk skalar dan bentuk matriks, termasuk kolom bias;
- membedakan residual, loss per data, dan rata-rata loss, serta menghitung MSE dan MAE;
- menjelaskan mengapa MSE lebih peka terhadap pencilan daripada MAE;
- menurunkan solusi kuadrat terkecil (*least squares*) untuk satu fitur dan menyatakannya sebagai persamaan normal;
- menyelesaikan regresi dua fitur dengan persamaan normal secara manual;
- menjelaskan sifat residual (jumlah nol dan tegak lurus terhadap fitur) pada solusi kuadrat terkecil;
- menerapkan *gradient descent* pada regresi linear dan menjelaskan pengaruh penskalaan fitur;
- menafsirkan koefisien dan mengenali asumsi serta keterbatasan regresi linear.

## Prasyarat

[Part 1](01-pengantar-matematika-ml.md) dan [Part 2](02-representasi-data-dan-feature-engineering.md) (matriks data, kolom bias, jebakan dummy); [Level 2 Part 4](../level-2-aljabar-linear-dan-kalkulus/04-sistem-persamaan-linear-matriks.md) (sistem linear), [Part 8](../level-2-aljabar-linear-dan-kalkulus/08-turunan-parsial-dan-gradien.md) (gradien), dan [Part 11](../level-2-aljabar-linear-dan-kalkulus/11-optimasi-dan-gradient-descent.md) (gradient descent); [Level 3 Part 11](../level-3-probabilitas-dan-statistika/11-korelasi-kovarians-dan-standardisasi.md) (kovarians, korelasi).

## Masalah yang Ingin Diselesaikan

Kita ingin memprediksi **bilangan kontinu** $y$ (harga, suhu, waktu) dari satu atau lebih fitur. **Regresi linear** mengasumsikan bahwa prediksi adalah **kombinasi linear** dari fitur ditambah konstanta, lalu mencari bobot yang membuat prediksi sedekat mungkin dengan target pada data latih.

## Intuisi dan Istilah Penting

**Model.** Untuk satu fitur: $\hat{y} = w\,x + b$ (garis lurus, dengan kemiringan $w$ dan titik potong $b$). Untuk $d$ fitur:

$$
\hat{y} = w_1 x_1 + w_2 x_2 + \dots + w_d x_d + b = \mathbf{w}\cdot\mathbf{x} + b
$$

**Bentuk matriks.** Tambahkan kolom angka $1$ di depan $X$ (kolom bias, Part 2) dan masukkan $b$ ke dalam vektor bobot, $\tilde{\mathbf{w}} = (b, w_1, \dots, w_d)$. Dengan $\tilde{X}$ berukuran $n \times (d + 1)$:

$$
\hat{\mathbf{y}} = \tilde{X}\,\tilde{\mathbf{w}}
$$

Satu perkalian matriks menghasilkan prediksi untuk semua $n$ contoh sekaligus. Untuk menyederhanakan penulisan, di bagian-bagian di bawah kita menulis $X$ dan $\mathbf{w}$ untuk bentuk yang sudah memuat kolom bias bila tidak ambigu.

**Residual dan loss.** Residual contoh ke-$i$: $r^{(i)} = \hat{y}^{(i)} - y^{(i)}$ (bertanda). Dua loss yang umum:

$$
\mathrm{MSE} = \frac{1}{n}\sum_{i=1}^{n}\bigl(r^{(i)}\bigr)^2, \qquad
\mathrm{MAE} = \frac{1}{n}\sum_{i=1}^{n}\bigl\lvert r^{(i)} \bigr\rvert
$$

Yang dibedakan di sini: **residual** $r^{(i)}$ (punya tanda), **loss per data** $\ell^{(i)} = (r^{(i)})^2$ atau $\lvert r^{(i)} \rvert$, dan **rata-rata loss** (MSE atau MAE).

### MSE vs MAE

Kuadrat menghukum residual besar jauh lebih berat. Untuk residual $1, -1, 2, -2, 10$: MSE $= \dfrac{1 + 1 + 4 + 4 + 100}{5} = 22$ sedangkan MAE $= \dfrac{1 + 1 + 2 + 2 + 10}{5} = 3{,}2$. Tanpa residual $10$: MSE $= 2{,}5$ dan MAE $= 1{,}5$. Satu pencilan menaikkan MSE sepuluh kali lipat tetapi MAE hanya dua kali lipat. Akibatnya:

| | MSE | MAE |
|---|---|---|
| Kepekaan terhadap pencilan | tinggi | lebih rendah |
| Turunan | mulus (mudah dioptimasi) | tidak terdefinisi di residual $0$ |
| Solusi | rumus tertutup (di bawah) | perlu optimasi numerik |
| "Pusat" yang diminimalkan | rata-rata | median |

### Solusi Kuadrat Terkecil untuk Satu Fitur

Minimalkan $L(w, b) = \dfrac{1}{n}\sum_i (w x_i + b - y_i)^2$. Di titik minimum kedua turunan parsial nol (Level 2 Part 8):

$$
\frac{\partial L}{\partial b} = \frac{2}{n}\sum_i r_i = 0, \qquad
\frac{\partial L}{\partial w} = \frac{2}{n}\sum_i x_i\,r_i = 0
$$

dengan $r_i = w x_i + b - y_i$. Dua persamaan ini punya arti:

1. $\sum_i r_i = 0$: **jumlah residual nol**.
2. $\sum_i x_i r_i = 0$: residual **tegak lurus** (dot product nol) terhadap kolom fitur.

Dari yang pertama: $b = \bar{y} - w\,\bar{x}$ (garis melewati titik pusat $(\bar{x}, \bar{y})$). Dimasukkan ke yang kedua, dan memakai $\sum x_i(y_i - \bar{y}) = \sum (x_i - \bar{x})(y_i - \bar{y})$ serta $\sum x_i(x_i - \bar{x}) = \sum (x_i - \bar{x})^2$:

$$
w = \frac{\sum_i (x_i - \bar{x})(y_i - \bar{y})}{\sum_i (x_i - \bar{x})^2} = \frac{S_{xy}}{S_{xx}}, \qquad
b = \bar{y} - w\,\bar{x}
$$

Penyebut dan pembilangnya adalah jumlah kuadrat deviasi dan jumlah hasil kali deviasi dari Level 3 (kovarians dan varians dengan faktor $n - 1$ dibatalkan), sehingga $w = \dfrac{s_{xy}}{s_x^2} = r\,\dfrac{s_y}{s_x}$ dengan $r$ korelasi Pearson.

### Persamaan Normal (Bentuk Umum)

Dalam bentuk matriks, kondisi turunan nol menjadi $\tilde{X}^{\top}(\tilde{X}\tilde{\mathbf{w}} - \mathbf{y}) = \mathbf{0}$, yaitu **persamaan normal**:

$$
\bigl(\tilde{X}^{\top}\tilde{X}\bigr)\,\tilde{\mathbf{w}} = \tilde{X}^{\top}\mathbf{y}
$$

Ini adalah sistem persamaan linear $A\mathbf{x} = \mathbf{b}$ dengan $A = \tilde{X}^{\top}\tilde{X}$ ukuran $(d + 1) \times (d + 1)$ yang diselesaikan dengan eliminasi Gauss (Level 2 Part 4). Syarat solusi tunggal: $\tilde{X}^{\top}\tilde{X}$ punya invers, yaitu kolom-kolom $\tilde{X}$ **independen linear**. Itu tidak terpenuhi bila ada kolom duplikat atau jebakan variabel dummy (Part 2), atau bila banyak contoh lebih sedikit dari banyak parameter ($n < d + 1$); dalam kasus itu ada tak hingga banyak solusi.

Arti geometris: prediksi $\hat{\mathbf{y}} = \tilde{X}\tilde{\mathbf{w}}$ adalah **proyeksi** vektor $\mathbf{y}$ ke ruang yang direntang kolom-kolom $\tilde{X}$, sehingga selisihnya (residual) tegak lurus terhadap semua kolom: $\tilde{X}^{\top}\mathbf{r} = \mathbf{0}$.

### *Gradient Descent* pada Regresi Linear

Alternatif untuk rumus langsung, yang tetap bisa dipakai bila $d$ sangat besar atau data tidak muat di memori: gradien MSE (Level 2 Part 8 dan 11):

$$
\nabla L = \frac{2}{n}\,\tilde{X}^{\top}\bigl(\tilde{X}\tilde{\mathbf{w}} - \mathbf{y}\bigr), \qquad
\tilde{\mathbf{w}} \leftarrow \tilde{\mathbf{w}} - \eta\,\nabla L
$$

dengan $\eta$ *learning rate*. Karena MSE untuk model linear adalah fungsi kuadrat, ia berbentuk mangkuk dan *gradient descent* aman bila $\eta < 2/\lambda_{\max}$, dengan $\lambda_{\max}$ nilai eigen terbesar dari $\frac{2}{n}\tilde{X}^{\top}\tilde{X}$ (Level 2 Part 11).

**Mengapa penskalaan fitur penting.** Kecepatan konvergensi ditentukan oleh perbandingan nilai eigen terbesar dan terkecil, disebut **bilangan kondisi** (*condition number*). Fitur berskala sangat berbeda atau sangat berkorelasi membuat mangkuk berbentuk lonjong sempit: $\eta$ harus kecil agar arah curam stabil, sehingga arah landai maju sangat lambat. Standardisasi menyamakan skala dan memperbaiki bilangan kondisi.

### Menafsirkan Koefisien

- $w_j$: perubahan prediksi bila $x_j$ naik $1$ satuan **dengan fitur lain tetap**. Nilainya bergantung satuan fitur (menukar m² ke cm² mengubah $w_j$).
- $b$: prediksi ketika semua fitur $0$. Sering tidak bermakna (tidak ada rumah dengan luas $0$).
- Tanda $w_j$ menunjukkan arah hubungan, tetapi **bukan sebab-akibat**, dan fitur yang saling berkorelasi tinggi membuat nilai $w_j$ tidak stabil.

## Rumus dan Penjelasan Setiap Simbol

| Konsep | Rumus | Arti simbol dan syarat |
|---|---|---|
| Model | $\hat{y} = \mathbf{w}\cdot\mathbf{x} + b$ | $\mathbf{w} \in \mathbb{R}^d$, $b \in \mathbb{R}$ |
| Bentuk matriks | $\hat{\mathbf{y}} = \tilde{X}\tilde{\mathbf{w}}$ | $\tilde{X}$: $n \times (d + 1)$ dengan kolom 1 |
| Residual | $r^{(i)} = \hat{y}^{(i)} - y^{(i)}$ | bertanda |
| MSE, MAE | $\frac{1}{n}\sum r^2$, $\frac{1}{n}\sum \lvert r \rvert$ | rata-rata loss |
| Satu fitur | $w = S_{xy}/S_{xx}$, $b = \bar{y} - w\bar{x}$ | $S_{xx} > 0$ |
| Persamaan normal | $(\tilde{X}^{\top}\tilde{X})\tilde{\mathbf{w}} = \tilde{X}^{\top}\mathbf{y}$ | perlu $\tilde{X}^{\top}\tilde{X}$ punya invers |
| Gradien MSE | $\nabla L = \frac{2}{n}\tilde{X}^{\top}(\tilde{X}\tilde{\mathbf{w}} - \mathbf{y})$ | vektor $d + 1$ elemen |
| Pembaruan | $\tilde{\mathbf{w}} \leftarrow \tilde{\mathbf{w}} - \eta\nabla L$ | $0 < \eta < 2/\lambda_{\max}$ |
| Sifat residual | $\sum r_i = 0$, $\tilde{X}^{\top}\mathbf{r} = \mathbf{0}$ | pada solusi kuadrat terkecil dengan bias |

## Contoh Numerik Langkah demi Langkah

### Contoh 1 — Satu fitur dengan rumus

Data: $x = 1, 2, 3, 4$ dan $y = 3, 5, 6, 9$ (sama dengan Part 1).

1. Rata-rata: $\bar{x} = 2{,}5$, $\bar{y} = 5{,}75$.
2. Deviasi $x$: $-1{,}5;\ -0{,}5;\ 0{,}5;\ 1{,}5$. Deviasi $y$: $-2{,}75;\ -0{,}75;\ 0{,}25;\ 3{,}25$.
3. $S_{xy} = (-1{,}5)(-2{,}75) + (-0{,}5)(-0{,}75) + (0{,}5)(0{,}25) + (1{,}5)(3{,}25) = 4{,}125 + 0{,}375 + 0{,}125 + 4{,}875 = 9{,}5$.
4. $S_{xx} = 2{,}25 + 0{,}25 + 0{,}25 + 2{,}25 = 5$.
5. $w = \dfrac{9{,}5}{5} = 1{,}9$ dan $b = 5{,}75 - 1{,}9(2{,}5) = 5{,}75 - 4{,}75 = 1$.
6. Prediksi: $2{,}9;\ 4{,}8;\ 6{,}7;\ 8{,}6$. Residual: $-0{,}1;\ -0{,}2;\ 0{,}7;\ -0{,}4$.
7. Periksa: jumlah residual $= 0$; $\sum x_i r_i = -0{,}1 - 0{,}4 + 2{,}1 - 1{,}6 = 0$. MSE $= \dfrac{0{,}01 + 0{,}04 + 0{,}49 + 0{,}16}{4} = 0{,}175$.

Hasil ini sama dengan pencarian kisi di Part 1: kini kita punya **rumus** yang menjawabnya langsung.

### Contoh 2 — Persamaan normal untuk satu fitur

Dengan data Contoh 1: $n = 4$, $\sum x = 10$, $\sum x^2 = 30$, $\sum y = 23$, $\sum xy = 3 + 10 + 18 + 36 = 67$.

1. Persamaan normal (kolom bias dan fitur): $\begin{bmatrix} 4 & 10 \\ 10 & 30 \end{bmatrix}\begin{bmatrix} b \\ w \end{bmatrix} = \begin{bmatrix} 23 \\ 67 \end{bmatrix}$.
2. Dari baris pertama: $b = \dfrac{23 - 10w}{4}$.
3. Ke baris kedua: $10 \cdot \dfrac{23 - 10w}{4} + 30w = 67 \Rightarrow 57{,}5 - 25w + 30w = 67 \Rightarrow 5w = 9{,}5 \Rightarrow w = 1{,}9$.
4. $b = \dfrac{23 - 19}{4} = 1$.

Sama dengan Contoh 1. Determinan matriks $4 \cdot 30 - 10^2 = 20 \neq 0$, jadi solusi tunggal.

### Contoh 3 — Dua fitur (data simulasi rumah)

Pakai tabel rumah dari Part 2 dengan dua fitur: luas $L$ (m²) dan kamar $K$, serta harga $H$ (juta).

| Rumah | $L$ | $K$ | $H$ |
|---|---|---|---|
| 1 | 60 | 2 | 500 |
| 2 | 90 | 3 | 900 |
| 3 | 45 | 1 | 250 |
| 4 | 120 | 4 | 1100 |
| 5 | 70 | 2 | 650 |

Rata-rata: $\bar{L} = 77$, $\bar{K} = 2{,}4$, $\bar{H} = 680$. Dengan **memusatkan** data, bias menghilang dan sistem menjadi $2 \times 2$ (lebih mudah dihitung manual).

1. Deviasi: $l = -17, 13, -32, 43, -7$; $k = -0{,}4;\ 0{,}6;\ -1{,}4;\ 1{,}6;\ -0{,}4$; $h = -180, 220, -430, 420, -30$.
2. Jumlah: $\sum l^2 = 3380$, $\sum lk = 131$, $\sum k^2 = 5{,}2$, $\sum lh = 37\,950$, $\sum kh = 1490$.
3. Persamaan normal terpusat: $\begin{bmatrix} 3380 & 131 \\ 131 & 5{,}2 \end{bmatrix}\begin{bmatrix} w_1 \\ w_2 \end{bmatrix} = \begin{bmatrix} 37\,950 \\ 1490 \end{bmatrix}$.
4. Determinan: $3380 \cdot 5{,}2 - 131^2 = 17\,576 - 17\,161 = 415$.
5. Dengan invers $2 \times 2$ (Level 2 Part 3): $w_1 = \dfrac{37\,950 \cdot 5{,}2 - 131 \cdot 1490}{415} = \dfrac{197\,340 - 195\,190}{415} = \dfrac{2150}{415} \approx 5{,}1807$ dan $w_2 = \dfrac{3380 \cdot 1490 - 131 \cdot 37\,950}{415} = \dfrac{5\,036\,200 - 4\,971\,450}{415} = \dfrac{64\,750}{415} \approx 156{,}0241$.
6. Bias: $b = \bar{H} - w_1\bar{L} - w_2\bar{K} = 680 - 5{,}1807(77) - 156{,}0241(2{,}4) \approx -93{,}3735$.
7. Prediksi: $529{,}52;\ 840{,}96;\ 295{,}78;\ 1152{,}41;\ 581{,}33$. Residual: $29{,}52;\ -59{,}04;\ 45{,}78;\ 52{,}41;\ -68{,}67$.
8. Periksa sifat: $\sum r = 0$, $\sum L_i r_i = 0$, $\sum K_i r_i = 0$ (sampai pembulatan).
9. Ukuran: MSE $\approx 2783{,}13$, RMSE $\approx 52{,}76$ juta, MAE $\approx 51{,}08$ juta.

Interpretasi: tiap tambahan 1 m² (kamar tetap) menaikkan prediksi sekitar $5{,}18$ juta; tiap tambahan kamar (luas tetap) sekitar $156{,}02$ juta. Prediksi untuk rumah $80\ \text{m}^2$ dengan $3$ kamar: $-93{,}37 + 5{,}1807(80) + 156{,}0241(3) \approx 789{,}16$ juta.

**Peringatan: koefisien tidak stabil.** Luas dan kamar berkorelasi sangat tinggi ($r = 0{,}988$) dan datanya hanya 5 rumah. Bila satu rumah dibuang dan model dihitung ulang, $w_1$ berkisar dari $-10$ sampai $13{,}3$ dan $w_2$ dari $-50$ sampai $525$. Prediksi dalam rentang data masih wajar, tetapi koefisien individual tidak dapat ditafsirkan dengan yakin. (Data kecil ini hanya untuk latihan hitung, bukan untuk kesimpulan.)

### Contoh 4 — *Gradient descent* dan pengaruh penskalaan

Pada data Contoh 3 (dengan bias), kita jalankan *gradient descent* dengan $\eta = 1/\lambda_{\max}$ dan berhenti ketika MSE berada dalam 1% dari nilai minimum.

1. Fitur mentah: nilai eigen terbesar $\frac{2}{n}\tilde{X}^{\top}\tilde{X} \approx 13\,225$, jadi $\eta \approx 7{,}56 \times 10^{-5}$. Dibutuhkan sekitar **233.000 iterasi**.
2. Fitur distandardisasi ($z$-score): nilai eigen terbesar $\approx 3{,}18$, $\eta \approx 0{,}314$. Dibutuhkan **5 iterasi**.
3. Alasannya: kovarians fitur mentah memiliki nilai eigen $\approx 0{,}031$ dan $\approx 846{,}3$ (perbandingan $\approx 27\,600$), sedangkan fitur terstandardisasi menghasilkan matriks korelasi dengan nilai eigen $1 \pm r$, yaitu $0{,}012$ dan $1{,}988$ (perbandingan $\approx 167$).
4. Penskalaan tidak mengubah solusi optimum prediksi (model sama, hanya parametrisasi berbeda), tetapi sangat mempercepat konvergensi.

### Contoh 5 — Pencilan: MSE vs MAE

Data $x = 1, 2, 3, 4, 5$ dan $y = 2, 4, 6, 8, 40$ (titik terakhir pencilan).

1. Kuadrat terkecil: $\bar{x} = 3$, $\bar{y} = 12$; $S_{xy} = (-2)(-10) + (-1)(-8) + 0 + (1)(-4) + (2)(28) = 80$; $S_{xx} = 10$; $w = 8$, $b = 12 - 24 = -12$.
2. Prediksi: $-4, 4, 12, 20, 28$ (target $2, 4, 6, 8, 40$). Satu pencilan memutar garis begitu kuat sehingga empat titik lain meleset jauh (misalnya $x = 1$: prediksi $-4$, target $2$).
3. Garis yang meminimalkan MAE (mencoba semua garis melalui dua titik data) adalah $y = 2x$ (w $= 2$, $b = 0$): ia melewati empat titik pertama dan mengabaikan pencilan.
4. MSE garis kuadrat terkecil $= 72$ vs MSE garis $y = 2x$ $= 180$; tetapi MAE garis kuadrat terkecil $= 7{,}2$ vs MAE garis $y = 2x$ $= 6$. Masing-masing optimal menurut ukurannya sendiri.

Kesimpulan: pilihan loss **mengubah model**. Bila pencilan adalah kesalahan data, MSE menyesatkan; bila pencilan penting, MAE bisa mengabaikan informasi.

## Interpretasi, Asumsi, dan Keterbatasan

- **Linearitas.** Regresi linear mengasumsikan hubungan linear terhadap fitur yang diberikan. Hubungan lengkung dapat ditangani dengan fitur baru (Part 2), bukan dengan model yang sama pada fitur asli.
- **Independensi dan varians galat konstan.** Untuk menafsirkan ketidakpastian (interval kepercayaan koefisien) biasanya diasumsikan galat independen, berbaur seragam di sekitar nol, dan kira-kira Normal. Asumsi galat Normal membuat meminimalkan MSE setara dengan kemungkinan maksimum (Level 3 Part 8 dan 10). Untuk sekadar memprediksi, asumsi ini tidak wajib, tetapi bila dilanggar kesimpulan inferensial menjadi kurang andal.
- **Kolinearitas.** Fitur yang sangat berkorelasi (atau bergantung linear) membuat $\tilde{X}^{\top}\tilde{X}$ hampir atau sepenuhnya singular dan koefisien tidak stabil.
- **Pencilan** sangat memengaruhi garis kuadrat terkecil (Contoh 5).
- **Ekstrapolasi.** Prediksi di luar rentang fitur latih tidak dijamin masuk akal (misalnya harga negatif).
- **Bukan sebab-akibat.** Koefisien menggambarkan asosiasi pada data, bukan efek intervensi.
- **$n$ kecil dan $d$ besar.** Bila $n < d + 1$, $\tilde{X}^{\top}\tilde{X}$ singular dan banyak solusi memberi loss nol; regularisasi (Part 12) diperlukan.

## Hubungan dengan ML dan NLP

- **Baseline kuat.** Regresi linear adalah baseline standar untuk regresi, dan model lain (pohon, SVM) dibandingkan dengannya.
- **Dasar bagi model lain.** Regresi logistik (Part 4), SVM linear (Part 9), dan jaringan saraf melanjutkan gagasan $\mathbf{w}\cdot\mathbf{x} + b$ dengan loss berbeda.
- **NLP.** Regresi linear pada fitur TF-IDF dapat dipakai untuk memprediksi skor kontinu dari teks (misalnya skor urgensi 1 sampai 10 yang ditetapkan petugas, sebagai contoh konseptual). Pada teks, $d$ biasanya jauh lebih besar dari $n$, jadi persamaan normal murni singular dan regularisasi Ridge (Part 12) hampir selalu dipakai. Untuk mengklasifikasikan Emergency vs Non-Emergency, regresi logistik lebih tepat daripada regresi linear pada label 0/1 (Part 4).
- **Pemantauan.** Residual yang menunjukkan pola (misalnya melengkung) menandakan model kurang sesuai.

### Python: kuadrat terkecil, persamaan normal, dan *gradient descent*

Hanya pustaka standar. Satu fitur:

```python
from statistics import mean, stdev
import math

x = [1, 2, 3, 4]
y = [3, 5, 6, 9]
xb, yb = mean(x), mean(y)
Sxy = sum((a - xb) * (c - yb) for a, c in zip(x, y))
Sxx = sum((a - xb) ** 2 for a in x)
w = Sxy / Sxx
b = yb - w * xb
print(Sxy, Sxx, w, b)                                   # 9.5 5.0 1.9 1.0
r = [w * a + b - c for a, c in zip(x, y)]
print([round(v, 2) for v in r], round(sum(r), 10) + 0.0, round(sum(a * v for a, v in zip(x, r)), 10) + 0.0)
# [-0.1, -0.2, 0.7, -0.4] 0.0 0.0
print(round(sum(v * v for v in r) / len(r), 4))         # 0.175
```

Dua fitur dengan persamaan normal terpusat:

```python
L = [60, 90, 45, 120, 70]
K = [2, 3, 1, 4, 2]
H = [500, 900, 250, 1100, 650]

def pusat(v):
    m = mean(v)
    return [a - m for a in v]

l, k, h = pusat(L), pusat(K), pusat(H)
dot = lambda u, v: sum(a * c for a, c in zip(u, v))
Sll, Slk, Skk, Slh, Skh = dot(l, l), dot(l, k), dot(k, k), dot(l, h), dot(k, h)
print(Sll, Slk, round(Skk, 4), Slh, round(Skh, 4))       # 3380 131.0 5.2 37950 1490.0

det = Sll * Skk - Slk ** 2
w1 = (Slh * Skk - Slk * Skh) / det
w2 = (Sll * Skh - Slk * Slh) / det
b0 = mean(H) - w1 * mean(L) - w2 * mean(K)
print(round(det, 4), round(w1, 4), round(w2, 4), round(b0, 4))   # 415.0 5.1807 156.0241 -93.3735

pred = [b0 + w1 * a + w2 * c for a, c in zip(L, K)]
res = [p - t for p, t in zip(pred, H)]
print([round(v, 2) for v in res])                        # [29.52, -59.04, 45.78, 52.41, -68.67]
print(round(sum(res), 6) + 0.0, round(dot(L, res), 6) + 0.0, round(dot(K, res), 6) + 0.0)   # 0.0 0.0 0.0
mse = sum(v * v for v in res) / 5
mae = sum(abs(v) for v in res) / 5
sst = sum((t - mean(H)) ** 2 for t in H)
print(round(mse, 2), round(math.sqrt(mse), 2), round(mae, 2), round(1 - sum(v * v for v in res) / sst, 4))
# 2783.13 52.76 51.08 0.9686
print(round(b0 + w1 * 80 + w2 * 3, 2))                   # 789.16
```

*Gradient descent* dengan fitur mentah dan terstandardisasi (iterasi sampai MSE berada dalam 1% dari minimum; $\eta$ dipilih $\approx 1/\lambda_{\max}$ seperti di Contoh 4):

```python
def gd(fitur, eta, toleransi=0.01, maks=500000):
    n = len(H)
    X = [[1.0] + list(f) for f in fitur]
    wv = [0.0] * 3
    for t in range(1, maks + 1):
        p = [sum(a * c for a, c in zip(row, wv)) for row in X]
        e = [pi - ti for pi, ti in zip(p, H)]
        g = [2 / n * sum(e[i] * X[i][j] for i in range(n)) for j in range(3)]
        wv = [wj - eta * gj for wj, gj in zip(wv, g)]
        loss = sum((sum(a * c for a, c in zip(row, wv)) - ti) ** 2 for row, ti in zip(X, H)) / n
        if loss <= mse * (1 + toleransi):
            return t
    return None

zl = [(a - mean(L)) / stdev(L) for a in L]
zk = [(a - mean(K)) / stdev(K) for a in K]
print(gd(list(zip(zl, zk)), 0.314))                      # 5
print(gd(list(zip(L, K)), 7.56e-05))                     # 233049
```

**Opsional: bandingkan dengan scikit-learn.** Blok ini membutuhkan scikit-learn dan NumPy, yang **tidak** dipasang oleh proyek ini dan tidak dijalankan oleh situs. Keluaran dijalankan dengan scikit-learn 1.9.0 pada Python 3.14.

```python
import numpy as np
from sklearn.linear_model import LinearRegression

Xm = np.column_stack([L, K])
m = LinearRegression().fit(Xm, H)
print(m.coef_.round(4), round(float(m.intercept_), 4))   # [  5.1807 156.0241] -93.3735
print(round(float(m.predict([[80, 3]])[0]), 2))          # 789.16
```

Interpretasi: perhitungan manual dan `LinearRegression` memberi koefisien yang sama. Dua perhitungan *gradient descent* menunjukkan bahwa standardisasi memangkas jumlah iterasi dari ratusan ribu menjadi 5 pada data ini.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Menyamakan residual, loss per data, dan MSE | berbeda: bertanda, tak negatif, satu angka | tulis satu per satu |
| Memakai MSE tanpa memeriksa pencilan | garis tertarik pencilan | periksa data, pertimbangkan MAE atau penanganan pencilan |
| Menafsirkan $w_j$ tanpa memperhatikan satuan dan fitur lain | bergantung skala dan kolinearitas | sebut "dengan fitur lain tetap", periksa korelasi |
| Memakai persamaan normal pada $X$ yang kolomnya bergantung linear | $X^{\top}X$ singular | buang kolom, regularisasi |
| Lupa kolom bias | garis dipaksa melewati titik asal | tambah kolom 1 |
| Menjalankan *gradient descent* pada fitur berskala jauh berbeda | konvergensi sangat lambat atau tidak stabil | standardisasi |
| Mengekstrapolasi jauh di luar data | linearitas belum tentu berlaku | batasi pemakaian pada rentang data |
| Menganggap $R^2$ tinggi berarti model benar | bisa *overfitting* atau mengabaikan asumsi | uji pada data validasi |
| Menyimpulkan sebab-akibat dari koefisien | koefisien adalah asosiasi | butuh desain kausal |

## Latihan Mandiri

1. Data $x = 1, 2, 3$ dan $y = 2, 4, 5$. Hitung $w$, $b$ dengan rumus kuadrat terkecil, residual, dan MSE.
2. Untuk hasil soal 1, periksa bahwa $\sum r_i = 0$ dan $\sum x_i r_i = 0$.
3. Untuk residual $1, -1, 2, -2, 10$ hitung MSE dan MAE, lalu hitung ulang tanpa residual $10$. Apa yang kamu simpulkan?
4. Persamaan normal $\begin{bmatrix} 5 & 15 \\ 15 & 55 \end{bmatrix}\begin{bmatrix} b \\ w \end{bmatrix} = \begin{bmatrix} 20 \\ 70 \end{bmatrix}$. Selesaikan $b$ dan $w$ dengan eliminasi.
5. Untuk data soal 1, mulai dari $(w, b) = (0, 0)$ dengan $\eta = 0{,}05$, hitung gradien MSE terhadap $w$ dan $b$ dan satu langkah *gradient descent*.
6. Model rumah $\hat{H} = -93{,}37 + 5{,}18L + 156{,}02K$. Prediksi harga rumah $L = 80$, $K = 3$. Apakah bijak memprediksi $L = 30$, $K = 10$? Mengapa koefisien $K$ sebaiknya tidak ditafsirkan sendirian?
7. Mengapa tidak ada solusi tunggal bila ada dua contoh ($n = 2$) dan dua fitur ditambah bias? Apa yang bisa dilakukan?
8. Matriks korelasi dua fitur adalah $\begin{bmatrix} 1 & 0{,}9 \\ 0{,}9 & 1 \end{bmatrix}$. Hitung nilai eigen ($1 \pm r$) dan bilangan kondisi. Bagaimana bila korelasinya $0$?
9. Data $x = 1, \dots, 5$ dan $y = 2, 4, 6, 8, 40$. Hitung garis kuadrat terkecil, bandingkan dengan garis $y = 2x$, dan jelaskan perbedaan loss yang dioptimalkan.
10. Periksa asumsi pada skenario: (a) memprediksi tinggi anak dari tinggi orang tua pada rentang 150 sampai 190 cm; (b) memprediksi penjualan harian dengan data deret waktu yang kelihatan berpola musiman; (c) memakai fitur "harga dalam rupiah" dan "harga dalam ribu rupiah" sekaligus.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $\bar{x} = 2$, $\bar{y} = \dfrac{11}{3} \approx 3{,}667$. $S_{xy} = (-1)(-1{,}667) + 0 + (1)(1{,}333) = 3$. $S_{xx} = 2$. $w = 1{,}5$, $b = 3{,}667 - 1{,}5(2) = 0{,}667$. Prediksi: $2{,}167;\ 3{,}667;\ 5{,}167$. Residual: $0{,}167;\ -0{,}333;\ 0{,}167$. MSE $= \dfrac{0{,}0278 + 0{,}1111 + 0{,}0278}{3} \approx 0{,}0556$.

??? success "Soal 2"
    $\sum r = 0{,}167 - 0{,}333 + 0{,}167 = 0$. $\sum x r = 1(0{,}167) + 2(-0{,}333) + 3(0{,}167) = 0{,}167 - 0{,}667 + 0{,}5 = 0$. Keduanya nol, sesuai sifat solusi kuadrat terkecil.

??? success "Soal 3"
    Dengan $10$: MSE $= 22$, MAE $= 3{,}2$. Tanpa $10$: MSE $= \dfrac{1 + 1 + 4 + 4}{4} = 2{,}5$, MAE $= \dfrac{1 + 1 + 2 + 2}{4} = 1{,}5$. MSE naik $8{,}8$ kali, MAE hanya $2{,}1$ kali: MSE jauh lebih peka terhadap satu residual besar.

??? success "Soal 4"
    Dari baris pertama $5b + 15w = 20 \Rightarrow b = 4 - 3w$. Ke baris kedua: $15(4 - 3w) + 55w = 70 \Rightarrow 60 + 10w = 70 \Rightarrow w = 1$, lalu $b = 1$. (Persamaan ini berasal dari $x = 1, \dots, 5$ dengan $\sum y = 20$ dan $\sum xy = 70$.)

??? success "Soal 5"
    $\dfrac{\partial L}{\partial w} = \dfrac{2}{3}\sum x_i(\hat{y}_i - y_i)$; dengan $\hat{y} = 0$: $\dfrac{2}{3}[1(-2) + 2(-4) + 3(-5)] = \dfrac{2}{3}(-25) \approx -16{,}667$. $\dfrac{\partial L}{\partial b} = \dfrac{2}{3}(-2 - 4 - 5) \approx -7{,}333$. Pembaruan: $w = 0 - 0{,}05(-16{,}667) = 0{,}8333$, $b = 0 - 0{,}05(-7{,}333) = 0{,}3667$.

??? success "Soal 6"
    $\hat{H}(80, 3) = -93{,}37 + 414{,}46 + 468{,}07 = 789{,}16$ juta. $L = 30$, $K = 10$ tidak bijak: kombinasi itu jauh di luar data latih (luas $45$ sampai $120$, kamar $1$ sampai $4$), sehingga ekstrapolasi tidak dapat dipercaya. Koefisien $K$ tidak stabil karena luas dan kamar berkorelasi sangat tinggi ($0{,}988$) dan datanya sedikit; yang andal hanyalah prediksi gabungan dalam rentang data.

??? success "Soal 7"
    Parameter ada tiga ($b$, $w_1$, $w_2$) tetapi hanya dua persamaan, sehingga $\tilde{X}^{\top}\tilde{X}$ ($3 \times 3$) memiliki rank paling banyak $2$: singular. Ada tak hingga banyak bobot yang membuat loss nol. Solusi: tambah data, kurangi fitur, atau pakai regularisasi (Part 12).

??? success "Soal 8"
    Nilai eigen: $1 + 0{,}9 = 1{,}9$ dan $1 - 0{,}9 = 0{,}1$; bilangan kondisi $= \dfrac{1{,}9}{0{,}1} = 19$. Bila korelasi $0$: nilai eigen $1$ dan $1$, bilangan kondisi $1$ (mangkuk bulat, konvergensi tercepat).

??? success "Soal 9"
    Kuadrat terkecil: $w = 8$, $b = -12$; prediksi $-4, 4, 12, 20, 28$; MSE $= 72$, MAE $= 7{,}2$. Garis $y = 2x$: MSE $= 180$, MAE $= 6$. Garis pertama meminimalkan MSE sehingga lebih memperhatikan pencilan; garis kedua meminimalkan MAE sehingga mengabaikan pencilan. Pilihan bergantung apakah pencilan itu kesalahan atau informasi penting.

??? success "Soal 10"
    (a) Cukup wajar: hubungan kira-kira linear pada rentang itu; tetap hati-hati dengan ekstrapolasi. (b) Melanggar independensi dan linearitas: pola musiman dan korelasi antar waktu perlu fitur tambahan (misalnya bulan) atau model deret waktu. (c) Dua fitur yang sepenuhnya bergantung linear (satu kelipatan konstan yang lain): $X^{\top}X$ singular; buang salah satu.

## Ringkasan Materi

- Model $\hat{y} = \mathbf{w}\cdot\mathbf{x} + b$; bentuk matriks $\hat{\mathbf{y}} = \tilde{X}\tilde{\mathbf{w}}$ dengan kolom bias.
- Residual (bertanda), loss per data, dan rata-rata loss; MSE menghukum residual besar lebih keras daripada MAE (peka pencilan).
- Solusi kuadrat terkecil: satu fitur $w = S_{xy}/S_{xx}$, $b = \bar{y} - w\bar{x}$; umum: persamaan normal $(\tilde{X}^{\top}\tilde{X})\tilde{\mathbf{w}} = \tilde{X}^{\top}\mathbf{y}$ yang butuh kolom independen.
- Pada solusi: jumlah residual nol dan residual tegak lurus terhadap tiap kolom fitur (proyeksi).
- *Gradient descent*: $\nabla L = \frac{2}{n}\tilde{X}^{\top}(\tilde{X}\tilde{\mathbf{w}} - \mathbf{y})$; standardisasi memperbaiki bilangan kondisi dan sangat mempercepat konvergensi.
- Koefisien menggambarkan asosiasi, bergantung satuan, dan tidak stabil bila fitur berkorelasi tinggi.
- Asumsi: linearitas, independensi, galat berbaur seragam; pencilan dan ekstrapolasi perlu hati-hati.

## Checklist Penguasaan

- [ ] Menulis model regresi linear dalam bentuk skalar dan matriks.
- [ ] Menghitung residual, MSE, dan MAE, dan menjelaskan kepekaan terhadap pencilan.
- [ ] Menurunkan dan memakai $w = S_{xy}/S_{xx}$ dan $b = \bar{y} - w\bar{x}$.
- [ ] Menyelesaikan persamaan normal dua fitur secara manual.
- [ ] Menjelaskan sifat residual pada solusi kuadrat terkecil.
- [ ] Menjalankan *gradient descent* dan menjelaskan pengaruh penskalaan.
- [ ] Menafsirkan koefisien dengan benar dan menyebut asumsi serta keterbatasan.

## Prasyarat untuk Materi Berikutnya

[Part 4 — Matematika Regresi Logistik](04-regresi-logistik.md) mengubah skor linear $\mathbf{w}\cdot\mathbf{x} + b$ menjadi probabilitas dengan fungsi sigmoid untuk klasifikasi, dengan loss yang berbeda dari MSE. Pastikan gradien, *gradient descent*, dan perbedaan skor dengan probabilitas sudah jelas.
