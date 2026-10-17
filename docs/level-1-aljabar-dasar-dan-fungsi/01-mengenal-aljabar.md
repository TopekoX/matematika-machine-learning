# Part 1 — Mengenal Aljabar

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan apa itu aljabar dan mengapa huruf dipakai sebagai pengganti bilangan;
- membedakan variabel, konstanta, koefisien, dan suku dalam sebuah bentuk aljabar;
- membedakan ekspresi, persamaan, dan identitas;
- menerjemahkan kalimat sederhana menjadi ekspresi aljabar;
- mensubstitusi nilai (termasuk negatif dan lebih dari satu variabel) ke dalam ekspresi;
- membaca notasi aljabar dengan benar, termasuk perbedaan seperti $-x^2$ dan $(-x)^2$.

## Prasyarat

[Level 0 — Part 7](../level-0-fondasi-matematika/07-variabel-dan-persamaan.md) (pengantar variabel dan persamaan) serta [Part 5](../level-0-fondasi-matematika/05-pangkat-dan-akar.md) (pangkat). Bagian ini mengulang pengantar tersebut secukupnya, lalu memperdalamnya.

## Mengapa Materi Ini Penting?

Seluruh model Machine Learning ditulis dalam bahasa aljabar. Rumus seperti $\hat{y} = wx + b$ memuat huruf yang masing-masing punya peran berbeda: masukan, parameter, dan keluaran. Jika kamu tidak bisa membedakan perannya, rumus itu hanya terlihat seperti deretan huruf. Aljabar juga melatih berpikir **umum**: satu rumus yang berlaku untuk banyak kasus.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Apa Itu Aljabar?

**Aljabar** (*algebra*) adalah cabang matematika yang memakai **huruf** untuk menggantikan bilangan sehingga kita bisa menulis aturan yang berlaku umum.

Mulai dari contoh sehari-hari. Harga satu buku Rp5.000.

- 1 buku: $5.000 \times 1 = 5.000$.
- 2 buku: $5.000 \times 2 = 10.000$.
- 3 buku: $5.000 \times 3 = 15.000$.

Daripada menulis satu per satu, kita tulis satu aturan: jika banyak buku adalah $n$ (huruf yang mewakili "banyak buku, bisa berapa saja"), maka

$$
\text{total} = 5.000 \times n
$$

Aturan ini cocok untuk $n = 1$, $n = 100$, atau bilangan lain.

Contoh lain adalah pola bilangan $3, 5, 7, 9, \ldots$. Bilangan ke-1 adalah 3, ke-2 adalah 5, dan seterusnya. Setiap kali naik 2. Aturannya: bilangan ke-$n$ adalah $2n + 1$. Cek: $n = 1$ memberi $2(1) + 1 = 3$; $n = 4$ memberi $2(4) + 1 = 9$. Dengan aturan itu, bilangan ke-10 langsung diketahui: $2(10) + 1 = 21$.

Jadi aljabar adalah cara menulis **aturan** dan **pola** secara ringkas.

### 2. Mengapa Aljabar Penting untuk Machine Learning?

Bayangkan memprediksi harga rumah dari luasnya. Tanpa aljabar, kita harus menyimpan tabel panjang berisi pasangan luas dan harga. Dengan aljabar, cukup satu aturan:

$$
\hat{y} = w x + b
$$

Huruf $x$ mewakili luas rumah apa pun, dan $\hat{y}$ mewakili prediksi harganya. Selain itu, $w$ dan $b$ adalah bilangan yang diatur selama model belajar.

Tiga alasan aljabar penting:

1. **Satu rumus, banyak data.** Rumus yang sama berlaku untuk jutaan data.
2. **Parameter bisa diubah.** Huruf $w$ dan $b$ memungkinkan kita berbicara tentang "mengubah model" tanpa menyebut angka.
3. **Bahasa bersama.** Buku, makalah, dan dokumentasi ML semuanya memakai notasi ini.

### 3. Variabel

**Variabel** (*variable*) adalah huruf yang mewakili bilangan. Ada dua peran, tergantung konteks:

- **Nilai yang bisa berubah.** Pada biaya parkir $C = 2.000 + 1.000t$, huruf $t$ adalah lama parkir dalam jam. Nilai $t$ bisa 1, 2, 3, atau lainnya, dan $C$ ikut berubah.
- **Nilai yang belum diketahui.** Pada $x + 3 = 7$, huruf $x$ mewakili satu bilangan yang belum kita ketahui dan sedang dicari.

Konteks menentukan perannya. Selalu tanyakan: "Apakah nilai huruf ini boleh bermacam-macam, atau ia sedang dicari?"

Pemberian nama bebas, tetapi ada kebiasaan: $x$, $y$, $z$ untuk besaran umum; $n$, $k$ untuk banyaknya; $t$ untuk waktu; $p$ dan $l$ untuk panjang dan lebar.

### 4. Konstanta

**Konstanta** (*constant*) adalah nilai yang tetap dalam suatu persoalan.

- Pada $C = 2.000 + 1.000t$, angka $2.000$ dan $1.000$ adalah konstanta (biaya tetap dan tarif per jam).
- $\pi \approx 3{,}14159$ adalah konstanta yang nilainya sama di semua konteks.

Sebuah huruf juga bisa berperan sebagai konstanta. Pada $y = ax + b$, huruf $a$ dan $b$ dianggap konstanta selama kita membahas satu garis tertentu, meskipun nilainya belum disebut. Itulah sebabnya tidak ada huruf yang "selalu variabel" atau "selalu konstanta"; **perannya ditentukan oleh konteks**.

### 5. Koefisien

**Koefisien** (*coefficient*) adalah bilangan yang **mengalikan** variabel (atau variabel berpangkat) dalam sebuah suku.

| Suku | Koefisien | Alasan |
|---|---|---|
| $5x$ | $5$ | $5x = 5 \times x$ |
| $-7y$ | $-7$ | tanda minus ikut koefisien |
| $x$ | $1$ | $x = 1 \times x$ |
| $-x$ | $-1$ | $-x = -1 \times x$ |
| $\dfrac{x}{2}$ | $\dfrac{1}{2}$ | $\dfrac{x}{2} = \dfrac{1}{2} \times x$ |
| $0{,}3t^2$ | $0{,}3$ | koefisien $t^2$ |

Koefisien bisa bilangan bulat, pecahan, desimal, positif, atau negatif. Koefisien memberitahu "berapa kali" variabel itu dihitung.

### 6. Suku dalam Bentuk Aljabar

**Suku** (*term*) adalah bagian bentuk aljabar yang dipisahkan oleh tanda $+$ atau $-$. Tanda di depan sebuah suku ikut menjadi bagian suku itu.

Bentuk $3x^2 - 5x + 2$ memiliki tiga suku:

| Suku | Koefisien | Variabel | Keterangan |
|---|---|---|---|
| $3x^2$ | $3$ | $x^2$ | suku berpangkat dua |
| $-5x$ | $-5$ | $x$ | tanda minus ikut suku |
| $2$ | | | suku **konstan** (tanpa variabel) |

Perhatikan: pada $3x^2 - 5x + 2$, suku kedua adalah $-5x$ (bukan $5x$). Karena pengurangan sama dengan penjumlahan lawan, bentuk ini sama dengan $3x^2 + (-5x) + 2$.

Banyaknya suku memberi nama sehari-hari: satu suku ($7x$), dua suku ($x + 4$), tiga suku ($x^2 + 2x + 1$). Bentuk dengan banyak suku disebut **suku banyak** (polinomial), yang dibahas di Part 9.

**Suku sejenis** adalah suku-suku dengan bagian variabel yang **sama persis**: $3x$ dan $-8x$ sejenis; $3x$ dan $3x^2$ tidak sejenis; $3x$ dan $3y$ tidak sejenis. Suku sejenis dijumlahkan di Part 2.

### 7. Ekspresi Aljabar

**Ekspresi aljabar** (*algebraic expression*) adalah rangkaian bilangan, variabel, dan operasi (tambah, kurang, kali, bagi, pangkat) **tanpa tanda sama dengan** dan tanpa tanda perbandingan.

Contoh ekspresi: $2x + 3$, $a^2 - b^2$, $\dfrac{p + q}{2}$.

Ekspresi bukan kalimat yang benar atau salah, melainkan "kata benda" yang menyatakan sebuah nilai (setelah variabelnya diisi).

**Menerjemahkan kalimat menjadi ekspresi.** Perhatikan kata kuncinya dan urutan:

| Kalimat | Ekspresi | Catatan |
|---|---|---|
| lima lebih banyak dari $n$ | $n + 5$ | |
| $3x$ dikurangi 5 | $3x - 5$ | |
| **5 kurang dari** $3x$ | $3x - 5$ | urutan terbalik dibanding kalimatnya |
| dua kali jumlah $a$ dan $b$ | $2(a + b)$ | kurung wajib |
| dua kali $a$, ditambah $b$ | $2a + b$ | tanpa kurung |
| kuadrat dari selisih $p$ dan $q$ | $(p - q)^2$ | selisih dulu, baru dikuadratkan |
| sepertiga dari $x$, ditambah 4 | $\dfrac{x}{3} + 4$ | |

!!! warning "Hati-hati dengan 'kurang dari'"
    "5 kurang dari $3x$" berarti $3x - 5$, **bukan** $5 - 3x$. Bandingkan "5 kurang dari 12" yang bernilai $12 - 5 = 7$.

### 8. Perbedaan Ekspresi dan Persamaan

| Jenis | Ciri | Contoh | Benar atau salah? |
|---|---|---|---|
| **Ekspresi** | tanpa tanda $=$ | $2x + 3$ | tidak berlaku |
| **Persamaan** (*equation*) | dua ekspresi dengan $=$, benar hanya untuk **nilai tertentu** | $2x + 3 = 11$ | benar untuk $x = 4$ saja |
| **Identitas** (*identity*) | dua ekspresi dengan $=$, benar untuk **semua** nilai | $2(x + 1) = 2x + 2$ | benar untuk semua $x$ |
| **Pertidaksamaan** (*inequality*) | memakai $<$, $>$, $\leq$, $\geq$ | $x > 3$ | benar untuk banyak nilai |

Menyelesaikan persamaan berarti mencari nilai variabel yang membuatnya benar. Identitas tidak "diselesaikan"; ia hanya dibuktikan benar. Contoh: $2(x + 1) = 2x + 2$. Cek $x = 5$: sisi kiri $2 \times 6 = 12$, sisi kanan $10 + 2 = 12$. Cek $x = -3$: kiri $2(-2) = -4$, kanan $-6 + 2 = -4$. Keduanya sama untuk setiap nilai $x$, karena itulah sifat distributif.

### 9. Substitusi Nilai ke dalam Ekspresi

**Substitusi** adalah mengganti variabel dengan bilangan lalu menghitung. Hasilnya disebut **nilai ekspresi**.

**Prosedur:**

1. Tulis ulang ekspresi, ganti setiap huruf dengan bilangannya dalam **kurung**.
2. Hitung sesuai urutan operasi (kurung, pangkat, kali-bagi, tambah-kurang).
3. Tulis hasilnya.

**Contoh dua variabel.** Luas persegi panjang $L = p \times l$. Untuk $p = 5$ dan $l = 3$: $L = 5 \times 3 = 15$.

**Contoh dengan negatif.** Hitung $3a^2 - 2b$ untuk $a = -2$ dan $b = 5$:

1. Tulis dalam kurung: $3(-2)^2 - 2(5)$.
2. Pangkat dulu: $(-2)^2 = 4$, jadi $3 \times 4 - 2 \times 5$.
3. Kali: $12 - 10$.
4. Hasil: $2$.

Kurung di langkah 1 penting. Jika $-2$ ditulis tanpa kurung, $-2^2$ akan terbaca $-4$ dan hasilnya salah.

### 10. Membaca Notasi Aljabar

Aljabar punya aturan penulisan ringkas:

- **Kali tidak ditulis.** $2x$ berarti $2 \times x$, dan $xy$ berarti $x \times y$. Angka ditulis di depan huruf.
- **Pangkat hanya berlaku pada yang tepat di depannya.** $2x^2$ berarti $2 \times (x \times x)$, sedangkan $(2x)^2$ berarti $(2x) \times (2x)$.
- **Tanda minus dan pangkat.** $-x^2$ berarti $-(x^2)$, sedangkan $(-x)^2 = x^2$.
- **Garis pecahan adalah kurung tak terlihat.** $\dfrac{a + b}{2}$ berarti $(a + b) \div 2$.
- **Tulisan $x/2$ dan $\dfrac{1}{2}x$** sama nilainya: $x$ dibagi 2.
- **Titik** $a \cdot b$ juga berarti kali, dipakai agar tidak tertukar dengan huruf $x$.

Contoh dengan $x = -3$:

| Ekspresi | Perhitungan | Hasil |
|---|---|---|
| $-x^2$ | $-(-3)^2 = -9$ | $-9$ |
| $(-x)^2$ | $(3)^2$ | $9$ |
| $2x^2$ | $2 \times (-3)^2 = 2 \times 9$ | $18$ |
| $(2x)^2$ | $(-6)^2$ | $36$ |

Empat ekspresi yang tampak mirip menghasilkan empat nilai berbeda. Karena itu, baca notasi dengan teliti.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Aljabar (*algebra*) | menulis aturan umum dengan huruf | $\text{total} = 5.000n$ |
| Variabel (*variable*) | huruf pengganti bilangan yang berubah atau belum diketahui | $x$, $n$, $t$ |
| Konstanta (*constant*) | nilai yang tetap | $2.000$ pada $2.000 + 1.000t$ |
| Koefisien (*coefficient*) | bilangan pengali variabel | $-5$ pada $-5x$ |
| Suku (*term*) | bagian yang dipisah $+$ atau $-$ | $3x^2$, $-5x$, $2$ |
| Suku konstan | suku tanpa variabel | $2$ |
| Suku sejenis | suku dengan bagian variabel yang sama persis | $3x$ dan $-8x$ |
| Ekspresi (*expression*) | rangkaian bilangan, variabel, operasi tanpa $=$ | $2x + 3$ |
| Persamaan (*equation*) | dua ekspresi sama, benar untuk nilai tertentu | $2x + 3 = 11$ |
| Identitas (*identity*) | dua ekspresi sama untuk semua nilai | $2(x+1) = 2x + 2$ |
| Substitusi | mengganti variabel dengan bilangan | $x = 4$ |
| Nilai ekspresi | hasil setelah substitusi | $2(4) + 3 = 11$ |

## Rumus dan Penjelasan Setiap Simbol

**Bentuk umum sebuah suku:**

$$
k\,x^n
$$

- $k$ adalah **koefisien**, bilangan sembarang.
- $x$ adalah **variabel**.
- $n$ adalah **pangkat** (eksponen), bilangan cacah ($0, 1, 2, \ldots$).
- Contoh: $-5x$ memiliki $k = -5$ dan $n = 1$; suku konstan $2$ dapat ditulis $2x^0$ karena $x^0 = 1$ (Level 0).

**Nilai ekspresi:** jika $E$ adalah sebuah ekspresi dalam variabel $x$, maka "nilai $E$ untuk $x = c$" dihitung dengan mengganti setiap $x$ dengan $c$. Berlaku untuk bilangan $c$ yang membuat semua operasi dalam $E$ terdefinisi (misalnya tidak ada pembagian dengan nol).

**Aturan pola contoh:** bilangan ke-$n$ pada pola $3, 5, 7, \ldots$ adalah $2n + 1$. Di sini $n$ adalah bilangan asli ($1, 2, 3, \ldots$) yang menunjukkan nomor urut.

## Penjelasan Langkah demi Langkah

Cara menerjemahkan kalimat menjadi ekspresi:

1. **Tentukan besaran yang tidak diketahui atau berubah**, lalu beri nama huruf. Tulis artinya ("misalkan $n$ adalah banyak buku").
2. **Pisahkan kalimat menjadi potongan** menurut operasi: "dua kali", "jumlah", "kurang dari".
3. **Tulis tiap potongan sebagai ekspresi kecil**, lalu gabungkan. Gunakan kurung bila sebuah potongan harus dihitung lebih dulu.
4. **Uji dengan angka.** Isi nilai sederhana pada ekspresi dan cek apakah sesuai kalimat.

## Contoh Soal dan Pembahasan

### Contoh 1 — Menerjemahkan kalimat

Tulis ekspresi untuk "5 kurang dari tiga kali sebuah bilangan", lalu cek untuk bilangan 4.

1. Misalkan bilangannya $x$.
2. "Tiga kali sebuah bilangan" adalah $3x$.
3. "5 kurang dari" $3x$ berarti $3x$ dikurangi 5, yaitu $3x - 5$.
4. Cek dengan $x = 4$: tiga kali 4 adalah 12, dan 5 kurang dari 12 adalah 7. Ekspresi: $3(4) - 5 = 12 - 5 = 7$.

### Contoh 2 — Mengenali bagian ekspresi

Pada $5x^2 - 3x + 7$, sebutkan variabel, suku, koefisien, dan suku konstan.

1. Variabel: $x$.
2. Suku (pisahkan dengan tanda): $5x^2$, $-3x$, dan $7$.
3. Koefisien: $5$ (untuk $x^2$) dan $-3$ (untuk $x$).
4. Suku konstan: $7$.

### Contoh 3 — Substitusi dua variabel

Hitung $\dfrac{a^2 + 3b}{2}$ untuk $a = -4$ dan $b = 2$.

1. Substitusi dengan kurung: $\dfrac{(-4)^2 + 3(2)}{2}$.
2. Pembilang: $(-4)^2 = 16$ dan $3 \times 2 = 6$, jadi $16 + 6 = 22$.
3. Bagi 2: $\dfrac{22}{2} = 11$.

### Contoh 4 — Ekspresi, persamaan, atau identitas?

Klasifikasikan: (a) $4(x - 1)$ (b) $4(x - 1) = 12$ (c) $4(x - 1) = 4x - 4$.

1. (a) tidak ada tanda $=$, jadi **ekspresi**.
2. (b) ada $=$. Benar hanya jika $4(x - 1) = 12$, yaitu $x - 1 = 3$, jadi $x = 4$. Ini **persamaan**.
3. (c) ada $=$. Dengan sifat distributif, $4(x - 1) = 4x - 4$ benar untuk semua $x$ (cek $x = 0$: $-4 = -4$; $x = 10$: $36 = 36$). Ini **identitas**.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** Koefisien pada $-y$ adalah $-1$. Pada $\dfrac{y}{4}$ adalah $\dfrac{1}{4}$.

**Sedang.** Hitung $C = 2.000 + 1.000t$ untuk $t = 3$: $2.000 + 1.000(3) = 5.000$ (rupiah). Konstanta $2.000$ adalah biaya awal, dan koefisien $1.000$ adalah tambahan per jam.

**Sedang.** Hitung $2x^2$ dan $(2x)^2$ untuk $x = 3$. $2(3)^2 = 2 \times 9 = 18$ dan $(2 \cdot 3)^2 = 6^2 = 36$.

**Menantang.** Pola $4, 7, 10, 13, \ldots$ naik 3 setiap kali. Cobalah $3n + 1$: $n = 1$ memberi 4, $n = 2$ memberi 7, $n = 4$ memberi 13. Cocok. Bilangan ke-20 adalah $3(20) + 1 = 61$.

## Visualisasi atau Interpretasi Grafik

Hubungan biaya parkir $C = 2.000 + 1.000t$ dapat dilihat sebagai grafik garis. Setiap kenaikan 1 jam menambah 1.000 rupiah, sehingga garisnya lurus. Grafik fungsi dipelajari di Part 5 dan 8, jadi di sini cukup tabel nilai:

| $t$ (jam) | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| $C$ (rupiah) | 2.000 | 3.000 | 4.000 | 5.000 | 6.000 |

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| "5 kurang dari $3x$" ditulis $5 - 3x$ | urutan "kurang dari" terbalik | $3x - 5$ |
| Menganggap $-x^2 = (-x)^2$ | pangkat hanya mengenai $x$ | $-x^2 = -(x^2)$; $(-x)^2 = x^2$ |
| Mengira koefisien $x$ adalah 0 | $x = 1 \times x$ | koefisiennya 1 |
| Membaca $2x$ untuk $x = 5$ sebagai "25" (angka digabung) | $2x$ berarti $2 \times x$ | $2 \times 5 = 10$ |
| Lupa kurung saat substitusi negatif | tanda minus ikut dikuadratkan atau tidak | $3(-2)^2 = 12$, bukan $-12$ |
| Menyebut $2x + 3 = 11$ sebagai ekspresi | ada tanda $=$ | itu persamaan |
| Menganggap huruf yang sama selalu berarti hal yang sama di soal berbeda | variabel bebas dipilih per konteks | baca konteks soal |
| Menyatakan $2(x+1) = 2x + 2$ diselesaikan menjadi $x = \ldots$ | identitas benar untuk semua $x$ | tidak ada nilai $x$ yang "dicari" |

## Hubungan dengan Machine Learning

- **Masukan dan keluaran.** Pada $\hat{y} = wx + b$, $x$ adalah masukan (variabel), $\hat{y}$ adalah keluaran, dan $w$, $b$ adalah **parameter** yang berperan seperti konstanta saat model dipakai, tetapi diubah saat model belajar.
- **Banyak variabel.** Model nyata memakai beberapa masukan: $\hat{y} = w_1 x_1 + w_2 x_2 + b$ (angka kecil di kanan bawah adalah subskrip yang menandai nomor).
- **Substitusi = prediksi.** Setiap kali model memprediksi, komputer mengganti $x$ dengan data lalu menghitung ekspresi.
- **Konteks menentukan peran.** Saat memakai model, $w$ dan $b$ tetap. Saat melatih model, $w$ dan $b$ yang dicari. Huruf yang sama, peran berbeda, persis seperti di bagian variabel.

Catatan: contoh di bawah memakai angka fiktif. Pelatihan model dibahas jauh di level berikutnya.

### Python: ekspresi, substitusi, dan tabel nilai

Variabel di Python menyimpan bilangan. Python tidak mengenal penulisan `2x`; operator kali `*` wajib ditulis.

```python
a = -2
b = 5
nilai = 3 * a ** 2 - 2 * b
print(nilai)   # 2
```

Python mengikuti urutan operasi: `a ** 2` (pangkat) dikerjakan dulu, lalu `3 * ...`. Untuk memeriksa perbedaan notasi:

```python
x = -3
print(-x ** 2)     # -9, artinya -(x ** 2)
print((-x) ** 2)   # 9
print(2 * x ** 2)  # 18
print((2 * x) ** 2) # 36
```

Fungsi `def` menulis ekspresi sebagai aturan yang bisa dipakai berulang. Berikut biaya parkir $C = 2.000 + 1.000t$:

```python
def biaya(t):
    return 2000 + 1000 * t

for t in [0, 1, 2, 3, 4]:
    print(t, biaya(t))
# 0 2000
# 1 3000
# 2 4000
# 3 5000
# 4 6000
```

Dan model fiktif $\hat{y} = 3x + 10$:

```python
w = 3
b = 10
x = 5
y_topi = w * x + b
print(y_topi)   # 25
```

Baris `y_topi = w * x + b` adalah substitusi: ganti `w`, `x`, `b` dengan nilainya, hitung, lalu simpan hasilnya.

## Latihan Mandiri

1. Pada $7a - 4b^2 + 9$, sebutkan semua suku, koefisien $a$, koefisien $b^2$, suku konstan, dan variabelnya.
2. Tiket konser Rp50.000 per orang. Jika $n$ adalah banyak tiket dan $T$ adalah total harga, tulis rumusnya. Mana yang variabel dan mana yang konstanta?
3. Terjemahkan: (a) 5 kurang dari $3x$ (b) dua kali jumlah $a$ dan $b$ (c) kuadrat dari selisih $p$ dan $q$ (d) sepertiga dari $x$ ditambah 4
4. Klasifikasikan (ekspresi, persamaan, identitas, atau pertidaksamaan): (a) $3x + 2$ (b) $3x + 2 = 11$ (c) $2(x + 1) = 2x + 2$ (d) $x > 3$ (e) $4(x - 1)$
5. Hitung $3a^2 - 2b$ untuk (a) $a = -2$, $b = 5$ (b) $a = 1$, $b = -4$.
6. Luas persegi panjang $L = p \times l$ dan keliling $K = 2(p + l)$. Hitung $L$ dan $K$ untuk $p = 12$ dan $l = 7{,}5$.
7. Untuk $x = -3$, hitung $-x^2$, $(-x)^2$, $2x^2$, dan $(2x)^2$.
8. Pola $4, 7, 10, 13, \ldots$: tulis rumus bilangan ke-$n$ dan hitung bilangan ke-20.
9. Biaya parkir $C = 2.000 + 1.000t$. Hitung $C$ untuk $t = 0$, $t = 1$, dan $t = 4$. Apa arti angka $2.000$ dan $1.000$?
10. Model fiktif $\hat{y} = 3x + 10$. Hitung $\hat{y}$ untuk $x = 5$ dan $x = 0$. Apa arti angka 3 dan 10?

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    Suku: $7a$, $-4b^2$, dan $9$ (tanda minus ikut suku kedua). Koefisien $a$ adalah $7$. Koefisien $b^2$ adalah $-4$. Suku konstan: $9$. Variabel: $a$ dan $b$.

??? success "Soal 2"
    $T = 50.000 \times n$. Konstanta: $50.000$ (harga per tiket tetap). Variabel: $n$ (banyak tiket bisa berubah) dan $T$ (total ikut berubah).

??? success "Soal 3"
    (a) $3x - 5$. (b) $2(a + b)$. (c) $(p - q)^2$. (d) $\dfrac{x}{3} + 4$.

??? success "Soal 4"
    (a) ekspresi. (b) persamaan (benar hanya untuk $x = 3$). (c) identitas (distributif). (d) pertidaksamaan. (e) ekspresi.

??? success "Soal 5"
    (a) $3(-2)^2 - 2(5) = 3 \times 4 - 10 = 12 - 10 = 2$.
    (b) $3(1)^2 - 2(-4) = 3 + 8 = 11$ (mengurangi $-8$ sama dengan menambah 8).

??? success "Soal 6"
    $L = 12 \times 7{,}5 = 90$. $K = 2(12 + 7{,}5) = 2 \times 19{,}5 = 39$.

??? success "Soal 7"
    $-x^2 = -(-3)^2 = -9$. $(-x)^2 = (3)^2 = 9$. $2x^2 = 2 \times 9 = 18$. $(2x)^2 = (-6)^2 = 36$.

??? success "Soal 8"
    Pola naik 3 setiap kali, jadi mulai dari $3n$: $3, 6, 9, 12$. Hasil yang dibutuhkan lebih besar 1, jadi rumusnya $3n + 1$. Cek: $n = 1$ memberi 4, $n = 4$ memberi 13. Bilangan ke-20: $3(20) + 1 = 61$.

??? success "Soal 9"
    $t = 0$: $2.000$. $t = 1$: $3.000$. $t = 4$: $2.000 + 4.000 = 6.000$. Angka $2.000$ adalah biaya tetap (muncul bahkan saat $t = 0$). Angka $1.000$ adalah tambahan biaya per jam.

??? success "Soal 10"
    $x = 5$: $3(5) + 10 = 25$. $x = 0$: $10$. Angka 3 adalah koefisien: setiap kenaikan $x$ sebesar 1, $\hat{y}$ naik 3. Angka 10 adalah nilai $\hat{y}$ saat $x = 0$.

## Ringkasan Materi

- Aljabar memakai huruf untuk menulis aturan dan pola yang berlaku umum.
- Variabel mewakili bilangan yang berubah atau belum diketahui; konstanta bernilai tetap; peran huruf ditentukan konteks.
- Koefisien adalah pengali variabel (termasuk 1 dan $-1$ yang tidak ditulis). Suku dipisahkan oleh tanda $+$ atau $-$, dan tanda ikut suku.
- Ekspresi tidak punya tanda $=$. Persamaan benar hanya untuk nilai tertentu. Identitas benar untuk semua nilai.
- Substitusi: ganti variabel dengan bilangan dalam kurung, lalu ikuti urutan operasi.
- Notasi: $2x = 2 \times x$; $-x^2 \neq (-x)^2$; $2x^2 \neq (2x)^2$; garis pecahan berfungsi sebagai kurung.

## Checklist Penguasaan

- [ ] Menjelaskan beda variabel sebagai nilai yang berubah dan sebagai nilai yang dicari.
- [ ] Menyebut koefisien, suku, dan suku konstan pada bentuk aljabar.
- [ ] Menerjemahkan kalimat (termasuk "kurang dari") menjadi ekspresi.
- [ ] Membedakan ekspresi, persamaan, identitas, dan pertidaksamaan.
- [ ] Mensubstitusi nilai negatif dan dua variabel dengan benar.
- [ ] Membedakan $-x^2$, $(-x)^2$, $2x^2$, dan $(2x)^2$.
- [ ] Menulis ekspresi sederhana sebagai fungsi Python.

## Prasyarat untuk Materi Berikutnya

[Part 2 — Operasi Bentuk Aljabar](02-operasi-bentuk-aljabar.md) membutuhkan: mengenali suku dan koefisien, memahami suku sejenis, dan mensubstitusi nilai untuk memeriksa hasil penyederhanaan. Pastikan semua butir di checklist di atas sudah terpenuhi.
