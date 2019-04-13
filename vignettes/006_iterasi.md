---
title: "Iterasi"
author: "Muhammad Aswan Syahputra"
date: "4/9/2019"
output:
  html_document: 
    df_print: default
    fig_height: 6
    fig_width: 9
    highlight: textmate
    keep_md: yes
    theme: yeti
    toc: yes
    toc_collapsed: yes
    toc_float: yes
editor_options: 
  chunk_output_type: inline
---



Di bahasa pemrograman R, iterasi atau perulangan dapat dilakukan dengan beberapa cara, misalnya menggunakan `for loop`, keluarga `apply` (dari paket `base`), dan keluarga `map` (dari paket `purrr`).

Dalam contoh kasus ini, Anda diminta untuk mengimpor beberapa berkas `csv` secara sekaligus dan membuatnya ke dalam satu dataframe. Aktifkanlah paket yang memiliki fungsi untuk membuka berkas `csv`!


```r
library(readr)
```

```
## Warning: package 'readr' was built under R version 3.5.3
```

Berkas csv yang akan Anda impor memiliki informasi mengenai data lelang kota Bandung selama beberapa tahun, dimulai dari tahun 2013 hingga 2017. Data masing-masing tahun disimpan dalam berkas csv yang berbeda dan tersedia pada direktori 'data-raw'. Melalui '*File pane*' di RStudio (kanan bawah), Anda dapat bernavigasi ke direktori tersebut. Apakah nama berkas csv yang akan segera Anda impor? Bagaimana pola penamaan berkas-berkas tersebut?

Selain melihat nama berkas secara manual, Anda juga dapat menggunakan fungsi `list.files()` melalui R untuk membuat daftar nama berkas dari suatu direktori. Dapatkah Anda membuat daftar nama berkas yang Akan diimpor dengan menggunakan fungsi tersebut? Gunakan pola penamaan berkas yang telah Anda pelajari sebagai isian untuk argumen `pattern` pada fungsi `list.files`!


```r
daftar_berkas <- list.files("../data-raw", pattern = "lelang-bandung", full.names = TRUE)
daftar_berkas
```

```
## [1] "../data-raw/001_lelang-bandung_2013.csv"
## [2] "../data-raw/002_lelang-bandung_2014.csv"
## [3] "../data-raw/003_lelang-bandung_2015.csv"
## [4] "../data-raw/004_lelang-bandung_2016.csv"
## [5] "../data-raw/005_lelang-bandung_2017.csv"
```

Anda telah memiliki daftar nama berkas yang akan diimpor. Sekarang Anda akan mempelajari cara membuka berkas tersebut secara sekaligus melalui iterasi.

## For Loop

Cara pertama yang akan Anda pelajari adalah dengan menggunakan `for loop`. Prinsip dan struktur penggunaannya adalah sebagai berikut:

```
input <- sesuatu

output <- vector(mode, length)      # 1. Output
for (i in seq_along(output)) {      # 2. Urutan
  output[[i]] <- fungsi(input[[i]]) # 3. Badan
}
output
```

Dapatkah Anda melengkapi kode berikut untuk mengimpor berkas-berkas csv yang tercatat dalam obyek 'daftar_berkas'? Simpanlah `output` dengan nama `output_for`! Anda juga dapat membuka obyek `output_for` pada konsol untuk dapat melihat struktur data tersebut dengan lebih jelas.


```r
daftar_berkas
```

```
## [1] "../data-raw/001_lelang-bandung_2013.csv"
## [2] "../data-raw/002_lelang-bandung_2014.csv"
## [3] "../data-raw/003_lelang-bandung_2015.csv"
## [4] "../data-raw/004_lelang-bandung_2016.csv"
## [5] "../data-raw/005_lelang-bandung_2017.csv"
```

```r
output_for <- vector(mode = "list", length = length(daftar_berkas))
for (i in seq_along(output_for)) {
  output_for[[i]] <- read_csv(daftar_berkas[[i]])
}
output_for
```

```
## [[1]]
## # A tibble: 680 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>       <lgl>    <date>           <lgl>     
##  1        5260 J5-Peningk~ NA       2013-02-19       NA        
##  2      171260 B2-Peningk~ NA       2013-02-21       NA        
##  3      374260 J126-Penin~ NA       2013-04-24       NA        
##  4      452260 J140-Penin~ NA       2013-05-19       NA        
##  5      521260 Belanja Mo~ NA       2013-05-20       NA        
##  6      571260 Pengadaan ~ NA       2013-06-11       NA        
##  7      727260 Pembanguna~ NA       2013-08-07       NA        
##  8      628260 SS-06 Bang~ NA       2013-06-25       NA        
##  9      772260 Belanja Mo~ NA       2013-08-13       NA        
## 10      889260 PKP1-Penga~ NA       2013-09-18       NA        
## # ... with 670 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[2]]
## # A tibble: 763 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <lgl>     
##  1     1002260 Jasa Konsu~       NA 2014-02-21       NA        
##  2     1003260 Jasa Konsu~   625017 2014-02-21       NA        
##  3     1008260 Pengoperas~       NA 2014-03-03       NA        
##  4     1007260 Pengoperas~       NA 2014-03-03       NA        
##  5     1009260 Jasa Konsu~   792674 2014-03-10       NA        
##  6     1013260 Kajian Mod~       NA 2014-03-12       NA        
##  7     1014260 Belanja Pr~       NA 2014-03-14       NA        
##  8     1016260 Jasa Tenag~       NA 2014-03-18       NA        
##  9     1015260 Jasa Konsu~       NA 2014-03-18       NA        
## 10     1017260 Jasa Tenag~       NA 2014-03-18       NA        
## # ... with 753 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[3]]
## # A tibble: 638 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <lgl>     
##  1     2161260 Belanja Ba~       NA 2015-01-06       NA        
##  2     2164260 Pengadaan ~  2558197 2015-02-09       NA        
##  3     2166260 Perawatan ~       NA 2015-02-10       NA        
##  4     2165260 Perawatan ~       NA 2015-02-10       NA        
##  5     2163260 "Pengadaan~  2837522 2015-02-12       NA        
##  6     2173260 Peningkata~  3024560 2015-02-17       NA        
##  7     2174260 Peningkata~  3024515 2015-02-17       NA        
##  8     2178260 Peningkata~  3024663 2015-02-17       NA        
##  9     2172260 Peningkata~  3024534 2015-02-17       NA        
## 10     2167260 Peningkata~  3024431 2015-02-17       NA        
## # ... with 628 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[4]]
## # A tibble: 785 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1     2994260 Belanja Ba~  5035789 2015-12-11       <NA>      
##  2     2995260 Belanja Ja~  5026929 2015-12-11       <NA>      
##  3     2996260 Belanja Ma~  5027086 2015-12-11       <NA>      
##  4     2997260 Belanja Ja~  5027006 2015-12-11       <NA>      
##  5     2998260 Belanja Ja~  5037194 2015-12-11       <NA>      
##  6     2999260 Belanja Ja~  5037172 2015-12-11       <NA>      
##  7     3000260 Belanja Ja~  5037210 2015-12-11       <NA>      
##  8     3007260 Jasa Penga~  5054575 2015-12-11       <NA>      
##  9     3013260 Penyediaan~  5057427 2015-12-11       <NA>      
## 10     3014260 Pengadaan ~  5043143 2016-01-12       <NA>      
## # ... with 775 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[5]]
## # A tibble: 414 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1     4542260 Pengadaan ~ 11633430 2017-09-08       <NA>      
##  2     4135260 Belanja Al~ 11005893 2017-05-03       <NA>      
##  3     4244260 Belanja Ja~ 11367750 2017-06-09       <NA>      
##  4     4462260 Belanja Ja~ 12437947 2017-08-11       <NA>      
##  5     4393260 Belanja Ja~ 11999277 2017-07-27       <NA>      
##  6     4231260 Belanja ja~ 12162438 2017-05-31       <NA>      
##  7     4233260 Belanja ja~ 12156963 2017-05-31       <NA>      
##  8     4234260 Belanja ja~ 12150139 2017-05-31       <NA>      
##  9     4232260 Belanja ja~ 12150318 2017-05-31       <NA>      
## 10     4238260 Belanja ja~ 12156804 2017-05-31       <NA>      
## # ... with 404 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <dbl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
```

Selamat! Anda telah berhasil mengimpor berkas-berkas csv tersebut. Namun, jika diperhatikan tipe dari obyek 'output_for' adalah berupa list sedangkan yang diinginkan adalah berupa dataframe. Anda dapat menggunakan fungsi `rbind()` untuk menggabungkan dua dataframe, namun pada kasus ini Anda memiliki lima dataframe sehingga fungsi `rbind()` tidak bisa langsung digunakan. Untuk menggabungkan lebih dari dua dataframe, Anda dapat menggunakan bantuan dari fungsi `Reduce` seperti contoh berikut:


```r
typeof(output_for) # cek tipe dari output_for
```

```
## [1] "list"
```

```r
output_for <- Reduce(rbind, output_for)
output_for
```

```
## # A tibble: 3,280 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1        5260 J5-Peningk~       NA 2013-02-19       <NA>      
##  2      171260 B2-Peningk~       NA 2013-02-21       <NA>      
##  3      374260 J126-Penin~       NA 2013-04-24       <NA>      
##  4      452260 J140-Penin~       NA 2013-05-19       <NA>      
##  5      521260 Belanja Mo~       NA 2013-05-20       <NA>      
##  6      571260 Pengadaan ~       NA 2013-06-11       <NA>      
##  7      727260 Pembanguna~       NA 2013-08-07       <NA>      
##  8      628260 SS-06 Bang~       NA 2013-06-25       <NA>      
##  9      772260 Belanja Mo~       NA 2013-08-13       <NA>      
## 10      889260 PKP1-Penga~       NA 2013-09-18       <NA>      
## # ... with 3,270 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <dbl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
```

## Keluarga `apply`

Sekarang Anda akan menggunakan jenis iterasi yang umumnya digunakan di R, yaitu menggunakan keluarga `apply` (`apply`, `lapply`, `sapply`, `vapply`, dan seterusnya). Dalam contoh ini Anda akan menggunakan fungsi `lapply()` dengan struktur penulisan seperti berikut:

```
input <- sesuatu
output <- lapply(input, nama_fungsi)
```

Silakan lengkapi kode berikut untuk mengimpor berkas csv dengan menggunakan fungsi `lapply()`! Simpanlah `output` dengan nama `output_lapply`.


```r
daftar_berkas
```

```
## [1] "../data-raw/001_lelang-bandung_2013.csv"
## [2] "../data-raw/002_lelang-bandung_2014.csv"
## [3] "../data-raw/003_lelang-bandung_2015.csv"
## [4] "../data-raw/004_lelang-bandung_2016.csv"
## [5] "../data-raw/005_lelang-bandung_2017.csv"
```

```r
output_lapply <- lapply(daftar_berkas,read_csv)
output_lapply
```

```
## [[1]]
## # A tibble: 680 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>       <lgl>    <date>           <lgl>     
##  1        5260 J5-Peningk~ NA       2013-02-19       NA        
##  2      171260 B2-Peningk~ NA       2013-02-21       NA        
##  3      374260 J126-Penin~ NA       2013-04-24       NA        
##  4      452260 J140-Penin~ NA       2013-05-19       NA        
##  5      521260 Belanja Mo~ NA       2013-05-20       NA        
##  6      571260 Pengadaan ~ NA       2013-06-11       NA        
##  7      727260 Pembanguna~ NA       2013-08-07       NA        
##  8      628260 SS-06 Bang~ NA       2013-06-25       NA        
##  9      772260 Belanja Mo~ NA       2013-08-13       NA        
## 10      889260 PKP1-Penga~ NA       2013-09-18       NA        
## # ... with 670 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[2]]
## # A tibble: 763 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <lgl>     
##  1     1002260 Jasa Konsu~       NA 2014-02-21       NA        
##  2     1003260 Jasa Konsu~   625017 2014-02-21       NA        
##  3     1008260 Pengoperas~       NA 2014-03-03       NA        
##  4     1007260 Pengoperas~       NA 2014-03-03       NA        
##  5     1009260 Jasa Konsu~   792674 2014-03-10       NA        
##  6     1013260 Kajian Mod~       NA 2014-03-12       NA        
##  7     1014260 Belanja Pr~       NA 2014-03-14       NA        
##  8     1016260 Jasa Tenag~       NA 2014-03-18       NA        
##  9     1015260 Jasa Konsu~       NA 2014-03-18       NA        
## 10     1017260 Jasa Tenag~       NA 2014-03-18       NA        
## # ... with 753 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[3]]
## # A tibble: 638 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <lgl>     
##  1     2161260 Belanja Ba~       NA 2015-01-06       NA        
##  2     2164260 Pengadaan ~  2558197 2015-02-09       NA        
##  3     2166260 Perawatan ~       NA 2015-02-10       NA        
##  4     2165260 Perawatan ~       NA 2015-02-10       NA        
##  5     2163260 "Pengadaan~  2837522 2015-02-12       NA        
##  6     2173260 Peningkata~  3024560 2015-02-17       NA        
##  7     2174260 Peningkata~  3024515 2015-02-17       NA        
##  8     2178260 Peningkata~  3024663 2015-02-17       NA        
##  9     2172260 Peningkata~  3024534 2015-02-17       NA        
## 10     2167260 Peningkata~  3024431 2015-02-17       NA        
## # ... with 628 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[4]]
## # A tibble: 785 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1     2994260 Belanja Ba~  5035789 2015-12-11       <NA>      
##  2     2995260 Belanja Ja~  5026929 2015-12-11       <NA>      
##  3     2996260 Belanja Ma~  5027086 2015-12-11       <NA>      
##  4     2997260 Belanja Ja~  5027006 2015-12-11       <NA>      
##  5     2998260 Belanja Ja~  5037194 2015-12-11       <NA>      
##  6     2999260 Belanja Ja~  5037172 2015-12-11       <NA>      
##  7     3000260 Belanja Ja~  5037210 2015-12-11       <NA>      
##  8     3007260 Jasa Penga~  5054575 2015-12-11       <NA>      
##  9     3013260 Penyediaan~  5057427 2015-12-11       <NA>      
## 10     3014260 Pengadaan ~  5043143 2016-01-12       <NA>      
## # ... with 775 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <lgl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
## 
## [[5]]
## # A tibble: 414 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1     4542260 Pengadaan ~ 11633430 2017-09-08       <NA>      
##  2     4135260 Belanja Al~ 11005893 2017-05-03       <NA>      
##  3     4244260 Belanja Ja~ 11367750 2017-06-09       <NA>      
##  4     4462260 Belanja Ja~ 12437947 2017-08-11       <NA>      
##  5     4393260 Belanja Ja~ 11999277 2017-07-27       <NA>      
##  6     4231260 Belanja ja~ 12162438 2017-05-31       <NA>      
##  7     4233260 Belanja ja~ 12156963 2017-05-31       <NA>      
##  8     4234260 Belanja ja~ 12150139 2017-05-31       <NA>      
##  9     4232260 Belanja ja~ 12150318 2017-05-31       <NA>      
## 10     4238260 Belanja ja~ 12156804 2017-05-31       <NA>      
## # ... with 404 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <dbl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
```

Dapatkah Anda memeriksa jenis dari obyek `output_lapply`? Tuliskanlah kode untuk mengubah `output_lapply` menjadi satu dataframe dan simpanlah hasilnya dengan nama `output_lapply`!


```r
typeof(output_lapply) # cek jenis dari output_lapply
```

```
## [1] "list"
```

```r
output_lapply <- Reduce(rbind, output_lapply)
output_lapply
```

```
## # A tibble: 3,280 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1        5260 J5-Peningk~       NA 2013-02-19       <NA>      
##  2      171260 B2-Peningk~       NA 2013-02-21       <NA>      
##  3      374260 J126-Penin~       NA 2013-04-24       <NA>      
##  4      452260 J140-Penin~       NA 2013-05-19       <NA>      
##  5      521260 Belanja Mo~       NA 2013-05-20       <NA>      
##  6      571260 Pengadaan ~       NA 2013-06-11       <NA>      
##  7      727260 Pembanguna~       NA 2013-08-07       <NA>      
##  8      628260 SS-06 Bang~       NA 2013-06-25       <NA>      
##  9      772260 Belanja Mo~       NA 2013-08-13       <NA>      
## 10      889260 PKP1-Penga~       NA 2013-09-18       <NA>      
## # ... with 3,270 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <dbl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
```

## Keluarga `map`

Terakhir, Anda akan melakukan impor data dengan menggunakan keluarga `map` dari paket `purrr`. Adapun fungsi yang akan digunakan adalah `map_dfr` (silakan baca dokumentasi dengan menjalankan `?map_dfr`). Struktur penulisan kode di keluarga `map` serupa dengan penulisan kode di keluarga `apply`, yaitu sebagai berikut:

```
input <- sesuatu
output <- lapply(input, nama_fungsi)
```

Lengkapi kode berikut untuk mengimpor berkas csv dengan menggunakan `map_dfr` dan simpanlah `output` dengan nama `output_map`. Dapatkah Anda menemukan perbedaan antara hasil dari fungsi `map_dfr` dengan `for loop` dan `lapply` pada contoh sebelumnya? Jalankan `class()` pada `output_map`. Mengapa kita tidak perlu menggunakan `rbind` dan `Reduce`?


```r
library(purrr) # aktifkan paket purrr
```

```
## Warning: package 'purrr' was built under R version 3.5.3
```

```r
daftar_berkas
```

```
## [1] "../data-raw/001_lelang-bandung_2013.csv"
## [2] "../data-raw/002_lelang-bandung_2014.csv"
## [3] "../data-raw/003_lelang-bandung_2015.csv"
## [4] "../data-raw/004_lelang-bandung_2016.csv"
## [5] "../data-raw/005_lelang-bandung_2017.csv"
```

```r
output_map <- map_dfr(daftar_berkas,read_csv)
output_map
```

```
## # A tibble: 3,280 x 32
##    kode_lelang nama_lelang kode_rup tanggal_pembuat~ keterangan
##          <dbl> <chr>          <dbl> <date>           <chr>     
##  1        5260 J5-Peningk~       NA 2013-02-19       <NA>      
##  2      171260 B2-Peningk~       NA 2013-02-21       <NA>      
##  3      374260 J126-Penin~       NA 2013-04-24       <NA>      
##  4      452260 J140-Penin~       NA 2013-05-19       <NA>      
##  5      521260 Belanja Mo~       NA 2013-05-20       <NA>      
##  6      571260 Pengadaan ~       NA 2013-06-11       <NA>      
##  7      727260 Pembanguna~       NA 2013-08-07       <NA>      
##  8      628260 SS-06 Bang~       NA 2013-06-25       <NA>      
##  9      772260 Belanja Mo~       NA 2013-08-13       <NA>      
## 10      889260 PKP1-Penga~       NA 2013-09-18       <NA>      
## # ... with 3,270 more rows, and 27 more variables: tahapan_lelang <chr>,
## #   instansi <chr>, satuan_kerja <chr>, kategori <chr>,
## #   metode_pengadaan <chr>, metode_kualifikasi <chr>,
## #   metode_dokumen <chr>, metode_evaluasi <chr>, tahun_anggaran <chr>,
## #   tahun <dbl>, pagu <dbl>, hps <dbl>, cara_pembayaran <chr>,
## #   pembebanan_ta <chr>, sumber_pendanaan <chr>, lokasi <chr>,
## #   kualifikasi_usaha <chr>, peserta_lelang <dbl>, agency <chr>,
## #   satker <chr>, nama_pemenang <chr>, alamat <chr>, npwp <chr>,
## #   hasil_negosiasi <dbl>, harga_penawaran <dbl>, harga_terkoreksi <dbl>,
## #   gagal_lelang <chr>
```

```r
class(output_map)
```

```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```
