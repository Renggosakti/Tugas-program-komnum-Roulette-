# Tugas-program-komnum-Roulette
# 📐 Integrasi Numerik Metode Trapesium (Trapesium Segmen Berganda)

**Nama**: Arya Rangga Putra Pratama
**NRP**: 5025241072
**Kelas**: Komnum A
**Soal**: Nomor 40

Repositori ini berisi implementasi Python untuk menghitung luas di bawah kurva fungsi menggunakan **metode trapesium segmen berganda**, serta membandingkannya dengan hasil integral eksak menggunakan `sympy`.

---

## 📘 Deskripsi Fungsi

Fungsi yang digunakan:

\$f(x) = 3x^5 - 8x^4\$

* Batas integral: \$a = 4,\ b = 16\$
* Jumlah segmen: \$n = 4\$

---

## 📐 Rumus Metode Trapesium

$$
L = h \left( \frac{f(x_0)}{2} + f(x_1) + f(x_2) + \dots + f(x_{n-1}) + \frac{f(x_n)}{2} \right)
$$

Dengan:

* \$h = \frac{b - a}{n}\$
* \$x\_0, x\_1, ..., x\_n\$ adalah titik partisi dari interval $\[a, b]\$

---

## 📦 Modul yang Digunakan

```python
import numpy as np
import sympy as sp
```

* `numpy (np)` : untuk operasi numerik seperti array dan linspace.
* `sympy (sp)` : untuk menghitung integral eksak secara simbolik.

---

## 🧩 Kelas: TrapeziumIntegrator

### 🔧 `__init__(self, func, a, b)`

Inisialisasi objek integrator. Parameter:

* `func`: fungsi f(x) yang akan diintegralkan
* `a`: batas bawah integral
* `b`: batas atas integral

```python
def __init__(self, func, a, b):
    self.func = func
    self.a = a
    self.b = b
```

Penjelasan:

* `self`: referensi ke objek itu sendiri (wajib di method OOP Python).
* `self.func`, `self.a`, dan `self.b` menyimpan data ke dalam atribut kelas.

---

### 📍 `evaluate_function(self, x_points)`

Evaluasi nilai fungsi untuk setiap titik \$x\_i\$ dalam array `x_points`.

```python
def evaluate_function(self, x_points):
    return np.array([self.func(x) for x in x_points])
```

Penjelasan:

* `x_points`: array titik x yang sudah dibagi.
* `self.func(x)`: memanggil fungsi f(x) untuk setiap x.
* Output: array nilai f(x).

---

### 📐 `calculate_integral(self, n_segments)`

Menghitung luas menggunakan metode trapesium segmen berganda.

```python
def calculate_integral(self, n_segments):
    h = (self.b - self.a) / n_segments
    x_points = np.linspace(self.a, self.b, n_segments + 1)
    y_points = self.evaluate_function(x_points)
    luas = h * (0.5 * y_points[0] + 0.5 * y_points[-1] + np.sum(y_points[1:-1]))
    return x_points, y_points, round(luas, 2)
```

Penjelasan:

* `h`: panjang setiap segmen.
* `x_points`: titik-titik pembagi dari a ke b.
* `y_points`: hasil f(x) dari tiap titik.
* `luas`: hasil perhitungan rumus trapesium.

---

### 🎯 `calculate_true_integral(self)`

Menghitung nilai eksak dari integral menggunakan SymPy.

```python
def calculate_true_integral(self):
    x = sp.Symbol('x')
    true_integral = sp.integrate(self.func(x), (x, self.a, self.b))
    return float(true_integral.evalf())
```

Penjelasan:

* `sp.Symbol('x')`: mendefinisikan variabel simbolik.
* `sp.integrate`: menghitung integral tak tentu/tertentu.
* `evalf()`: konversi ke bentuk float.

---

### 📉 `calculate_error(self, luas_numerik, luas_eksak)`

Menghitung galat relatif antara hasil numerik dan eksak.

```python
def calculate_error(self, luas_numerik, luas_eksak):
    error = abs((luas_eksak - luas_numerik) / luas_eksak) * 100
    return round(error, 2)
```

Penjelasan:

* Formula: \$\text{Error} = \left| \frac{I\_{eksak} - I\_{numerik}}{I\_{eksak}} \right| \times 100\$.
* Hasil dibulatkan dua desimal.

---

## 🚀 Fungsi Main

```python
def main():
    def f(x):
        return 3 * x**5 - 8 * x**4

    a, b = 4, 16
    n_segments = 4

    integrator = TrapeziumIntegrator(f, a, b)
    x_points, y_points, luas_numerik = integrator.calculate_integral(n_segments)
    luas_eksak = integrator.calculate_true_integral()
    error = integrator.calculate_error(luas_numerik, luas_eksak)

    print("=== Langkah 1: Evaluasi Fungsi ===")
    for x, y in zip(x_points, y_points):
        print(f"f({int(x)}) = {round(y, 2)}")

    print("\n=== Langkah 2: Hitung Luas (Trapesium) ===")
    print(f"h = (16-4)/4 = 3")
    print(f"Luas = 3 * [0.5*f(4) + 0.5*f(16) + f(7) + f(10) + f(13)]")
    print(f"      = {luas_numerik}")

    print("\n=== Langkah 3: Hitung Error ===")
    print(f"Luas Eksak (Sympy) = {round(luas_eksak, 2)}")
    print(f"Error (%) = |({round(luas_eksak, 2)} - {luas_numerik}) / {round(luas_eksak, 2)}| * 100 = {error}%")
```

### Penjelasan Fungsi `main()`:

* `def f(x)`: Mendefinisikan fungsi matematika \$f(x) = 3x^5 - 8x^4\$.
* `a, b = 4, 16`: Menentukan batas bawah dan atas integral.
* `n_segments = 4`: Menentukan jumlah segmen untuk metode trapesium.
* `integrator = TrapeziumIntegrator(f, a, b)`: Membuat objek dari kelas `TrapeziumIntegrator`.
* `calculate_integral`: Menghitung nilai luas secara numerik.
* `calculate_true_integral`: Menghitung nilai eksak menggunakan integral simbolik.
* `calculate_error`: Menghitung galat/error antara hasil numerik dan eksak.
* `print(...)`: Mencetak semua langkah dan hasil ke terminal sesuai urutan pengerjaan.

---

## 📊 Contoh Output

```
=== Langkah 1: Evaluasi Fungsi ===
f(4) = 1024.0
f(7) = 31213.0
f(10) = 220000.0
f(13) = 885391.0
f(16) = 2621440.0

=== Langkah 2: Hitung Luas (Trapesium) ===
h = (16-4)/4 = 3
Luas = 3 * [0.5*f(4) + 0.5*f(16) + f(7) + f(10) + f(13)]
      = 7343508.0

=== Langkah 3: Hitung Error ===
Luas Eksak (Sympy) = 6710476.8
Error (%) = |(6710476.8 - 7343508.0) / 6710476.8| * 100 = 9.43%
```

---

## 📘 Catatan Akhir

* Program ini sesuai dengan soal nomor 40 kelas Komnum A.
* Dapat dimodifikasi untuk jumlah segmen yang lebih banyak atau fungsi yang berbeda.
* Dokumentasi ini mencakup seluruh proses perhitungan dan analisis galat.
