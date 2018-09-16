---
layout:     post
title:      Alasan Kenapa Harus Menggunakan Firefox Untuk Web Development
subtitle:   
date:       2018-09-15
author:     Bambang S
header-img: img/firefox.png
catalog: false
tags:
    - Firefox
    - Web
    - Javascript
---

Saya tidak tahu berapa usia kamu tapi sebagai seorang yang telah lahir di tahun 90an, saya ingat kemajuan dan kemunduran firefox dari berbagi aspek. Firefox muncul sebagai _open source_ kompetitor terhadap Internet Explorer 6. Jika dibandingan dengan Internet explorer 6, Firefox memiliki kelebihan yaitu, pengguna dapat menambahkan fitur dengan berbagai ekstensi dan bisa mengganti tampilan tema. Semua orang menyukainya.

Beberapa tahun kemudian, ada pemain baru datang yaitu Chrome. Chrome datang dan terus meroket, dengan cepat melampaui kompetitor. Dan faktanya, Chrome telah banyak digunakan oleh banyak orang, dalam waktu yang singkat, Chrome menjadi salah satu diantara produk Google yang paling berharga. Saya yakin kamu membaca artikel ini dengan browser chrome :smirk:

Sebagai developer, saya tahu betapa sulitnya untuk menyenangkan pengguna. Dan juga dengan perkembangan teknologi web yang semakin kompleks, aplikasi dan perangkat lunak pada umumnya bermasalah pada tingginya penggunaan RAM dan CPU ketika diawal-awal. Hal itu mungkin kamu berpikir untuk beralih ke browser selain Chrome.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Close to 7 million lines of changed code later and we present the BRAND NEW FIREFOX.<br><br>Fast, fierce &amp; for good.<a href="https://t.co/jNYcP3XFMy">https://t.co/jNYcP3XFMy</a></p>&mdash; Firefox 🔥 (@firefox) <a href="https://twitter.com/firefox/status/930435170288656384?ref_src=twsrc%5Etfw">November 14, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


Pada tahun 2017, Firefox Quantum dirilis untuk pengguna Android, Linux, IOS, Mac dan Windows. Lebih dari satu dekade setelah Mozilla merilis edisi pertama Firefox yang ikonik. Selain mendapatkan tampilan yang lebih modern, dirumorkan dapat memuat website dua kali lebih cepat daripada Firefox 6 dan juga menggunakan 30% lebih sedikit memori daripada Chrome.

<iframe width="560" height="315" src="https://www.youtube.com/embed/YIywpvHewc0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


Setelah 10 tahun di industri, Mozilla harus mencari cara untuk membedakan Firefox dari kompetitor tanpa kehilangan powernya. Jadi, untuk mengambil keuntungan Firefox menguatkan di pemrosesan komputer, hampir 4 juta baris kode ditulis ulang dengan bantuan teknologi andalanya yaitu [servo](https://servo.org/), [Rust](https://www.rust-lang.org/en-US/), [Quantum Stylo](https://wiki.mozilla.org/Quantum/Stylo).

Menurut pendapat saya, Firefox Qunatum memiliki performa yang bagus jika dibandingkan dengan browser yang lainnya. Dan ini alasannya..

### Teknologi Terbaru

#### [Web Assembly](https://developer.mozilla.org/en-US/docs/WebAssembly)

Menjalankan aplikasi yang besar, cepat, atau aplikasi online yang kompleks merupakan impian kebanyakan developer. Selain itu dengan peningkatan teknologi terkini seperti foto dan video editing, 3D game editing, dan VR/AR, espektasi pengguna online meningkat secara signifikan. _Javascript Engine_ seperti Google V8, Mozilla SpiderMonkey dan Microsoft Chakra telah dikembangkan untuk mendapatkan performa yang cepat untuk operasional.

Sampai saat ini, hal ini juga mendatangkan kemungkinan untuk menjalankan Unity dan Unreal Game Engine didalam Firefox. Dan sekarang, Browser lainnya juga telah memberikan dukungan juga.

#### [A-Frame dan WebVR](https://aframe.io/)

Salah satu hal inovasi terbaru yaitu Virtual Reality atau bisa disebut VR. Dengan perangkat _smartphone_, browser, dan produk seperti Oculus Rift dan HTC Vive, untuk perangkat lain juga terus dibuat. Mozilla telah menjadi pemain di bidang pengembangan infrastruktur WebVR, tapi juga bekerja keras untuk terus meningkatkan performa browsernya. Terima kasih Mozilla telah mensupport A-Frame, salah satu framework yang baik dalam membuat WebVR.

#### [_Project Common Voice_](https://voice.mozilla.org/en/new)

Apple Siri, Microsoft Cortana, Amazon Echo, dan Google Home semuanya mengadopsi _close source_ untuk teknologi _speech recognigtion_ yang dapat mengenal perintah dari suara. Dan apa yang terjadi ? Mozilla sekarang merilis _Common Voice_, sebuah inisiasi _open source_ untuk membuat _voice recognition_ dan tersedia untuk siapapun.

Siapapun dapat berkontribusi untuk _Common Voice_ dengan membaca kalimat dengan suara keras dan mengajarkan isyarat / petunjuk ke mesin. Kamu dapat juga memverifikasi transkrip dari _common voice_ untuk memastikan _recognition engine_ benar pada tracknya.

#### [_Firefox Devtools_](https://mozilladevelopers.github.io/playground/)

Mengikuti perubahan didalam Firefox saya sebutkan diatas, kamu mungkin tidak terkejut untuk mendengar banyak perbaikan yang telah dibuat untuk _DevTools_. Debugger.html salah satunya.

### _General - Inspect Tool_

#### Ganti Tema

![Change Theme](https://bamsarts.github.io/img/devtools.gif)

_Developer tool_ baru hadir dengan tiga opsi tema yang berbeda yaitu, _dark_, _clear_, dan _Firebug_._Firebug tool_ yang cukup populer sekarang masih digunakan oleh banyak orang, meskipun pengembangannya telah terhenti. Bahkan ada pos blog untuk membandingkan penggunaan warna, yang bisa dilihat di [Firefox Nightly News](https://blog.nightly.mozilla.org/2017/09/11/developer-tools-visual-refresh-coming-to-nightly/)


#### _CSS Grid_

![CSS Grid](https://bamsarts.github.io/img/cssgrid.gif)

Satu diantara inovasi terbaru didalam CSS yaitu CSS _Grid Layout_. Dengan _DevTools_, kamu dapat melihat "display:grid" di elemen. Kamu juga dapat dengan mudah mengaktifkan atau menonaktifkan fitur seperti _line numbers, area names, atau extend lines infinitely_. Untuk lebih lanjut bisa kunjungi [https://hacks.mozilla.org/2017/06/new-css-grid-layout-panel-in-firefox-nightly/](https://hacks.mozilla.org/2017/06/new-css-grid-layout-panel-in-firefox-nightly/)

#### BOX Model

![CSS Grid](https://bamsarts.github.io/img/cssgrid.gif)

Nilai margin dan padding didalam elemen dapat sewaktu-waktu seperti puzzle. Dengan struktur box model, kamu dapat dengan mudah melihat dan mengganti seberapa banyak ruang yang terisi dengan properti seperti _margin, padding, dan border_. Untuk lebih lanjut bisa kunjungi [https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Box_model](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Box_model)

#### CSS Variable

![CSS Variables](https://bamsarts.github.io/img/cssVaribale.gif)

Inovasi CSS lainnya yaitu mengenalkan variabel. Meskipun belum semua browser mendukungya, penggunaannya pasti akan meningkat setiap waktu. Seperti namanya, kamu dapat menetapkan apapun nilainya ke variabel. Ingin mengecek berapa nilainya ? caranya mudah cukup menghover mouse ke objeknya. Untuk lebih lanjut bisa kunjungi [https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)

#### _Add/Remove Class_

![Add Class](https://bamsarts.github.io/img/addClass.gif)

Dengan menekan tombol .cls di sisi kanan dari _Dashboard DevTools_, kamu dapat dengan mudah menambahkan dan menghilangkan class CSS dari elemen HTML yang kamu inspect.

#### Hover

![hover](https://bamsarts.github.io/img/hover.gif)

Dengan menekan tombol sebelah kiri .cls, kamu dapat dengan mudah untuk mengetes keadaan _hover, active, dan focus_ di elemen yang sedang dicoba.

#### _Font Family_

Ketika kamu menginpect elemen, kamu dapat juga melihat jenis font yang digunakan di dalam elemen tersebut, dan juga bagaimana font tersebut ditambahkan

#### Animasi

![Animasi](https://bamsarts.github.io/img/animasi.gif)

Animasi merupakan pengembangan populer lainnya. Kamu dapat menjalankan animasi secara perlahan, mempercepat, bahkan mengentikannya.

### Console

#### Console Log

![Console Log](https://bamsarts.github.io/img/console.gif)

Ketika saya pindah ke bagian _console_, kamu dapat dengan mudah memeriksa objek. Plus, struktur keynya yang dapat dengan mudah menseleksi objeknya dan menyembunyikannya.

#### Console.group

![Console Group](https://bamsarts.github.io/img/consoleGroup.gif)

Tahukah kamu, bahwa kamu dapat membuat event lebih teratur dan mudah dibaca dengan console.group() dan groupCollapsed()

#### Breakpoint

![Breakpoint](https://bamsarts.github.io/img/breakpoint.gif)

Alat yang bagus untuk _javascript debugging_ yang sangat diperlukan. Dengan _breakpoint_, kamu dengan cepat menyisipkan _breakpoint_ dan memeriksa _scope_ secara detail.