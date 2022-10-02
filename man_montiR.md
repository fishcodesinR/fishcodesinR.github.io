---
title: Panduan montiR
has_children: true
nav_order: 2
---

## Panduan montiR

`montiR` (Model biOmass diNamik singkaT dI R) merupakan package model biomas dinamik untuk pengelolaan perikanan yang ditulis menggunakan R. Model surplus produksi ini dibangun atas asumsi non-equilibrium yang dipopulerkan penggunaannya oleh Hilborn dan Walters (1992).

Penjelasan mengenai latar belakang, kelebihan dan kekurangan dari tiap metode surplus produksi juga akan dijelaskan sebagai pendahuluan untuk menjelaskan mengenai evolusi metode surplus produksi hingga didapatkan metode yang menghasilkan estimasi lebih baik dengan asumsi non-equilibrium yang dihitung dengan data fitting menggunakan observation error (Polacheck et al., 1993).

Diasumsikan pengguna package ini sudah mengetahui dasar penggunaan R dan sudah menginstal R beserta Rstudio di komputer masing-masing. Diharapkan pengguna paling tidak sudah membaca Chapter 1 dari [buku online ini](https://bookdown.org/moh_rosidi2610/Metode_Numerik/intro.html). 

Langkah analisis yang ditampilkan di website ini dibuat secara ringkas dan sederhana untuk pengguna R pemula. Untuk yang tertarik mengeksplor lebih lanjut mengenai kode R yang digunakan untuk melakukan analisis surplus produksi ini, dipersilahkan untuk membuka [link github](https://github.com/habeebollah/montiR) dan mempelajari langkah-langkah analisis yang digunakan. Kode dibuat sesederhana mungkin agar dapat dengan mudah dipahami oleh pengguna R pemula.

## Instalasi
Anda dapat mengunduh dan menginstal package ini dengan mengetikkan kode ini pada konsole di Rstudio:
```
devtools::install_github("habeebollah/montiR")
```

Setelah terinstal, setiap kali akan menjalankan package ini pengguna perlu mengaktifkan dengan mengetikkan
```markdown
library("montiR")
```


## Referensi
Adams, P. 1980. Life history patterns in marine fishes and their consequences for fisheries management, Fishery Bulletin, 78(1), pp. 1–12.

Fox Jr, W. W. 1970. An exponential surplus‐yield model for optimizing exploited fish populations. Transactions of the American Fisheries Society, 99(1), 80-88.

Hilborn, Ray, and Carl J. Walters, eds. Quantitative fisheries stock assessment: choice, dynamics and uncertainty. Springer Science & Business Media, 1992.

Kawasaki, T. 1980. Fundamental Relations among the Selections of Life History in the Marine Teleosts, Bulletin of the Japanese Society of Scientific Fisheriess, 463(3), pp. 289–293.

Kawasaki, T. 1983. Why do some pelagic fishes have wide fluctuations in their numbers? Biological basis of fluctuation from the viewpoint of evolutionary ecology, FAO Fisheries Report (FAO).

Magnusson, A., & Hilborn, R. 2007. What makes fisheries data informative?. Fish and Fisheries, 8(4), 337-358.
Pella, J. J., & Tomlinson, P. K. 1969. A generalized stock production model.

Polacheck, T., Hilborn, R., and A.E. Punt. 1993. Fitting surplus production models: Comparing methods and measuring uncertainty. Canadian Journal of Fisheries and Aquatic Sciences, 50: 2597-2607.

Punt, A. E. 2010. Harvest control rules and fisheries management. Handbook of marine fisheries conservation and management, 582-594.

Punt, A. E., & Hilborn, R. 1996. Biomass dynamic models. FAO Computerized Information Series Fisheries, 10, 1-62.

Schaefer, M. B. 1954. Some aspects of the dynamics of populations important to the management of the commercial marine fisheries.

Schaefer, M. B. 1957. A study of the dynamics of the fishery for yellow fin tuna in the eastern tropical Pacific Ocean. Bull. Inter-Amer. Trop. Tuna Comm., 2, 247-285.

Sparre, P., & Venema, S. C. 1998. Introduction to fish stock assessment. Part 1: Manual. FAO Fisheries Technical Paper, 306(1). 

Uhler, R. S. 1980. Least squares regression estimates of the Schaefer production model: some Monte Carlo simulation results. Canadian Journal of Fisheries and Aquatic Sciences, 37(8), 1284-1294.

Walters, C. J., & Hilborn, R. 1976. Adaptive control of fishing systems. Journal of the Fisheries Board of Canada, 33(1), 145-159.


