# Solusi numerik differensial
## Apa itu
Persamaan diferensial biasa atau PDB adalah persamaan differensial yang mana fungsi yang tidak diketahui (variabel terikat) adalah fungsi dari variabel yang bebas tunggal. Bisa dibilang fungsi ini disebut juga dengan fungsi riil atau kompleks, tapi juga bisa disebut fungsi vektor atau matriks

## Metode Euler
Metode ini adalah metode paling sederhana yang diturunkan dari deret taylor.

$$
y_{i+1} = y_{i} + f_({x_{i},y_{i}}) h
$$

## Algoritma
1. Tentukan titik awal integrasi $x_{0}$ dan $y_{0}$
2. Tentukan jumlah iterasi ($n$) dan ukuran step ($h$)
3. Eksekusi

## Kode
membuat fungsi, isi dari function numerik ini bisa diganti sesuai kebutuhan
```python
def numeric(x,y,h): 
    return y + (x*math.sqrt(y) * h)
```
kemudian bisa langsung diproses
```python
import math

# Contoh fungsi, bisa direplace
def numeric(x,y,h): 
    return y + (x*math.sqrt(y) * h)

x = 0
y = 1
h = 0.25
interval = 1

while x <= interval:
    res = numeric(x,y,h)
    print("x = {}".format(x))
    print("y = {}\n".format(res))
    y = res
    x+=h
```
## Contoh
Selesaikan persamaan differensial $\frac{dy}{dx} = x\sqrt{y}$ pada interval x = 0 s/d x = 1, h = ¼. Pada saat x = 0, nilai y = 1

```
x = 0
y = 1.0

x = 0.25
y = 1.0625

x = 0.5
y = 1.191347050800552

x = 0.75
y = 1.396001136405279

x = 1.0
y = 1.6913823663866876
```