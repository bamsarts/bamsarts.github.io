---
layout:     post
title:      Memaksimalkan Aplikasi React
subtitle:   
date:       2018-05-06
author:     bamsarts
header-img: img/post-2-header.jpg
catalog: true
tags:
    - react
    - reactnative
    - javascript
---


Saya telah mengerjakan aplikasi web yang cukup besar beberapa tahun terakhir. Mmbuat dari titik nol, dengan teman-teman developer yang dapat _scale_ sampai sekarang. Mungkin sekarang sudah digunakan oleh jutaan pengguna :satisfied:. Dalam membangun aplikasi React terkadang jika tidak memulai dengan struktur folder yang baik, ini dapat mengalami kesulitan dalam menjaga code anda tetap terorganisir.

Seperti artikel yang ditulis dari [Nathanel Beisigel](http://engineering.kapost.com/author/nathanaelbeisiegel/) dimana dia menjelaskan strategi dalam mengatur aplikasi react yang besar, tapi saya masih belum puas dengan pendekatan yang dilakukan. Jadi saya memutuskan untuk meluangkan waktu untuk mencari tahu cara terbaik untuk project aplikasi react saya suatu saat kedepan.

Nb : Saya menggunakan Redux dalam semua contoh pada artikel ini. Jika kamu tidak tahu dengan Redux, kamu dapat mengetahui pada [dokumentasi resminya](https://redux.js.org/). Semua contoh berbasis pada ReactJS, tapi kamu dapat menggunakan. Strukturnya sama persis dengan aplikasi React Native.


### Tantangan apa ketika kamu membangun sebuah aplikasi ?

Ini merupakan apa yang telah terjadi atau yang akan terjadi pada hampir semua _developer_ selama masa karirnya :

- Kamu membangun sebuah aplikasi untuk _client_ dengan tim dari beberapa _developer_, semuanya berjalan dengan lancar :smiley:
- _Client_ kamu membutuhkan fitur baru sama dengan _task_ baru :open_mouth:
- _Client_ kamu bertanya untuk menghilangkan beberapa fitur dan ditambahkan fitur baru, hal ini menjadi rumit. Kamu tidak memikirkannya, tapi kamu membuatya berfungsi meskipun tidak sempurna :relieved:
- _Client_ kamu menginginkan kamu untuk merubah fitur yang lain, menghapus fitur lain dan menambahkan fitur lain yang tidak diharapkan.