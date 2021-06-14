# Solusi numerik persamaan aljabar linear
## Gauss
Metode gauss merupakan metode yang terdiri dari dua langkah yaitu eliminasi maju dan subtitusi mundur
### Algoritma
Pertama kita tanyakan dulu matriks 2D dengan panjang dan lebar berapa yang akan diproses sekaligus nilai tiap sel matriksnya
```python
n = int(input("Berapa variabel? "))
mtx = [[float(input("Nilai untuk [{}][{}]=".format(i,j))) for j in range(n+1)] for i in range(n)]
```
lanjut untuk memproses matriknya, disini metode gauss memproses dengan cara membuat nilai matriks diagonal bernilai 1 dan sel lainnya bernilai 0

$\begin{bmatrix}1 & 0 & 0\\0 & 1 & 0\\0 & 0 & 1\end{bmatrix} = \begin{bmatrix}X\\Y\\Z\end{bmatrix}$

untuk mendapatkan hasil seperti itu maka diperlukan looping di setiap array dalam array (multidimensi) karena bentuk dari matriks tersebut jika di tampilkan dalam program adalah
```
[
    [1,0,0],
    [0,1,0],
    [0,0,1]
]
```
untuk mendapatkan nilai 1, cukup dengan membagi satu array dengan nilai array pertama dan untuk mendapatkan nilai 0 cukup dengan mengurangi satu array dengan nilai array ke-n (tergantung posisi). Maka kode nya:
```python
for i in range(len(mtx)):
    for j in range(len(mtx)):
        if mtx[i][i] != 1:
            mtx[i] = [t/mtx[i][i] for t in mtx[i]]
        if i == j:
            continue
        rasio = mtx[j][i]
        for k in range(i, len(mtx[i])):
            mtx[j][k] = mtx[j][k] - rasio * mtx[i][k]
        print("\nStep {},{}".format(i,j))
        for _ in mtx:
            print(_)
```
### Kode
```python
n = int(input("Berapa variabel? "))
mtx = [[float(input("Nilai untuk [{}][{}]=".format(i,j))) for j in range(n+1)] for i in range(n)]
for i in range(len(mtx)):
    for j in range(len(mtx)):
        if mtx[i][i] != 1:
            mtx[i] = [t/mtx[i][i] for t in mtx[i]]
        if i == j:
            continue
        rasio = mtx[j][i]
        for k in range(i, len(mtx[i])):
            mtx[j][k] = mtx[j][k] - rasio * mtx[i][k]
        print("\nStep {},{}".format(i,j))
        for _ in mtx:
            print(_)
```
### Contoh
```
Berapa variabel? 3
Nilai untuk [0][0]=2
Nilai untuk [0][1]=1
Nilai untuk [0][2]=4
Nilai untuk [0][3]=8
Nilai untuk [1][0]=3
Nilai untuk [1][1]=2
Nilai untuk [1][2]=1
Nilai untuk [1][3]=10
Nilai untuk [2][0]=1
Nilai untuk [2][1]=3
Nilai untuk [2][2]=3
Nilai untuk [2][3]=8

Step 0,1
[1.0, 0.5, 2.0, 4.0]
[0.0, 0.5, -5.0, -2.0]
[1.0, 3.0, 3.0, 8.0]

Step 0,2
[1.0, 0.5, 2.0, 4.0]
[0.0, 0.5, -5.0, -2.0]
[0.0, 2.5, 1.0, 4.0]

Step 1,0
[1.0, 0.0, 7.0, 6.0]
[0.0, 1.0, -10.0, -4.0]
[0.0, 2.5, 1.0, 4.0]

Step 1,2
[1.0, 0.0, 7.0, 6.0]
[0.0, 1.0, -10.0, -4.0]
[0.0, 0.0, 26.0, 14.0]

Step 2,0
[1.0, 0.0, 0.0, 2.230769230769231]
[0.0, 1.0, -10.0, -4.0]
[0.0, 0.0, 1.0, 0.5384615384615384]

Step 2,1
[1.0, 0.0, 0.0, 2.230769230769231]
[0.0, 1.0, 0.0, 1.3846153846153841]
[0.0, 0.0, 1.0, 0.5384615384615384]
```
## Jacobi
Metode jacobi adalah metode untuk menghasilkan variable x yang baru dengan menggunakan nilai x yang menjadi nilai dari satu persamaan, contohnya:

$\begin{bmatrix}10 & -1 & 2 & 0\\-2 & 11 & -1 & 3\\2 & -1 & 10 & -1\\0 & 3 & -1 & 8\end{bmatrix} \begin{bmatrix}x_{1}\\x_{2}\\x_{3}\\x_{4}\end{bmatrix} = \begin{bmatrix}6\\25\\-11\\15\end{bmatrix}$

untuk mendapatkan nilai $x_{1}^{(1)},x_{2}^{(1)},x_{3}^{(1)},x_{4}^{(1)}$ adalah dengan cara:

$$
x_{1} = (x_{2}-2x_{3}+6)/10
$$

$$
x_{2} = (x_{1}+x_{3}-3x_{4} + 25)/11
$$

$$
x_{3} = (-2x_{1}+x_{2}+x_{4}-11)/10
$$

$$
x_{4} = (-3x_{2}+x_{3}+15)/8
$$ 

dan di misalkan nilai x bernilai 0 maka hasinya:

$$
x_{1} = 0.6000
$$

$$
x_{2} = 2.2727
$$

$$
x_{3} = -1.000
$$

$$
x_{4} = 1.8750
$$
### Algoritma
Untuk menyelasaikan metode jacobi dalam program maka dibutuhkan matriks tentunya dan juga iterasi (generasi x). Tidak lupa nilai awal x
```python
n = int(input("Berapa variabel? "))
iterasi = int(input("Berapa iterasi? "))
mtx = [[float(input("Nilai untuk [{}][{}]=".format(i,j))) for j in range(n+1)] for i in range(n)]
starter = [float(input("Nilai awal X{}=".format(i+1))) for i in range(n)]
```
kemudian membuat tempat penampungan sementara nilai x lama dan x baru ketika nantinya program dijalankan, karena metode yang dipakai adalah looping, karena itu kita membuatnya sebagai variable global
```python
res = [0 for i in mtx]
tmp = [0 for i in mtx]
```
### Kode
```python
n = int(input("Berapa variabel? "))
iterasi = int(input("Berapa iterasi? "))
mtx = [[float(input("Nilai untuk [{}][{}]=".format(i,j))) for j in range(n+1)] for i in range(n)]
starter = [float(input("Nilai awal X{}=".format(i+1))) for i in range(n)]
res = [0 for i in mtx]
tmp = [0 for i in mtx]
for h in range(iterasi):
    for i in range(len(mtx)):
        holder = mtx[i][-1]
        for j in range(len(mtx[i])-1):
            if i == j:
                continue
            holder -= mtx[i][j]*res[j]
        tmp[i] = holder/mtx[i][i]
    res = tmp
    [print("X{}={}".format(k, res[k]), end='\t') for k in range(len(res))]
    print("")
```
### Contoh
```
Berapa variabel? 4
Berapa iterasi? 4
Nilai untuk [0][0]=10
Nilai untuk [0][1]=-1
Nilai untuk [0][2]=2
Nilai untuk [0][3]=0
Nilai untuk [0][4]=6
Nilai untuk [1][0]=-1
Nilai untuk [1][1]=11
Nilai untuk [1][2]=-1
Nilai untuk [1][3]=3
Nilai untuk [1][4]=25
Nilai untuk [2][0]=2
Nilai untuk [2][1]=-1
Nilai untuk [2][2]=10
Nilai untuk [2][3]=-1
Nilai untuk [2][4]=-11
Nilai untuk [3][0]=0
Nilai untuk [3][1]=3
Nilai untuk [3][2]=-1
Nilai untuk [3][3]=8
Nilai untuk [3][4]=15
Nilai awal X1=0
Nilai awal X2=0
Nilai awal X3=0
Nilai awal X4=0
X0=0.6  X1=2.272727272727273    X2=-1.1 X3=1.875
X0=1.0472727272727274   X1=1.756570247933884    X2=-0.946297520661157   X3=1.0979989669421488
X0=0.9649165289256197   X1=1.9749656461307288   X2=-0.9856868444778362  X3=1.011177027141247
X0=0.9946339335086402   X1=1.997765091600642    X2=-0.998032574827539   X3=1.0010840187963168
```
## Gauss Seidel
Metode seidel pada dasarnya sama dengan metode jacobi hanya saja ketika nilai terbaru dari x (misal $x_{1}^{(1)}$ sudah ditemukan maka akan langsung dipakai untuk mencari nilai x selanjutnya)
### Algoritma
Yang berbeda disini adalah kita membuat penampung x menjadi satu, jadi tidak ada wadah terpisah antara x lama dan x baru
```python
res = [0 for i in mtx]
```
### Kode
```python
n = int(input("Berapa variabel? "))
iterasi = int(input("Berapa iterasi? "))
mtx = [[float(input("Nilai untuk [{}][{}]=".format(i,j))) for j in range(n+1)] for i in range(n)]
starter = [float(input("Nilai awal X{}=".format(i+1))) for i in range(n)]
res = [0 for i in mtx]
for h in range(iterasi):
    for i in range(len(mtx)):
        holder = mtx[i][-1]
        for j in range(len(mtx[i])-1):
            if i == j:
                continue
            holder -= mtx[i][j]*res[j]
        res[i] = holder/mtx[i][i]
        [print("X{}={}".format(k, res[k]), end='\t') for k in range(len(res))]
        print("")
```
### Contoh
```
Berapa variabel? 4
Berapa iterasi? 4
Nilai untuk [0][0]=10
Nilai untuk [0][1]=-1
Nilai untuk [0][2]=2
Nilai untuk [0][3]=0
Nilai untuk [0][4]=6
Nilai untuk [1][0]=-1
Nilai untuk [1][1]=11
Nilai untuk [1][2]=-1
Nilai untuk [1][3]=3
Nilai untuk [1][4]=25
Nilai untuk [2][0]=2
Nilai untuk [2][1]=-1
Nilai untuk [2][2]=10
Nilai untuk [2][3]=-1
Nilai untuk [2][4]=-11
Nilai untuk [3][0]=0
Nilai untuk [3][1]=3
Nilai untuk [3][2]=-1
Nilai untuk [3][3]=8
Nilai untuk [3][4]=15
Nilai awal X1=0
Nilai awal X2=0
Nilai awal X3=0
Nilai awal X4=0
X0=0.6  X1=0    X2=0    X3=0
X0=0.6  X1=2.3272727272727276   X2=0    X3=0
X0=0.6  X1=2.3272727272727276   X2=-0.9872727272727271  X3=0
X0=0.6  X1=2.3272727272727276   X2=-0.9872727272727271  X3=0.8788636363636363
X0=1.0301818181818183   X1=2.3272727272727276   X2=-0.9872727272727271  X3=0.8788636363636363
X0=1.0301818181818183   X1=2.0369380165289255   X2=-0.9872727272727271  X3=0.8788636363636363
X0=1.0301818181818183   X1=2.0369380165289255   X2=-1.0144561983471074  X3=0.8788636363636363
X0=1.0301818181818183   X1=2.0369380165289255   X2=-1.0144561983471074  X3=0.9843412190082644
X0=1.006585041322314    X1=2.0369380165289255   X2=-1.0144561983471074  X3=0.9843412190082644
X0=1.006585041322314    X1=2.003555016904583    X2=-1.0144561983471074  X3=0.9843412190082644
X0=1.006585041322314    X1=2.003555016904583    X2=-1.002527384673178   X3=0.9843412190082644
X0=1.006585041322314    X1=2.003555016904583    X2=-1.002527384673178   X3=0.9983509455766342
X0=1.000860978625094    X1=2.003555016904583    X2=-1.002527384673178   X3=0.9983509455766342
X0=1.000860978625094    X1=2.000298250656547    X2=-1.002527384673178   X3=0.9983509455766342
X0=1.000860978625094    X1=2.000298250656547    X2=-1.0003072761017007  X3=0.9983509455766342
X0=1.000860978625094    X1=2.000298250656547    X2=-1.0003072761017007  X3=0.9998497464910823
```