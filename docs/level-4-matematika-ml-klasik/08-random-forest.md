# Part 8 — Matematika Random Forest

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan mengapa satu pohon keputusan tidak stabil dan bagaimana ensemble membantu;
- menjelaskan sampel *bootstrap*, peluang sebuah contoh terambil, dan data *out-of-bag* (OOB);
- menjelaskan *bagging* dan *voting* atau *averaging* sebagai penggabung prediksi;
- menurunkan $\mathrm{Var}(\text{rata-rata}) = \rho\sigma^2 + \frac{(1 - \rho)\sigma^2}{B}$ dan menafsirkannya;
- menjelaskan peran pemilihan fitur acak dalam menurunkan korelasi antar pohon;
- menjalankan *bagging* dengan *stump* secara manual;
- menjelaskan mengapa banyak pohon meningkatkan stabilitas tetapi tidak menjamin hasil selalu lebih baik;
- menyebut hiperparameter utama dan batasan Random Forest, serta hubungannya dengan klasifikasi teks.

## Prasyarat

[Part 7 — Matematika Decision Tree](07-decision-tree.md); [Level 3 Part 7](../level-3-probabilitas-dan-statistika/07-variabel-acak-dan-distribusi.md) (varians) dan [Part 11](../level-3-probabilitas-dan-statistika/11-korelasi-kovarians-dan-standardisasi.md) (kovarians dan korelasi); [Level 3 Part 9](../level-3-probabilitas-dan-statistika/09-distribusi-sampling-dan-clt.md) (bootstrap); [Level 3 Part 8](../level-3-probabilitas-dan-statistika/08-distribusi-probabilitas-umum.md) (Binomial).

## Masalah yang Ingin Diselesaikan

Satu pohon yang dalam punya **bias rendah** (ia dapat menyesuaikan diri dengan pola rumit) tetapi **varians tinggi**: data latih yang sedikit berbeda menghasilkan pohon yang sangat berbeda dan prediksi yang berubah-ubah (Part 7). Gagasan *Random Forest*: **latih banyak pohon yang berbeda-beda, lalu gabungkan prediksinya**. Rata-rata dari banyak prediksi yang berfluktuasi independen jauh lebih stabil daripada satu prediksi, sama seperti rata-rata sampel lebih stabil daripada satu observasi (Level 3 Part 9).

## Intuisi dan Istilah Penting

### 1. Sampel *Bootstrap*

Dari data latih berukuran $n$, **sampel bootstrap** diambil dengan **pengembalian** sebanyak $n$ kali. Beberapa contoh terambil lebih dari sekali, sebagian tidak terambil sama sekali. Peluang sebuah contoh tertentu **tidak** terambil dalam satu kali penarikan adalah $1 - \frac{1}{n}$, sehingga setelah $n$ penarikan:

$$
P(\text{tidak terambil}) = \Bigl(1 - \frac{1}{n}\Bigr)^{n} \;\xrightarrow{\,n \to \infty\,}\; e^{-1} \approx 0{,}368
$$

Jadi sekitar **63,2%** contoh unik masuk sampel bootstrap, dan sekitar **36,8%** tidak. Untuk $n = 10$: $P(\text{terambil}) = 1 - 0{,}9^{10} = 0{,}6513$; $n = 100$: $0{,}6340$; $n = 1000$: $0{,}6323$. Contoh yang tidak terambil untuk sebuah pohon disebut **out-of-bag** (OOB) bagi pohon itu.

### 2. *Bagging*

***Bootstrap aggregating*** (*bagging*): latih $B$ pohon, masing-masing pada satu sampel bootstrap sendiri, lalu gabungkan:

- **Klasifikasi:** *voting* mayoritas, atau rata-rata peluang kelas dari tiap pohon.
- **Regresi:** rata-rata prediksi $\hat{y}(\mathbf{x}) = \dfrac{1}{B}\sum_{b=1}^{B} T_b(\mathbf{x})$.

Karena tiap pohon melihat data yang berbeda, kesalahan "kebetulan" mereka tidak sama, dan penggabungan meredam kesalahan itu.

### 3. *Random Forest* = *Bagging* + Fitur Acak

Pada *bagging* biasa, pohon-pohon masih mirip karena mereka memilih fitur kuat yang sama di akar. ***Random Forest*** menambahkan satu langkah: **di setiap pemisah**, hanya $m$ fitur yang dipilih **acak** (dari $d$ fitur) yang dipertimbangkan, misalnya $m \approx \sqrt{d}$ untuk klasifikasi. Ini membuat pohon lebih berbeda satu sama lain (korelasi lebih rendah), yang penting karena alasan di bagian berikut.

### 4. Mengapa Merata-ratakan Menurunkan Varians

Misalkan $B$ prediktor $T_1, \dots, T_B$ masing-masing bervarians $\sigma^2$ dan **korelasi berpasangan** $\rho$. Varians rata-ratanya (memakai $\mathrm{Var}(\sum T_b) = \sum\mathrm{Var} + \sum_{b \neq b'}\mathrm{Cov}$, Level 3 Part 11):

$$
\mathrm{Var}\!\left(\frac{1}{B}\sum_{b=1}^{B}T_b\right) = \frac{1}{B^2}\bigl[B\sigma^2 + B(B - 1)\rho\sigma^2\bigr] = \rho\,\sigma^2 + \frac{(1 - \rho)\,\sigma^2}{B}
$$

Dua suku: $\rho\sigma^2$ **tidak mengecil** dengan menambah pohon, sedangkan $\dfrac{(1 - \rho)\sigma^2}{B}$ menyusut. Untuk $\sigma^2 = 1$:

| Korelasi $\rho$ | $B = 1$ | $B = 10$ | $B = 100$ | $B = 1000$ | $B \to \infty$ |
|---|---|---|---|---|---|
| $0$ | $1$ | $0{,}1$ | $0{,}01$ | $0{,}001$ | $0$ |
| $0{,}5$ | $1$ | $0{,}55$ | $0{,}505$ | $0{,}5005$ | $0{,}5$ |
| $0{,}9$ | $1$ | $0{,}91$ | $0{,}901$ | $0{,}9001$ | $0{,}9$ |

Pelajaran: (1) menambah pohon menurunkan varians, tetapi hanya sampai batas $\rho\sigma^2$; (2) bila pohon sangat berkorelasi ($\rho = 0{,}9$), ratusan pohon hampir tidak membantu; (3) itulah fungsi pemilihan fitur acak: menurunkan $\rho$ walaupun varians tiap pohon $\sigma^2$ sedikit naik.

**Yang tidak diubah ensemble: bias.** Rata-rata prediktor yang semuanya bias ke arah yang sama tetap bias ke arah itu. *Bagging* terutama mengurangi **varians**.

**Peluang voting mayoritas** (bila tiap pohon benar dengan peluang $p$ **dan saling independen**): benar bila lebih dari separuh pohon benar. Dengan $p = 0{,}6$: $B = 1$: $0{,}6$; $B = 11$: $0{,}7535$; $B = 51$: $0{,}9265$; $B = 101$: $0{,}9791$ (distribusi Binomial, Level 3 Part 8). Namun bila $p < 0{,}5$, voting justru memperburuk ($p = 0{,}45$, $B = 101$: $0{,}1562$), dan pada kenyataannya pohon tidak independen, sehingga peningkatan jauh lebih kecil daripada hitungan ideal ini.

### 5. Estimasi *Out-of-Bag* (OOB)

Setiap contoh latih tidak dipakai oleh sekitar 36,8% pohon. Prediksi OOB sebuah contoh dihitung hanya dari pohon yang **tidak** melihatnya. Mengevaluasi prediksi OOB pada seluruh contoh memberi taksiran galat generalisasi tanpa data validasi terpisah (mirip validasi silang). Taksirannya bisa bervariasi pada data kecil.

### 6. Pentingnya Fitur

Pentingnya sebuah fitur pada hutan adalah rata-rata, di seluruh pohon, dari jumlah penurunan ketidakmurnian (berbobot ukuran simpul) oleh pemisah pada fitur itu (Part 7). Ini ringkasan yang berguna tetapi punya bias: condong ke fitur dengan banyak nilai unik, membagi kredit di antara fitur yang berkorelasi, dan dihitung dari data latih. Ia bukan ukuran sebab-akibat.

### 7. Hiperparameter

| Hiperparameter | Pengaruh |
|---|---|
| Jumlah pohon $B$ | lebih banyak menurunkan varians sampai batas; biaya hitung naik linear |
| $m$ (fitur per pemisah) | kecil: pohon beragam ($\rho$ rendah) tetapi tiap pohon lebih lemah; besar: kebalikannya |
| `max_depth`, `min_samples_leaf` | mengatur kompleksitas tiap pohon |
| ukuran sampel bootstrap | umumnya $n$ |

## Rumus dan Penjelasan Setiap Simbol

| Konsep | Rumus | Arti simbol dan syarat |
|---|---|---|
| Peluang tidak terambil | $\bigl(1 - \frac{1}{n}\bigr)^n \to e^{-1} \approx 0{,}368$ | $n$ ukuran data latih |
| Prediksi regresi | $\hat{y} = \frac{1}{B}\sum_b T_b(\mathbf{x})$ | $T_b$ pohon ke-$b$ |
| Voting | $\hat{y} = \arg\max_c \sum_b \mathbb{1}[T_b(\mathbf{x}) = c]$ | mayoritas suara |
| Peluang kelas | $\hat{P}(c \mid \mathbf{x}) = \frac{1}{B}\sum_b \hat{P}_b(c \mid \mathbf{x})$ | rata-rata peluang tiap pohon |
| Varians rata-rata | $\rho\sigma^2 + \frac{(1 - \rho)\sigma^2}{B}$ | $\sigma^2$ varians tiap prediktor, $\rho$ korelasi berpasangan |
| Peluang voting benar | $\sum_{k > B/2}\binom{B}{k}p^k(1 - p)^{B - k}$ | pohon independen dengan akurasi $p$ |
| Fitur per pemisah | $m \approx \sqrt{d}$ (klasifikasi) | $d$ banyak fitur |

## Contoh Numerik Langkah demi Langkah

### Contoh 1 — *Bagging* dengan lima *stump*

Data simulasi satu fitur: $x = 1, \dots, 8$, label $y = 0, 0, 1, 0, 0, 1, 1, 1$ (sama dengan Part 7). Titik $x = 3$ berlabel 1 di antara label 0 adalah "derau". *Stump* adalah pohon dengan satu pemisah. Lima sampel bootstrap (ukuran 8, dengan pengembalian, seed tetap), dan stump terbaik pada tiap sampel menurut *information gain*:

| Pohon | Sampel bootstrap (nilai $x$) | Unik | Ambang | Kiri ($x \leq t$) | Kanan |
|---|---|---|---|---|---|
| 1 | 1, 5, 5, 7, 7, 7, 8, 8 | 4 | $6{,}0$ | 0 | 1 |
| 2 | 2, 3, 3, 3, 4, 5, 5, 6 | 5 | $3{,}5$ | **1** | **0** |
| 3 | 2, 2, 2, 5, 6, 6, 7, 8 | 5 | $5{,}5$ | 0 | 1 |
| 4 | 1, 1, 2, 4, 5, 6, 8, 8 | 6 | $5{,}5$ | 0 | 1 |
| 5 | 1, 2, 4, 4, 6, 6, 7, 8 | 6 | $5{,}0$ | 0 | 1 |

Pohon 2 aneh: sampelnya memuat tiga salinan $x = 3$ (berlabel 1) dan hanya sedikit titik besar sehingga ia belajar aturan terbalik ("$x \leq 3{,}5$ berarti 1"). Itulah variasi akibat sampel yang berbeda. Prediksi tiap pohon dan hasil voting mayoritas:

| $x$ | Pohon 1 | 2 | 3 | 4 | 5 | Suara 1 | Voting | Label sebenarnya |
|---|---|---|---|---|---|---|---|---|
| 1 | 0 | 1 | 0 | 0 | 0 | 1 | **0** | 0 |
| 2 | 0 | 1 | 0 | 0 | 0 | 1 | **0** | 0 |
| 3 | 0 | 1 | 0 | 0 | 0 | 1 | **0** | 1 |
| 4 | 0 | 0 | 0 | 0 | 0 | 0 | **0** | 0 |
| 5 | 0 | 0 | 0 | 0 | 0 | 0 | **0** | 0 |
| 6 | 0 | 0 | 1 | 1 | 1 | 3 | **1** | 1 |
| 7 | 1 | 0 | 1 | 1 | 1 | 4 | **1** | 1 |
| 8 | 1 | 0 | 1 | 1 | 1 | 4 | **1** | 1 |

Pohon 2 salah pada $x = 1, 2, 6, 7, 8$ (lima dari delapan titik) dan hanya benar pada $x = 3, 4, 5$, tetapi pada $x = 1, 2, 6, 7, 8$ empat pohon lain **mengalahkan** suaranya. (Pada $x = 3$ keempat pohon itu mengalahkan suara pohon 2 yang kebetulan benar, karena label 1 di sana adalah derau.) Voting benar pada 7 dari 8 titik. Satu-satunya yang salah adalah $x = 3$ (label derau), yang juga salah pada stump tunggal terbaik ($x \leq 5{,}5$). Jadi di sini hutan **tidak** lebih baik daripada satu stump pada data latih (keduanya $7/8$); keunggulan ensemble terutama soal stabilitas pada data baru, dan data kecil ini hanya memperlihatkan mekanismenya.

### Contoh 2 — Varians rata-rata prediktor

Tiga puluh pohon memberi prediksi dengan varians $\sigma^2 = 4$ (simpangan baku $2$).

1. Bila saling independen ($\rho = 0$): $\mathrm{Var} = \dfrac{4}{30} = 0{,}1333$, simpangan baku $0{,}365$.
2. Bila berkorelasi $\rho = 0{,}6$: $\mathrm{Var} = 0{,}6 \times 4 + \dfrac{0{,}4 \times 4}{30} = 2{,}4 + 0{,}0533 = 2{,}4533$, simpangan baku $1{,}566$.
3. Dengan korelasi tinggi, 30 pohon hampir tidak lebih stabil daripada satu pohon ($\sigma = 2$): $\rho\sigma^2 = 2{,}4$ mendominasi.

Untuk mengurangi varians, turunkan $\rho$ (fitur acak, sampel berbeda), bukan hanya menambah $B$.

### Contoh 3 — Peluang voting mayoritas

Setiap pohon benar dengan peluang $p = 0{,}6$ dan independen. Hitung peluang voting benar untuk $B = 3$.

1. Voting benar bila minimal $2$ dari $3$ pohon benar.
2. $P(2) = \dbinom{3}{2}(0{,}6)^2(0{,}4) = 3 \times 0{,}36 \times 0{,}4 = 0{,}432$.
3. $P(3) = (0{,}6)^3 = 0{,}216$.
4. Total $= 0{,}648$, lebih tinggi dari $0{,}6$. Dengan $B = 101$: $0{,}9791$.
5. Peringatan: jika $p = 0{,}45$ (lebih buruk dari tebakan), $B = 101$ memberi $0{,}1562$: voting memperkuat kesalahan. Dan bila pohon berkorelasi, peningkatannya jauh lebih kecil.

### Contoh 4 — Peluang terambil dalam bootstrap

Untuk $n = 5$: $P(\text{contoh tertentu tidak terambil}) = 0{,}8^5 = 0{,}3277$, sehingga $P(\text{terambil}) = 0{,}6723$. Untuk $n = 100$: $0{,}99^{100} = 0{,}3660$, $P(\text{terambil}) = 0{,}6340$. Batas $n \to \infty$: $e^{-1} = 0{,}3679$, $P = 0{,}6321$. Jadi tiap pohon melatih sekitar dua pertiga contoh unik, dan sepertiga sisanya menjadi data OOB pohon itu.

### Contoh 5 — Perbandingan pohon tunggal dan hutan (simulasi)

Data sintetis dari `make_classification` (300 contoh, 20 fitur, 5 informatif, 10% label diacak), dibagi 70/30; diulang pada 20 dataset berbeda (seed 0 sampai 19) dengan satu pohon dan 100 pohon (`n_estimators=100`).

| | Rata-rata akurasi uji (20 dataset) | Dataset di mana lebih baik |
|---|---|---|
| Satu pohon | $0{,}7400$ | 4 dari 20 |
| Random Forest (100 pohon) | $0{,}8072$ | 16 dari 20 |

Random Forest lebih baik rata-rata dan pada 16 dari 20 dataset, **tetapi pohon tunggal menang pada 4 dataset**: ensemble tidak menjamin hasil lebih baik pada setiap data. Ini data simulasi, bukan hasil eksperimen pada data nyata.

Pada satu dataset (seed 2): pohon tunggal akurasi latih $1{,}0$ dan uji $0{,}744$ (menghafal); hutan akurasi latih $1{,}0$ dan uji $0{,}956$. Pengaruh jumlah pohon pada dataset yang sama (satu pohon dengan bootstrap dan fitur acak): $B = 1$: $0{,}789$; $5$: $0{,}878$; $25$: $0{,}933$; $100$: $0{,}956$; $300$: $0{,}933$. Naik cepat lalu mendatar dan berfluktuasi (tidak naik terus). Dengan `max_features` semua fitur: $0{,}889$ vs $\sqrt{d}$: $0{,}956$, memperlihatkan manfaat fitur acak pada dataset ini.

## Interpretasi, Asumsi, dan Keterbatasan

- **Mengurangi varians, bukan bias.** Bila pohon sama-sama terlalu sederhana, hutan tetap bias.
- **Batas oleh korelasi.** Menambah pohon berhenti membantu setelah $\rho\sigma^2$ mendominasi.
- **Tidak menjamin hasil lebih baik.** Contoh 5: pohon tunggal menang pada sebagian dataset; pada data kecil atau sangat sederhana hutan tidak selalu berguna.
- **Kurang dapat ditafsirkan.** Ratusan pohon tidak lagi berupa aturan yang dapat dibaca; hanya ringkasan seperti pentingnya fitur yang tersedia, dengan bias yang disebutkan.
- **Biaya.** Waktu latih dan memori naik kira-kira linear dengan $B$.
- **Tidak mengekstrapolasi.** Prediksi tetap berada dalam rentang target latih untuk regresi.
- **Pada teks berdimensi tinggi dan jarang**, Random Forest bisa bekerja, tetapi model linear (regresi logistik, SVM linear) sering kompetitif atau lebih baik dan jauh lebih ringan; hasilnya bergantung data.
- **OOB** adalah taksiran yang berisik pada data kecil.
- **Kelas tidak seimbang.** Voting mayoritas cenderung menguntungkan kelas besar; bobot kelas, *resampling*, atau ambang peluang dapat dipakai (Part 11).

## Hubungan dengan ML dan NLP

- **Ensemble sebagai penstabil.** Gagasan merata-ratakan prediktor yang tidak sempurna muncul di banyak tempat; *bagging* adalah bentuk paling sederhana.
- **Tanpa penskalaan.** Seperti pohon, hutan tidak peka skala fitur (Part 7).
- **Teks.** Pada klasifikasi Emergency vs Non-Emergency, hutan atas fitur BoW atau TF-IDF dapat menangkap interaksi antar kata, tetapi dengan dimensi besar dan data sedikit risiko *overfitting* tetap ada; bandingkan selalu dengan baseline linear (Part 4 dan 9) dan Naive Bayes (Part 6).
- **Pentingnya fitur** dipakai untuk eksplorasi kata yang paling sering dipakai pemisah, bukan untuk kesimpulan sebab-akibat.
- **Validasi OOB** memberi cara cepat memantau galat tanpa memisahkan data validasi, dengan catatan bahwa ia berisik.

### Python: bootstrap, *stump*, dan variansi dari nol

Hanya pustaka standar.

```python
import math, random
from collections import Counter
from statistics import mean, pvariance

print([(n, round(1 - (1 - 1 / n) ** n, 4)) for n in (5, 10, 100, 1000)], round(1 - 1 / math.e, 4))
# [(5, 0.6723), (10, 0.6513), (100, 0.634), (1000, 0.6323)] 0.6321

def peluang_voting(B, p):
    return sum(math.comb(B, k) * p ** k * (1 - p) ** (B - k) for k in range(B // 2 + 1, B + 1))

print([(B, round(peluang_voting(B, 0.6), 4)) for B in (1, 3, 11, 51, 101)])
# [(1, 0.6), (3, 0.648), (11, 0.7535), (51, 0.9265), (101, 0.9791)]
print([(B, round(peluang_voting(B, 0.45), 4)) for B in (1, 101)])      # [(1, 0.45), (101, 0.1562)]

for rho in (0, 0.5, 0.9):
    print(rho, [round(rho + (1 - rho) / B, 4) for B in (1, 10, 100, 1000)])
# 0 [1.0, 0.1, 0.01, 0.001]
# 0.5 [1.0, 0.55, 0.505, 0.5005]
# 0.9 [1.0, 0.91, 0.901, 0.9001]

rng = random.Random(0)
def simulasi(rho, B, ulang=20000):
    hasil = []
    for _ in range(ulang):
        z0 = rng.gauss(0, 1)                            # komponen bersama (sumber korelasi)
        t = [math.sqrt(rho) * z0 + math.sqrt(1 - rho) * rng.gauss(0, 1) for _ in range(B)]
        hasil.append(mean(t))
    return pvariance(hasil)

print([(rho, B, round(simulasi(rho, B), 3)) for rho, B in ((0, 10), (0.5, 10), (0.5, 100), (0.9, 100))])
# [(0, 10, 0.1), (0.5, 10, 0.553), (0.5, 100, 0.512), (0.9, 100, 0.91)]
```

*Bagging* lima *stump*:

```python
def entropy(label):
    n = len(label)
    return -sum((k / n) * math.log2(k / n) for k in Counter(label).values()) if n else 0

def stump(xs, ys):
    terbaik = None
    nilai = sorted(set(xs))
    for a, b in zip(nilai, nilai[1:]):
        t = (a + b) / 2
        kiri = [c for v, c in zip(xs, ys) if v <= t]
        kanan = [c for v, c in zip(xs, ys) if v > t]
        g = entropy(ys) - len(kiri) / len(ys) * entropy(kiri) - len(kanan) / len(ys) * entropy(kanan)
        if terbaik is None or g > terbaik[0] + 1e-12:
            terbaik = (g, t, Counter(kiri).most_common(1)[0][0], Counter(kanan).most_common(1)[0][0])
    return terbaik

x = [1, 2, 3, 4, 5, 6, 7, 8]
y = [0, 0, 1, 0, 0, 1, 1, 1]
rng2 = random.Random(0)
pohon = []
for b in range(5):
    idx = [rng2.randrange(8) for _ in range(8)]
    g, t, kiri, kanan = stump([x[i] for i in idx], [y[i] for i in idx])
    pohon.append((t, kiri, kanan))
    print(b + 1, sorted(x[i] for i in idx), len(set(idx)), t, kiri, kanan)
# 1 [1, 5, 5, 7, 7, 7, 8, 8] 4 6.0 0 1
# 2 [2, 3, 3, 3, 4, 5, 5, 6] 5 3.5 1 0
# 3 [2, 2, 2, 5, 6, 6, 7, 8] 5 5.5 0 1
# 4 [1, 1, 2, 4, 5, 6, 8, 8] 6 5.5 0 1
# 5 [1, 2, 4, 4, 6, 6, 7, 8] 6 5.0 0 1

for v in x:
    suara = [kr if v <= t else kn for t, kr, kn in pohon]
    print(v, suara, Counter(suara).most_common(1)[0][0], y[v - 1])
# 1 [0, 1, 0, 0, 0] 0 0
# 2 [0, 1, 0, 0, 0] 0 0
# 3 [0, 1, 0, 0, 0] 0 1
# 4 [0, 0, 0, 0, 0] 0 0
# 5 [0, 0, 0, 0, 0] 0 0
# 6 [0, 0, 1, 1, 1] 1 1
# 7 [1, 0, 1, 1, 1] 1 1
# 8 [1, 0, 1, 1, 1] 1 1
```

**Opsional: bandingkan dengan scikit-learn.** Blok ini membutuhkan scikit-learn dan NumPy, yang **tidak** dipasang oleh proyek ini dan tidak dijalankan oleh situs. Keluaran dijalankan dengan scikit-learn 1.9.0 pada Python 3.14; data adalah simulasi.

```python
import numpy as np
from sklearn.datasets import make_classification
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split

a_pohon, a_hutan = [], []
for rs in range(20):
    X, yy = make_classification(n_samples=300, n_features=20, n_informative=5,
                                n_redundant=2, flip_y=0.1, random_state=rs)
    Xa, Xb, ya, yb = train_test_split(X, yy, test_size=0.3, random_state=rs)
    a_pohon.append(DecisionTreeClassifier(random_state=0).fit(Xa, ya).score(Xb, yb))
    a_hutan.append(RandomForestClassifier(n_estimators=100, random_state=0).fit(Xa, ya).score(Xb, yb))
print(round(float(np.mean(a_pohon)), 4), round(float(np.mean(a_hutan)), 4),
      sum(q > p for p, q in zip(a_pohon, a_hutan)), sum(q < p for p, q in zip(a_pohon, a_hutan)))
# 0.74 0.8072 16 4
```

Interpretasi: hitungan manual (peluang bootstrap, varians rata-rata, voting stump) sesuai teori, dan simulasi variansi cocok dengan rumus $\rho + (1 - \rho)/B$. Perbandingan scikit-learn menunjukkan hutan unggul rata-rata tetapi bukan pada setiap dataset.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Mengira lebih banyak pohon selalu lebih baik | varians berhenti turun di $\rho\sigma^2$ dan hasil berfluktuasi | pilih $B$ cukup besar lalu perhatikan $\rho$ |
| Mengira Random Forest mengurangi bias | terutama mengurangi varians | gunakan pohon yang cukup ekspresif |
| Menganggap pohon dalam hutan independen | mereka berbagi data dan fitur kuat | gunakan fitur acak untuk menurunkan $\rho$ |
| Menganggap OOB identik dengan galat uji | OOB berisik pada data kecil | pakai sebagai taksiran, bukan kepastian |
| Menafsirkan pentingnya fitur sebagai sebab-akibat | bias dan data latih | hati-hati |
| Menganggap hutan selalu mengalahkan model lain | bergantung data (Contoh 5) | bandingkan dengan baseline |
| Lupa bahwa voting memperburuk bila $p < 0{,}5$ | mayoritas dari prediktor buruk tetap buruk | pastikan pohon lebih baik dari acak |
| Menghitung peluang kelas hutan sebagai peluang terkalibrasi | rata-rata proporsi daun kasar | periksa kalibrasi |
| Mengabaikan kelas tidak seimbang | mayoritas menang | bobot kelas atau ambang |

## Latihan Mandiri

1. Hitung peluang sebuah contoh **tidak** terambil dan terambil dalam sampel bootstrap untuk $n = 5$ dan $n = 100$. Berapa batasnya untuk $n$ besar?
2. Tiga pohon masing-masing benar dengan peluang $0{,}6$ dan independen. Hitung peluang voting mayoritas benar. Ulangi dengan $p = 0{,}45$ dan jelaskan hasilnya.
3. Hitung $\mathrm{Var}$ rata-rata $B = 10$ prediktor dengan $\sigma^2 = 1$ untuk $\rho = 0$, $0{,}5$, dan $0{,}9$.
4. Tiga puluh pohon dengan $\sigma^2 = 4$ dan $\rho = 0{,}6$. Hitung $\mathrm{Var}$ dan simpangan baku rata-ratanya, dan bandingkan dengan $\rho = 0$.
5. Turunkan $\mathrm{Var}\bigl(\frac{1}{B}\sum T_b\bigr) = \rho\sigma^2 + \frac{(1 - \rho)\sigma^2}{B}$ dari $\mathrm{Var}(\sum T_b) = B\sigma^2 + B(B - 1)\rho\sigma^2$.
6. Mengapa pemilihan fitur acak di setiap pemisah menurunkan $\rho$, dan apa harganya?
7. Pada Contoh 1, mengapa pohon 2 mempelajari aturan terbalik? Bagaimana ensemble menanggulanginya?
8. Pada simulasi Contoh 5, hutan lebih baik pada 16 dari 20 dataset dan pohon tunggal pada 4. Apa artinya bagi klaim "Random Forest selalu lebih baik"?
9. Sebuah hutan memiliki 100 pohon dan data latih berukuran 200. Kira-kira berapa contoh yang menjadi OOB bagi satu pohon, dan berapa pohon (rata-rata) yang tidak melihat sebuah contoh tertentu?
10. Jelaskan dalam beberapa kalimat kapan Random Forest layak dicoba untuk klasifikasi teks dan kapan model linear mungkin lebih sesuai.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $n = 5$: tidak terambil $0{,}8^5 = 0{,}3277$; terambil $0{,}6723$. $n = 100$: tidak terambil $0{,}99^{100} = 0{,}3660$; terambil $0{,}6340$. Batas $n \to \infty$: tidak terambil $e^{-1} \approx 0{,}368$; terambil $\approx 0{,}632$.

??? success "Soal 2"
    $p = 0{,}6$: $P(2) = 3(0{,}36)(0{,}4) = 0{,}432$; $P(3) = 0{,}216$; total $0{,}648$ (lebih baik dari $0{,}6$). $p = 0{,}45$: $P(2) = 3(0{,}2025)(0{,}55) = 0{,}3341$; $P(3) = 0{,}0911$; total $0{,}4253$ (lebih buruk dari $0{,}45$). Voting memperkuat arah mayoritas: bila prediktor lebih baik dari acak ia membaik, bila lebih buruk ia memburuk.

??? success "Soal 3"
    $\rho + \dfrac{1 - \rho}{10}$: $\rho = 0$: $0{,}1$. $\rho = 0{,}5$: $0{,}5 + 0{,}05 = 0{,}55$. $\rho = 0{,}9$: $0{,}9 + 0{,}01 = 0{,}91$.

??? success "Soal 4"
    $\rho = 0{,}6$: $0{,}6(4) + \dfrac{0{,}4(4)}{30} = 2{,}4 + 0{,}0533 = 2{,}4533$; simpangan baku $\approx 1{,}566$. $\rho = 0$: $\dfrac{4}{30} = 0{,}1333$; simpangan baku $\approx 0{,}365$. Korelasi tinggi menahan penurunan varians: hampir tidak lebih stabil daripada satu pohon ($\sigma = 2$).

??? success "Soal 5"
    $\mathrm{Var}\bigl(\frac{1}{B}\sum T_b\bigr) = \dfrac{1}{B^2}\mathrm{Var}(\sum T_b) = \dfrac{1}{B^2}\bigl[B\sigma^2 + B(B - 1)\rho\sigma^2\bigr] = \dfrac{\sigma^2}{B} + \dfrac{(B - 1)\rho\sigma^2}{B}$. Pecah: $\dfrac{(B - 1)\rho\sigma^2}{B} = \rho\sigma^2 - \dfrac{\rho\sigma^2}{B}$. Maka total $= \rho\sigma^2 + \dfrac{\sigma^2 - \rho\sigma^2}{B} = \rho\sigma^2 + \dfrac{(1 - \rho)\sigma^2}{B}$.

??? success "Soal 6"
    Tanpa pemilihan fitur acak, semua pohon cenderung memilih fitur terkuat yang sama di akar sehingga mirip (korelasi tinggi). Dengan hanya $m$ fitur acak yang dipertimbangkan per pemisah, pohon dipaksa memakai fitur berbeda sehingga lebih beragam: $\rho$ turun. Harganya: tiap pohon bisa lebih lemah (tidak selalu memakai fitur terbaik) dan bias sedikit naik, tetapi total varians hutan biasanya turun lebih banyak.

??? success "Soal 7"
    Sampel bootstrap pohon 2 berisi tiga salinan $x = 3$ (label 1) dan hanya satu titik besar ($x = 6$), sehingga di sampel itu aturan "$x \leq 3{,}5 \Rightarrow 1$" tampak paling memisahkan. Itu variasi akibat sampel. Ensemble menanggulanginya karena empat pohon lain memberi aturan sebaliknya dan suara mereka mengalahkan pohon 2 pada $x = 1, 2, 6, 7, 8$.

??? success "Soal 8"
    Hutan lebih baik rata-rata dan pada mayoritas dataset, tetapi pohon tunggal menang pada 4 dari 20, jadi klaim "selalu lebih baik" keliru. Ensemble menurunkan varians yang menguntungkan secara rata-rata, tetapi tidak menjamin hasil lebih baik pada setiap data.

??? success "Soal 9"
    Sekitar $36{,}8\%$ dari 200 $= 73{,}6 \approx 74$ contoh adalah OOB bagi satu pohon. Sebuah contoh tertentu tidak dilihat oleh sekitar $36{,}8\%$ dari 100 pohon, yaitu sekitar $37$ pohon.

??? success "Soal 10"
    Random Forest layak dicoba bila diharapkan ada interaksi antar kata atau fitur lain yang dapat ditangkap pohon, dan data cukup banyak untuk menghindari *overfitting*; ia tidak peka skala dan mudah dipakai. Model linear (regresi logistik, SVM linear, Naive Bayes) sering lebih sesuai untuk teks berdimensi sangat tinggi dan jarang karena lebih ringan, lebih mudah ditafsirkan, dan sering sama baiknya. Keputusan akhir bergantung validasi pada data yang bersangkutan.

## Ringkasan Materi

- Satu pohon: bias rendah, varians tinggi. Random Forest melatih banyak pohon pada sampel *bootstrap* dan pada tiap pemisah hanya mempertimbangkan $m$ fitur acak, lalu menggabungkan lewat voting atau rata-rata.
- Sampel bootstrap memuat sekitar $63{,}2\%$ contoh unik; sisanya ($36{,}8\%$) adalah OOB untuk taksiran galat tanpa data validasi terpisah.
- Varians rata-rata: $\rho\sigma^2 + \frac{(1 - \rho)\sigma^2}{B}$. Menambah pohon menurunkan hanya suku kedua; fitur acak menurunkan $\rho$.
- Ensemble mengurangi varians, bukan bias; voting memperburuk bila prediktor lebih buruk dari acak.
- Banyak pohon meningkatkan stabilitas tetapi tidak menjamin hasil lebih baik; pada simulasi pohon tunggal menang pada 4 dari 20 dataset.
- Kekurangan: kurang dapat dibaca, biaya lebih besar, dan pada teks berdimensi tinggi model linear sering kompetitif.
- Contoh NLP dan simulasi di sini bukan hasil eksperimen nyata.

## Checklist Penguasaan

- [ ] Menjelaskan sampel bootstrap dan peluang $0{,}632$ / $0{,}368$.
- [ ] Menjelaskan *bagging*, voting, dan rata-rata.
- [ ] Menurunkan dan menafsirkan $\rho\sigma^2 + \frac{(1 - \rho)\sigma^2}{B}$.
- [ ] Menjelaskan peran fitur acak dalam menurunkan korelasi pohon.
- [ ] Menjalankan *bagging* stump manual dan membaca hasil voting.
- [ ] Menjelaskan mengapa ensemble tidak menjamin hasil lebih baik.
- [ ] Menyebut hiperparameter utama dan keterbatasan Random Forest.

## Prasyarat untuk Materi Berikutnya

[Part 9 — Matematika Support Vector Machine](09-support-vector-machine.md) kembali ke model linear, kini dengan gagasan geometris: memilih batas keputusan yang memaksimalkan **margin** antar kelas. Pastikan norma, dot product, dan jarak titik ke bidang (Level 2 Part 2) serta batas keputusan regresi logistik (Part 4) sudah jelas.
