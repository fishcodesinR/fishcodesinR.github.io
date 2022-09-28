---
title: Penghitungan
parent: Panduan montiR
nav_order: 1
---

### Penghitungan Surplus production dengan asumsi non-equilibrium menggunakan data fitting

Metode time series fitting dengan observation error disebut sebagai metode yang lebih baik dibandingkan dengan dua metode lain (metode equilibrium dengan linear regression maupun multiple regression) yang digunakan untuk melakukan estimasi parameter dalam model surplus produksi, termasuk juga menghasilkan estimasi yang lebih akurat dibanding dengan metode time series fitting dengan process error (Hilborn dan Walter, 1992; Polacheck, et al. 1993; Punt dan Hilborn, 1996). Disini akan dibahas langkah yang disarankan untuk melakukan analisis dengan data fitting untuk meningkatkan akurasi perhitungan MSY, Bmsy dan Emsy.

Pendekatan ini selanjutnya lebih dikenal dengan sebutan Biomass Dynamic Model, dengan variasi menggunakan metode yang berdasarkan dari Schaefer (1954), Fox (1970) dan Pella-Tomlinson (1969). Saat ini `montiR` dibangun dengan model Schaefer yang dituliskan dengan

$B_{t+1} = {B_{t} + rB_{t} (1- {B_{t} \over K}) - C_{t}}$

dimana:

$B_{t}$ = biomass yang dimanfaatkan pada awal tahun $t$

$r$ = laju pertumbuhan intrinsik

$K$ = carrying capacity

$C_{t}$ = jumlah tangkapan (volume) pada tahun $t$

dengan

$I_{t+1} = {C_{t} \over E_{t}} = q B_{t} e^\epsilon$

dimana: 

$I_{t}$ = catch per unit of effort (CPUE) atau indeks kelimpahan pada tahun $t$

$E_{t}$ = upaya penangkapan pada tahun $t$

$q$ = catchability

$\epsilon$ = observation error


Penggunaan `montiR` diawali dengan penyediaan data yang meliputi data tahun, data tangkapan serta data upaya yang dibuat dalam dataframe. Berikut adalah contoh untuk membuat dataframe sebagai input untuk analisis `montiR` menggunakan data tangkapan Yellowfin Tuna di East Pacific (Schaefer, 1957).

```markdown
df <- data.frame(year=c(1934:1955),
                 catch=c(60913,72294,78353,91522,78288,110417,114590,76841,41965,50058,64094,
                        89194,129701,160134,200340,192458,224810,183685,192234,138918,138623,140581),
                 effort=c(5879,6295,6771,8233,6830,10488,10801,9584,5961,5930,6397,9377,13958,
                        20381,23984,23013,31856,18726,31529,36423,24995,17806))
```


#### a. Data plotting

Langkah paling penting sebelum melakukan analisis adalah memeriksa apakah data yang akan digunakan memenuhi persyaratan dan asumsi yang dibutuhkan untuk analisis biomass dynamic model, termasuk memilih jenis langkah apa yang harus dilakukan ketika data yang dibutuhkan tidak memenuhi asumsi. Para ahli statistik dan pemodelan matematik selalu memulai analisisnya dengan, "Plot your data!". 

Langkah untuk melihat grafik jumlah tangkapan (catch), upaya (effort) serta catch per unit of effort (CPUE)/indeks kelimpahan dari data yang dimiliki dapat dilakukan dengan mudah menggunakan kode dan contoh data yang tersedia sebagaimana berikut:

```markdown
plotInit(df=df.goodcontrast0)
plotInit(df=df.onewaytrip0)
```

Disini kita akan melihat dua jenis data yang biasanya terdapat pada perikanan, goodcontrast dan onewaytrip. Biomass dynamic model dengan menggunakan metode data fitting mensyaratkan data yang memiliki kontras yang cukup pada Catch per Unit Effort (CPUE), ditunjukkan dengan adanya kontras data yang baik (i.e. memiliki representasi pola turun dan naik) serta paling tidak memiliki 20 tahun entry untuk tangkapan dan upaya (Punt & Hilborn, 1996; Magnusson & Hilborn, 2007). Contoh dari data yang memiliki kontras yang cukup dapat dilihat pada `df.goodcontrast0` dan `df.namibianCatch`, dimana contoh plot dari `df.goodcontrast0` dapat dilihat berikut:

![Tipe data dengan good contrast](/img/img_goodcontrast0.png)

Contoh data yang tidak memiliki kontras yang baik dapat dilihat pada `df.onewaytrip0` dan `df.eastpacCatch`. Dapat dilihat bahwa data tangkapan dan upaya memiliki pola meningkat dengan tidak memiliki pola menurun sebagaimana yang ditunjukkan dari plot `df.onewaytrip0` berikut:

![Tipe data one way trip](/img/img_onewaytrip0.png)

Jenis data yang berbeda harus dianalisis menggunakan cara yang berbeda pula. Hal ini akan dijelaskan lebih lanjut di bagian selanjutnya.

Pada tahap ini diharapkan data sudah melalui langkah data standardization yang biasanya diolah dengan menggunakan Generalized Linear Model.


#### b. Estimasi parameter surplus production dengan data fitting

Tool ini melakukan estimasi parameter K, B0, r, q dan menentukan jumlah tangkapan ikan lestari (MSY), biomassa ikan lestari (Bmsy), serta upaya penangkapan ikan lestari (Emsy) menggunakan data runut waktu dengan asumsi non-equilibrium untuk model Schaefer.

Proses estimasi diawali dengan mencari angka awal yang diperkirakan sesuai dengan parameter K, B0, r, q. Hal ini dilakukan dengan menduga angka parameter, kemudian dilihat apakah grafik yang dihasilkan dari parameter yang kita duga memberikan grafik yang sesuai dengan data yang akan kita lakukan pendugaannya. Misalnya kita akan menduga parameter surplus production dari data `df.namibianCatch`, maka cara estimasinya dilakukan dengan mencari angka awal terlebih dahulu dengan:

```markdown
library('montiR')

K <- 1700
B0 <- K
r <- 0.3
q <- 0.0003

inpars <- c(K, B0, r, q)
Par_init(inpars=inpars, df=df.namibianCatch)

```

Mencari angka awal ini dilakukan dengan merubah angka K, r dan q sehingga dirasa grafik hasil dari angka awal (Estimation) dirasa sudah sesuai (fit) dengan data yang akan kita analisis (Observation). Sebagai panduan pada tahapan ini, angka r seharusnya berada pada rentang antara 0-1.3 dan angka q biasanya berada pada rentang antara 0-0.1.

Setelah angka awal didapat, maka langkah selanjutnya adalah melakukan optimasi parameter melalui Maximum Likelihood Estimation dengan observation error. Observation error menggunakan asumsi bahwa terdapat kesalahan pada hubungan antara biomass dan indeks kelimpahan, sehingga parameter ini perlu untuk diestimasi. Indeks kelimpahan diasumsikan mengikuti distribusi log-normal untuk melakukan estimasi parameter K, B0, r, q dan sigma (observation error).

Proses estimasi parameter ini dilakukan dengan langkah sebagai berikut:

```markdown
library('montiR')

calc.MSY(K=1000,
         B0=1000,
         r=0.2,
         q=0.00025,
         s.sigma=0.1,
         df=df.goodcontrast,
         plot=TRUE)
```

Setelah input diatas dijalankan, akan dihasilkan estimasi parameter K, B0, r, q dan observation error beserta estimasi MSY, upaya pada MSY, Biomass pada MSY serta tingkat pemanfaatan pada MSY dan tingkat pemanfaatan pada upaya optimal ketika MSY sebagai berikut:

```markdown
$Parameter
    SPpar  fitted_pars
1       K 9.957554e+02
2      B0 8.793875e+02
3       r 1.793476e-01
4       q 2.747262e-04
5 s.sigma 4.524280e-02

$MSY
       MSY     Emsy     Bmsy E.rate_MSY E.rate_Emsy
1 44.64659 326.4116 497.8777  0.3886082  0.05315383
```

Ketika `plot=TRUE`, maka secara otomatis akan ditampilkan grafik hasil dimana garis estimation (warna merah) akan fit dengan garis data observation (warna biru).

![Perbandingan fitting dari data Observation dan Estimation](/img/img_goodcontrastfitted.png)

Data frame `$Parameter` menyimpan angka estimasi hasil optimasi (`fitted_pars`). Sebagai panduan untuk memeriksa hasil parameter, angka K biasanya selalu lebih tinggi dibanding B0, angka r seharusnya berada pada rentang antara 0-1 dan angka q biasanya berada pada rentang antara 0-0.1.

{: .catatan }
Jika angka hasil optimasi masih belum sesuai dengan panduan sederhana ini, proses optimasi dapat dilakukan secara manual dengan angka awal dapat dibatasi menggunakan batas bawah (lower) dan batas (upper) atas serta merubah metode optimasi menjadi "L-BFGS-B". Penggunaan constrain seperti yang dilakukan dibawah ini perlu dilakukan dengan hati-hati untuk dapat menghasilkan estimasi yang akurat.

```markdown

inpars <- c(log(K), log(B0), log(r), log(q), log(s.sigma))

fit <- optim(par = inpars,
             fn = Par.min,
             df = df.goodcontrast,
             method = "L-BFGS-B",
             lower = xxxx,
             upper = xxxx
 )

 vals <- exp(fit$par)
 res <- data.frame("SPpar" = c("K", "B0", "r", "q", "s.sigma"),
                   "fitted_pars" = vals)
 res
```


Untuk data yang memiliki tipe one way trip, input yang digunakan dalam optimasi perlu disesuaikan terlebih dahulu. Penjelasan lebih lanjut untuk metode ini akan ditulis beberapa waktu kedepan. Selain itu, tool ini sudah disesuaikan untuk kebutuhan data yang terbatas (dapat mengakomodasi hilangnya sedikit input data upaya penangkapan). 

#### c. Menghitung standard error dari reference point

Tool ini mengestimasi jumlah stok ikan yang lestari (Bmsy), jumlah tangkapan ikan lestari (MSY) dan upaya penangkapan ikan lestari (Emsy) serta menghitung standard error menggunakan data runut waktu dengan asumsi non-equilibrium untuk model Schaefer.

Penghitungan standard error dari reference point dapat dilakukan dengan menggunakan output dari analisis sebelumnya sebagai input untuk analisis berikut

```markdown
library(montiR)

calc.SE(MSY=44.64659,
        Emsy=326.4116,
        Bmsy=497.8777,
        s.sigma=0.1,
        df=df.goodcontrast)
```

Setelah kode diatas dijalankan, akan dihasilkan estimasi parameter untuk Bmsy, MSY, Emsy dengan perhitungan standard error-nya. Untuk kebutuhan pengelolaan, dipersilahkan untuk menggunakan parameter yang dihasilkan berikut:

```markdown
  ManagPar  fitted_pars      std_err
1     Bmsy  45.31956545  3.065096475
2      MSY 359.41474740 24.908587929
3     Emsy 530.77542342 46.651955308
4    sigma   0.04765599  0.007520254
```
