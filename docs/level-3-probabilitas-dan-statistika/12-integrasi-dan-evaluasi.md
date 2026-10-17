# Part 12 — Integrasi dan Evaluasi Level 3

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- merangkum konsep dan rumus utama statistika dan probabilitas dari Level 3;
- menghubungkan statistik deskriptif, probabilitas, distribusi, inferensi, dan korelasi dalam satu studi kasus evaluasi model;
- menghitung presisi dengan Teorema Bayes dan menjelaskan pengaruh tingkat dasar;
- menghitung interval kepercayaan dan melakukan uji hipotesis untuk akurasi model;
- menyelesaikan latihan bertingkat yang menggabungkan beberapa part;
- menjawab pertanyaan konseptual dengan alasan, bukan hanya rumus;
- menilai kesiapan untuk Level 4 dan mengetahui bagian mana yang perlu diulang.

## Prasyarat

[Part 1](01-pengantar-statistika.md) sampai [Part 11](11-korelasi-kovarians-dan-standardisasi.md). Pada Part 10, estimasi kemungkinan maksimum cukup dipahami secara pengantar.

## Mengapa Materi Ini Penting?

Konsep yang dipelajari terpisah baru benar-benar berguna saat disatukan. Mengevaluasi satu model Machine Learning dengan jujur memakai hampir semuanya sekaligus: data uji adalah sampel (Part 1), ringkasannya statistik deskriptif (Part 2–3), kinerja dinyatakan dengan peluang bersyarat dan Teorema Bayes (Part 4–6), jumlah prediksi benar berdistribusi Binomial (Part 7–8), akurasi punya galat baku (Part 9), kesimpulan dinyatakan dengan interval dan uji hipotesis (Part 10), dan fitur disiapkan dengan korelasi dan standardisasi (Part 11).

## Konsep Dasar dengan Bahasa Sederhana

### 1. Peta Level 3

**Statistika (data yang ada):**

| Part | Konsep inti | Rumus atau gagasan kunci |
|---|---|---|
| 1 | populasi, sampel, parameter, statistik, jenis dan skala data, sampling, bias | $\mu$ vs $\bar{x}$; sampel besar tidak memperbaiki bias |
| 2 | mean, median, modus, kuartil, IQR, varians, simpangan baku | $s^2 = \frac{1}{n-1}\sum(x_i - \bar{x})^2$; $\mathrm{Var}(aX + b)$ |
| 3 | histogram, boxplot, bentuk distribusi, pencilan, kemiringan | pagar $Q_1 - 1{,}5\,\text{IQR}$ dan $Q_3 + 1{,}5\,\text{IQR}$ |

**Probabilitas:**

| Part | Konsep inti | Rumus atau gagasan kunci |
|---|---|---|
| 4 | ruang sampel, kejadian, aturan peluang, pencacahan | $P(A^c) = 1 - P(A)$; $P(A \cup B) = P(A) + P(B) - P(A \cap B)$ |
| 5 | peluang bersyarat, independensi, peluang total | $P(A \mid B) = P(A \cap B)/P(B)$; $P(A \cap B) = P(A)P(B)$ bila independen |
| 6 | Teorema Bayes, prior dan posterior, Naive Bayes | $P(A \mid B) = \dfrac{P(B \mid A)P(A)}{P(B)}$ |

**Distribusi dan inferensi:**

| Part | Konsep inti | Rumus atau gagasan kunci |
|---|---|---|
| 7 | variabel acak, PMF, PDF, CDF, nilai harapan, varians | $E[X] = \sum x\,p(x)$; $\mathrm{Var}(X) = E[X^2] - \mu^2$ |
| 8 | Bernoulli, Binomial, Uniform, Normal, Poisson, $z$-score | $z = (x - \mu)/\sigma$; Binomial: $E = np$, $\mathrm{Var} = np(1 - p)$ |
| 9 | distribusi sampling, galat baku, CLT | $\mathrm{SE}(\bar{X}) = \sigma/\sqrt{n}$; $\mathrm{SE}(\hat{p}) = \sqrt{p(1-p)/n}$ |
| 10 | estimasi, interval kepercayaan, uji hipotesis, nilai-p, Tipe I/II | estimasi $\pm$ nilai kritis $\times$ galat baku |

**Hubungan antar variabel:**

| Part | Konsep inti | Rumus atau gagasan kunci |
|---|---|---|
| 11 | kovarians, korelasi, standardisasi, Min-Max | $r = s_{xy}/(s_x s_y)$; $z = (x - \bar{x})/s$ |

### 2. Studi Kasus Terpadu: Mengevaluasi Filter Spam

Sebuah filter spam diuji pada **200 e-mail** yang tidak dipakai saat pelatihan. Dari 200 e-mail itu, 60 benar-benar spam (30%) dan 140 bukan. Hasilnya:

| | Diprediksi spam | Diprediksi bukan spam | Jumlah |
|---|---|---|---|
| **Sebenarnya spam** | $TP = 51$ | $FN = 9$ | 60 |
| **Sebenarnya bukan spam** | $FP = 21$ | $TN = 119$ | 140 |
| **Jumlah** | 72 | 128 | 200 |

Tabel ini adalah **tabel kontingensi** (Part 5) yang di dunia ML disebut **matriks konfusi** (*confusion matrix*). Mari kita periksa dengan alat dari seluruh level.

**Langkah 1: kerangka sampel (Part 1).** Populasi yang ingin kita ketahui adalah **semua e-mail masa depan** yang akan diterima pengguna. 200 e-mail uji adalah sampel dari populasi itu; akurasi sebenarnya $p$ adalah parameter yang tidak diketahui, dan akurasi uji $\hat{p}$ adalah statistik. Pertanyaan pertama: apakah sampel ini mewakili? Bila 200 e-mail itu diambil dari satu pengguna saja atau dari bulan tertentu, kesimpulannya bisa bias, dan menambah e-mail dari sumber yang sama tidak memperbaikinya.

**Langkah 2: ukuran-ukuran kinerja (Part 4–5).** Dari tabel:

- Akurasi $= \dfrac{TP + TN}{n} = \dfrac{51 + 119}{200} = 0{,}85$.
- Recall (sensitivitas) $= P(\text{prediksi spam} \mid \text{spam}) = \dfrac{51}{60} = 0{,}85$.
- Tingkat positif palsu $= P(\text{prediksi spam} \mid \text{bukan spam}) = \dfrac{21}{140} = 0{,}15$.
- Presisi $= P(\text{spam} \mid \text{prediksi spam}) = \dfrac{51}{72} \approx 0{,}7083$.

**Langkah 3: Teorema Bayes dan tingkat dasar (Part 6).** Presisi adalah posterior. Dengan prior (tingkat spam) $0{,}30$, recall $0{,}85$, dan positif palsu $0{,}15$:

$$
\text{presisi} = \frac{0{,}85 \times 0{,}30}{0{,}85 \times 0{,}30 + 0{,}15 \times 0{,}70} = \frac{0{,}255}{0{,}255 + 0{,}105} \approx 0{,}7083
$$

Sama dengan hitungan langsung dari tabel. Sekarang bayangkan filter ini dipakai pada kotak masuk yang hanya 5% spam. Recall dan positif palsu (sifat filter) tidak berubah, tetapi presisinya berubah:

$$
\text{presisi} = \frac{0{,}85 \times 0{,}05}{0{,}85 \times 0{,}05 + 0{,}15 \times 0{,}95} = \frac{0{,}0425}{0{,}0425 + 0{,}1425} \approx 0{,}2297
$$

Hanya sekitar 23% e-mail yang ditandai spam benar-benar spam. **Presisi bergantung pada tingkat dasar**, jadi angka dari data uji hanya berlaku bila proporsi spam di dunia nyata mirip dengan data uji.

**Langkah 4: model peluang (Part 7–8).** Bila setiap e-mail diklasifikasikan benar dengan peluang $p$ dan e-mail independen, jumlah prediksi benar $X \sim \mathrm{Binomial}(200, p)$. Dengan $p = 0{,}85$: $E[X] = 170$ dan $\sigma = \sqrt{200 \times 0{,}85 \times 0{,}15} \approx 5{,}05$. Jadi 170 benar dari 200 adalah hasil yang sangat wajar bila $p = 0{,}85$.

**Langkah 5: ketidakpastian (Part 9).** Akurasi uji adalah proporsi sampel. Galat bakunya:

$$
\mathrm{SE} = \sqrt{\frac{0{,}85 \times 0{,}15}{200}} \approx 0{,}0252
$$

Bila akurasi sebenarnya $0{,}85$, akurasi uji dari 200 e-mail biasanya berkisar $0{,}85 \pm 2(0{,}0252)$, yaitu sekitar $0{,}80$ sampai $0{,}90$.

**Langkah 6: inferensi (Part 10).** Interval kepercayaan 95% untuk akurasi sebenarnya:

$$
0{,}85 \pm 1{,}96(0{,}0252) = [0{,}8005;\ 0{,}8995]
$$

Tafsirnya: prosedur yang menghasilkan interval ini memuat akurasi sebenarnya pada sekitar 95% pengulangan. Syarat pendekatan Normal terpenuhi ($200 \times 0{,}85 = 170$ dan $200 \times 0{,}15 = 30$, keduanya $\geq 10$).

Apakah filter ini lebih baik daripada **baseline** "tebak selalu bukan spam"? Baseline itu akurasinya sama dengan proporsi bukan spam, yaitu $0{,}70$. Uji satu sisi: $H_0: p = 0{,}70$ lawan $H_1: p > 0{,}70$.

$$
z = \frac{0{,}85 - 0{,}70}{\sqrt{0{,}70 \times 0{,}30/200}} = \frac{0{,}15}{0{,}0324} \approx 4{,}63
$$

Nilai-p $\approx 1{,}8 \times 10^{-6}$ dengan pendekatan Normal, dan $6{,}5 \times 10^{-7}$ dengan perhitungan Binomial eksak. Keduanya jauh di bawah $0{,}05$: kita menolak $H_0$. Selisih 15 poin ($0{,}85$ vs $0{,}70$) juga besar secara praktis, bukan hanya signifikan secara statistik. Namun perhatikan batasnya: kesimpulan ini hanya tentang akurasi, yang pada data tidak seimbang bukan satu-satunya ukuran; presisi 70,8% dan peluang positif palsu 15% mungkin tidak dapat diterima untuk e-mail penting.

**Langkah 7: fitur dan korelasi (Part 11).** Misalkan pada 10 e-mail contoh kita catat jumlah tautan $x$ dan label ($1 =$ spam):

| | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| Tautan | 0 | 1 | 0 | 2 | 5 | 6 | 1 | 7 | 0 | 4 |
| Spam | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 1 |

Rata-rata tautan pada spam $5{,}5$ dan pada bukan spam $0{,}67$; korelasi Pearson antara tautan dan label (0/1) $r \approx 0{,}933$. Fitur ini tampak informatif, tetapi: sampelnya hanya 10 e-mail (korelasi sangat tidak pasti); korelasi tidak berarti tautan **menyebabkan** spam (itu cara penulis spam bekerja); dan bila fitur ini dipakai pada model berbasis jarak, ia perlu distandardisasi dengan rata-rata dan simpangan baku dari **data latih saja**.

**Langkah 8: daftar periksa kejujuran.** Sebelum melaporkan hasil: (1) apakah sampel uji mewakili? (2) apakah contoh uji independen (tidak ada duplikat atau e-mail dari rantai yang sama)? (3) apakah data uji dipakai berkali-kali untuk memilih model (risiko banyak pengujian, Part 10)? (4) apakah tingkat dasar di dunia nyata mirip dengan data uji (Part 6)? (5) apakah ketidakpastian (interval) dilaporkan?

### 3. Kebiasaan yang Perlu Dibawa ke Level Berikutnya

- **Mulai dari pertanyaan dan populasi**: apa yang ingin diketahui, dan tentang siapa atau apa?
- **Lihat datanya** sebelum percaya pada angka ringkasan.
- **Pisahkan statistik dari parameter**, dan laporkan ketidakpastian.
- **Periksa asumsi**: independensi, sampel mewakili, bentuk distribusi, tingkat dasar.
- **Tafsirkan dengan tepat**: nilai-p dan interval kepercayaan punya arti yang spesifik; korelasi bukan sebab-akibat.
- **Hindari kebocoran data** dan pengujian berulang pada data uji yang sama.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Dari part |
|---|---|---|
| Matriks konfusi | tabel kontingensi prediksi vs kenyataan | 5, 6 |
| Recall / sensitivitas | $P(\text{prediksi positif} \mid \text{positif})$ | 6 |
| Presisi | $P(\text{positif} \mid \text{prediksi positif})$, sebuah posterior | 6 |
| Tingkat positif palsu | $P(\text{prediksi positif} \mid \text{negatif})$ | 6, 10 |
| Baseline | model sederhana pembanding (misalnya kelas mayoritas) | 2, 6 |
| Galat baku | simpangan baku dari distribusi sampling | 9 |
| Interval kepercayaan | rentang wajar dari prosedur estimasi | 10 |
| Nilai-p | peluang hasil seekstrem itu bila $H_0$ benar | 10 |
| Kebocoran data | informasi data uji masuk ke pelatihan | 5, 11 |

## Rumus dan Penjelasan Setiap Simbol

Rumus utama sudah dijelaskan per part (lihat tabel di bagian 1). Rumus yang dipakai bersama dalam studi kasus:

| Langkah | Rumus | Arti simbol |
|---|---|---|
| Akurasi | $\dfrac{TP + TN}{n}$ | $n = TP + FN + FP + TN$ |
| Recall | $\dfrac{TP}{TP + FN}$ | peluang prediksi positif bagi yang positif |
| Positif palsu | $\dfrac{FP}{FP + TN}$ | peluang prediksi positif bagi yang negatif |
| Presisi (Bayes) | $\dfrac{r\,\pi}{r\,\pi + f\,(1 - \pi)}$ | $r$ recall, $f$ tingkat positif palsu, $\pi$ prevalensi (tingkat dasar) |
| Galat baku akurasi | $\sqrt{\hat{p}(1 - \hat{p})/n}$ | $\hat{p}$ akurasi uji |
| CI 95% | $\hat{p} \pm 1{,}96\,\mathrm{SE}$ | syarat: $n\hat{p}, n(1 - \hat{p}) \gtrsim 10$ |
| Uji vs baseline | $z = \dfrac{\hat{p} - p_0}{\sqrt{p_0(1 - p_0)/n}}$ | $p_0$ akurasi baseline |

## Penjelasan Langkah demi Langkah

**Menyelesaikan soal terpadu (strategi umum):**

1. **Baca soal dan petakan** ke part: apakah ini ringkasan data, peluang, distribusi, inferensi, atau hubungan variabel?
2. **Tulis yang diketahui** (data, parameter, ukuran sampel) dan yang ditanyakan.
3. **Pilih alat** dan periksa syarat (independen, hasil sama mungkin, $n$ cukup besar, $P(B) > 0$).
4. **Hitung langkah demi langkah** dengan satu operasi per baris.
5. **Periksa dengan cara lain**: tabel dan rumus, frekuensi alami, atau simulasi.
6. **Tafsirkan** dalam kalimat, sebut asumsi, dan jangan melampaui apa yang bisa disimpulkan data.

## Contoh Soal dan Pembahasan

### Contoh 1 — Presisi dan tingkat dasar

Sebuah model deteksi penipuan memiliki recall $0{,}90$ dan tingkat positif palsu $0{,}02$. Hitung presisi bila penipuan 5% dari transaksi dan bila 0,5%.

1. Prevalensi 5%: $\dfrac{0{,}90 \times 0{,}05}{0{,}90 \times 0{,}05 + 0{,}02 \times 0{,}95} = \dfrac{0{,}045}{0{,}045 + 0{,}019} = \dfrac{0{,}045}{0{,}064} \approx 0{,}7031$.
2. Prevalensi 0,5%: $\dfrac{0{,}90 \times 0{,}005}{0{,}90 \times 0{,}005 + 0{,}02 \times 0{,}995} = \dfrac{0{,}0045}{0{,}0045 + 0{,}0199} \approx 0{,}1844$.
3. Model yang sama menghasilkan presisi $70{,}3\%$ pada prevalensi tinggi dan hanya $18{,}4\%$ pada prevalensi rendah. Tingkat dasar sangat menentukan.

### Contoh 2 — Interval kepercayaan dan uji untuk proporsi

Dari 150 pengguna, 45 mengklik tawaran. Hitung CI 95% untuk proporsi klik, lalu uji $H_0: p = 0{,}25$ lawan $H_1: p \neq 0{,}25$.

1. $\hat{p} = \dfrac{45}{150} = 0{,}3$; syarat: $n\hat{p} = 45 \geq 10$, $n(1 - \hat{p}) = 105 \geq 10$.
2. $\mathrm{SE} = \sqrt{\dfrac{0{,}3 \times 0{,}7}{150}} = 0{,}0374$. CI $= 0{,}3 \pm 1{,}96(0{,}0374) = [0{,}227;\ 0{,}373]$.
3. Uji: $z = \dfrac{0{,}3 - 0{,}25}{\sqrt{0{,}25 \times 0{,}75/150}} = \dfrac{0{,}05}{0{,}0354} = 1{,}414$. Nilai-p dua sisi $\approx 0{,}157$.
4. Gagal menolak $H_0$. Konsisten dengan CI: $0{,}25$ berada di dalam $[0{,}227;\ 0{,}373]$. Data belum cukup membuktikan bahwa laju klik berbeda dari $25\%$.

### Contoh 3 — Distribusi sampling dari dadu

Satu dadu adil punya $\mu = 3{,}5$ dan $\sigma \approx 1{,}708$. Untuk 30 lemparan, berapa peluang rata-rata lebih dari $3{,}8$?

1. $\mathrm{SE} = \dfrac{1{,}708}{\sqrt{30}} \approx 0{,}3118$.
2. $z = \dfrac{3{,}8 - 3{,}5}{0{,}3118} \approx 0{,}962$ (pendekatan Normal lewat CLT, $n = 30$).
3. $P(\bar{X} > 3{,}8) = 1 - \Phi(0{,}962) \approx 0{,}168$.

### Contoh 4 — Menafsirkan korelasi

Pada 10 e-mail contoh, $r \approx 0{,}933$ antara jumlah tautan dan label spam. Kesimpulan mana yang sah?

1. Sah: pada sampel ini, e-mail dengan lebih banyak tautan cenderung spam (rata-rata $5{,}5$ vs $0{,}67$).
2. Belum sah: bahwa hubungan itu kuat pada populasi (butuh interval atau uji, dan $n = 10$ sangat kecil).
3. Tidak sah: bahwa menambah tautan **menyebabkan** e-mail menjadi spam.
4. Perhatian: korelasi hanya menangkap hubungan linear dan peka terhadap pencilan; diagram pencar perlu dilihat.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** Dari tabel konfusi: akurasi $= \dfrac{51 + 119}{200} = 0{,}85$, recall $= 0{,}85$.

**Sedang.** Standardisasi nilai $x = 65$ dengan rata-rata latih $50$ dan simpangan baku $10$: $z = 1{,}5$. Peluang nilai lebih rendah bila Normal: $\Phi(1{,}5) \approx 0{,}9332$.

**Sedang.** Binomial $n = 200$, $p = 0{,}85$: $E[X] = 170$, $\sigma \approx 5{,}05$. Hasil di rentang $170 \pm 10$ (sekitar dua simpangan baku) mencakup kira-kira 95% kemungkinan.

**Menantang.** Sepuluh konfigurasi hiperparameter diuji pada data uji yang sama dengan $\alpha = 0{,}05$ masing-masing. Bila **tidak ada** yang benar-benar lebih baik, peluang minimal satu tampak "signifikan" adalah $1 - 0{,}95^{10} \approx 0{,}401$, sekitar 40%.

## Visualisasi atau Interpretasi Grafik

Visualisasi kunci Level 3, dan di part mana masing-masing dibahas:

- **Histogram dan boxplot** (Part 3): bentuk distribusi, kemiringan, dan pencilan.
- **Diagram pohon dan tabel kontingensi** (Part 5–6): peluang bersyarat dan Bayes.
- **PMF, PDF, dan CDF** (Part 7–8): distribusi peluang dan Normal.
- **Histogram distribusi sampling** (Part 9): CLT.
- **Interval kepercayaan** (Part 10): rentang wajar dengan titik estimasi.
- **Diagram pencar** (Part 11): hubungan dan pencilan.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| Melaporkan akurasi tanpa konteks tingkat dasar | akurasi menyesatkan pada data tidak seimbang | laporkan presisi, recall, dan baseline |
| Mengira presisi sifat tetap model | presisi bergantung prevalensi | hitung ulang dengan Bayes |
| Menyebut hasil "pasti" dari satu sampel uji | ada variasi sampling | laporkan galat baku atau interval |
| Menafsirkan nilai-p sebagai peluang hipotesis benar | nilai-p dihitung bila $H_0$ benar | tafsir yang tepat (Part 10) |
| Menyimpulkan sebab-akibat dari korelasi | faktor pengganggu | korelasi bukan sebab-akibat |
| Menganggap data harus Normal | banyak data miring | periksa distribusi |
| Menghitung statistik penskalaan dari seluruh data | kebocoran data | dari data latih saja |
| Menguji banyak model pada data uji yang sama lalu melaporkan yang terbaik | positif palsu menumpuk | pisahkan data validasi dan uji |
| Mengabaikan independensi contoh | galat baku terlalu kecil | periksa duplikat dan ketergantungan |

## Hubungan dengan Machine Learning

- **Evaluasi model** adalah penerapan langsung seluruh level: sampel uji, ringkasan, peluang bersyarat, distribusi Binomial, galat baku, interval kepercayaan, dan uji hipotesis.
- **Pemilihan metrik.** Pada data tidak seimbang, akurasi saja menyesatkan; presisi dan recall (peluang bersyarat) lebih informatif, dan keduanya berhubungan lewat Teorema Bayes.
- **Naive Bayes** adalah penerapan langsung Part 5–6; **regresi logistik** (Level 4) memakai distribusi Bernoulli dan kemungkinan maksimum; **regresi linear** memakai kovarians, korelasi, dan asumsi tentang galat.
- **Praproses fitur** memakai statistik deskriptif, standardisasi, dan pemeriksaan pencilan dan distribusi.
- **Ketidakpastian** harus selalu menyertai angka kinerja, terutama saat membandingkan model.
- **Level 4** menerapkan Level 2 (aljabar linear dan kalkulus) dan Level 3 (peluang dan statistika) pada algoritma klasik.

### Python: studi kasus terpadu

Hanya pustaka standar. Seed tetap, Python 3.14.

```python
import math, random
from statistics import mean, pstdev, NormalDist
Z = NormalDist()

TP, FN, FP, TN = 51, 9, 21, 119
n = TP + FN + FP + TN
akurasi = (TP + TN) / n
recall = TP / (TP + FN)
fpr = FP / (FP + TN)
presisi = TP / (TP + FP)
print(n, akurasi, recall, fpr, round(presisi, 4))      # 200 0.85 0.85 0.15 0.7083

def presisi_bayes(prevalensi, recall, fpr):
    return recall * prevalensi / (recall * prevalensi + fpr * (1 - prevalensi))

print(round(presisi_bayes(0.30, recall, fpr), 4), round(presisi_bayes(0.05, recall, fpr), 4))
# 0.7083 0.2297
```

Interval kepercayaan, uji terhadap baseline, dan simulasi:

```python
se = math.sqrt(akurasi * (1 - akurasi) / n)
print(round(se, 4), round(akurasi - 1.96 * se, 4), round(akurasi + 1.96 * se, 4))
# 0.0252 0.8005 0.8995

z = (akurasi - 0.70) / math.sqrt(0.70 * 0.30 / n)
print(round(z, 3), f"{1 - Z.cdf(z):.2e}")                      # 4.629 1.84e-06
p_eksak = sum(math.comb(n, k) * 0.7 ** k * 0.3 ** (n - k) for k in range(170, n + 1))
print(f"{p_eksak:.2e}")                                         # 6.52e-07

rng = random.Random(3)
akurasi_uji = [sum(rng.random() < 0.85 for _ in range(200)) / 200 for _ in range(10000)]
print(round(mean(akurasi_uji), 4), round(pstdev(akurasi_uji), 4), round(se, 4))
# 0.8498 0.0253 0.0252
```

Korelasi dan penskalaan pada contoh tautan:

```python
tautan = [0, 1, 0, 2, 5, 6, 1, 7, 0, 4]
spam = [0, 0, 0, 0, 1, 1, 0, 1, 0, 1]

def pearson(a, b):
    ma, mb = mean(a), mean(b)
    return sum((p - ma) * (q - mb) for p, q in zip(a, b)) / math.sqrt(
        sum((p - ma) ** 2 for p in a) * sum((q - mb) ** 2 for q in b))

print(mean(tautan), round(pearson(tautan, spam), 4))           # 2.6 0.9331
print(mean(t for t, s in zip(tautan, spam) if s == 1),
      round(mean(t for t, s in zip(tautan, spam) if s == 0), 4))   # 5.5 0.6667
print(round(1 - 0.95 ** 10, 4))                                 # 0.4013
```

Interpretasi: presisi turun dari $0{,}708$ ke $0{,}230$ bila prevalensi turun dari 30% ke 5% tanpa perubahan pada filter; akurasi uji $0{,}85$ jauh melampaui baseline $0{,}70$ ($p < 10^{-5}$); dan simulasi menunjukkan galat baku $\approx 0{,}025$ yang cocok dengan rumus. Kode hanya memeriksa hitungan, bukan pengganti penalaran tentang asumsi.

## Latihan Campuran Bertingkat

### Tingkat 1 — Dasar

1. Tentukan jenis dan skala: (a) suhu dalam °C, (b) peringkat lomba, (c) golongan darah, (d) tinggi badan.
2. Untuk data $3, 5, 7, 7, 8, 10$ hitung rata-rata, median, modus, dan simpangan baku sampel.
3. $P(A) = 0{,}5$, $P(B) = 0{,}3$, $P(A \cap B) = 0{,}1$. Hitung $P(A \cup B)$ dan peluang tidak satu pun terjadi.
4. Dua dadu dilempar. Berapa peluang jumlahnya 9?
5. Dari 100 pelanggan, 40 perempuan dan 60 laki-laki. Dari perempuan, 10 berhenti berlangganan; dari laki-laki, 15 berhenti. Hitung $P(\text{berhenti} \mid \text{perempuan})$ dan $P(\text{berhenti} \mid \text{laki-laki})$, lalu tentukan apakah berhenti dan jenis kelamin independen.
6. $X \sim \mathrm{Binomial}(4, 0{,}5)$. Hitung $P(X = 2)$.
7. $X \sim \mathcal{N}(60, 10^2)$. Hitung $P(X < 75)$.
8. Simpangan baku populasi $12$, sampel $n = 36$. Hitung galat baku rata-rata sampel.

### Tingkat 2 — Menengah

1. Untuk data $4, 6, 7, 8, 9, 10, 12, 40$ hitung ringkasan lima angka, pagar boxplot, pencilan, dan bandingkan rata-rata dengan median.
2. Prevalensi 3%, sensitivitas 92%, tingkat positif palsu 8%. Hitung peluang sakit bila tes positif.
3. $X \sim \mathrm{Poisson}(2{,}5)$. Hitung $P(X = 3)$ dan $P(X \geq 1)$.
4. Dari 150 pengguna, 45 mengklik. Hitung CI 95% untuk proporsi klik dan periksa syaratnya.
5. $H_0: \mu = 20$, $\sigma = 5$ diketahui, $n = 25$, $\bar{x} = 22$. Hitung $z$, nilai-p dua sisi, dan keputusan pada $\alpha = 0{,}05$.
6. Data $x = 1, 2, 3, 4$ dan $y = 2, 4, 5, 9$. Hitung kovarians sampel dan korelasi Pearson.
7. Statistik data latih: rata-rata $50$, simpangan baku $10$, minimum $20$, maksimum $80$. Hitung nilai $z$ dan Min-Max untuk data uji $65$ dan $90$.
8. Seorang analis berkata "nilai-p $= 0{,}03$ berarti peluang $H_0$ benar hanya 3%". Koreksi pernyataan itu.

### Tingkat 3 — Terpadu

1. Sebuah model benar pada 170 dari 200 contoh uji. Hitung CI 95% akurasi, lalu uji terhadap baseline 0,70 (satu sisi).
2. Model yang sama punya recall $0{,}85$ dan tingkat positif palsu $0{,}15$. Hitung presisi bila prevalensi 30% dan bila 5%, dan jelaskan perbedaannya.
3. Rata-rata 30 lemparan dadu: hitung peluang rata-rata lebih dari $3{,}8$.
4. Berapa contoh uji yang dibutuhkan agar margin kesalahan CI 95% untuk proporsi paling besar $0{,}03$ (anggap $p \approx 0{,}5$)?
5. Sepuluh konfigurasi diuji pada data uji yang sama dengan $\alpha = 0{,}05$. Bila tidak ada yang benar-benar lebih baik, berapa peluang minimal satu tampak signifikan? Apa saran perbaikan prosedurnya?
6. Pada 10 e-mail contoh, korelasi antara jumlah tautan dan label spam adalah $0{,}933$. Tulis tiga hal yang bisa dan tiga hal yang tidak bisa disimpulkan, dan jelaskan cara menyiapkan fitur ini bila dipakai pada model berbasis jarak.

## Kunci Jawaban dan Pembahasan

??? success "Tingkat 1"
    1. (a) numerik kontinu, skala interval. (b) kategorik ordinal. (c) kategorik nominal. (d) numerik kontinu, skala rasio.
    2. Jumlah $40$, $n = 6$, rata-rata $6{,}667$. Median $= \dfrac{7 + 7}{2} = 7$. Modus $7$. $s^2 = \dfrac{29{,}33}{5} \approx 5{,}867$, $s \approx 2{,}422$.
    3. $P(A \cup B) = 0{,}5 + 0{,}3 - 0{,}1 = 0{,}7$; tidak satu pun: $0{,}3$.
    4. Jumlah 9: $(3,6), (4,5), (5,4), (6,3)$, yaitu $4$ dari $36$: $\dfrac{1}{9} \approx 0{,}1111$.
    5. $P(\text{berhenti} \mid \text{perempuan}) = \dfrac{10}{40} = 0{,}25$; $P(\text{berhenti} \mid \text{laki-laki}) = \dfrac{15}{60} = 0{,}25$. Peluang keseluruhan berhenti $= \dfrac{25}{100} = 0{,}25$. Sama, jadi independen (pada data ini).
    6. $\dbinom{4}{2}(0{,}5)^4 = \dfrac{6}{16} = 0{,}375$.
    7. $z = 1{,}5$, $\Phi(1{,}5) \approx 0{,}9332$.
    8. $\mathrm{SE} = \dfrac{12}{6} = 2$.

??? success "Tingkat 2"
    1. Diurutkan: $4, 6, 7, 8, 9, 10, 12, 40$. Minimum $4$, $Q_1 = 6{,}75$, median $8{,}5$, $Q_3 = 10{,}5$, maksimum $40$. $\text{IQR} = 3{,}75$. Pagar: $6{,}75 - 5{,}625 = 1{,}125$ dan $10{,}5 + 5{,}625 = 16{,}125$. Pencilan: $40$. Rata-rata $12$ jauh di atas median $8{,}5$ karena pencilan.
    2. $P(+) = 0{,}92(0{,}03) + 0{,}08(0{,}97) = 0{,}0276 + 0{,}0776 = 0{,}1052$. Posterior $= \dfrac{0{,}0276}{0{,}1052} \approx 0{,}2624$.
    3. $P(X = 3) = \dfrac{e^{-2{,}5}(2{,}5)^3}{6} \approx 0{,}2138$. $P(X \geq 1) = 1 - e^{-2{,}5} \approx 0{,}9179$.
    4. $\hat{p} = 0{,}3$, $n\hat{p} = 45$ dan $n(1 - \hat{p}) = 105$ (cukup). $\mathrm{SE} = 0{,}0374$. CI $= [0{,}227;\ 0{,}373]$.
    5. $\mathrm{SE} = \dfrac{5}{5} = 1$, $z = 2$, nilai-p $= 2(0{,}0228) \approx 0{,}0455 < 0{,}05$: tolak $H_0$. CI 95%: $22 \pm 1{,}96 = [20{,}04;\ 23{,}96]$ tidak memuat $20$.
    6. $\bar{x} = 2{,}5$, $\bar{y} = 5$. Deviasi $x$: $-1{,}5, -0{,}5, 0{,}5, 1{,}5$. Deviasi $y$: $-3, -1, 0, 4$. Hasil kali: $4{,}5, 0{,}5, 0, 6$, jumlah $11$. $s_{xy} = \dfrac{11}{3} \approx 3{,}667$. $s_x^2 = \dfrac{5}{3}$, $s_y^2 = \dfrac{26}{3}$. $r = \dfrac{11}{\sqrt{5 \times 26}} = \dfrac{11}{11{,}402} \approx 0{,}9648$.
    7. $65$: $z = \dfrac{65 - 50}{10} = 1{,}5$; Min-Max $= \dfrac{65 - 20}{60} = 0{,}75$. $90$: $z = 4$; Min-Max $= \dfrac{70}{60} \approx 1{,}1667$, di luar $[0, 1]$ karena data uji melampaui maksimum data latih.
    8. Salah. Nilai-p adalah peluang mendapat hasil sekurang-kurangnya seekstrem yang teramati **dengan anggapan $H_0$ benar** (di sini 3%). Ia tidak menghitung peluang $H_0$ benar; untuk itu diperlukan prior dan Teorema Bayes.

??? success "Tingkat 3"
    1. $\hat{p} = 0{,}85$, $\mathrm{SE} = 0{,}0252$, CI 95% $= [0{,}8005;\ 0{,}8995]$. Uji: $z = \dfrac{0{,}15}{0{,}0324} \approx 4{,}63$, nilai-p $\approx 1{,}8 \times 10^{-6}$ (eksak $6{,}5 \times 10^{-7}$). Tolak $H_0$: akurasi jelas melampaui baseline $0{,}70$.
    2. Prevalensi 30%: presisi $\approx 0{,}7083$. Prevalensi 5%: $\dfrac{0{,}0425}{0{,}0425 + 0{,}1425} \approx 0{,}2297$. Recall dan positif palsu tetap, tetapi dengan lebih sedikit spam sebenarnya, positif palsu dari jumlah besar e-mail sah mengalahkan positif benar. Presisi adalah posterior yang bergantung pada tingkat dasar.
    3. $\mathrm{SE} = \dfrac{1{,}708}{\sqrt{30}} \approx 0{,}3118$; $z = 0{,}962$; $P \approx 1 - 0{,}8320 = 0{,}168$.
    4. Margin $= 1{,}96\sqrt{\dfrac{0{,}25}{n}} \leq 0{,}03 \Rightarrow n \geq \left(\dfrac{1{,}96 \times 0{,}5}{0{,}03}\right)^2 \approx 1067{,}1$, jadi $n = 1068$.
    5. $1 - 0{,}95^{10} \approx 0{,}401$. Saran: pisahkan data validasi (untuk memilih konfigurasi) dari data uji (untuk evaluasi akhir sekali saja), tetapkan hipotesis dan jumlah percobaan di awal, atau gunakan koreksi untuk banyak pengujian.
    6. Bisa: pada sampel ini spam punya rata-rata tautan lebih banyak ($5{,}5$ vs $0{,}67$); hubungan linear positif kuat pada sampel; fitur ini kandidat yang layak diperiksa lebih lanjut. Tidak bisa: bahwa hubungan itu kuat pada populasi (sampel hanya 10, ketidakpastian besar); bahwa tautan menyebabkan spam; bahwa fitur ini cukup untuk model tanpa fitur lain atau tanpa validasi. Penyiapan: hitung rata-rata dan simpangan baku dari data latih saja, standardisasi data latih dan uji dengan statistik itu, dan periksa pencilan (satu e-mail dengan puluhan tautan akan menggeser rata-rata).

## Evaluasi Akhir Level 3

Evaluasi ini menilai **pemahaman alasan**. Jawab dengan kalimatmu sendiri lalu bandingkan dengan jawaban contoh.

1. Mengapa sampel besar tidak memperbaiki sampel yang bias?
2. Kapan median lebih tepat daripada rata-rata, dan mengapa varians sampel memakai $n - 1$?
3. Mengapa $P(A \mid B)$ tidak sama dengan $P(B \mid A)$? Beri contoh.
4. Apa beda kejadian saling lepas dan independen?
5. Mengapa tes yang akurat bisa menghasilkan banyak positif palsu pada kejadian langka?
6. Apa beda PMF, PDF, dan CDF, dan mengapa $P(X = x) = 0$ pada variabel kontinu?
7. Apa beda simpangan baku dan galat baku, dan apa kata CLT?
8. Apa tafsir yang tepat bagi interval kepercayaan 95% dan nilai-p?
9. Mengapa korelasi tidak sama dengan sebab-akibat, dan apa yang tidak ditangkap oleh korelasi Pearson?
10. Mengapa statistik penskalaan dihitung dari data latih saja, dan apakah semua algoritma perlu penskalaan?

??? success "Jawaban contoh evaluasi"
    1. Bias adalah kesalahan sistematis dari cara memilih sampel; sampel besar dengan cara yang sama hanya mengulang kesalahan yang sama dengan lebih meyakinkan. Yang mengecil dengan $n$ hanyalah kesalahan acak.
    2. Median lebih tepat bila data miring atau berpencilan karena tidak ditarik nilai ekstrem. Varians sampel memakai $n - 1$ karena deviasi dihitung dari $\bar{x}$ yang paling dekat ke sampel, sehingga pembagi $n$ cenderung terlalu kecil; $n - 1$ membuatnya tak bias.
    3. Karena ruang sampel yang menjadi syarat berbeda. Contoh: $P(\text{gratis} \mid \text{spam}) = 0{,}8$ tetapi $P(\text{spam} \mid \text{gratis}) \approx 0{,}77$ (angka berbeda, dan bisa sangat berbeda bila tingkat dasar berbeda). Teorema Bayes menghubungkan keduanya.
    4. Saling lepas: tidak bisa terjadi bersamaan ($P(A \cap B) = 0$). Independen: mengetahui satu tidak mengubah peluang yang lain ($P(A \cap B) = P(A)P(B)$). Dua kejadian saling lepas yang masing-masing berpeluang positif justru tidak independen.
    5. Karena presisi adalah posterior yang bergantung pada tingkat dasar. Bila kejadian langka, kelompok negatif sangat besar, sehingga persentase kecil positif palsu dari kelompok itu dapat melampaui jumlah positif benar dari kelompok kecil.
    6. PMF memberi peluang tiap nilai diskret; PDF adalah kepadatan (luasnya adalah peluang) untuk variabel kontinu; CDF adalah $P(X \leq x)$ untuk keduanya. Peluang satu titik kontinu nol karena luas di atas titik adalah nol; yang bermakna adalah peluang rentang.
    7. Simpangan baku menggambarkan sebaran individu; galat baku $\sigma/\sqrt{n}$ menggambarkan sebaran rata-rata sampel. CLT: untuk $n$ cukup besar dan sampel independen, rata-rata sampel kira-kira Normal walaupun populasinya tidak.
    8. Interval 95%: prosedur yang menghasilkannya memuat parameter pada sekitar 95% pengulangan (bukan peluang parameter ada di interval tertentu). Nilai-p: peluang hasil seekstrem yang teramati bila $H_0$ benar (bukan peluang $H_0$ benar).
    9. Korelasi dapat berasal dari faktor pengganggu, arah terbalik, atau kebetulan. Korelasi Pearson hanya menangkap hubungan **linear** dan peka pencilan, sehingga hubungan kuat tetapi melengkung dapat memberi $r \approx 0$.
    10. Agar tidak ada informasi data uji yang bocor ke pelatihan, dan agar penskalaan meniru kondisi nyata. Tidak semua algoritma membutuhkannya: pohon keputusan dan Naive Bayes Gaussian tidak peka skala, sedangkan model berbasis jarak dan gradien umumnya terbantu.

**Cara menilai diri.** Jawaban bernilai penuh jika memuat alasan dan contoh angka, bukan hanya definisi. Targetkan lancar pada minimal 8 dari 10 butir sebelum melanjutkan.

## Persiapan Menuju Level 4

[Level 4 — Matematika untuk Machine Learning Klasik](../level-4-matematika-ml-klasik/index.md) (regresi linear, regresi logistik, optimisasi, regularisasi, PCA, k-means) sekarang tersedia. Pemetaan kesiapan:

| Dibutuhkan Level 4 | Berasal dari | Ulangi jika |
|---|---|---|
| Matriks, perkalian matriks, invers | Level 2 Part 3–4 | belum lancar menghitung $X\mathbf{w}$ dan menyelesaikan sistem |
| Gradien dan gradient descent | Level 2 Part 8, 11 | belum bisa menghitung gradien kerugian |
| Nilai eigen dan vektor eigen (untuk PCA) | Level 2 Part 5 | belum paham ide arah yang hanya diskalakan |
| Kovarians dan matriks kovarians | Part 11 | belum bisa menghitung $s_{xy}$ dan $r$ |
| Distribusi Bernoulli dan Normal | Part 8 | belum paham peluang label dan galat |
| Kemungkinan maksimum (pengantar) | Part 10 | belum paham "parameter yang membuat data paling mungkin" |
| Standardisasi dan jarak | Part 11 | belum paham mengapa fitur diskalakan |
| Interval kepercayaan dan uji | Part 10 | belum bisa menyatakan ketidakpastian kinerja |

**Kriteria minimal:** kuasai Part 2, 4–8, dan 11 serta ide dasar Part 9–10, dan kembali ke Level 2 untuk aljabar linear dan kalkulus yang akan dipakai bersama. Jika evaluasi akhir belum 8 dari 10, ulangi part terkait terlebih dahulu.

!!! note "Yang belum dibahas di Level 3"
    Uji khi-kuadrat, ANOVA, regresi, inferensi Bayes lengkap (posterior atas parameter), metode kemungkinan maksimum lanjutan, distribusi multivariat (misalnya Normal multivariat), dan koreksi uji banyak secara mendalam belum termasuk. Topik-topik itu dibahas bila dibutuhkan di level lanjutan.

## Ringkasan Materi

- Level 3 menyatukan **statistika** (meringkas dan menarik kesimpulan dari data) dan **probabilitas** (mengukur ketidakpastian).
- Alur terpadu evaluasi model: sampel uji $\to$ matriks konfusi $\to$ recall, positif palsu, presisi (Bayes) $\to$ galat baku $\to$ interval kepercayaan dan uji terhadap baseline $\to$ laporan dengan asumsi dan batas.
- **Presisi** adalah posterior dan bergantung pada tingkat dasar; recall dan positif palsu adalah sifat model.
- Akurasi uji adalah proporsi sampel dengan galat baku $\sqrt{\hat{p}(1 - \hat{p})/n}$; sampel $n$ lebih besar mengecilkannya.
- Nilai-p dan interval kepercayaan punya tafsir yang spesifik; banyak pengujian menaikkan positif palsu.
- Korelasi bukan sebab-akibat; standardisasi dilakukan dengan statistik data latih dan tidak wajib untuk semua algoritma.
- Kebiasaan: lihat datanya, periksa asumsi, laporkan ketidakpastian, hindari kebocoran data.

## Checklist Penguasaan

- [ ] Menjelaskan dengan kalimat sendiri alur evaluasi model dari sampel uji sampai kesimpulan.
- [ ] Menghitung presisi dengan Bayes dan menjelaskan pengaruh tingkat dasar.
- [ ] Menghitung CI dan melakukan uji untuk proporsi.
- [ ] Mengerjakan Tingkat 1 dan 2 dengan benar, dan sebagian besar Tingkat 3.
- [ ] Menjawab minimal 8 dari 10 butir evaluasi konseptual dengan alasan.
- [ ] Mengetahui bagian Level 3 yang perlu diulang sebelum Level 4.

## Prasyarat untuk Materi Berikutnya

[Level 4 — Matematika untuk Machine Learning Klasik](../level-4-matematika-ml-klasik/index.md) sekarang tersedia. Jika kamu memenuhi kriteria kesiapan di atas, kamu siap melanjutkannya.
