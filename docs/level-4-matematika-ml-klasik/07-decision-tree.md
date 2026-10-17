# Part 7 — Matematika Decision Tree

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan pohon keputusan sebagai rangkaian pertanyaan ya-atau-tidak yang membagi data;
- menghitung *entropy* dan *Gini impurity* dari distribusi kelas pada sebuah simpul;
- menghitung *information gain* (atau penurunan Gini) untuk sebuah pemisah dan memilih pemisah terbaik secara manual;
- mencari ambang terbaik pada fitur numerik;
- menjelaskan pemisah untuk pohon regresi dengan penurunan jumlah kuadrat galat;
- menjelaskan mengapa pohon yang terlalu dalam mengalami *overfitting* dan bagaimana membatasinya;
- menjelaskan keunggulan dan keterbatasan pohon, termasuk ketidakstabilannya;
- menghubungkan pohon dengan klasifikasi teks dan dengan *Random Forest* (Part 8).

## Prasyarat

[Part 1](01-pengantar-matematika-ml.md) (*overfitting*), [Part 2](02-representasi-data-dan-feature-engineering.md) (fitur biner dari BoW), [Level 1 Part 10](../level-1-aljabar-dasar-dan-fungsi/10-fungsi-eksponensial-dan-logaritma.md) (logaritma basis 2), dan [Level 3 Part 4](../level-3-probabilitas-dan-statistika/04-dasar-probabilitas.md) (peluang dan frekuensi relatif).

## Masalah yang Ingin Diselesaikan

Kita ingin klasifikasi (atau regresi) yang dapat **dibaca manusia** sebagai aturan: "jika pesan memuat kata *besok*, maka Non-Emergency; jika tidak dan memuat kata *kebakaran*, maka Emergency". **Pohon keputusan** (*decision tree*) membangun aturan seperti itu dari data: mulai dari seluruh data, ia berulang kali mengajukan pertanyaan tentang satu fitur yang **memisahkan kelas sebaik mungkin**, sampai simpul-simpulnya (hampir) murni.

Istilah: **simpul akar** (*root*) memuat seluruh data; **simpul dalam** memuat sebuah pertanyaan (pemisah, *split*); **daun** (*leaf*) adalah simpul akhir yang memberi prediksi. Prediksi daun pada klasifikasi adalah **kelas mayoritas** (atau proporsi kelas sebagai peluang kasar) pada contoh latih yang jatuh di daun itu; pada regresi adalah **rata-rata target**-nya.

## Intuisi dan Istilah Penting

### 1. Ketidakmurnian (*Impurity*)

Sebuah simpul dikatakan **murni** bila semua contohnya satu kelas. Kita butuh angka yang mengukur seberapa "bercampur" suatu simpul. Misalkan $p_c$ adalah proporsi kelas $c$ pada simpul.

**Entropy** (dalam satuan bit, logaritma basis 2):

$$
H = -\sum_{c} p_c \log_2 p_c \qquad (0\log_2 0 := 0)
$$

**Gini impurity**:

$$
G = 1 - \sum_{c} p_c^2
$$

Keduanya bernilai $0$ pada simpul murni dan maksimum pada campuran seimbang. Untuk dua kelas dengan proporsi kelas pertama $p$:

| $p$ | Entropy $H$ | Gini $G$ |
|---|---|---|
| $0$ atau $1$ | $0$ | $0$ |
| $0{,}1$ | $0{,}469$ | $0{,}18$ |
| $0{,}2$ | $0{,}722$ | $0{,}32$ |
| $0{,}3$ | $0{,}881$ | $0{,}42$ |
| $0{,}4$ | $0{,}971$ | $0{,}48$ |
| $0{,}5$ | $1{,}000$ | $0{,}50$ |

Contoh tiga kelas dengan jumlah $2, 1, 1$ ($p = 0{,}5;\ 0{,}25;\ 0{,}25$): $H = -[0{,}5\log_2 0{,}5 + 2 \times 0{,}25\log_2 0{,}25] = 0{,}5 + 1 = 1{,}5$ bit; $G = 1 - (0{,}25 + 0{,}0625 + 0{,}0625) = 0{,}625$.

Intuisi *entropy*: jumlah rata-rata pertanyaan ya-atau-tidak yang dibutuhkan untuk menebak kelas sebuah contoh. Campuran 50/50 butuh 1 pertanyaan; simpul murni 0.

### 2. *Information Gain*

Sebuah pemisah membagi simpul induk (berisi $n$ contoh) menjadi anak-anak dengan ukuran $n_k$. **Information gain** adalah penurunan *entropy* rata-rata berbobot:

$$
\mathrm{IG} = H(\text{induk}) - \sum_{k} \frac{n_k}{n}\,H(\text{anak}_k)
$$

Bobot $\dfrac{n_k}{n}$ memastikan anak yang besar lebih diperhitungkan. Versi Gini memakai $G$ sebagai pengganti $H$ (disebut penurunan Gini). Pemisah terbaik adalah yang **memaksimalkan** gain. Algoritmanya **rakus** (*greedy*): pada tiap simpul ia memilih pemisah terbaik saat itu tanpa melihat apakah itu terbaik untuk pohon secara keseluruhan.

### 3. Menentukan Pemisah

- **Fitur biner** (misalnya kata ada atau tidak): satu pemisah per fitur.
- **Fitur numerik**: urutkan nilai unik, lalu coba **titik tengah** antar nilai berurutan sebagai ambang ($x \leq t$ ke kiri, $x > t$ ke kanan). Pilih ambang dengan gain terbesar.
- **Fitur kategorik**: pisahkan per kategori atau kelompok kategori.

### 4. Kapan Berhenti: *Overfitting* dan Pengendaliannya

Pohon yang dibiarkan tumbuh sampai semua daun murni **menghafal** data latih (galat latih $0$), termasuk derau. Pengendali umum (hiperparameter, dipilih lewat validasi):

| Hiperparameter | Arti |
|---|---|
| `max_depth` | kedalaman maksimum |
| `min_samples_split` | minimum contoh agar simpul boleh dipisah |
| `min_samples_leaf` | minimum contoh di setiap daun |
| gain minimum | pemisah hanya bila gain cukup besar |
| *pruning* | memangkas cabang yang tidak membantu pada validasi |

Pohon cenderung punya **varians tinggi**: perubahan kecil pada data (atau seri antar pemisah) dapat menghasilkan pohon yang sangat berbeda. Itu motivasi *Random Forest* di Part 8.

### 5. Pohon Regresi

Untuk target kontinu, ketidakmurnian diganti **jumlah kuadrat galat** (SSE) terhadap rata-rata simpul: $\mathrm{SSE} = \sum_i (y_i - \bar{y})^2$. Pemisah terbaik meminimalkan jumlah SSE kedua anak, dan prediksi daun adalah rata-rata target di daun itu.

### 6. Sifat Penting Pohon

- **Tidak peka skala fitur**: pemisah hanya bergantung pada urutan nilai, sehingga standardisasi tidak mengubah pohon (Part 2).
- **Menangani fitur campuran** (numerik dan kategorik) dan interaksi antar fitur secara alami.
- **Batas keputusan sejajar sumbu** dan **konstan per bagian**: sulit menangkap batas miring atau halus, dan tidak mengekstrapolasi di luar rentang data.
- **Bias terhadap fitur yang banyak nilai uniknya.** Fitur dengan banyak nilai (misalnya ID) mudah menghasilkan gain besar yang kebetulan. Kriteria seperti rasio gain mengoreksinya.
- **Bukan "paling akurat".** Satu pohon sering kalah dibanding model lain; kekuatannya pada keterbacaan dan sebagai komponen ensemble.

## Rumus dan Penjelasan Setiap Simbol

| Konsep | Rumus | Arti simbol dan syarat |
|---|---|---|
| Proporsi kelas | $p_c = n_c/n$ | $n_c$ contoh kelas $c$ di simpul, $n$ total di simpul |
| *Entropy* | $H = -\sum_c p_c\log_2 p_c$ | $0\log_2 0 := 0$; maks. $\log_2 K$ untuk $K$ kelas |
| Gini | $G = 1 - \sum_c p_c^2$ | maks. $1 - 1/K$ |
| *Information gain* | $H(\text{induk}) - \sum_k \frac{n_k}{n}H(\text{anak}_k)$ | $n_k$ ukuran anak ke-$k$ |
| Penurunan Gini | $G(\text{induk}) - \sum_k \frac{n_k}{n}G(\text{anak}_k)$ | sama, dengan $G$ |
| Ambang numerik | $t = (v_i + v_{i+1})/2$ | $v_i < v_{i+1}$ nilai unik berurutan |
| SSE (regresi) | $\sum_i (y_i - \bar{y})^2$ | $\bar{y}$ rata-rata target di simpul |
| Pentingnya fitur | $\sum_{\text{simpul pada fitur } j}\frac{n_{\text{simpul}}}{n}\,\mathrm{IG}$ | dinormalkan agar jumlah $1$ |

## Contoh Numerik Langkah demi Langkah

### Contoh 1 — Memilih pemisah akar pada korpus simulasi

Pakai korpus 10 pesan dari Part 2 (5 Emergency, 5 Non-Emergency; data simulasi) dengan fitur biner "pesan memuat kata $w$". *Entropy* akar: $p_E = p_N = 0{,}5$, jadi $H = 1$ bit ($G = 0{,}5$).

Beberapa pemisah:

| Pemisah | Memuat kata | Tidak memuat | $H$ anak | $\mathrm{IG}$ |
|---|---|---|---|---|
| **besok** | E 0, N 4 (murni) | E 5, N 1 | $0$ dan $0{,}6500$ | $1 - 0{,}6\times0{,}65 = \mathbf{0{,}6100}$ |
| segera | E 3, N 0 (murni) | E 2, N 5 | $0$ dan $0{,}8631$ | $1 - 0{,}7\times0{,}8631 = 0{,}3958$ |
| tolong | E 4, N 1 | E 1, N 4 | $0{,}7219$ dan $0{,}7219$ | $1 - 0{,}7219 = 0{,}2781$ |
| baru | E 0, N 2 (murni) | E 5, N 3 | $0$ dan $0{,}9544$ | $1 - 0{,}8\times0{,}9544 = 0{,}2365$ |
| di | E 2, N 3 | E 3, N 2 | $0{,}9710$ dan $0{,}9710$ | $0{,}0290$ |

Contoh perhitungan untuk "besok": anak yang tidak memuat berisi 6 pesan (5 E, 1 N): $p_E = \dfrac{5}{6}$, $p_N = \dfrac{1}{6}$, $H = -\dfrac{5}{6}\log_2\dfrac{5}{6} - \dfrac{1}{6}\log_2\dfrac{1}{6} = 0{,}2192 + 0{,}4308 = 0{,}6500$. Bobot anak: $\dfrac{4}{10}$ (memuat, $H = 0$) dan $\dfrac{6}{10}$. $\mathrm{IG} = 1 - 0{,}4 \times 0 - 0{,}6 \times 0{,}65 = 0{,}61$.

Pemisah akar terbaik: **besok** ($\mathrm{IG} = 0{,}61$). Dengan Gini juga "besok" terbaik (penurunan $0{,}3333$), disusul segera ($0{,}2143$) dan tolong ($0{,}18$). Kata "di" hampir tidak membedakan kelas ($\mathrm{IG} = 0{,}029$), sesuai intuisi.

### Contoh 2 — Membangun pohon sampai murni

Setelah pemisah "besok": cabang "memuat" berisi 4 pesan N (murni, daun N). Cabang "tidak memuat" berisi 6 pesan (5 E, 1 N: pesan 8, "makan siang di kantin jalan baru"), $H = 0{,}65$. Cari pemisah terbaik di cabang ini:

| Pemisah | Hasil | $\mathrm{IG}$ |
|---|---|---|
| baru (atau makan, siang, kantin) | memuat: N 1; tidak: E 5 (keduanya murni) | $\mathbf{0{,}6500}$ |
| tolong | memuat: E 4; tidak: E 1, N 1 | $0{,}3167$ |
| jalan | memuat: E 1, N 1; tidak: E 4 | $0{,}3167$ |
| segera | memuat: E 3; tidak: E 2, N 1 | $0{,}1909$ |

Kata **baru**, **makan**, **siang**, dan **kantin** semuanya memberi gain penuh ($0{,}65$) karena semuanya hanya muncul di pesan 8: **seri**. Algoritma harus memilih salah satu (di sini pertama menurut abjad, *baru*; implementasi seperti scikit-learn memilih acak, sehingga pohon berbeda untuk `random_state` berbeda). Hasil pohon:

```text
besok?
├─ ya  → N   (4 pesan)
└─ tidak → baru?
          ├─ ya  → N   (1 pesan)
          └─ tidak → E   (5 pesan)
```

Galat latih $0$. Namun cabang terakhir bergantung pada satu kata ("baru") yang hanya kebetulan muncul di satu pesan: contoh *overfitting* pada data kecil. Pohon yang hanya satu pemisah ("besok"): $9$ dari $10$ benar ($90\%$ pada data latih), tetapi lebih sederhana dan mungkin lebih andal pada data baru. Memilih kedalaman dilakukan dengan validasi.

**Pentingnya fitur.** "besok": $\dfrac{10}{10} \times 0{,}61 = 0{,}61$; "baru": $\dfrac{6}{10} \times 0{,}65 = 0{,}39$. Setelah dinormalkan (jumlah $1$): besok $61\%$, baru $39\%$.

### Contoh 3 — Ambang untuk fitur numerik

Data simulasi satu fitur: $x = 1, \dots, 8$ dengan label $y = 0, 0, 1, 0, 0, 1, 1, 1$. *Entropy* akar: empat 0 dan empat 1, $H = 1$.

| Ambang $t$ | Kiri ($x \leq t$) | Kanan | $H$ kiri | $H$ kanan | $\mathrm{IG}$ |
|---|---|---|---|---|---|
| $1{,}5$ | 0:1 | 0:3, 1:4 | $0$ | $0{,}9852$ | $0{,}1379$ |
| $2{,}5$ | 0:2 | 0:2, 1:4 | $0$ | $0{,}9183$ | $0{,}3113$ |
| $3{,}5$ | 0:2, 1:1 | 0:2, 1:3 | $0{,}9183$ | $0{,}9710$ | $0{,}0488$ |
| $4{,}5$ | 0:3, 1:1 | 0:1, 1:3 | $0{,}8113$ | $0{,}8113$ | $0{,}1887$ |
| $\mathbf{5{,}5}$ | 0:4, 1:1 | 1:3 | $0{,}7219$ | $0$ | $\mathbf{0{,}5488}$ |
| $6{,}5$ | 0:4, 1:2 | 1:2 | $0{,}9183$ | $0$ | $0{,}3113$ |
| $7{,}5$ | 0:4, 1:3 | 1:1 | $0{,}9852$ | $0$ | $0{,}1379$ |

Contoh: $t = 5{,}5$: kiri berisi $x = 1..5$ (label $0, 0, 1, 0, 0$): $H = -0{,}8\log_2 0{,}8 - 0{,}2\log_2 0{,}2 = 0{,}7219$; kanan $x = 6, 7, 8$ murni ($H = 0$). $\mathrm{IG} = 1 - \dfrac{5}{8}(0{,}7219) - \dfrac{3}{8}(0) = 0{,}5488$. Ambang terbaik: $x \leq 5{,}5$. Titik $x = 3$ berlabel 1 di antara label 0 adalah "derau" yang membuat daun kiri tidak murni. Memaksanya murni (pemisah tambahan di sekitar $x = 3$) adalah *overfitting*.

### Contoh 4 — Pohon regresi

Data simulasi: $x = 1, 2, 3, 10, 11, 12$ dengan $y = 5, 6, 5, 20, 21, 19$. SSE akar terhadap rata-rata $12{,}67$: $325{,}33$. Jumlah SSE kedua anak untuk beberapa ambang:

| Ambang | SSE kiri + SSE kanan |
|---|---|
| $1{,}5$ | $254{,}8$ |
| $2{,}5$ | $171{,}25$ |
| $\mathbf{6{,}5}$ | $\mathbf{2{,}667}$ |
| $10{,}5$ | $164{,}0$ |
| $11{,}5$ | $277{,}2$ |

Ambang $6{,}5$: kiri $y = 5, 6, 5$ (rata-rata $5{,}33$, SSE $0{,}667$), kanan $y = 20, 21, 19$ (rata-rata $20$, SSE $2$). Total $2{,}667$, turun dari $325{,}33$ (penurunan $322{,}67$). Prediksi: $5{,}33$ untuk $x \leq 6{,}5$, dan $20$ untuk $x > 6{,}5$. Perhatikan bahwa prediksi berbentuk **tangga**: konstan per daun.

### Contoh 5 — Entropy dan Gini pada beberapa simpul

Hitung $H$ dan $G$ untuk simpul dengan (a) 6 E dan 2 N; (b) 4 E dan 4 N; (c) 8 E dan 0 N.

1. (a) $p = (0{,}75;\ 0{,}25)$: $H = -0{,}75\log_2 0{,}75 - 0{,}25\log_2 0{,}25 = 0{,}3113 + 0{,}5 = 0{,}8113$; $G = 1 - (0{,}5625 + 0{,}0625) = 0{,}375$.
2. (b) $p = (0{,}5;\ 0{,}5)$: $H = 1$; $G = 0{,}5$.
3. (c) murni: $H = 0$; $G = 0$.

Semakin seimbang, semakin besar kedua ukuran. Bentuknya mirip sehingga pemisah yang dipilih biasanya sama atau sangat dekat.

## Interpretasi, Asumsi, dan Keterbatasan

- **Keterbacaan.** Aturan pohon dapat dibaca, tetapi pohon dalam dengan ratusan daun tidak lagi mudah dibaca.
- **Rakus.** Pemisah terbaik lokal belum tentu menghasilkan pohon terbaik secara global.
- **Varians tinggi.** Perubahan kecil pada data atau pemecah seri menghasilkan pohon berbeda (Contoh 2).
- **Fitur langka yang kebetulan memisahkan.** Kata yang muncul di satu pesan dapat tampak "informatif" pada data kecil; validasi diperlukan.
- **Kelas tidak seimbang.** Daun kelas minoritas kecil; peluang daun (proporsi) sangat kasar. Bobot kelas atau ambang dapat membantu (Part 11).
- **Tidak ada algoritma yang selalu unggul.** Pohon tunggal sering kurang akurat dibanding ensemble atau model linear pada data teks berdimensi tinggi; hasil bergantung data.

## Hubungan dengan ML dan NLP

- **Aturan yang dapat dijelaskan.** Pada teks, pohon dapat menunjukkan kata mana yang memisahkan kelas pertama kali (di sini "besok"), berguna untuk analisis kesalahan.
- **Teks berdimensi tinggi.** Dengan ribuan kata, banyak kata hanya muncul di beberapa dokumen dan bisa memberi gain kebetulan; satu pohon mudah *overfit*. Pembatasan kedalaman, kriteria berhenti, dan ensemble (Part 8) umum dipakai.
- **Tanpa penskalaan.** Berbeda dengan KNN, pohon tidak membutuhkan standardisasi; fitur TF-IDF dan fitur numerik lain dapat dicampur.
- **Dasar ensemble.** *Random Forest* (Part 8) dan *gradient boosting* memakai banyak pohon.

### Python: entropy, gain, dan pemisah dari nol

Hanya pustaka standar. Korpus simulasi dari Part 2:

```python
import math
from collections import Counter

korpus = [("tolong ada kebakaran di gedung", "E"), ("kecelakaan di jalan tolong kirim ambulans", "E"),
          ("banjir naik tolong evakuasi segera", "E"), ("ada korban kecelakaan butuh ambulans segera", "E"),
          ("kebakaran besar tolong damkar segera", "E"), ("jadwal rapat besok pagi di gedung", "N"),
          ("tolong kirim catatan kuliah besok", "N"), ("makan siang di kantin jalan baru", "N"),
          ("kuliah besok pagi di gedung baru", "N"), ("ada diskon jalan sehat besok", "N")]
data = [(set(t.split()), c) for t, c in korpus]
kosa = sorted(set(w for s, _ in data for w in s))

def entropy(label):
    n = len(label)
    return -sum((k / n) * math.log2(k / n) for k in Counter(label).values()) if n else 0

def gini(label):
    n = len(label)
    return 1 - sum((k / n) ** 2 for k in Counter(label).values()) if n else 0

def gain(d, kata, impuritas=entropy):
    ya = [c for s, c in d if kata in s]
    tidak = [c for s, c in d if kata not in s]
    semua = [c for _, c in d]
    n = len(d)
    return impuritas(semua) - len(ya) / n * impuritas(ya) - len(tidak) / n * impuritas(tidak), Counter(ya), Counter(tidak)

print(entropy([c for _, c in data]), gini([c for _, c in data]))      # 1.0 0.5
print([(p, round(entropy(['a'] * round(10 * p) + ['b'] * (10 - round(10 * p))), 3)) for p in (0.1, 0.3, 0.5)])
# [(0.1, 0.469), (0.3, 0.881), (0.5, 1.0)]

for k in ("besok", "segera", "tolong", "baru", "di"):
    g, ya, tidak = gain(data, k)
    print(k, round(g, 4), dict(ya), dict(tidak))
# besok 0.61 {'N': 4} {'E': 5, 'N': 1}
# segera 0.3958 {'E': 3} {'E': 2, 'N': 5}
# tolong 0.2781 {'E': 4, 'N': 1} {'E': 1, 'N': 4}
# baru 0.2365 {'N': 2} {'E': 5, 'N': 3}
# di 0.029 {'E': 2, 'N': 3} {'E': 3, 'N': 2}

urut = sorted(((gain(data, k)[0], k) for k in kosa), reverse=True)[:3]
print([(round(g, 4), k) for g, k in urut])        # [(0.61, 'besok'), (0.3958, 'segera'), (0.2781, 'tolong')]
urut_gini = sorted(((gain(data, k, gini)[0], k) for k in kosa), reverse=True)[:3]
print([(round(g, 4), k) for g, k in urut_gini])   # [(0.3333, 'besok'), (0.2143, 'segera'), (0.18, 'tolong')]

anak = [(s, c) for s, c in data if "besok" not in s]       # cabang "tidak memuat besok"
print(len(anak), round(entropy([c for _, c in anak]), 4))                          # 6 0.65
for k in ("baru", "tolong", "jalan", "segera"):
    print(k, round(gain(anak, k)[0], 4))
# baru 0.65
# tolong 0.3167
# jalan 0.3167
# segera 0.1909
```

Ambang untuk fitur numerik dan pohon regresi (Contoh 3 dan 4):

```python
x = [1, 2, 3, 4, 5, 6, 7, 8]
y = [0, 0, 1, 0, 0, 1, 1, 1]
for a, b in zip(x, x[1:]):
    t = (a + b) / 2
    kiri = [c for v, c in zip(x, y) if v <= t]
    kanan = [c for v, c in zip(x, y) if v > t]
    ig = entropy(y) - len(kiri) / 8 * entropy(kiri) - len(kanan) / 8 * entropy(kanan)
    print(t, round(ig, 4))
# 1.5 0.1379
# 2.5 0.3113
# 3.5 0.0488
# 4.5 0.1887
# 5.5 0.5488
# 6.5 0.3113
# 7.5 0.1379

def sse(v):
    m = sum(v) / len(v)
    return sum((a - m) ** 2 for a in v)

xr, yr = [1, 2, 3, 10, 11, 12], [5, 6, 5, 20, 21, 19]
print(round(sse(yr), 2))                                                           # 325.33
for t in (1.5, 2.5, 6.5, 10.5, 11.5):
    print(t, round(sse([c for a, c in zip(xr, yr) if a <= t]) + sse([c for a, c in zip(xr, yr) if a > t]), 3))
# 1.5 254.8
# 2.5 171.25
# 6.5 2.667
# 10.5 164.0
# 11.5 277.2
```

**Opsional: bandingkan dengan scikit-learn.** Blok ini membutuhkan scikit-learn, yang **tidak** dipasang oleh proyek ini dan tidak dijalankan oleh situs. Keluaran dijalankan dengan scikit-learn 1.9.0 pada Python 3.14.

```python
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.tree import DecisionTreeClassifier, export_text

cv = CountVectorizer(binary=True)
X = cv.fit_transform([t for t, _ in korpus])
yy = [c for _, c in korpus]
pohon = DecisionTreeClassifier(criterion="entropy", random_state=0).fit(X, yy)
print(export_text(pohon, feature_names=list(cv.get_feature_names_out())))
# |--- besok <= 0.50
# |   |--- baru <= 0.50
# |   |   |--- class: E
# |   |--- baru >  0.50
# |   |   |--- class: N
# |--- besok >  0.50
# |   |--- class: N
print(pohon.feature_importances_.max().round(2), cv.get_feature_names_out()[pohon.feature_importances_.argmax()])   # 0.61 besok
```

Interpretasi: pemisah akar "besok" dan kepentingan $0{,}61$ sama dengan hitungan manual. Pada pemisah kedua, scikit-learn dapat memilih "baru", "makan", atau "kantin" tergantung `random_state` (seri), seperti yang kita bahas pada Contoh 2.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Lupa bobot $n_k/n$ pada *information gain* | anak kecil dan besar dihitung sama | rata-rata berbobot |
| Menjumlahkan *entropy* anak tanpa bobot | gain keliru | bobot ukuran anak |
| Membiarkan pohon tumbuh sampai semua daun murni | *overfitting* | batasi kedalaman, minimum contoh, atau pangkas |
| Menganggap pemisah seri tidak penting | pohon berbeda untuk data yang sama | sadari ketidakstabilan |
| Menganggap pohon butuh standardisasi | tidak peka skala | tidak perlu |
| Menafsirkan peluang daun sebagai peluang terkalibrasi | daun kecil memberi proporsi kasar | periksa kalibrasi |
| Percaya pentingnya fitur sebagai sebab-akibat | hanya kontribusi pada pemisahan data latih | hati-hati |
| Memakai fitur dengan banyak nilai unik (misalnya ID) | gain kebetulan besar | buang atau pakai rasio gain |
| Mengharapkan ekstrapolasi | prediksi konstan di luar rentang | gunakan model lain |

## Latihan Mandiri

1. Hitung $H$ dan $G$ untuk simpul dengan (a) 6 E dan 2 N; (b) 4 E dan 4 N; (c) 8 E dan 0 N.
2. Hitung $H$ dan $G$ untuk simpul tiga kelas dengan jumlah $2, 1, 1$.
3. Hitung *information gain* pemisah "tolong" pada korpus 10 pesan (memuat: E 4, N 1; tidak: E 1, N 4). Tunjukkan perhitungan *entropy* tiap anak.
4. Untuk pemisah "segera" (memuat: E 3; tidak: E 2, N 5), hitung *entropy* anak dan *information gain*, lalu bandingkan dengan "besok" ($0{,}61$).
5. Hitung penurunan Gini untuk pemisah "besok" (memuat: N 4; tidak: E 5, N 1).
6. Data $x = 1, \dots, 8$ dan $y = 0, 0, 1, 0, 0, 1, 1, 1$. Hitung *information gain* untuk ambang $t = 4{,}5$ dan $t = 5{,}5$, dan tentukan ambang terbaik.
7. Mengapa pohon yang tumbuh sampai semua daun murni cenderung *overfit*? Sebutkan tiga cara membatasi pertumbuhan.
8. Pada Contoh 2, mengapa "baru", "makan", "siang", dan "kantin" seri? Apa akibat seri itu bagi stabilitas pohon?
9. Pohon regresi: $x = 1, 2, 3, 10, 11, 12$ dan $y = 5, 6, 5, 20, 21, 19$. Hitung SSE total kedua anak untuk ambang $6{,}5$ dan prediksi tiap daun.
10. Hitung pentingnya fitur (dinormalkan) untuk pohon dua pemisah: "besok" di akar (10 contoh, $\mathrm{IG} = 0{,}61$) dan "baru" pada simpul berisi 6 contoh ($\mathrm{IG} = 0{,}65$). Apa kelemahan memakai nilai ini sebagai ukuran "pengaruh"?

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) $p = (0{,}75;\ 0{,}25)$: $H = 0{,}8113$; $G = 1 - (0{,}5625 + 0{,}0625) = 0{,}375$. (b) $H = 1$; $G = 0{,}5$. (c) murni: $H = 0$; $G = 0$.

??? success "Soal 2"
    $p = (0{,}5;\ 0{,}25;\ 0{,}25)$. $H = 0{,}5(1) + 0{,}25(2) + 0{,}25(2) = 1{,}5$ bit. $G = 1 - (0{,}25 + 0{,}0625 + 0{,}0625) = 0{,}625$.

??? success "Soal 3"
    Memuat (E 4, N 1): $p = (0{,}8;\ 0{,}2)$, $H = -0{,}8\log_2 0{,}8 - 0{,}2\log_2 0{,}2 = 0{,}2575 + 0{,}4644 = 0{,}7219$. Tidak memuat (E 1, N 4): $H = 0{,}7219$. $\mathrm{IG} = 1 - \dfrac{5}{10}(0{,}7219) - \dfrac{5}{10}(0{,}7219) = 1 - 0{,}7219 = 0{,}2781$.

??? success "Soal 4"
    Memuat (E 3): murni, $H = 0$. Tidak memuat (E 2, N 5): $p = (\frac{2}{7}, \frac{5}{7})$, $H = -\frac{2}{7}\log_2\frac{2}{7} - \frac{5}{7}\log_2\frac{5}{7} = 0{,}5164 + 0{,}3467 = 0{,}8631$. $\mathrm{IG} = 1 - \dfrac{3}{10}(0) - \dfrac{7}{10}(0{,}8631) = 1 - 0{,}6042 = 0{,}3958$. Lebih kecil dari "besok" ($0{,}61$), jadi "besok" lebih baik: ia menyingkirkan lebih banyak ketidakmurnian (anak murni terbesar dan anak lain yang hampir murni).

??? success "Soal 5"
    Akar $G = 0{,}5$. Anak memuat (N 4): $G = 0$. Anak tidak memuat (E 5, N 1): $G = 1 - (\frac{25}{36} + \frac{1}{36}) = 1 - \frac{26}{36} = 0{,}2778$. Penurunan Gini $= 0{,}5 - \dfrac{4}{10}(0) - \dfrac{6}{10}(0{,}2778) = 0{,}5 - 0{,}1667 = 0{,}3333$.

??? success "Soal 6"
    $t = 4{,}5$: kiri $x = 1..4$ (labels $0, 0, 1, 0$: 3 nol dan 1 satu), $H = 0{,}8113$; kanan $x = 5..8$ ($0, 1, 1, 1$): $H = 0{,}8113$; $\mathrm{IG} = 1 - 0{,}5(0{,}8113) - 0{,}5(0{,}8113) = 0{,}1887$. $t = 5{,}5$: $\mathrm{IG} = 0{,}5488$ (lihat Contoh 3). Ambang terbaik $t = 5{,}5$.

??? success "Soal 7"
    Pohon dalam membuat daun untuk setiap kelompok kecil, termasuk derau, sehingga mengikuti kebetulan data latih (galat latih nol tetapi galat data baru besar). Cara membatasi: `max_depth` (kedalaman maksimum), `min_samples_leaf` (minimum contoh per daun), gain minimum untuk pemisah, atau *pruning*; semuanya dipilih lewat data validasi.

??? success "Soal 8"
    Keempat kata hanya muncul di pesan 8 (satu pesan N di cabang itu), sehingga semuanya memisahkan pesan 8 dari lima pesan E dengan sempurna: gain sama ($0{,}65$). Karena seri, pilihan antar kata bersifat sewenang-wenang (alfabetis atau acak). Akibatnya pohon bisa berbeda untuk data yang sama (stabilitas rendah), dan pemisah yang dipilih bergantung pada kata yang kebetulan unik di satu pesan, bukan pola umum.

??? success "Soal 9"
    Ambang $6{,}5$: kiri $y = 5, 6, 5$ (rata-rata $5{,}333$; SSE $= 0{,}111 + 0{,}444 + 0{,}111 = 0{,}667$); kanan $y = 20, 21, 19$ (rata-rata $20$; SSE $= 0 + 1 + 1 = 2$). Total $2{,}667$. Prediksi: $5{,}333$ untuk $x \leq 6{,}5$ dan $20$ untuk $x > 6{,}5$.

??? success "Soal 10"
    Kontribusi: "besok" $\dfrac{10}{10} \times 0{,}61 = 0{,}61$; "baru" $\dfrac{6}{10} \times 0{,}65 = 0{,}39$. Total $1{,}00$, jadi besok $61\%$ dan baru $39\%$. Kelemahan: ini hanya ukuran seberapa banyak fitur dipakai untuk menurunkan ketidakmurnian pada data **latih**; kata "baru" tampak penting hanya karena kebetulan unik di satu pesan; nilainya juga bias terhadap fitur dengan banyak nilai unik dan tidak menunjukkan sebab-akibat.

## Ringkasan Materi

- Pohon keputusan membagi data dengan pertanyaan tentang satu fitur; daun memberi kelas mayoritas (klasifikasi) atau rata-rata (regresi).
- Ketidakmurnian: *entropy* $H = -\sum p_c\log_2 p_c$ dan Gini $G = 1 - \sum p_c^2$; keduanya $0$ pada simpul murni dan maksimum pada campuran seimbang.
- *Information gain* $=$ $H$(induk) $-$ rata-rata berbobot $H$(anak); pemisah dipilih rakus dengan gain terbesar. Fitur numerik: coba titik tengah antar nilai.
- Regresi: pemisah meminimalkan jumlah SSE anak; daun memberi rata-rata.
- Pohon yang dibiarkan tumbuh menghafal data (*overfitting*) dan tidak stabil (seri, perubahan kecil); batasi kedalaman, minimum contoh, atau pangkas, dan pilih lewat validasi.
- Pohon tidak peka skala, menangani fitur campuran, tetapi batasnya sejajar sumbu dan tidak mengekstrapolasi.
- Pada NLP, pohon memberi aturan kata yang dapat dibaca tetapi mudah *overfit* pada teks berdimensi tinggi; angka contoh adalah simulasi.

## Checklist Penguasaan

- [ ] Menghitung *entropy* dan Gini sebuah simpul.
- [ ] Menghitung *information gain* dan memilih pemisah terbaik secara manual.
- [ ] Mencari ambang terbaik pada fitur numerik.
- [ ] Menjelaskan pohon regresi dengan SSE.
- [ ] Menjelaskan *overfitting* pada pohon dan cara membatasinya.
- [ ] Menyebut sifat pohon (tidak peka skala, tidak stabil, sejajar sumbu).

## Prasyarat untuk Materi Berikutnya

[Part 8 — Matematika Random Forest](08-random-forest.md) mengatasi varians tinggi pohon dengan merata-ratakan banyak pohon yang dilatih pada sampel bootstrap dan subset fitur acak. Pastikan konsep varians (Level 3 Part 7) dan bootstrap (Level 3 Part 9) sudah jelas.
