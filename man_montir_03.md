---
title: Proyeksi
parent: Panduan montiR
nav_order: 1
---


## Membuat proyeksi atas kebijakan reference point berdasar tingkat pemanfaatan perikanan

Tool ini akan membuat grafik proyeksi biomass per biomass at msy (B/Bmsy) dan fishing mortality per fishing mortality pada msy (F/Fmsy) sebagai panduan untuk melihat kebijakan yang akan dibuat berdasarkan MSY dan Emsy sebagai reference point. Proyeksi dibuat dengan pendekatan stochastic dengan menunjukkan rentang kepercayaan yang dihitung dari banyaknya simulasi yang dilakukan.

Jumlah tangkapan yang diperbolehkan dapat ditetapkan sesuai kebutuhan, termasuk juga dapat memanfaatkan hasil perhitungan tingkat pemanfaatan dari langkah sebelumnya untuk menghitung berapa idealnya jumlah tangkapan ditetapkan berdasar atas aturan atau kesepakatan yang dibuat dalam pengelolaan.

Contoh penggunaan penghitungan ini mengasumsikan akan dibuat proyeksi sejauh 30 tahun ke depan dengan jumlah ulangan simulasi sebanyak 100 kali dan direncanakan pengelolaan akan menggunakan TAC = 0.8MSY:
```markdown
fit <- calc.MSY(K=1000,
                B0=1000,
                r=0.2,
                q=0.00025,
                s.sigma=0.1,
                df=df.goodcontrast)

run.Proj(inpars=fit[[1]][,2],
         df=df.goodcontrast,
         nyears=30,
         nsims=100,
         TAC=1,
         plot=TRUE)
```

Grafik proyeksi akan muncul ketika `plot=TRUE` dengan garis proyeksi berwarna biru, rentang 95% confidence interval menggunakan garis putus berwarna merah dan data observasi berwarna hitam. Kolom sebelah kiri merupakan kolom proyeksi berdasarkan atas kuota MSY, sedangkan kolom sebelah kanan merupakan proyeksi berdasarkan atas pengaturan jumlah upaya pada MSY.

![Perbandingan fitting dari data Observation dan Estimation](/img/img_goodcontrastproyeksi.png)

Selain menampilkan grafik diatas, proyeksi ini juga akan menampilkan angka hasil perhitungan yang dapat dilihat pada konsol.

{: .catatan }
Cara penghitungan dan proyeksi jumlah tangkapan yang diperbolehkan diatas termasuk dalam kelompok constant catch harvest strategy. Penggunaan harvest strategy jenis ini perlu dilakukan dengan hati-hati dan membutuhkan pengumpulan data yang sering, apalagi jika pendugaan stok ikan dilakukan menggunakan data hasil tangkap dan upaya seperti yang dilakukan diatas. Harvest strategy ini memberikan kepastian informasi berapa jumlah tangkapan atau berapa jumlah upaya yang dapat digunakan sehingga sangat bermanfaat bagi pengambil kebijakan dan industri yang membutuhkan kepastian investasi. Tetapi perlu juga diketahui bahwa metode ini berpotensi dapat mendorong habisnya ikan terutama ketika terjadi ketidakpastian di alam (Punt 2010).
