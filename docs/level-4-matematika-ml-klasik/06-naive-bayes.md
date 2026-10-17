# Part 6 — Matematika Naive Bayes

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menurunkan klasifikasi Naive Bayes dari Teorema Bayes dan asumsi independensi bersyarat;
- membedakan prior, likelihood, evidence, dan posterior pada klasifikasi;
- menghitung Multinomial Naive Bayes dari frekuensi kata, termasuk Laplace smoothing;
- menghitung Gaussian Naive Bayes untuk fitur kontinu;
- menjelaskan Bernoulli Naive Bayes (kehadiran kata) dan perbedaannya dengan Multinomial;
- menjelaskan pemakaian logaritma untuk menghindari *underflow*;
- menjelaskan akibat asumsi "naif" pada keyakinan model dan mengapa klasifikasi masih sering berfungsi;
- menghubungkan Naive Bayes dengan regresi logistik dan dengan klasifikasi teks.

## Prasyarat

[Level 3 Part 6 — Teorema Bayes](../level-3-probabilitas-dan-statistika/06-teorema-bayes.md) (prior, likelihood, posterior, pengantar Naive Bayes, Laplace smoothing), [Level 3 Part 5](../level-3-probabilitas-dan-statistika/05-probabilitas-bersyarat-dan-independensi.md) (independensi), [Level 3 Part 8](../level-3-probabilitas-dan-statistika/08-distribusi-probabilitas-umum.md) (distribusi Normal), [Part 2](02-representasi-data-dan-feature-engineering.md) (BoW), dan [Part 4](04-regresi-logistik.md) (log-odds).

## Masalah yang Ingin Diselesaikan

Kita ingin mengklasifikasikan contoh $\mathbf{x} = (x_1, \dots, x_d)$ ke kelas $C$ dengan peluang posterior $P(C \mid \mathbf{x})$. Teorema Bayes memberi

$$
P(C \mid \mathbf{x}) = \frac{P(\mathbf{x} \mid C)\,P(C)}{P(\mathbf{x})}
$$

Masalahnya, $P(\mathbf{x} \mid C)$ (peluang **seluruh kombinasi** fitur) sulit ditaksir: untuk $d$ fitur biner ada $2^d$ kombinasi, jauh melebihi jumlah data. **Naive Bayes** menyederhanakannya dengan satu asumsi kuat.

## Intuisi dan Istilah Penting

### 1. Asumsi Independensi Bersyarat

Asumsi "naif": diberikan kelas $C$, **fitur-fitur saling independen**:

$$
P(\mathbf{x} \mid C) = \prod_{j=1}^{d} P(x_j \mid C)
$$

Maka, karena penyebut $P(\mathbf{x})$ sama untuk semua kelas:

$$
P(C \mid \mathbf{x}) \;\propto\; P(C)\prod_{j=1}^{d} P(x_j \mid C)
\qquad
\hat{C} = \arg\max_{C}\ P(C)\prod_{j=1}^{d} P(x_j \mid C)
$$

- **Prior** $P(C)$: seberapa umum kelas $C$ (dari proporsi di data latih).
- **Likelihood** $P(x_j \mid C)$: peluang fitur $x_j$ bila kelasnya $C$ (ditaksir dari data latih per kelas).
- **Posterior**: skor di atas setelah dinormalkan agar jumlah antar kelas $1$.

Yang perlu ditaksir hanya $P(C)$ dan $P(x_j \mid C)$ per fitur, jauh lebih sedikit parameter daripada $P(\mathbf{x} \mid C)$ utuh. Tidak ada optimasi iteratif: seluruh "pelatihan" adalah **menghitung** frekuensi (atau rata-rata dan varians).

### 2. Bentuk Logaritma

Perkalian banyak peluang kecil menghasilkan angka yang sangat kecil dan komputer menyatakannya sebagai $0$ (*underflow*): $0{,}001^{120} = 10^{-360}$ menjadi $0{,}0$ pada bilangan desimal 64-bit. Karena $\ln$ naik terus-menerus, kelas dengan skor terbesar tetap sama jika kita membandingkan **log-skor**:

$$
\ln P(C \mid \mathbf{x}) + \text{konstanta} = \ln P(C) + \sum_{j=1}^{d}\ln P(x_j \mid C)
$$

Perkalian menjadi penjumlahan: $\ln 0{,}001^{120} = 120\ln 0{,}001 = -828{,}93$, angka biasa yang aman. Untuk mendapatkan posterior dua kelas dari log-skor $\ell_1, \ell_0$: $P(C = 1 \mid \mathbf{x}) = \dfrac{1}{1 + e^{\ell_0 - \ell_1}}$, yaitu sigmoid dari selisih log-skor.

### 3. Tiga Varian

Pilihan model $P(x_j \mid C)$ menentukan varian:

| Varian | Fitur | $P(x_j \mid C)$ | Cocok untuk |
|---|---|---|---|
| **Multinomial** | hitungan (banyak kata) | proporsi kata $j$ di dokumen kelas $C$ | klasifikasi teks (BoW, hitungan) |
| **Bernoulli** | biner (kata ada atau tidak) | peluang kata ada di dokumen kelas $C$ | teks pendek, fitur biner |
| **Gaussian** | kontinu | kepadatan Normal $\mathcal{N}(\mu_{jC}, \sigma^2_{jC})$ | fitur numerik |

### 4. Multinomial Naive Bayes dan Laplace Smoothing

Bayangkan semua dokumen kelas $C$ digabung menjadi satu "tas kata" besar. Peluang kata $w$ pada kelas $C$ diperkirakan dari proporsinya, dengan **smoothing** $\alpha$ (Laplace bila $\alpha = 1$):

$$
P(w \mid C) = \frac{n_{wC} + \alpha}{N_C + \alpha\,\lvert V \rvert}
$$

- $n_{wC}$: berapa kali kata $w$ muncul di semua dokumen latih kelas $C$;
- $N_C$: total kata di semua dokumen latih kelas $C$ ($= \sum_w n_{wC}$);
- $\lvert V \rvert$: ukuran kosakata;
- $\alpha \geq 0$: *smoothing*. $\alpha = 0$ tanpa smoothing.

Penyebut $N_C + \alpha\lvert V \rvert$ memastikan $\sum_w P(w \mid C) = 1$ ($\alpha$ ditambahkan ke tiap kata dari $\lvert V \rvert$ kata). Untuk dokumen dengan hitungan kata $x_w$ (jumlah kemunculan kata $w$):

$$
\ln P(C \mid \text{dok}) \;\propto\; \ln P(C) + \sum_{w \in V} x_w\,\ln P(w \mid C)
$$

Kata yang muncul dua kali berkontribusi dua kali. Kata di luar kosakata latih diabaikan.

**Mengapa smoothing diperlukan.** Tanpa smoothing, kata yang belum pernah muncul di kelas $C$ memberi $P(w \mid C) = 0$ sehingga seluruh skor kelas itu menjadi $0$ (atau $\ln 0 = -\infty$), tidak peduli bukti dari kata lain. Smoothing memberi sedikit peluang pada kata yang belum teramati. Besar $\alpha$ mengatur seberapa kuat: $\alpha$ besar menarik semua peluang ke seragam (kurang yakin).

**Hubungan dengan regresi logistik.** Untuk dua kelas, log-odds Multinomial NB adalah

$$
\ln\frac{P(C = 1 \mid \text{dok})}{P(C = 0 \mid \text{dok})} = \ln\frac{P(1)}{P(0)} + \sum_w x_w\,\ln\frac{P(w \mid 1)}{P(w \mid 0)}
$$

yaitu **fungsi linear dari hitungan kata**, dengan bobot $w_w = \ln\frac{P(w \mid 1)}{P(w \mid 0)}$. Jadi Naive Bayes adalah klasifikator linear seperti regresi logistik (Part 4); bedanya, bobotnya dihitung langsung dari frekuensi (model **generatif**: memodelkan $P(\mathbf{x} \mid C)$ dan $P(C)$), sedangkan regresi logistik mengoptimasi bobot untuk meminimalkan *log loss* (model **diskriminatif**: memodelkan $P(C \mid \mathbf{x})$ langsung).

### 5. Gaussian Naive Bayes

Untuk fitur kontinu, asumsikan dalam tiap kelas fitur $j$ berdistribusi Normal dengan rata-rata $\mu_{jC}$ dan varians $\sigma_{jC}^2$ yang ditaksir dari data latih kelas $C$ (rata-rata dan varians dengan pembagi $n_C$, estimasi kemungkinan maksimum):

$$
P(x_j \mid C) = \frac{1}{\sqrt{2\pi\sigma_{jC}^2}}\exp\!\left(-\frac{(x_j - \mu_{jC})^2}{2\sigma_{jC}^2}\right)
$$

Perhatikan bahwa ini adalah **kepadatan**, bukan peluang (Level 3 Part 7); ia boleh lebih dari $1$. Ketika dibandingkan antar kelas dengan fitur yang sama, itu tidak masalah, karena posterior dinormalkan. Varians yang sangat kecil (atau nol) menghasilkan kepadatan yang meledak, sehingga implementasi menambahkan bilangan kecil pada varians (*variance smoothing*).

### 6. Bernoulli Naive Bayes (Ringkas)

Fitur biner $x_j \in \{0, 1\}$ (kata ada atau tidak). Dengan $\theta_{jC} = P(x_j = 1 \mid C)$ (smoothing biasanya $\frac{n_{jC} + 1}{n_C + 2}$, Level 3 Part 6):

$$
P(\mathbf{x} \mid C) = \prod_j \theta_{jC}^{\,x_j}\,\bigl(1 - \theta_{jC}\bigr)^{1 - x_j}
$$

Berbeda dengan Multinomial, Bernoulli **juga memakai ketiadaan kata** ($1 - \theta_{jC}$ untuk kata yang tidak ada), dan tidak peduli berapa kali sebuah kata muncul.

### 7. Akibat Asumsi "Naif"

Fitur nyata hampir selalu saling berkorelasi bersyarat pada kelas (misalnya "kebakaran" dan "damkar" cenderung muncul bersama). Naive Bayes memperlakukan keduanya sebagai bukti yang **independen**, sehingga **menghitung bukti yang sama berulang kali**. Contoh ekstrem: bila satu fitur punya rasio likelihood $4$ (odds naik 4 kali) dan ia **diduplikasi**, model menghitungnya dua kali: odds naik $4 \times 4 = 16$ kali.

| Kondisi | Odds posterior (prior 0,5) | Peluang |
|---|---|---|
| satu fitur (rasio likelihood 4) | $4$ | $0{,}8$ |
| fitur yang sama diduplikasi | $16$ | $0{,}9412$ |

Konsekuensi: **peluang** yang dihasilkan Naive Bayes sering **terlalu ekstrem** (mendekati 0 atau 1) dan tidak terkalibrasi, walaupun **peringkat** antar kelas, dan karenanya keputusan klasifikasi, sering masih baik. Gunakan untuk memutuskan kelas atau membandingkan skor, bukan sebagai peluang yang dapat dipercaya tanpa kalibrasi (Part 11).

## Rumus dan Penjelasan Setiap Simbol

| Konsep | Rumus | Arti simbol dan syarat |
|---|---|---|
| Aturan Naive Bayes | $\hat{C} = \arg\max_C P(C)\prod_j P(x_j \mid C)$ | independen bersyarat pada $C$ |
| Log-skor | $\ln P(C) + \sum_j \ln P(x_j \mid C)$ | menghindari *underflow* |
| Posterior dua kelas | $\dfrac{1}{1 + e^{\ell_0 - \ell_1}}$ | $\ell_c$ log-skor kelas $c$ |
| Prior | $P(C) = \dfrac{n_C^{\text{dok}}}{n}$ | proporsi dokumen latih kelas $C$ |
| Multinomial | $P(w \mid C) = \dfrac{n_{wC} + \alpha}{N_C + \alpha\lvert V \rvert}$ | $N_C$ total kata kelas $C$, $\lvert V \rvert$ kosakata |
| Skor dokumen | $\ln P(C) + \sum_w x_w\ln P(w \mid C)$ | $x_w$ hitungan kata $w$ |
| Gaussian | $P(x_j \mid C) = \mathcal{N}(x_j; \mu_{jC}, \sigma_{jC}^2)$ | kepadatan, bukan peluang |
| Bernoulli | $\prod_j \theta_{jC}^{x_j}(1 - \theta_{jC})^{1 - x_j}$ | $x_j \in \{0, 1\}$ |

## Contoh Numerik Langkah demi Langkah

### Contoh 1 — Dua fitur dengan Teorema Bayes

Prior $P(E) = 0{,}3$, $P(N) = 0{,}7$. Fitur kehadiran kata A dan B (keduanya hadir pada pesan baru): $P(A \mid E) = 0{,}8$, $P(A \mid N) = 0{,}2$, $P(B \mid E) = 0{,}6$, $P(B \mid N) = 0{,}3$.

1. Skor E: $0{,}3 \times 0{,}8 \times 0{,}6 = 0{,}144$.
2. Skor N: $0{,}7 \times 0{,}2 \times 0{,}3 = 0{,}042$.
3. Jumlah: $0{,}186$. Posterior E: $\dfrac{0{,}144}{0{,}186} \approx 0{,}7742$; posterior N: $0{,}2258$.
4. Prediksi: E. Perhatikan bahwa prior $0{,}3$ (E jarang) tidak cukup melawan bukti dari dua kata yang sama-sama lebih mungkin pada E.

### Contoh 2 — Multinomial Naive Bayes pada korpus simulasi

Pakai korpus 10 pesan dari Part 2 (5 E, 5 N; data simulasi). Klasifikasikan **"tolong ada banjir di jalan"** dengan $\alpha = 1$.

**Statistik latih.**

1. Total kata kelas E: $N_E = 27$; kelas N: $N_N = 28$ (jumlah $55$). Ukuran kosakata $\lvert V \rvert = 29$. Prior: $P(E) = P(N) = \dfrac{5}{10} = 0{,}5$.
2. Hitungan kata pesan baru:

| Kata | $n_{wE}$ | $n_{wN}$ |
|---|---|---|
| tolong | 4 | 1 |
| ada | 2 | 1 |
| banjir | 1 | 0 |
| di | 2 | 3 |
| jalan | 1 | 2 |

**Peluang kata dengan smoothing.** Penyebut E: $27 + 29 = 56$; penyebut N: $28 + 29 = 57$.

| Kata | $P(w \mid E) = \dfrac{n_{wE} + 1}{56}$ | $P(w \mid N) = \dfrac{n_{wN} + 1}{57}$ |
|---|---|---|
| tolong | $\dfrac{5}{56} = 0{,}08929$ | $\dfrac{2}{57} = 0{,}03509$ |
| ada | $\dfrac{3}{56} = 0{,}05357$ | $\dfrac{2}{57} = 0{,}03509$ |
| banjir | $\dfrac{2}{56} = 0{,}03571$ | $\dfrac{1}{57} = 0{,}01754$ |
| di | $\dfrac{3}{56} = 0{,}05357$ | $\dfrac{4}{57} = 0{,}07018$ |
| jalan | $\dfrac{2}{56} = 0{,}03571$ | $\dfrac{3}{57} = 0{,}05263$ |

**Skor.**

3. Skor E $= 0{,}5 \times 0{,}08929 \times 0{,}05357 \times 0{,}03571 \times 0{,}05357 \times 0{,}03571 \approx 1{,}634 \times 10^{-7}$.
4. Skor N $= 0{,}5 \times 0{,}03509 \times 0{,}03509 \times 0{,}01754 \times 0{,}07018 \times 0{,}05263 \approx 3{,}989 \times 10^{-8}$.
5. Dalam log: $\ell_E = \ln 0{,}5 + \sum \ln P(w \mid E) = -15{,}627$; $\ell_N = -17{,}037$. Selisih $\ell_E - \ell_N = 1{,}410$.
6. Posterior E $= \dfrac{1}{1 + e^{-1{,}410}} = \dfrac{1{,}634 \times 10^{-7}}{1{,}634 \times 10^{-7} + 3{,}989 \times 10^{-8}} \approx 0{,}8038$.

Pesan diklasifikasikan **Emergency** dengan skor $\approx 80\%$. Kata "banjir" ($1$ kali di E, $0$ di N) dan "tolong" paling banyak menyumbang bukti ke E; "di" dan "jalan" justru sedikit mendukung N.

**Tanpa smoothing** ($\alpha = 0$): $P(\text{banjir} \mid N) = \dfrac{0}{28} = 0$, sehingga skor N $= 0$ dan posterior E menjadi tepat $1$, padahal kita hanya tidak pernah melihat "banjir" di pesan N pada 5 contoh. Itulah masalah frekuensi nol.

**Pengaruh $\alpha$** (pesan yang sama): $\alpha = 0{,}1$: posterior E $= 0{,}9704$; $\alpha = 1$: $0{,}8038$; $\alpha = 5$: $0{,}6185$. $\alpha$ lebih besar membuat model kurang yakin karena peluang kata ditarik mendekati seragam. $\alpha$ adalah hiperparameter yang dipilih lewat validasi.

Pesan lain, **"jadwal kuliah besok di gedung"**: posterior E $= 0{,}0179$ (hasil sebaliknya, **Non-Emergency**, dengan keyakinan $98{,}2\%$).

### Contoh 3 — Gaussian Naive Bayes

Dua fitur kontinu (data simulasi): banyak kata pesan $x_1$ dan banyak tanda seru $x_2$. Tiga contoh per kelas:

| Kelas | $(x_1, x_2)$ |
|---|---|
| E | $(12, 3),\ (8, 2),\ (10, 4)$ |
| N | $(6, 0),\ (9, 1),\ (7, 0)$ |

**Statistik per kelas** (varians dengan pembagi $n_C = 3$):

| | $\mu_{1}$ | $\sigma_1^2$ | $\mu_{2}$ | $\sigma_2^2$ |
|---|---|---|---|---|
| E | $10$ | $\dfrac{4 + 4 + 0}{3} = 2{,}667$ | $3$ | $\dfrac{0 + 1 + 1}{3} = 0{,}667$ |
| N | $7{,}333$ | $1{,}556$ | $0{,}333$ | $0{,}222$ |

Klasifikasikan $\mathbf{x} = (9, 2)$ dengan prior sama ($0{,}5$).

1. $P(x_1 = 9 \mid E) = \dfrac{1}{\sqrt{2\pi \cdot 2{,}667}}\exp\!\left(-\dfrac{(9 - 10)^2}{2 \cdot 2{,}667}\right) = 0{,}2443 \times e^{-0{,}1875} = 0{,}2025$.
2. $P(x_2 = 2 \mid E) = \dfrac{1}{\sqrt{2\pi \cdot 0{,}667}}\exp\!\left(-\dfrac{(2 - 3)^2}{2 \cdot 0{,}667}\right) = 0{,}4886 \times e^{-0{,}75} = 0{,}2308$.
3. $P(x_1 = 9 \mid N) = 0{,}1310$ dan $P(x_2 = 2 \mid N) = 0{,}00163$ (nilai $x_2 = 2$ sangat jauh dari rata-rata kelas N yang $0{,}333$).
4. Skor E $= 0{,}5 \times 0{,}2025 \times 0{,}2308 = 0{,}02337$. Skor N $= 0{,}5 \times 0{,}1310 \times 0{,}00163 = 0{,}000107$.
5. Posterior E $= \dfrac{0{,}02337}{0{,}02337 + 0{,}000107} \approx 0{,}9954$.

Prediksi: E. Fitur tanda seru ($x_2 = 2$) paling menentukan karena kelas N hampir tidak pernah punya tanda seru sebanyak itu.

### Contoh 4 — Duplikasi fitur menggelembungkan keyakinan

Satu fitur dengan rasio likelihood $4$ dan prior $0{,}5$: odds posterior $= 1 \times 4 = 4$, peluang $= \dfrac{4}{1 + 4} = 0{,}8$. Bila fitur yang sama dimasukkan dua kali, Naive Bayes mengalikan rasio itu dua kali: odds $= 16$, peluang $= \dfrac{16}{17} = 0{,}9412$. Bukti yang sama tidak bertambah, tetapi model menjadi lebih yakin: ini konsekuensi langsung asumsi independensi.

### Contoh 5 — Underflow dan logaritma

Dokumen dengan 120 kata, masing-masing berpeluang $0{,}001$ pada suatu kelas.

1. Produk langsung: $0{,}001^{120} = 10^{-360}$, yang lebih kecil dari bilangan terkecil yang dapat disimpan bilangan desimal 64-bit ($\approx 10^{-308}$), sehingga menjadi $0{,}0$.
2. Jumlah log: $120 \times \ln 0{,}001 = 120 \times (-6{,}9078) = -828{,}93$: angka biasa.
3. Membandingkan dua kelas lewat log-skor memberi urutan yang sama seperti membandingkan produk.

## Interpretasi, Asumsi, dan Keterbatasan

- **Independensi bersyarat** hampir selalu hanya perkiraan kasar; ia menyebabkan peluang tidak terkalibrasi (bagian 7).
- **Kelangkaan data.** Naive Bayes butuh sedikit data untuk menaksir parameternya (hanya frekuensi), sehingga relatif baik pada data kecil dan dimensi tinggi.
- **Smoothing.** $\alpha$ memengaruhi keyakinan, dan perlu dipilih lewat validasi.
- **Kata di luar kosakata** diabaikan; perubahan topik antar waktu dapat menurunkan kinerja.
- **Gaussian NB** mengasumsikan fitur kira-kira Normal dalam tiap kelas dan independen; fitur yang jelas miring atau multimodal melanggarnya (transformasi seperti $\ln$ dapat membantu, Part 2).
- **Prior mengikuti data latih.** Bila proporsi kelas saat dipakai berbeda dari data latih, prior harus disesuaikan, sama seperti pengaruh tingkat dasar pada presisi (Level 3 Part 6).
- **Tidak ada algoritma yang selalu unggul.** Naive Bayes sering menjadi baseline yang kuat untuk teks, tetapi regresi logistik atau SVM linear sering lebih baik bila data cukup; hasil bergantung data.

## Hubungan dengan ML dan NLP

- **Klasifikasi teks.** Multinomial NB pada hitungan kata (atau, dalam praktik, pada TF-IDF) adalah baseline klasik: cepat, sederhana, dan bobotnya ($\ln\frac{P(w \mid 1)}{P(w \mid 0)}$) dapat dibaca sebagai kata mana yang menunjuk kelas mana.
- **Interpretasi kata.** Pada contoh: "banjir" punya rasio likelihood $\dfrac{2/56}{1/57} = 2{,}04$ untuk E dibanding N, sedangkan "di" $\dfrac{3/56}{4/57} = 0{,}76$ (sedikit condong ke N).
- **Kelas tidak seimbang.** Prior kelas mengikuti frekuensi data latih; untuk kelas darurat yang jarang, prior kecil menahan prediksi darurat, dan ambang atau prior dapat disesuaikan (Part 11).
- **Pengantar generatif vs diskriminatif.** NB memodelkan bagaimana data dihasilkan per kelas; regresi logistik memodelkan batas keputusan. Pada data kecil NB sering lebih stabil, pada data besar yang cukup regresi logistik sering lebih akurat (tidak selalu).
- **Teks pendek.** Kosakata kecil dan kata langka membuat smoothing sangat penting.

### Python: Multinomial dan Gaussian Naive Bayes dari nol

Hanya pustaka standar. Korpus simulasi dari Part 2:

```python
import math
from collections import Counter

korpus = [("tolong ada kebakaran di gedung", "E"), ("kecelakaan di jalan tolong kirim ambulans", "E"),
          ("banjir naik tolong evakuasi segera", "E"), ("ada korban kecelakaan butuh ambulans segera", "E"),
          ("kebakaran besar tolong damkar segera", "E"), ("jadwal rapat besok pagi di gedung", "N"),
          ("tolong kirim catatan kuliah besok", "N"), ("makan siang di kantin jalan baru", "N"),
          ("kuliah besok pagi di gedung baru", "N"), ("ada diskon jalan sehat besok", "N")]
cE = Counter(w for t, c in korpus if c == "E" for w in t.split())
cN = Counter(w for t, c in korpus if c == "N" for w in t.split())
V = sorted(set(cE) | set(cN))
NE, NN = sum(cE.values()), sum(cN.values())
print(len(V), NE, NN)                                       # 29 27 28

pesan = "tolong ada banjir di jalan".split()
print({w: (cE[w], cN[w]) for w in pesan})
# {'tolong': (4, 1), 'ada': (2, 1), 'banjir': (1, 0), 'di': (2, 3), 'jalan': (1, 2)}

def log_skor(kata, hitung, total, prior, alpha=1.0):
    return math.log(prior) + sum(math.log((hitung[w] + alpha) / (total + alpha * len(V))) for w in kata)

lE = log_skor(pesan, cE, NE, 0.5)
lN = log_skor(pesan, cN, NN, 0.5)
print(round(lE, 3), round(lN, 3), round(lE - lN, 3), round(1 / (1 + math.exp(lN - lE)), 4))
# -15.627 -17.037 1.41 0.8038

for alpha in (0.1, 1, 5):
    a = log_skor(pesan, cE, NE, 0.5, alpha)
    b = log_skor(pesan, cN, NN, 0.5, alpha)
    print(alpha, round(1 / (1 + math.exp(b - a)), 4))
# 0.1 0.9704
# 1 0.8038
# 5 0.6185

lain = "jadwal kuliah besok di gedung".split()
a, b = log_skor(lain, cE, NE, 0.5), log_skor(lain, cN, NN, 0.5)
print(round(1 / (1 + math.exp(b - a)), 4))                  # 0.0179
print(0.001 ** 120, round(120 * math.log(0.001), 2))        # 0.0 -828.93
```

Gaussian Naive Bayes (Contoh 3):

```python
E = [(12, 3), (8, 2), (10, 4)]
N = [(6, 0), (9, 1), (7, 0)]

def statistik(D):
    hasil = []
    for kolom in zip(*D):
        m = sum(kolom) / len(kolom)
        v = sum((a - m) ** 2 for a in kolom) / len(kolom)
        hasil.append((m, v))
    return hasil

sE, sN = statistik(E), statistik(N)
print([(round(m, 3), round(v, 3)) for m, v in sE], [(round(m, 3), round(v, 3)) for m, v in sN])
# [(10.0, 2.667), (3.0, 0.667)] [(7.333, 1.556), (0.333, 0.222)]

pdf = lambda x, m, v: math.exp(-(x - m) ** 2 / (2 * v)) / math.sqrt(2 * math.pi * v)
x = (9, 2)
kE = [pdf(xi, m, v) for xi, (m, v) in zip(x, sE)]
kN = [pdf(xi, m, v) for xi, (m, v) in zip(x, sN)]
print([round(v, 4) for v in kE], [round(v, 5) for v in kN])    # [0.2025, 0.2308] [0.13098, 0.00163]
skorE, skorN = 0.5 * kE[0] * kE[1], 0.5 * kN[0] * kN[1]
print(round(skorE, 5), round(skorN, 6), round(skorE / (skorE + skorN), 4))   # 0.02337 0.000107 0.9954
```

**Opsional: bandingkan dengan scikit-learn.** Blok ini membutuhkan scikit-learn dan NumPy, yang **tidak** dipasang oleh proyek ini dan tidak dijalankan oleh situs. Keluaran dijalankan dengan scikit-learn 1.9.0 pada Python 3.14.

```python
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB, GaussianNB

cv = CountVectorizer()
X = cv.fit_transform([t for t, _ in korpus])
y = [c for _, c in korpus]
nb = MultinomialNB(alpha=1.0).fit(X, y)
print(nb.classes_, nb.predict_proba(cv.transform(["tolong ada banjir di jalan"])).round(4))
# ['E' 'N'] [[0.8038 0.1962]]
print(nb.predict_proba(cv.transform(["jadwal kuliah besok di gedung"])).round(4))   # [[0.0179 0.9821]]

g = GaussianNB(var_smoothing=0).fit(np.array(E + N), ["E"] * 3 + ["N"] * 3)
print(g.predict_proba([[9, 2]]).round(4))                                          # [[0.9954 0.0046]]
```

Interpretasi: seluruh hasil manual (posterior $0{,}8038$, $0{,}0179$, dan $0{,}9954$) sama dengan keluaran `MultinomialNB` dan `GaussianNB`. `var_smoothing=0` dipakai agar sama dengan hitungan kita; nilai bawaan menambahkan bilangan sangat kecil pada varians.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Tidak memakai smoothing | satu kata baru membuat skor kelas nol | $\alpha > 0$ (Laplace) |
| Mengalikan banyak peluang langsung | *underflow* | jumlahkan logaritma |
| Menyamakan kepadatan Gaussian dengan peluang | kepadatan boleh lebih dari 1 | bandingkan antar kelas lalu normalkan |
| Mengira fitur harus benar-benar independen agar berfungsi | hanya diperlukan bahwa peringkat kelas tetap baik | sadari bahwa peluang bisa terlalu ekstrem |
| Memercayai peluang Naive Bayes sebagai peluang terkalibrasi | bukti berkorelasi dihitung berulang | kalibrasi atau pakai untuk keputusan |
| Memakai Multinomial pada fitur kontinu | model hitungan | Gaussian atau diskretisasi |
| Menghitung kosakata atau frekuensi dari data uji | kebocoran | dari data latih saja |
| Mengabaikan prior saat proporsi kelas berubah | prior mengikuti data latih | sesuaikan prior atau ambang |
| Menganggap Naive Bayes selalu paling cepat dan paling baik | bergantung data | bandingkan dengan baseline lain |

## Latihan Mandiri

1. Prior $P(E) = 0{,}4$, $P(\text{kata} \mid E) = 0{,}5$, $P(\text{kata} \mid N) = 0{,}1$. Hitung posterior E bila pesan memuat kata itu.
2. Prior $P(E) = 0{,}3$; $P(A \mid E) = 0{,}8$, $P(A \mid N) = 0{,}2$, $P(B \mid E) = 0{,}6$, $P(B \mid N) = 0{,}3$. Hitung posterior E bila A dan B hadir, dengan asumsi Naive Bayes.
3. Pada kelas dengan total kata $N_C = 20$ dan kosakata $\lvert V \rvert = 10$, sebuah kata muncul $3$ kali. Hitung $P(w \mid C)$ dengan $\alpha = 1$ dan $\alpha = 0$. Hitung juga untuk kata yang tidak pernah muncul.
4. Dari log-skor $\ell_E = -15{,}627$ dan $\ell_N = -17{,}037$, hitung posterior E.
5. Dari Contoh 2, hitung rasio likelihood untuk kata "tolong" ($P(\text{tolong} \mid E)/P(\text{tolong} \mid N)$) dan "di". Kata mana yang lebih menunjuk E dan mengapa?
6. Gaussian NB: kelas E memiliki $\mu = 10$, $\sigma^2 = 2{,}667$. Hitung $P(x = 9 \mid E)$.
7. Satu fitur dengan rasio likelihood $4$ diduplikasi. Hitung posterior (prior $0{,}5$) sebelum dan sesudah duplikasi, dan jelaskan arti hasilnya.
8. Hitung $0{,}001^{120}$ secara langsung dan lewat logaritma. Mengapa Naive Bayes bekerja dengan log-skor?
9. Pada Contoh 2, posterior E adalah $0{,}9704$ untuk $\alpha = 0{,}1$ dan $0{,}6185$ untuk $\alpha = 5$. Jelaskan pengaruh $\alpha$ pada keyakinan model dan bagaimana memilih $\alpha$.
10. Jelaskan mengapa Multinomial Naive Bayes dua kelas adalah klasifikator linear, tulis bobot tiap kata, dan bandingkan secara singkat dengan regresi logistik.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    $P(N) = 0{,}6$. Skor E $= 0{,}4 \times 0{,}5 = 0{,}2$. Skor N $= 0{,}6 \times 0{,}1 = 0{,}06$. Posterior E $= \dfrac{0{,}2}{0{,}26} \approx 0{,}7692$.

??? success "Soal 2"
    Skor E $= 0{,}3 \times 0{,}8 \times 0{,}6 = 0{,}144$. Skor N $= 0{,}7 \times 0{,}2 \times 0{,}3 = 0{,}042$. Posterior E $= \dfrac{0{,}144}{0{,}186} \approx 0{,}7742$.

??? success "Soal 3"
    $\alpha = 1$: $\dfrac{3 + 1}{20 + 10} = \dfrac{4}{30} = 0{,}1333$. $\alpha = 0$: $\dfrac{3}{20} = 0{,}15$. Kata tak pernah muncul: $\alpha = 1$: $\dfrac{0 + 1}{30} = 0{,}0333$; $\alpha = 0$: $0$ (membuat seluruh skor kelas nol).

??? success "Soal 4"
    $\ell_E - \ell_N = 1{,}410$. Posterior E $= \dfrac{1}{1 + e^{-1{,}410}} = \dfrac{1}{1 + 0{,}2441} \approx 0{,}8038$.

??? success "Soal 5"
    "tolong": $\dfrac{5/56}{2/57} = \dfrac{0{,}08929}{0{,}03509} = 2{,}545$. "di": $\dfrac{3/56}{4/57} = \dfrac{0{,}05357}{0{,}07018} = 0{,}763$. "tolong" menunjuk E (rasio di atas $1$) karena muncul 4 dari 27 kata E tetapi hanya 1 dari 28 kata N. "di" sedikit menunjuk N (rasio di bawah $1$) karena kata umum yang sedikit lebih sering di N.

??? success "Soal 6"
    $\dfrac{1}{\sqrt{2\pi \times 2{,}667}} = \dfrac{1}{4{,}0933} = 0{,}2443$. $\exp\!\left(-\dfrac{(9 - 10)^2}{2 \times 2{,}667}\right) = e^{-0{,}1875} = 0{,}8290$. Kepadatan $= 0{,}2443 \times 0{,}8290 \approx 0{,}2025$.

??? success "Soal 7"
    Sebelum: odds $= 4$, peluang $= \dfrac{4}{5} = 0{,}8$. Sesudah duplikasi: odds $= 4 \times 4 = 16$, peluang $= \dfrac{16}{17} = 0{,}9412$. Tidak ada bukti baru; model hanya menghitung bukti yang sama dua kali (melanggar independensi), sehingga terlalu yakin.

??? success "Soal 8"
    Langsung: $0{,}001^{120} = 10^{-360}$, yang di bawah rentang bilangan desimal 64-bit sehingga menjadi $0{,}0$ (*underflow*). Lewat logaritma: $120 \times \ln 0{,}001 = -828{,}93$. Naive Bayes memakai log-skor karena produk banyak peluang kecil cepat hilang, sedangkan jumlah log stabil dan tidak mengubah urutan kelas.

??? success "Soal 9"
    $\alpha$ kecil mempertahankan frekuensi mentah sehingga kata langka seperti "banjir" sangat berpengaruh (E $= 0{,}9704$). $\alpha$ besar menarik peluang tiap kata mendekati seragam sehingga bukti menjadi lemah dan model kurang yakin (E $= 0{,}6185$). $\alpha$ dipilih lewat validasi (misalnya mencoba beberapa nilai dan memilih yang paling baik pada data validasi, Part 11).

??? success "Soal 10"
    Log-odds $= \ln\dfrac{P(1)}{P(0)} + \sum_w x_w\ln\dfrac{P(w \mid 1)}{P(w \mid 0)}$: jumlah berbobot dari hitungan kata, yaitu fungsi linear dengan bobot $\ln\dfrac{P(w \mid 1)}{P(w \mid 0)}$ dan bias $\ln\dfrac{P(1)}{P(0)}$, bentuk yang sama dengan regresi logistik (Part 4). Bedanya: bobot NB dihitung dari frekuensi (generatif, dengan asumsi independensi), sedangkan regresi logistik mengoptimasi bobot dengan *log loss* (diskriminatif) dan tidak mengasumsikan independensi, sehingga bobotnya dapat mengoreksi fitur yang berkorelasi.

## Ringkasan Materi

- Naive Bayes: $\hat{C} = \arg\max_C P(C)\prod_j P(x_j \mid C)$, dengan asumsi fitur independen bersyarat pada kelas; dihitung dalam log-skor agar tidak *underflow*.
- Prior dari proporsi kelas; likelihood dari frekuensi (Multinomial, Bernoulli) atau Normal (Gaussian); "pelatihan" hanya menghitung frekuensi atau rata-rata dan varians.
- Multinomial NB: $P(w \mid C) = \frac{n_{wC} + \alpha}{N_C + \alpha\lvert V \rvert}$; smoothing mencegah skor nol dan mengatur keyakinan.
- Gaussian NB memakai kepadatan Normal per kelas dan fitur; Bernoulli NB juga memakai ketiadaan kata.
- Asumsi naif membuat bukti yang berkorelasi dihitung berulang: peluang terlalu ekstrem, tetapi keputusan sering tetap baik.
- NB dua kelas adalah klasifikator linear (log-odds linear terhadap hitungan kata), model generatif, sedangkan regresi logistik diskriminatif.
- Pada NLP, ia baseline klasik; angka contoh adalah simulasi.

## Checklist Penguasaan

- [ ] Menurunkan aturan Naive Bayes dari Bayes dan independensi bersyarat.
- [ ] Menghitung Multinomial NB dengan Laplace smoothing langkah demi langkah.
- [ ] Menghitung Gaussian NB (rata-rata, varians, kepadatan, posterior).
- [ ] Menjelaskan *underflow* dan pemakaian log-skor.
- [ ] Menjelaskan akibat asumsi independensi pada peluang yang dihasilkan.
- [ ] Menjelaskan hubungan Naive Bayes dengan regresi logistik.

## Prasyarat untuk Materi Berikutnya

[Part 7 — Matematika Decision Tree](07-decision-tree.md) memakai pendekatan yang lain lagi: membagi data dengan serangkaian pertanyaan ya-atau-tidak yang dipilih lewat ukuran ketidakmurnian (*entropy* dan Gini). Pastikan logaritma basis 2 dan konsep peluang kelas sudah jelas.
