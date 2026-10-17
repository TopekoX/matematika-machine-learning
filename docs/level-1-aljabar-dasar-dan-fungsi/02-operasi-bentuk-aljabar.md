# Part 2 — Operasi Bentuk Aljabar

## Tujuan Pembelajaran

Setelah mempelajari bagian ini, kamu mampu:

- menjelaskan mengapa hanya suku sejenis yang boleh digabung;
- menjumlah, mengurang, mengalikan, dan membagi bentuk aljabar sederhana;
- memakai sifat distributif dan menghilangkan tanda kurung dengan benar;
- menyederhanakan ekspresi aljabar langkah demi langkah;
- mengalikan dua bentuk linear dan memfaktorkan bentuk sederhana;
- memeriksa hasil aljabar dengan substitusi angka dan mengenali kesalahan umum.

## Prasyarat

[Part 1 — Mengenal Aljabar](01-mengenal-aljabar.md) (suku, koefisien, suku sejenis, substitusi) serta [Level 0 — Part 2](../level-0-fondasi-matematika/02-operasi-aritmatika-dasar.md) (sifat komutatif, asosiatif, distributif, bilangan negatif) dan [Part 5](../level-0-fondasi-matematika/05-pangkat-dan-akar.md) (aturan pangkat).

## Mengapa Materi Ini Penting?

Menyederhanakan ekspresi adalah keterampilan yang dipakai di hampir setiap turunan rumus Machine Learning. Misalnya, rumus kesalahan kuadrat $(\hat{y} - y)^2$ harus dijabarkan sebelum diubah lebih jauh. Selain itu, aljabar yang rapi mengurangi peluang salah hitung: dua ekspresi yang tampak berbeda sering ternyata sama.

## Konsep Dasar dengan Bahasa Sederhana

### 1. Menggabungkan Suku Sejenis

**Suku sejenis** adalah suku yang **bagian variabelnya sama persis**. Hanya suku sejenis yang boleh dijumlahkan atau dikurangkan menjadi satu suku.

Mulai dari analogi: 3 apel ditambah 2 apel menjadi 5 apel. Namun 3 apel ditambah 2 jeruk tidak menjadi "5 apeljeruk"; hasilnya tetap "3 apel dan 2 jeruk". Dalam aljabar, $x$ adalah "apel" dan $y$ adalah "jeruk".

$$
3x + 2x = 5x \qquad \text{tetapi} \qquad 3x + 2y \text{ tidak dapat disederhanakan}
$$

**Alasan yang tepat** memakai sifat distributif (Level 0, Part 2) secara terbalik:

$$
3x + 2x = (3 + 2)x = 5x
$$

Faktor $x$ yang sama ditarik keluar, lalu koefisiennya dijumlahkan. Pada $3x + 2y$, tidak ada faktor yang sama yang bisa ditarik keluar, jadi tidak ada penyederhanaan.

**Bukti dengan angka.** Ambil $x = 4$ dan $y = 5$:

- $3x + 2y = 12 + 10 = 22$.
- Jika disederhanakan menjadi $5xy$: $5 \times 4 \times 5 = 100$. Salah.
- Jika disederhanakan menjadi $5x$: $5 \times 4 = 20$. Salah.

Hasil bernilai 22 hanya benar untuk bentuk aslinya $3x + 2y$.

**Pangkat berbeda berarti tidak sejenis.** $3x$ dan $2x^2$ tidak sejenis. Dengan $x = 2$: $3x + 2x^2 = 6 + 8 = 14$, sedangkan $5x^2 = 20$ dan $5x^3 = 40$. Tidak ada yang cocok.

Cara menggabungkan: kelompokkan suku sejenis, jumlahkan koefisiennya (bawa tandanya), biarkan bagian variabel.

$$
6y^2 + 2y - 4y^2 + y = (6 - 4)y^2 + (2 + 1)y = 2y^2 + 3y
$$

Perhatikan suku $y$ yang koefisiennya 1 (tidak ditulis).

### 2. Penjumlahan Bentuk Aljabar

Menjumlahkan dua bentuk aljabar berarti menghilangkan kurung lalu menggabungkan suku sejenis. Tanda $+$ di depan kurung **tidak mengubah** tanda isi kurung.

$$
(3x + 2) + (5x - 7) = 3x + 2 + 5x - 7 = 8x - 5
$$

Langkah: suku $x$: $3x + 5x = 8x$; konstanta: $2 - 7 = -5$.

### 3. Pengurangan Bentuk Aljabar

Tanda $-$ di depan kurung **membalik tanda semua suku** di dalamnya, karena mengurangi sama dengan menjumlahkan lawan (Level 0, Part 2):

$$
(3x + 2) - (5x - 7) = 3x + 2 - 5x + 7 = -2x + 9
$$

Suku $-7$ di dalam kurung berubah menjadi $+7$. **Semua** suku dibalik, bukan hanya yang pertama.

Cek dengan $x = 1$: bentuk asal $(3 + 2) - (5 - 7) = 5 - (-2) = 7$. Bentuk hasil: $-2(1) + 9 = 7$. Cocok.

### 4. Perkalian Bentuk Aljabar

Perkalian suku tunggal: kalikan koefisien dengan koefisien, lalu variabel sejenis dengan aturan pangkat $a^m \times a^n = a^{m+n}$ (Level 0, Part 5).

- $3x \times 4x = (3 \times 4)(x \times x) = 12x^2$.
- $(-2a)(5a^2) = (-2 \times 5)(a \times a^2) = -10a^3$.
- $2x \times 3y = 6xy$.
- $(-3a)(-2b) = 6ab$ (negatif kali negatif).

Alasan di balik $3x \times 4x = 12x^2$: perkalian bersifat komutatif dan asosiatif, jadi urutannya boleh ditata ulang: $3 \times x \times 4 \times x = (3 \times 4) \times (x \times x)$.

### 5. Pembagian Bentuk Aljabar Sederhana

Bagi koefisien dengan koefisien, dan variabel dengan aturan $\dfrac{a^m}{a^n} = a^{m-n}$.

- $\dfrac{12x^2}{3x} = 4x$ (karena $\dfrac{12}{3} = 4$ dan $\dfrac{x^2}{x} = x$).
- $\dfrac{15a^3 b}{5ab} = 3a^2$.

Syarat: pembagi tidak boleh nol, jadi pada contoh pertama $x \neq 0$.

Membagi penjumlahan dengan sebuah bilangan: setiap suku dibagi. $\dfrac{6x + 9}{3} = \dfrac{6x}{3} + \dfrac{9}{3} = 2x + 3$.

!!! warning "Hanya pembilang yang boleh dipecah"
    $\dfrac{a + b}{c} = \dfrac{a}{c} + \dfrac{b}{c}$, tetapi $\dfrac{c}{a + b} \neq \dfrac{c}{a} + \dfrac{c}{b}$. Contoh angka: $\dfrac{12}{2 + 4} = 2$, sedangkan $\dfrac{12}{2} + \dfrac{12}{4} = 6 + 3 = 9$.

### 6. Sifat Distributif

Mulai dari angka. $3 \times (10 + 4)$: cara langsung $3 \times 14 = 42$. Cara distributif: $3 \times 10 + 3 \times 4 = 30 + 12 = 42$. Hasilnya sama.

Dalam aljabar, huruf menggantikan angka:

$$
a(b + c) = ab + ac, \qquad a(b - c) = ab - ac
$$

Artinya pengali di luar kurung **dikalikan ke setiap suku** di dalam kurung.

Contoh:

- $3(x + 4) = 3x + 12$.
- $-2(x - 5) = -2x + 10$ (perhatikan: $-2 \times (-5) = +10$).
- $x(x + 3) = x^2 + 3x$.

Pandangan luas: persegi panjang dengan lebar $3$ dan panjang $x + 4$ terdiri dari dua bagian, yaitu $3 \times x$ dan $3 \times 4$.

### 7. Menghilangkan Tanda Kurung

Aturan menghilangkan kurung:

1. Tanda $+$ atau tidak ada tanda di depan kurung: isi kurung ditulis apa adanya.
2. Tanda $-$ di depan kurung: semua tanda isi dibalik.
3. Bilangan atau variabel di depan kurung: kalikan ke setiap suku (distributif).

Untuk kurung bersarang, mulai dari **kurung paling dalam**:

$$
2\bigl[3x - (x - 4)\bigr] = 2\bigl[3x - x + 4\bigr] = 2\bigl[2x + 4\bigr] = 4x + 8
$$

Langkah: (a) kurung dalam $-(x - 4)$ menjadi $-x + 4$; (b) gabungkan $3x - x = 2x$; (c) bagikan 2.

### 8. Menyederhanakan Ekspresi Aljabar

Prosedur umum:

1. **Hilangkan kurung** (distributif, balik tanda bila ada minus).
2. **Kelompokkan suku sejenis.**
3. **Jumlahkan koefisien** tiap kelompok.
4. **Periksa** dengan substitusi angka.

Contoh: sederhanakan $2(x + 3) - 3(x - 1)$.

$$
\begin{aligned}
2(x + 3) - 3(x - 1) &= 2x + 6 - 3x + 3 \\
&= (2x - 3x) + (6 + 3) \\
&= -x + 9
\end{aligned}
$$

Cek $x = 2$: bentuk asal $2(5) - 3(1) = 10 - 3 = 7$; bentuk hasil $-2 + 9 = 7$. Cocok.

### 9. Perkalian Dua Bentuk Linear

**Bentuk linear** adalah bentuk aljabar dengan variabel berpangkat paling tinggi satu, misalnya $x + 2$. Mengalikan dua bentuk linear memakai distributif **dua kali**: setiap suku di kurung pertama dikalikan ke setiap suku di kurung kedua.

$$
(x + 2)(x + 3) = x \cdot x + x \cdot 3 + 2 \cdot x + 2 \cdot 3 = x^2 + 3x + 2x + 6 = x^2 + 5x + 6
$$

Pandangan luas persegi panjang dengan sisi $x + 2$ dan $x + 3$:

| | $x$ | $3$ |
|---|---|---|
| **$x$** | $x^2$ | $3x$ |
| **$2$** | $2x$ | $6$ |

Jumlahkan keempat sel: $x^2 + 3x + 2x + 6 = x^2 + 5x + 6$.

Contoh dengan tanda minus: $(2x + 1)(x - 3) = 2x^2 - 6x + x - 3 = 2x^2 - 5x - 3$. Cek $x = 1$: bentuk asal $3 \times (-2) = -6$; hasil $2 - 5 - 3 = -6$. Cocok.

Tiga bentuk khusus yang sering muncul:

$$
(a + b)^2 = a^2 + 2ab + b^2, \qquad (a - b)^2 = a^2 - 2ab + b^2, \qquad (a - b)(a + b) = a^2 - b^2
$$

Misalnya $(x + 3)^2 = x^2 + 6x + 9$, dan $(x - 4)(x + 4) = x^2 - 16$.

!!! warning "Kuadrat jumlah bukan jumlah kuadrat"
    $(x + 3)^2 \neq x^2 + 9$. Suku tengah $6x$ tidak boleh hilang. Dengan $x = 1$: $(1 + 3)^2 = 16$, sedangkan $1^2 + 9 = 10$.

### 10. Faktorisasi Dasar

**Faktorisasi** (memfaktorkan) adalah kebalikan dari menjabarkan: mengubah penjumlahan suku menjadi perkalian faktor.

**a. Mengeluarkan faktor persekutuan.** Cari pembagi bersama terbesar dari semua suku, lalu tarik keluar.

- $6x + 9 = 3(2x + 3)$ (FPB dari 6 dan 9 adalah 3).
- $4x^2 - 8x = 4x(x - 2)$ (faktor bersama: 4 dan $x$).

Periksa dengan menjabarkan: $3(2x + 3) = 6x + 9$.

**b. Bentuk $x^2 + bx + c$.** Cari dua bilangan yang **hasil kalinya $c$** dan **jumlahnya $b$**. Contoh $x^2 + 5x + 6$: pasangan dengan hasil kali 6 adalah $(1, 6)$ dan $(2, 3)$. Yang berjumlah 5 adalah $2$ dan $3$. Maka $x^2 + 5x + 6 = (x + 2)(x + 3)$.

Contoh dengan minus: $x^2 - 6x + 8$ butuh hasil kali $8$ dan jumlah $-6$. Pasangannya $-2$ dan $-4$. Maka $(x - 2)(x - 4)$.

**c. Selisih dua kuadrat.** $a^2 - b^2 = (a - b)(a + b)$. Contoh $x^2 - 16 = (x - 4)(x + 4)$.

Teknik ini terbatas pada bentuk sederhana dengan bilangan bulat. Bentuk lain dibahas di materi lanjut.

### 11. Kesalahan Umum dalam Operasi Aljabar

Daftar lengkap ada di bagian [Kesalahan Umum](#kesalahan-umum) di bawah.

## Istilah yang Perlu Dipahami

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| Suku sejenis (*like terms*) | suku dengan bagian variabel yang sama persis | $3x$ dan $-8x$ |
| Menjabarkan (*expand*) | menghilangkan kurung dengan distributif | $3(x + 4) = 3x + 12$ |
| Menyederhanakan (*simplify*) | menulis ekspresi dalam bentuk paling ringkas | $2x + 6 - 3x + 3 = -x + 9$ |
| Bentuk linear | variabel berpangkat paling tinggi satu | $2x + 1$ |
| Memfaktorkan (*factor*) | menulis jumlah sebagai hasil kali | $6x + 9 = 3(2x + 3)$ |
| Faktor persekutuan | faktor yang sama pada semua suku | $3$ pada $6x + 9$ |
| Selisih dua kuadrat | bentuk $a^2 - b^2$ | $x^2 - 16$ |

## Rumus dan Penjelasan Setiap Simbol

Pada semua rumus, $a$, $b$, $c$, dan $d$ adalah bilangan atau ekspresi sembarang. Semua rumus berikut adalah **identitas**: benar untuk semua nilai.

| Nama | Rumus | Keterangan |
|---|---|---|
| Distributif | $a(b + c) = ab + ac$ | pengali dibagikan ke tiap suku |
| Menggabungkan suku sejenis | $ax + bx = (a + b)x$ | distributif terbalik |
| Perkalian dua suku dua | $(a + b)(c + d) = ac + ad + bc + bd$ | empat hasil kali |
| Kuadrat jumlah | $(a + b)^2 = a^2 + 2ab + b^2$ | |
| Kuadrat selisih | $(a - b)^2 = a^2 - 2ab + b^2$ | |
| Selisih dua kuadrat | $(a - b)(a + b) = a^2 - b^2$ | |

Cara memakai: ganti $a$ dan $b$ dengan bilangan atau ekspresi, lalu hitung kedua sisi. Dengan $a = 5$ dan $b = 3$:

- $(a + b)^2 = 8^2 = 64$ dan $a^2 + 2ab + b^2 = 25 + 30 + 9 = 64$.
- $(a - b)(a + b) = 2 \times 8 = 16$ dan $a^2 - b^2 = 25 - 9 = 16$.

## Penjelasan Langkah demi Langkah

Cara memeriksa hasil aljabar dengan angka:

1. Pilih nilai sederhana untuk variabel (misalnya $x = 2$ atau $x = -1$). Hindari 0 dan 1 saja karena bisa kebetulan sama.
2. Hitung ekspresi **awal** dengan nilai itu.
3. Hitung ekspresi **hasil** dengan nilai yang sama.
4. Jika berbeda, ada kesalahan. Jika sama, hasil kemungkinan benar (uji dengan satu nilai lagi untuk lebih yakin).

Cara ini tidak membuktikan identitas, tetapi sangat efektif menangkap kesalahan.

## Contoh Soal dan Pembahasan

### Contoh 1 — Menyederhanakan dengan dua kurung

Sederhanakan $3(2x - 1) - 2(x + 4)$.

1. Jabarkan $3(2x - 1)$: $6x - 3$.
2. Jabarkan $-2(x + 4)$: $-2x - 8$ (minus ikut dikalikan).
3. Gabungkan: $6x - 3 - 2x - 8$.
4. Suku $x$: $6x - 2x = 4x$. Konstanta: $-3 - 8 = -11$.
5. Hasil: $4x - 11$.
6. Cek $x = 2$: awal $3(3) - 2(6) = 9 - 12 = -3$; hasil $8 - 11 = -3$. Cocok.

### Contoh 2 — Perkalian dua bentuk linear

Hitung $(x - 2)(x + 7)$.

1. Kalikan $x$ dengan $x$ dan $7$: $x^2 + 7x$.
2. Kalikan $-2$ dengan $x$ dan $7$: $-2x - 14$.
3. Gabungkan: $x^2 + 7x - 2x - 14 = x^2 + 5x - 14$.
4. Cek $x = 3$: awal $(1)(10) = 10$; hasil $9 + 15 - 14 = 10$. Cocok.

### Contoh 3 — Memfaktorkan

Faktorkan $x^2 + 7x + 10$ dan $4x^2 - 8x$.

1. Untuk $x^2 + 7x + 10$: cari dua bilangan dengan hasil kali $10$ dan jumlah $7$. Pasangan $(1, 10)$ berjumlah 11, pasangan $(2, 5)$ berjumlah 7. Maka $(x + 2)(x + 5)$.
2. Periksa: $(x + 2)(x + 5) = x^2 + 5x + 2x + 10 = x^2 + 7x + 10$. Benar.
3. Untuk $4x^2 - 8x$: faktor bersama terbesar adalah $4x$. Maka $4x(x - 2)$.
4. Periksa: $4x \cdot x - 4x \cdot 2 = 4x^2 - 8x$. Benar.

### Contoh 4 — Kurung bersarang

Sederhanakan $5 - \bigl[2x - (x - 3)\bigr]$.

1. Kurung dalam: $-(x - 3) = -x + 3$. Isinya menjadi $2x - x + 3 = x + 3$.
2. Ekspresi menjadi $5 - [x + 3]$.
3. Minus di depan kurung membalik tanda: $5 - x - 3$.
4. Gabungkan konstanta: $2 - x$.
5. Cek $x = 4$: awal $5 - [8 - 1] = 5 - 7 = -2$; hasil $2 - 4 = -2$. Cocok.

## Contoh Tambahan dengan Kesulitan Bertahap

**Mudah.** $4x + 7x - 3x = (4 + 7 - 3)x = 8x$.

**Sedang.** $(4a + 2b) - (a - 3b) = 4a + 2b - a + 3b = 3a + 5b$. Perhatikan $-(-3b) = +3b$.

**Sedang.** $\dfrac{12x + 18}{6} = \dfrac{12x}{6} + \dfrac{18}{6} = 2x + 3$.

**Menantang.** Kuadrat selisih: $(2x - 3)^2 = (2x)^2 - 2(2x)(3) + 3^2 = 4x^2 - 12x + 9$. Cek $x = 2$: $(1)^2 = 1$ dan $16 - 24 + 9 = 1$. Cocok.

## Visualisasi atau Interpretasi Grafik

Pandangan luas membantu memahami mengapa $(x + 3)^2 \neq x^2 + 9$. Persegi dengan sisi $x + 3$ terbagi menjadi empat bagian:

| | $x$ | $3$ |
|---|---|---|
| **$x$** | $x^2$ | $3x$ |
| **$3$** | $3x$ | $9$ |

Luas totalnya $x^2 + 3x + 3x + 9 = x^2 + 6x + 9$. Dua bagian persegi panjang ($3x$ dan $3x$) itulah suku tengah $6x$ yang sering terlupa.

## Kesalahan Umum

| Kesalahan | Mengapa salah | Yang benar |
|---|---|---|
| $3x + 2y = 5xy$ atau $5x$ | bukan suku sejenis | tidak disederhanakan; $x = 4$, $y = 5$ memberi $22$, bukan $100$ atau $20$ |
| $3x + 2x^2 = 5x^3$ | pangkat berbeda tidak sejenis | biarkan $3x + 2x^2$ |
| $(3x + 2) - (5x - 7) = -2x - 5$ | hanya tanda suku pertama yang dibalik | $-2x + 9$ |
| $2(x + 4) = 2x + 4$ | pengali tidak dikalikan ke semua suku | $2x + 8$ |
| $-2(x - 5) = -2x - 10$ | $(-2)(-5) = +10$ | $-2x + 10$ |
| $(x + 3)^2 = x^2 + 9$ | suku tengah hilang | $x^2 + 6x + 9$ |
| $\dfrac{c}{a + b} = \dfrac{c}{a} + \dfrac{c}{b}$ | hanya pembilang yang boleh dipecah | hitung penyebut dulu |
| $x \cdot x = 2x$ | perkalian, bukan penjumlahan | $x \cdot x = x^2$ |
| $5 - (x - 2) = 5 - x - 2$ | minus tidak membalik $-2$ | $5 - x + 2 = 7 - x$ |
| Memfaktorkan $x^2 + 5x + 6$ menjadi $(x + 1)(x + 6)$ | $1 + 6 = 7$, bukan 5 | $(x + 2)(x + 3)$ |

## Hubungan dengan Machine Learning

- **Menjabarkan kuadrat kesalahan.** Rumus $(\hat{y} - y)^2$ dijabarkan menjadi $\hat{y}^2 - 2\hat{y}y + y^2$ memakai kuadrat selisih. Bentuk ini muncul saat menurunkan rumus regresi.
- **Sifat distributif pada model.** Jika sebuah model linear dikalikan konstanta, $w(x_1 + x_2) = wx_1 + wx_2$. Sifat inilah yang membuat model linear mudah dianalisis.
- **Menyederhanakan sebelum menghitung.** Ekspresi yang disederhanakan lebih murah dihitung dan lebih mudah dicek.
- **Memeriksa dengan angka** adalah cara yang sama dengan pengujian kode: memasukkan beberapa nilai dan membandingkan hasil.

### Python: memeriksa identitas aljabar dengan angka

Python tidak melakukan aljabar simbolik secara bawaan, tetapi kita bisa memeriksa dua ekspresi dengan banyak nilai.

```python
def awal(x):
    return (x + 2) * (x + 3)

def hasil(x):
    return x ** 2 + 5 * x + 6

nilai_uji = [-3, -1, 0, 2, 5, 10]
print(all(awal(x) == hasil(x) for x in nilai_uji))   # True
```

Fungsi `all(...)` bernilai `True` jika **semua** pemeriksaan benar. Jika kamu salah menjabarkan, misalnya `x ** 2 + 6`, hasilnya `False`. Ini bukan bukti matematika, tetapi alat deteksi kesalahan yang cepat.

Memeriksa kesalahan umum $(x + 3)^2 \neq x^2 + 9$:

```python
x = 1
print((x + 3) ** 2)       # 16
print(x ** 2 + 9)         # 10, tidak sama
print(x ** 2 + 6 * x + 9) # 16, sama
```

Memeriksa kuadrat kesalahan dari $(\hat{y} - y)^2 = \hat{y}^2 - 2\hat{y}y + y^2$:

```python
y_topi, y = 12, 10
print((y_topi - y) ** 2)                   # 4
print(y_topi ** 2 - 2 * y_topi * y + y ** 2)  # 4
```

## Latihan Mandiri

1. Sederhanakan: (a) $4x + 7x - 3x$ (b) $5a + 2b - 3a + b$ (c) $6y^2 + 2y - 4y^2 + y$
2. Jumlahkan $(3x^2 - 2x + 5) + (x^2 + 6x - 8)$.
3. Kurangkan: (a) $(5x - 3) - (2x - 7)$ (b) $(4a + 2b) - (a - 3b)$
4. Kalikan: (a) $4x \cdot 5x^2$ (b) $(-3a)(-2b)$ (c) $2x \cdot (-3y)$
5. Bagi: (a) $\dfrac{20x^3}{4x}$ (b) $\dfrac{12x + 18}{6}$ (c) $\dfrac{9a^2 b}{3ab}$
6. Jabarkan: (a) $5(x + 3)$ (b) $-2(3x - 4)$ (c) $x(2x - 5)$
7. Sederhanakan $3(2x - 1) - 2(x + 4)$.
8. Kalikan: (a) $(x + 4)(x + 5)$ (b) $(x - 2)(x + 7)$ (c) $(2x + 1)(3x - 2)$
9. Faktorkan: (a) $8x + 12$ (b) $x^2 + 7x + 10$ (c) $x^2 - 16$ (d) $x^2 - 6x + 8$
10. Temukan kesalahan dan perbaiki: (a) $(x + 3)^2 = x^2 + 9$ (b) $2(x + 4) = 2x + 4$ (c) $5 - (x - 2) = 5 - x - 2$

## Kunci Jawaban dan Pembahasan

??? success "Soal 1"
    (a) $(4 + 7 - 3)x = 8x$.
    (b) Suku $a$: $5a - 3a = 2a$. Suku $b$: $2b + b = 3b$. Hasil $2a + 3b$.
    (c) Suku $y^2$: $6y^2 - 4y^2 = 2y^2$. Suku $y$: $2y + y = 3y$. Hasil $2y^2 + 3y$.

??? success "Soal 2"
    Suku $x^2$: $3x^2 + x^2 = 4x^2$. Suku $x$: $-2x + 6x = 4x$. Konstanta: $5 - 8 = -3$. Hasil $4x^2 + 4x - 3$.

??? success "Soal 3"
    (a) $5x - 3 - 2x + 7 = 3x + 4$.
    (b) $4a + 2b - a + 3b = 3a + 5b$.

??? success "Soal 4"
    (a) $4 \times 5 = 20$ dan $x \cdot x^2 = x^3$, jadi $20x^3$.
    (b) $(-3)(-2) = 6$, jadi $6ab$.
    (c) $2 \times (-3) = -6$, jadi $-6xy$.

??? success "Soal 5"
    (a) $\dfrac{20}{4} = 5$ dan $\dfrac{x^3}{x} = x^2$, jadi $5x^2$.
    (b) $\dfrac{12x}{6} + \dfrac{18}{6} = 2x + 3$.
    (c) $\dfrac{9}{3} = 3$, $\dfrac{a^2}{a} = a$, $\dfrac{b}{b} = 1$, jadi $3a$.

??? success "Soal 6"
    (a) $5x + 15$. (b) $-6x + 8$ (karena $-2 \times (-4) = 8$). (c) $2x^2 - 5x$.

??? success "Soal 7"
    $6x - 3 - 2x - 8 = 4x - 11$. Cek $x = 2$: awal $3(3) - 2(6) = -3$; hasil $8 - 11 = -3$.

??? success "Soal 8"
    (a) $x^2 + 5x + 4x + 20 = x^2 + 9x + 20$.
    (b) $x^2 + 7x - 2x - 14 = x^2 + 5x - 14$.
    (c) $6x^2 - 4x + 3x - 2 = 6x^2 - x - 2$.

??? success "Soal 9"
    (a) FPB 4: $4(2x + 3)$.
    (b) Hasil kali 10 dan jumlah 7: $2$ dan $5$. $(x + 2)(x + 5)$.
    (c) Selisih kuadrat: $(x - 4)(x + 4)$.
    (d) Hasil kali 8 dan jumlah $-6$: $-2$ dan $-4$. $(x - 2)(x - 4)$.

??? success "Soal 10"
    (a) Suku tengah hilang. Yang benar: $(x + 3)^2 = x^2 + 6x + 9$.
    (b) Pengali 2 tidak dikalikan ke suku kedua. Yang benar: $2x + 8$.
    (c) Minus di depan kurung harus membalik $-2$ menjadi $+2$. Yang benar: $5 - x + 2 = 7 - x$.

## Ringkasan Materi

- Hanya suku sejenis yang digabung: $ax + bx = (a + b)x$. $3x + 2y$ tidak dapat disederhanakan.
- Pengurangan membalik tanda **semua** suku di dalam kurung.
- Distributif: $a(b + c) = ab + ac$. Pengali dikalikan ke setiap suku, termasuk tandanya.
- Perkalian dua bentuk linear: kalikan setiap suku dengan setiap suku, lalu gabungkan suku sejenis.
- $(a + b)^2 = a^2 + 2ab + b^2$ dan $(a - b)(a + b) = a^2 - b^2$.
- Memfaktorkan adalah kebalikan menjabarkan: faktor persekutuan, $x^2 + bx + c$ dengan hasil kali $c$ dan jumlah $b$, atau selisih dua kuadrat.
- Periksa hasil dengan substitusi angka.

## Checklist Penguasaan

- [ ] Menjelaskan mengapa $3x + 2y$ tidak dapat digabung, dengan contoh angka.
- [ ] Menjumlah dan mengurangkan bentuk aljabar tanpa salah tanda.
- [ ] Mengalikan dan membagi suku tunggal memakai aturan pangkat.
- [ ] Menjabarkan dengan distributif, termasuk pengali negatif.
- [ ] Menghilangkan kurung bersarang dari dalam ke luar.
- [ ] Mengalikan dua bentuk linear dan mengenali kuadrat jumlah.
- [ ] Memfaktorkan bentuk sederhana dan memeriksanya dengan menjabarkan.
- [ ] Memeriksa hasil aljabar dengan substitusi angka.

## Prasyarat untuk Materi Berikutnya

[Part 3 — Persamaan Linear Satu Variabel](03-persamaan-linear-satu-variabel.md) membutuhkan: menyederhanakan ekspresi, menjabarkan kurung, dan menggabungkan suku sejenis, karena itu langkah pertama sebelum menyelesaikan persamaan.
