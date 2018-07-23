---
layout:     post
title:      Apakah React Native Layak dipelajari ?
subtitle:   Layak atau tidak, pasti ada hikmahnya :)
date:       2018-05-02
author:     Bambang S
header-img: img/Accenture-Full-Home.jpg
catalog: true
tags:
    - reactjs
    - reactnative
    - javascript
---

> Apa yang anda baca adalah pengalaman saya dengan React Native setelah selesainya mini proyek :)

ini adalah keuntungan dari React Native dari pembicaraan orang-orang di internet yang saya rangkum


- Performa kinerja aplikasi hampir setara dengan platform aslinnya (e.g android studio)
- Tidak perlu menduplikasi _business logic_ dan UI khususnya untuk setiap platform
- Cukup bisa bahasa javascript dapat _Multiplatform IOS & Android_
- Menghemat waktu dan biaya yang signifikan

Apa yang mereka katakan, React Native tampaknya sangat rendah pada kerugian sejauh ini. Sebagian besar dari mereka disebut dalam konteks "ini masih merupakan teknologi yang cukup". Lebih lanjut, saya akan mencoba untuk mengkonfirmasi poin ini atau menghilangkan prasangka dari mereka.

## _Project Toolbox_

Jadi ini adalah teknologi dan alat yang digunakan dalam proyek

## _Environment_

- _Atom + Nuclide + Flow + ESlint +_ sejumlah paket helper yang membantu. Awalnya saya mencoba untuk mencari tahu mana yang akan berguna dan mana yang tidak, tetapi kemudian saya bosan dengan itu dan menginstal semua yang mereka sarankan di tutorial dan panduan bantuan.
- Saya sangat menyarankan pengaturan Alur / _Flow_. Anda tidak ingin berakhir tanpa pemeriksaan _static type_.
- _ESlint linter_ juga harus disiapkan. Ada beberapa linter lainnya, Anda pasti membutuhkannya, jika tidak, kualitas kode JS Anda akan meninggalkan banyak hal yang diinginkan.
_ _Xcode, Android Studio_
- _Npm_ atau _yarn_ adalah paket manajer. Seperti yang saya temukan pada saat _development_, _yarn_ jauh lebih stabil jadi saya sarankan menggunakan _yarn_.
- _Firebase Authentication, Storage, Realtime Database_.


### _Framework & Library_

- _React Native_
- _Redux + Saga_. Investasikan waktu untuk mempelajari dua hal ini. Redux sempurna untuk menyimpan _state_ dan _Saga_ bagus dalam menerapkan berbagai efek. Keduanya bekerja dengan baik dalam hubungannya dan secara signifikan meningkatkan pengembangan dan pemeliharaan proyek.

#### Kesalahan Pertama

Di sinilah saya mengalami kesulitan untuk pertama kalinya. Saya hanya menguji untuk iOS dan setelah meluncurkan aplikasi di Android, saya menyadari beberapa gaya yang berfungsi dengan baik di iOS rusak untuk Android. Dalam banyak kasus, untuk memperbaiki masalah atau mencari solusi, yang Anda butuhkan hanyalah menggali bagian "masalah" di GitHub dan mencoba beberapa metode disana.

Namun, keberhasilan ini mungkin tergantung pada berbagai faktor dari kombinasi gaya hingga hierarki komponen dalam metode render Anda. Ini tidak mengganggu saya, karena yang harus saya lakukan hanyalah menguji kemampuan setiap komponen di semua platform. Bahkan, mencari solusi untuk jenis masalah ini secara akumulatif membutuhkan waktu terlalu banyak.

#### Library Pihak Ketiga

Ketika saya pindah, saya menemukan bahwa komponen React Native standar tidak cukup tetapi ada banyak _library_ dan komponen pihak ketiga khusus untuk React Native. Dan memang ada sesuatu untuk dipilih, sementara kualitas dan stabilitas, mayoritas komponen meninggalkan banyak hal yang diinginkan. Bahkan koneksi ke banyak komponen iOS berubah menjadi pencarian yang serius. Untuk Android, perintah _react-native link_ biasanya cukup. Saya membuat tahapan berikut:

- Instal seperti yang direkomendasikan untuk paket. Seperti ini _yarn add + pod install + combo react–native link_.
- Jika ini tidak berhasil, coba lakukan hal yang sama secara manual tetapi tanpa tautan _react-native link_ tetapi dengan penggunaan _cocoapods_.
- Jika ini tidak berhasil juga, cobalah menyalin semua dependensi asli dari _library_ ke dalam proyek Anda.
- Jika langkah sebelumnya gagal juga, ambil seluruh _library_, salin ke folder terpisah, temukan masalah di pengaturan _library_, perbaiki, lalu hubungkan dengan bantuan _yarn add_ dan _local path_ ke posisi _library_ aslinya.

Menggunakan banyak komponen sumber terbuka / _open source_ ini membuat saya merasa seperti seorang pencari ranjau di ladang ranjau tanpa tahu di mana ledakan berikutnya akan terjadi. Untuk membuatnya lebih buruk, satu komponen dapat bekerja dengan baik pada satu platform dan gagal pada yang lain, seperti di bawah _JS Wrapper_ pada umumnya, _library_ asli untuk setiap platform dapat berbeda dan begitu juga _behavior_ nya. Dengan demikian, bug mungkin berada di _library asli_ atau di _JS Wrapper__ (jika ada logika dan itu lebih dari sekadar pemetaan metode), atau bahkan di wrapper asli dari _native library_.

Anda mungkin bertanya, mengapa tidak menulis komponen dan _library sendiri_ ? Tetapi, apa gunanya menggunakan React Native? saya berada dalam skenario proyek bisnis dan mencoba menghemat waktu / uang untuk pengembangan, bukankah begitu ?

#### Fitur Pembunuh

React Native Hot / Live reload adalah fitur yang memungkinkan untuk menulis kode dan segera melihat perubahan tanpa membangun kembali aplikasi secara lengkap. Hal ini, fitur ini sangat penting untuk React Native karena ketidakpastian dan kelayakan paket yang terakhir dan pihak ketiga, di mana Anda harus selalu memeriksa hasil tindakan yang Anda ambil.

Mengembangkan pada platform asli tidak memerlukan banyak build, biasanya pemeriksaan device / emulator dilakukan setelah fungsionalitas selesai atau hampir selesai. Di antaranya, Anda tinggal menulis kode dan tidak terlalu takut untuk memberikan hasil yang berbeda dari yang Anda harapkan.

### Pengalaman Saya dengan React Native

- __Performance__ hampir setara dengan platform aslinya. Benar-benar begitu. Hingga Anda memiliki UI sederhana, tidak ada perhitungan dan pemrosesan yang rumit, dan jumlah komponennya rendah. Jika ini bukan fitur proyek Anda, tanpa ragu Anda akan menghadapi masalah produktivitas dan penggunaan memori. Beberapa dari mereka dapat diselesaikan dengan menggunakan _Saga_, sebagian _Debounce_ dan _Throttle_ ditambah partisi komponen yang benar untuk menghindari rendering ulang dari segala sesuatu pada perubahan terkecil. Beberapa masalah tidak akan selesai dan Anda harus menghadapinya.

- __Tidak perlu menduplikasi__ logika bisnis dan UI khususnya untuk setiap platform. Basis kode disatukan. Benar. Sekali lagi, jika logika bisnis Anda tidak terlalu rumit atau memakan banyak sumber daya dan integrasi platform asli tidak melampaui _React Native_. Juga, jika UI identik untuk kedua platform. Namun, sebagian besar proyek besar tidak memenuhi kedua kondisi ini, UI beradaptasi dengan spesifikasi platform, dan logika aplikasi membutuhkan setidaknya dukungan konkurensi. Selain itu, jika Anda memerlukan beberapa fungsi yang Bereaksi Tidak ada bawaan, Anda harus menerapkannya secara terpisah untuk setiap platform.

- __Tidak perlu tahu beberapa bahasa__, JavaScript hanya cukup. Maaf kawan, tidak kali ini. Menyimpang dari jalur normal, dan proyek akan membutuhkan pengetahuan mendalam tentang platform asli dari Anda. Jika Anda tidak memilikinya, Anda akan berakhir dengan proyek yang bekerja pada kekuatan misterius dan Tuhan yang tahu caranya. Dan jika Anda memiliki pengetahuan khusus, dan proyek Anda memiliki desain khusus, kinerja, dan persyaratan stabilitas, lalu mengapa Anda mengatur diri sendiri untuk membatasi dan kerumitan React Native ?

- __Tulis satu source code__ dan dengan senang hati berjalan di berbagai platform. Nggak. Saya harus mengatakan meskipun, perasaan senang melihat aplikasi tidak crash dalam keadaan ini lebih kuat daripada _native development_.

- __React Native tumbuh dengan pesat__. Benar-benar, bahkan terlalu cepat. Bayangkan Anda sedang mengerjakan sebuah proyek dan di suatu tempat di tengah-tengah pengembangan, Anda menemukan kesalahan kritis dalam library pihak ketiga, atau lebih buruk di _React Native_ itu sendiri. Tapi ini tidak begitu banyak masalah karena versi yang lebih baru secara teratur dirilis dan kemungkinan kesalahan mungkin diperbaiki dalam versi yang lebih baru. Yang perlu Anda lakukan hanyalah memperbarui ke versi terbaru. Di mana ini bisa membawa kita ? Benar, pengujian regresif dari seluruh aplikasi. Ini setengah masalahnya. Jika keberuntungan Anda gagal, komponen dan _library_ pihak ketiga mungkin berubah menjadi tidak kompatibel dengan versi React Native yang lebih baru.

- __Waktu & penghematan biaya yang signifikan__. Ini adalah titik yang paling diperdebatkan, saya percaya. Jika Anda ingin menerapkan prototipe cepat atau MVP kecil tanpa persyaratan khusus yang berkualitas, dalam stabilitas, dan kinerja. Jika Anda tidak memerlukan adaptasi UI ke platform, atau aplikasi Anda tidak berat pada fungsi platform spesifik, Anda dapat memperoleh manfaat dari Bawaan Bereaksi. Dalam kasus lain, Anda tidak hanya gagal menghemat waktu, tetapi juga membuang banyak energi pengembang pada produk yang tidak akan mendekati _native_. Jika Anda tidak dilengkapi dengan pengembang iOS / Android, tetapi Anda dalam pengembangan mobile, pengembang front-end dengan React Native adalah pilihan untuk Anda.


#### Kesimpulan


Teknologi baru menuju _cross-platform_ dan pengurangan biaya adalah hal yang indah. Namun, alat untuk mencapai itu harus dipilih tergantung pada tugas. Terlepas dari seberapa bagus dan populernya itu, itu harus relevan. Sebagai contoh :

- Tidak perlu menggunakan React Native lebih awal untuk proyek-proyek besar dengan integrasi mendalam ke platform asli dan standar tinggi untuk stabilitas dan kinerja. Ini tidak akan menghemat biaya.

- Saya mendorong Anda untuk tidak menggunakan solusi _serverless_ (Firebase, AWS Mobile Hub) untuk sistem yang memuat berat dengan logika yang rumit, yaitu di mana harus ada _backend_ yang lengkap dengan API khusus.

Semua upaya ini biasanya gagal dan sebagai hasilnya menyebabkan lebih banyak biaya tambahan untuk memperbaiki dampak dari pilihan awal yang salah.