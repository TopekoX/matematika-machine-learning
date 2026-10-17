# Part 5 — Loss Function

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- membedakan **loss per contoh**, **rata-rata loss**, dan **gradien loss**;
- menghitung **MSE**, **MAE**, ***binary cross-entropy*** (BCE), dan ***categorical cross-entropy*** (CCE) dengan angka sederhana;
- menjelaskan mengapa loss untuk **regresi** dan **klasifikasi** berbeda, termasuk dengan angka gradien;
- memasangkan aktivasi keluaran dengan loss yang sesuai dan menurunkan gradien $p-y$ pada pasangan softmax dengan CCE;
- menafsirkan **entropi**, **cross-entropy**, dan **KL divergence** secara singkat;
- mengenali dan menghindari masalah numerik pada loss berbasis logaritma.

## Prasyarat

[Part 4 — Fungsi Aktivasi](04-fungsi-aktivasi.md) (sigmoid dan softmax), [Level 4 Part 3 — Regresi Linear](../level-4-matematika-ml-klasik/03-regresi-linear.md) (MSE dan MAE), [Level 4 Part 4 — Regresi Logistik](../level-4-matematika-ml-klasik/04-regresi-logistik.md) (*log loss*), dan [Level 1 Part 10](../level-1-aljabar-dasar-dan-fungsi/10-fungsi-eksponensial-dan-logaritma.md) ($\ln$ dan sifat logaritma). Bagian ini mengulang rumus penting secara singkat.

## Masalah yang Ingin Diselesaikan

Pelatihan butuh satu bilangan yang mengatakan "seberapa buruk prediksi saat ini". Bilangan itu harus (1) kecil bila prediksi bagus dan besar bila buruk, dan (2) **mudah diturunkan** supaya gradiennya bisa dipakai untuk memperbaiki parameter.

Pilihan loss tidak sembarang. Keluaran regresi adalah bilangan bebas, sedangkan keluaran klasifikasi adalah peluang. Mengukur selisih bilangan (kuadrat atau mutlak) cocok untuk yang pertama, tetapi mengukur "seberapa yakin model pada kelas yang benar" (logaritma peluang) cocok untuk yang kedua. Bagian ini menunjukkan alasannya dengan angka.

## Intuisi dan Istilah Penting

- **Loss per contoh** $\ell^{(i)}$: ukuran kesalahan pada satu contoh.
- **Rata-rata loss** $L=\dfrac{1}{n}\sum_{i=1}^{n}\ell^{(i)}$: angka yang benar-benar diperkecil (atau pada *mini-batch*, rata-rata beberapa contoh).
- **Gradien loss**: $\partial L/\partial(\text{parameter})$. Gradien bukan loss; ia menunjukkan arah perbaikan.
- **Residual**: $\hat y-y$ pada regresi (bertanda).
- **Label *one-hot***: vektor berisi satu angka 1 pada kelas benar dan 0 pada kelas lain, misalnya kelas ke-2 dari 3 ditulis $(0,1,0)$.
- Keluaran model dan label berbeda jenis: $\hat y$ dan $y$ pada regresi adalah bilangan; $p$ adalah peluang, $y$ adalah label kelas.

## Rumus dan Penjelasan Setiap Simbol

Dalam semua rumus, $n$ adalah jumlah contoh, $y^{(i)}$ adalah target contoh ke-$i$, dan $\hat y^{(i)}$ atau $p^{(i)}$ adalah keluaran model.

### Regresi: MSE dan MAE

$$
L_{\text{MSE}}=\frac{1}{n}\sum_{i=1}^{n}\bigl(\hat y^{(i)}-y^{(i)}\bigr)^2,\qquad
L_{\text{MAE}}=\frac{1}{n}\sum_{i=1}^{n}\bigl\lvert\hat y^{(i)}-y^{(i)}\bigr\rvert
$$

$\hat y^{(i)}$ dan $y^{(i)}$ adalah bilangan. Gradien terhadap $\hat y^{(i)}$:

$$
\frac{\partial L_{\text{MSE}}}{\partial\hat y^{(i)}}=\frac{2}{n}\bigl(\hat y^{(i)}-y^{(i)}\bigr),\qquad
\frac{\partial L_{\text{MAE}}}{\partial\hat y^{(i)}}=\frac{1}{n}\operatorname{sign}\bigl(\hat y^{(i)}-y^{(i)}\bigr)
$$

Gradien MSE **proporsional dengan besar kesalahan** (kesalahan besar mendapat koreksi besar, dan kesalahan besar mendominasi). Gradien MAE selalu berukuran $1/n$ (kecuali tepat di $0$, di mana turunan tidak terdefinisi), sehingga lebih tahan terhadap pencilan tetapi tidak mengecil saat mendekati target. Keluaran regresi biasanya memakai aktivasi identitas.

### Klasifikasi biner: *binary cross-entropy*

Dengan $p=\sigma(z)$ peluang kelas 1 dan $y\in\{0,1\}$:

$$
\ell=-\bigl[\,y\ln p+(1-y)\ln(1-p)\,\bigr],\qquad L_{\text{BCE}}=\frac{1}{n}\sum_{i}\ell^{(i)}
$$

Untuk $y=1$ rumus menjadi $-\ln p$, dan untuk $y=0$ menjadi $-\ln(1-p)$: loss kecil bila peluang kelas yang benar besar, dan membesar tanpa batas bila peluang itu mendekati 0. Untuk satu contoh dengan sigmoid:

$$
\frac{\partial\ell}{\partial z}=p-y
$$

### Klasifikasi multikelas: *categorical cross-entropy*

Dengan $\mathbf{p}=\operatorname{softmax}(\mathbf{z})$ vektor peluang $K$ kelas dan $\mathbf{y}$ label *one-hot* (kelas benar $c$):

$$
\ell=-\sum_{k=1}^{K}y_k\ln p_k=-\ln p_c
$$

Karena $y_k=0$ kecuali $k=c$, hanya peluang kelas benar yang dihitung. **Gradien terhadap logit** $z_j$:

$$
\frac{\partial\ell}{\partial z_j}=-\frac{1}{p_c}\frac{\partial p_c}{\partial z_j}=-\frac{1}{p_c}\,p_c(\delta_{cj}-p_j)=p_j-\delta_{cj}=p_j-y_j
$$

Langkah kedua memakai Jacobian softmax dari [Part 4](04-fungsi-aktivasi.md). Hasilnya **bentuk sederhana yang sama dengan kasus biner**: gradien terhadap logit adalah "peluang prediksi dikurangi label".

### Entropi, cross-entropy, dan KL divergence

Untuk dua distribusi peluang $P$ (target) dan $Q$ (prediksi) pada $K$ kelas:

$$
H(P)=-\sum_k P_k\ln P_k,\qquad H(P,Q)=-\sum_k P_k\ln Q_k,\qquad D_{\mathrm{KL}}(P\,\Vert\,Q)=\sum_k P_k\ln\frac{P_k}{Q_k}
$$

- $H(P)$ (**entropi**): ketidakpastian rata-rata distribusi $P$ (nol bila $P$ pasti).
- $H(P,Q)$ (**cross-entropy**): biaya rata-rata memakai $Q$ untuk menggambarkan kejadian dari $P$.
- $D_{\mathrm{KL}}(P\Vert Q)$: selisih keduanya, $D_{\mathrm{KL}}(P\Vert Q)=H(P,Q)-H(P)\ge0$, bernilai $0$ hanya bila $P=Q$. **Tidak simetris**: $D_{\mathrm{KL}}(P\Vert Q)\neq D_{\mathrm{KL}}(Q\Vert P)$ pada umumnya.

Untuk label *one-hot*, $H(P)=0$ sehingga CCE sama dengan KL divergence; meminimalkan CCE berarti mendekatkan distribusi prediksi ke distribusi label. Itu juga berarti meminimalkan CCE sama dengan memaksimalkan *log-likelihood* (peluang data menurut model), konsep dari [Level 4 Part 4](../level-4-matematika-ml-klasik/04-regresi-logistik.md). Dengan cara yang sejajar, MSE sesuai dengan asumsi galat berdistribusi Normal, BCE dengan Bernoulli, dan CCE dengan distribusi kategorikal.

### Memasangkan aktivasi keluaran dan loss

| Masalah | Aktivasi keluaran | Loss | Gradien terhadap $z$ |
|---|---|---|---|
| Regresi | identitas | MSE (atau MAE) | $\dfrac{2}{n}(\hat y-y)$ per contoh (MSE) |
| Biner | sigmoid | BCE | $p-y$ |
| Multikelas (satu kelas benar) | softmax | CCE | $p_j-y_j$ |
| Multi-label | sigmoid per label | BCE per label, dijumlahkan | $p_k-y_k$ per label |

## Contoh Numerik Langkah demi Langkah

### Contoh 1 — MSE dan MAE

Target $y=(3,5,7)$ dan prediksi $\hat y=(2{,}5,\ 5{,}5,\ 9)$. Residual $\hat y-y=(-0{,}5,\ 0{,}5,\ 2)$.

$$
L_{\text{MSE}}=\frac{0{,}25+0{,}25+4}{3}=\frac{4{,}5}{3}=1{,}5,\qquad
L_{\text{MAE}}=\frac{0{,}5+0{,}5+2}{3}=\frac{3}{3}=1{,}0
$$

Kesalahan terbesar ($2$) memberi $4$ dari $4{,}5$ total kuadrat: MSE didominasi kesalahan besar. Gradien MSE terhadap tiap prediksi: $\tfrac{2}{3}(-0{,}5,\ 0{,}5,\ 2)=(-0{,}333,\ 0{,}333,\ 1{,}333)$; gradien MAE: $\tfrac13(-1,\ 1,\ 1)=(-0{,}333,\ 0{,}333,\ 0{,}333)$. Pada prediksi ketiga, MSE menekan empat kali lebih keras daripada MAE.

### Contoh 2 — *Binary cross-entropy*

Tiga pesan dengan label $y=(1,0,1)$ ($1$ = Emergency; **data contoh/simulasi**) dan peluang prediksi Emergency $p=(0{,}9,\ 0{,}2,\ 0{,}4)$.

| $i$ | $y$ | $p$ | Peluang kelas benar | $\ell=-\ln(\cdot)$ |
|---|---|---|---|---|
| 1 | $1$ | $0{,}9$ | $0{,}9$ | $0{,}1054$ |
| 2 | $0$ | $0{,}2$ | $1-0{,}2=0{,}8$ | $0{,}2231$ |
| 3 | $1$ | $0{,}4$ | $0{,}4$ | $0{,}9163$ |

$$
L_{\text{BCE}}=\frac{0{,}1054+0{,}2231+0{,}9163}{3}=\frac{1{,}2448}{3}\approx0{,}4149
$$

Contoh ketiga (yakin kurang dari $50\%$ pada kelas benar) menyumbang loss terbesar. Perhatikan bahwa keputusan dengan ambang $0{,}5$ salah di contoh ketiga, tetapi loss juga menghitung *seberapa* salah.

### Contoh 3 — *Categorical cross-entropy* dan gradiennya

Logit $\mathbf{z}=(2,1,0)$ memberi $\mathbf{p}=(0{,}6652,\ 0{,}2447,\ 0{,}0900)$ (Part 4).

- Jika kelas benar adalah kelas 1: $\ell=-\ln0{,}6652=0{,}4076$. Gradien $\mathbf{p}-\mathbf{y}=(0{,}6652-1,\ 0{,}2447,\ 0{,}0900)=(-0{,}3348,\ 0{,}2447,\ 0{,}0900)$.
- Jika kelas benar adalah kelas 3: $\ell=-\ln0{,}0900=2{,}4076$. Model yakin pada kelas 1 padahal yang benar kelas 3, sehingga loss besar.

Jumlah gradien $-0{,}3348+0{,}2447+0{,}0900=0$ (sama dengan sifat Jacobian di Part 4): menurunkan satu logit pasti menaikkan yang lain.

### Contoh 4 — Mengapa MSE kurang cocok untuk klasifikasi dengan sigmoid

Satu contoh dengan $y=1$. Bandingkan gradien terhadap logit $z$ dengan BCE ($p-y$) dan dengan MSE pada peluang, $\ell=(p-y)^2$, yang gradiennya $2(p-y)\,p(1-p)$ (rantai: $\partial\ell/\partial p\cdot\sigma'(z)$):

| $z$ | $p=\sigma(z)$ | gradien BCE | gradien MSE |
|---|---|---|---|
| $-4$ | $0{,}0180$ | $-0{,}9820$ | $-0{,}0347$ |
| $-2$ | $0{,}1192$ | $-0{,}8808$ | $-0{,}1850$ |
| $2$ | $0{,}8808$ | $-0{,}1192$ | $-0{,}0250$ |

Pada $z=-4$ model **sangat salah** (peluang $0{,}018$ untuk kelas benar). BCE memberi gradien besar ($-0{,}98$), tetapi MSE hanya $-0{,}035$ karena faktor $\sigma'(z)=p(1-p)$ yang mengecil. Jadi MSE + sigmoid hampir tidak memperbaiki justru ketika model paling salah. Faktor $\sigma'(z)$ pada BCE saling meniadakan dengan $1/p$ dari turunan logaritma sehingga hasilnya sederhana: $p-y$. Angka yang sama muncul di [Level 4 Part 4](../level-4-matematika-ml-klasik/04-regresi-logistik.md).

### Contoh 5 — Entropi, cross-entropy, dan KL

Target (distribusi "lunak") $P=(0{,}5,\ 0{,}5)$ dan prediksi $Q=(0{,}9,\ 0{,}1)$.

- $H(P)=-(0{,}5\ln0{,}5+0{,}5\ln0{,}5)=\ln2=0{,}6931$.
- $H(P,Q)=-(0{,}5\ln0{,}9+0{,}5\ln0{,}1)=0{,}0527+1{,}1513=1{,}2040$.
- $D_{\mathrm{KL}}(P\Vert Q)=1{,}2040-0{,}6931=0{,}5108$. Langsung dari definisi: $0{,}5\ln\frac{0{,}5}{0{,}9}+0{,}5\ln\frac{0{,}5}{0{,}1}=-0{,}2939+0{,}8047=0{,}5108$.
- Arah terbalik: $D_{\mathrm{KL}}(Q\Vert P)=0{,}9\ln\frac{0{,}9}{0{,}5}+0{,}1\ln\frac{0{,}1}{0{,}5}=0{,}5290-0{,}1609=0{,}3681$. Tidak sama dengan $0{,}5108$: KL tidak simetris.

Cross-entropy ($1{,}2040$) tidak bisa lebih kecil dari entropi target ($0{,}6931$); selisihnya adalah KL. Bila $Q=P$, cross-entropy sama dengan entropi dan KL bernilai $0$.

### Contoh 6 — Kestabilan numerik

Untuk $y=1$ dan logit sangat negatif $z=-800$, nilai $\sigma(-800)=1/(1+e^{800})$ **tidak dapat dihitung** karena $e^{800}$ meluap (*overflow*). Bentuk setara yang stabil memakai logit langsung:

$$
\ell(z,y)=\max(z,0)-z\,y+\ln\!\bigl(1+e^{-\lvert z\rvert}\bigr)
$$

Untuk $z=-800$, $y=1$: $\max(-800,0)=0$, $-zy=800$, $\ln(1+e^{-800})\approx0$, sehingga $\ell=800$. Bentuk ini tidak pernah memanggil eksponen bilangan besar positif. Untuk $z=-30$: $\ell\approx30{,}0000000000001$, sama dengan hitungan langsung $-\ln\sigma(-30)$. Pustaka Deep Learning menyediakan loss yang menerima logit untuk alasan ini.

## Interpretasi, Asumsi, dan Keterbatasan

- Loss yang rendah belum berarti model baik pada data baru; ukur pada data uji dan dengan metrik yang sesuai ([Level 4 Part 11](../level-4-matematika-ml-klasik/11-evaluasi-dan-pemilihan-model.md)).
- MSE peka terhadap pencilan; MAE lebih tahan tetapi gradiennya tidak mengecil mendekati target dan tidak terdefinisi tepat di nol (dalam praktik dipilih sembarang nilai di antara $-1$ dan $1$).
- Cross-entropy menghukum keyakinan yang salah sangat keras ($-\ln p\to\infty$ saat $p\to0$). Model yang terlalu yakin dapat menjadi masalah (kalibrasi); teknik seperti *label smoothing* melunakkan target.
- Pada data tak seimbang, rata-rata loss didominasi kelas mayoritas; pembobotan kelas atau metrik lain dapat dipakai ([Level 4 Part 11](../level-4-matematika-ml-klasik/11-evaluasi-dan-pemilihan-model.md)).
- Klaim "MSE ↔ Normal, BCE ↔ Bernoulli" adalah interpretasi peluang; tidak berarti data pasti memenuhi asumsi itu.

## Hubungan dengan Deep Learning dan NLP

Klasifikasi teks multikelas pada Transformer memakai CCE pada logit hasil classification head ([Part 14](14-fondasi-matematis-bert-dan-indobert.md)). Pelatihan model bahasa memakai CCE pada kosakata: tebakan token berikutnya atau token yang disembunyikan, dengan kelas sebanyak ukuran kosakata. Pada klasifikasi Emergency vs Non-Emergency (contoh/simulasi) dua kelas, BCE dengan sigmoid dan CCE dengan softmax dua keluaran menghasilkan model yang setara.

## Python

Pustaka standar saja.

```python
import math

def sigmoid(z):
    return 1 / (1 + math.exp(-z))

def softmax(zs):
    e = [math.exp(z) for z in zs]
    jumlah = sum(e)
    return [v / jumlah for v in e]

def mse(y, yh):
    return sum((a - b) ** 2 for a, b in zip(y, yh)) / len(y)

def mae(y, yh):
    return sum(abs(a - b) for a, b in zip(y, yh)) / len(y)

def bce(y, p):
    return sum(-(a * math.log(q) + (1 - a) * math.log(1 - q)) for a, q in zip(y, p)) / len(y)

def cce(c, p):               # c = indeks kelas benar (mulai dari 0)
    return -math.log(p[c])

def bce_logit(z, y):         # bentuk stabil: langsung dari logit
    return max(z, 0) - z * y + math.log(1 + math.exp(-abs(z)))

print(mse([3, 5, 7], [2.5, 5.5, 9]), mae([3, 5, 7], [2.5, 5.5, 9]))
print(round(bce([1, 0, 1], [0.9, 0.2, 0.4]), 4))
p = softmax([2, 1, 0])
print(round(cce(0, p), 4), round(cce(2, p), 4), [round(p[0] - 1, 4), round(p[1], 4), round(p[2], 4)])

# gradien terhadap logit untuk y = 1: log loss vs MSE
for z in (-4, -2, 2):
    q = sigmoid(z)
    print(z, round(q - 1, 4), round(2 * (q - 1) * q * (1 - q), 4))

# entropi silang dan KL, target (0.5, 0.5) dan prediksi (0.9, 0.1)
P, Q = [0.5, 0.5], [0.9, 0.1]
ce = -sum(a * math.log(b) for a, b in zip(P, Q))
h = -sum(a * math.log(a) for a in P)
print(round(ce, 4), round(h, 4), round(ce - h, 4))

# kestabilan numerik: logit sangat negatif, y = 1
print(bce_logit(-800, 1))
try:
    print(-math.log(sigmoid(-800)))
except OverflowError as e:
    print("OverflowError:", e)
```

Keluaran:

```text
1.5 1.0
0.4149
0.4076 2.4076 [-0.3348, 0.2447, 0.09]
-4 -0.982 -0.0347
-2 -0.8808 -0.185
2 -0.1192 -0.025
1.204 0.6931 0.5108
800.0
OverflowError: math range error
```

Semuanya sama dengan Contoh 1 sampai 6. Baris terakhir menunjukkan mengapa bentuk stabil diperlukan: hitungan langsung dari `sigmoid` gagal, sedangkan `bce_logit` memberi $800$.

## Kesalahan Umum

1. **Memakai MSE untuk klasifikasi dengan sigmoid atau softmax**, sehingga gradien mengecil justru saat model paling salah (Contoh 4).
2. **Menghitung `log(0)`.** Peluang tepat $0$ atau $1$ menghasilkan tak hingga; gunakan bentuk dari logit atau batasi nilai peluang.
3. **Memasukkan probabilitas ke loss yang mengharapkan logit** (atau sebaliknya), sehingga softmax/sigmoid terpakai dua kali.
4. **Salah memahami arah KL.** $D_{\mathrm{KL}}(P\Vert Q)\neq D_{\mathrm{KL}}(Q\Vert P)$.
5. **Menjumlahkan loss tanpa membagi $n$** lalu membandingkan antar-ukuran *batch*. Gunakan rata-rata atau catat konvensinya.
6. **Menyamakan loss rendah dengan akurasi tinggi.** Loss dan akurasi mengukur hal berbeda; keduanya bisa bergerak tidak seiring.
7. **Memakai softmax + CCE untuk masalah multi-label**, padahal label tidak saling meniadakan.

## Latihan Mandiri

1. Hitung MSE dan MAE untuk $y=(2,4,6,8)$ dan $\hat y=(3,3,6,10)$.
2. Dua model punya residual $(1,1,1,1)$ dan $(0,0,0,4)$. Hitung MAE dan MSE keduanya dan jelaskan perbedaannya.
3. Hitung BCE untuk satu contoh: (a) $y=1$, $p=0{,}7$; (b) $y=0$, $p=0{,}7$.
4. Hitung rata-rata BCE untuk $y=(1,1,0,0)$ dan $p=(0{,}8,\ 0{,}6,\ 0{,}3,\ 0{,}1)$.
5. Logit $\mathbf{z}=(1,0,-1)$ dan kelas benar adalah kelas 2. Hitung $\mathbf{p}$, CCE, dan gradien $\mathbf{p}-\mathbf{y}$.
6. Untuk $y=1$ dan $z=-4$, bandingkan gradien BCE dan gradien MSE terhadap logit dan jelaskan artinya.
7. Hitung $H(P)$, $H(P,Q)$, dan $D_{\mathrm{KL}}(P\Vert Q)$ untuk $P=Q=(0{,}5,0{,}5)$, lalu untuk $P=(0{,}5,0{,}5)$ dan $Q=(0{,}9,0{,}1)$.
8. Pilih aktivasi keluaran dan loss: (a) memprediksi harga rumah; (b) pesan Emergency atau bukan; (c) mengklasifikasikan pesan ke 4 kategori; (d) memberi tag yang dapat berjumlah lebih dari satu pada satu pesan.
9. Hitung $\ell$ dengan bentuk stabil untuk $z=-30$, $y=1$ dan bandingkan dengan $-\ln\sigma(-30)$. Mengapa bentuk stabil tetap berlaku untuk $z=-800$?
10. Target lunak $\mathbf{y}=(0{,}8,\,0{,}1,\,0{,}1)$ dan prediksi $\mathbf{p}=(0{,}7,\,0{,}2,\,0{,}1)$. Hitung cross-entropy, entropi target, dan KL.

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    Residual $\hat y-y=(1,-1,0,2)$. MSE $=\dfrac{1+1+0+4}{4}=1{,}5$. MAE $=\dfrac{1+1+0+2}{4}=1{,}0$.

??? success "Soal 2"
    Model A: MAE $=1$, MSE $=1$. Model B: MAE $=\dfrac{4}{4}=1$, MSE $=\dfrac{16}{4}=4$. MAE sama, MSE berbeda empat kali: MSE menghukum satu kesalahan besar jauh lebih keras daripada beberapa kesalahan kecil.

??? success "Soal 3"
    (a) $-\ln0{,}7=0{,}3567$. (b) $y=0$: $-\ln(1-0{,}7)=-\ln0{,}3=1{,}2040$. Peluang $0{,}7$ bagus bila kelas benar adalah 1 dan buruk bila kelas benar adalah 0.

??? success "Soal 4"
    Peluang kelas benar: $0{,}8,\ 0{,}6,\ 0{,}7,\ 0{,}9$. Loss: $0{,}2231,\ 0{,}5108,\ 0{,}3567,\ 0{,}1054$. Rata-rata $=\dfrac{1{,}1960}{4}\approx0{,}2990$.

??? success "Soal 5"
    $e^{1}=2{,}7183$, $e^{0}=1$, $e^{-1}=0{,}3679$, jumlah $4{,}0862$. $\mathbf{p}=(0{,}6652,\ 0{,}2447,\ 0{,}0900)$ (sama dengan Part 4 karena logit hanya digeser $-1$). Kelas benar 2: CCE $=-\ln0{,}2447=1{,}4076$. Gradien $\mathbf{p}-\mathbf{y}=(0{,}6652,\ 0{,}2447-1,\ 0{,}0900)=(0{,}6652,\ -0{,}7553,\ 0{,}0900)$, jumlah $0$.

??? success "Soal 6"
    $p=\sigma(-4)=0{,}0180$. BCE: $p-y=-0{,}9820$. MSE: $2(p-y)\,p(1-p)=2(-0{,}9820)(0{,}01766)=-0{,}0347$. Gradien MSE sekitar 28 kali lebih kecil, padahal model sangat salah. Pada pelatihan, koreksi akan sangat lambat.

??? success "Soal 7"
    $P=Q$: $H(P)=\ln2=0{,}6931$, $H(P,Q)=0{,}6931$, KL $=0$. $Q=(0{,}9,0{,}1)$: $H(P)=0{,}6931$, $H(P,Q)=1{,}2040$, KL $=0{,}5108$.

??? success "Soal 8"
    (a) identitas + MSE (atau MAE bila banyak pencilan). (b) sigmoid + BCE (atau softmax dua keluaran + CCE). (c) softmax + CCE. (d) sigmoid per tag + BCE per tag (multi-label).

??? success "Soal 9"
    $\ell=\max(-30,0)-(-30)(1)+\ln(1+e^{-30})=0+30+9{,}4\times10^{-14}\approx30{,}0000000000001$. Hitungan langsung $-\ln\sigma(-30)$ memberi nilai yang sama. Untuk $z=-800$ bentuk stabil hanya memakai $e^{-\lvert z\rvert}=e^{-800}\approx0$ (tidak meluap), sehingga $\ell=800$; hitungan langsung meluap pada $e^{800}$.

??? success "Soal 10"
    Cross-entropy $=-(0{,}8\ln0{,}7+0{,}1\ln0{,}2+0{,}1\ln0{,}1)=0{,}2853+0{,}1609+0{,}2303=0{,}6765$. Entropi target $=-(0{,}8\ln0{,}8+0{,}1\ln0{,}1+0{,}1\ln0{,}1)=0{,}1785+0{,}2303+0{,}2303=0{,}6390$. KL $=0{,}6765-0{,}6390\approx0{,}0375$ (nilai kecil karena prediksi dekat target).

## Ringkasan Materi

- **Loss per contoh**, **rata-rata loss**, dan **gradien** adalah tiga hal berbeda; pelatihan memperkecil rata-rata loss.
- Regresi: **MSE** ($\tfrac{2}{n}(\hat y-y)$ sebagai gradien) peka terhadap pencilan; **MAE** lebih tahan.
- Klasifikasi: **BCE** untuk biner dan **CCE** untuk multikelas; gradien terhadap logit adalah $p-y$ untuk pasangan sigmoid–BCE dan softmax–CCE.
- MSE dengan sigmoid memberi gradien kecil saat model sangat salah; cross-entropy tidak.
- **Entropi**, **cross-entropy**, dan **KL**: $D_{\mathrm{KL}}(P\Vert Q)=H(P,Q)-H(P)\ge0$, tidak simetris; untuk label *one-hot*, CCE sama dengan KL.
- Hitung loss dari logit dengan bentuk stabil untuk menghindari *overflow* dan $\ln0$.

## Checklist Penguasaan

- [ ] Menghitung MSE, MAE, BCE, dan CCE untuk contoh kecil.
- [ ] Menjelaskan perbedaan loss regresi dan klasifikasi dengan angka gradien.
- [ ] Menurunkan gradien $p_j-y_j$ untuk softmax dengan CCE.
- [ ] Memasangkan aktivasi keluaran dengan loss yang tepat.
- [ ] Menghitung entropi, cross-entropy, dan KL, serta menjelaskan sifatnya.
- [ ] Menjelaskan mengapa loss dihitung dari logit untuk kestabilan numerik.

## Prasyarat untuk Materi Berikutnya

[Part 6 — Backpropagation Mendalam](06-backpropagation-mendalam.md) memakai turunan loss dan aktivasi dari Part 4 dan 5 untuk menghitung gradien semua parameter dengan *chain rule*, dimulai dari satu neuron. Pastikan kamu hafal gradien terhadap logit untuk pasangan sigmoid–BCE ($p-y$) dan turunan sigmoid, tanh, serta ReLU.
