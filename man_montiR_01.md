---
title: Perkembangan
parent: Panduan montiR
nav_order: 1
---

## Perkembangan Metode Surplus Production 
### a. Surplus Production dengan asumsi equilibrium

Pendekatan ini akan memberikan estimasi MSY dan Emsy yang tinggi (Hilborn and Walter, 1992; Polacheck, et al. 1993), sehingga sangat tidak disarankan untuk dijadikan sebagai panduan dalam pengambilan kebijakan perikanan. Overestimasi reference point pada kondisi ketika status perikanan sedang dalam kondisi overexploited akan memberikan ilusi bahwa stok ikan masih banyak, sehingga dapat merugikan pelaku perikanan karena jumlah tangkapan yang rendah dan merugikan stok ikan karena semakin tingginya pemanfaatan. 

Penghitungan surplus production dengan menggunakan asumsi equilibrium menghitung MSY dan Emsy dengan linear regression sederhana (Sparre dan Venema, 1998) ataupun dengan menggunakan metode non-linear (Punt dan Hilborn, 1996). Selain menghitung MSY dan Emsy, penghitungan confident interval dan r-squared yang diterapkan pada metode linear regression juga disarankan untuk juga dilakukan.

Metode ini akan selalu menghasilkan angka MSY dan Emsy meskipun data yang digunakan berkualitas rendah dengan sedikit kontras (Hilborn dan Walters, 1992) ataupun memiliki data time series terbatas (Sparre dan Venema, 1998). Sebagaimana yang dijelaskan oleh Hilborn dan Walters (1992), biasanya nilai r-squared dari linear regression menunjukkan bahwa relasi antara CPUE dengan effort sangat tinggi sehingga memberikan kesan bahwa analisa yang dihasilkan dari metode ini benar secara statistik.


### b. Surplus production dengan asumsi non-equilibrium menggunakan multiple regression

Metode multiple regression merupakan metode selanjutnya yang digunakan untuk menghitung jumlah tangkapan ikan lestari (MSY) dan upaya penangkapan ikan lestari (Emsy) dengan model Schaefer. Metode ini menggunakan asumsi non-equilibrium dengan pendekatan least square (Walters and Hilborn, 1976) dan disebut menghasilkan bias dalam estimasi parameter surplus production, termasuk juga menghasilkan bias dalam proses lanjutan ketika parameter yang diestimasi digunakan untuk menghitung MSY dan Emsy (Uhler, 1979). Berdasar masukan ini, kemudian input yang digunakan dalam penghitungan multiple regression direvisi oleh Hilborn dan Walters (1992).

Penggunaan metode multiple regression ini sangat mudah dan cenderung akan mencari hubungan antar parameter yang diestimasi serta menghasilkan nilai r squared yang tinggi. Dapat dilihat bahwa metode multiple regression pasti akan menghasilkan estimasi, meskipun terkadang hasilnya terlihat kurang dapat dipercaya. Misalnya angka r (intrinsic growth parameter) yang minus. Hal ini terjadi karena metode surplus produksi dengan penghitungan menggunakan regression disebut memiliki kelemahan jika menggunakan data yang memiliki tipe one way trip (Hilborn dan Walters, 1992).


### c. Surplus production dengan asumsi non-equilibrium menggunakan time series data fitting yang mempertimbangkan observation error

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



