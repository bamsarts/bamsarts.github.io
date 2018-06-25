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


Saya telah mengerjakan aplikasi web yang cukup besar beberapa tahun terakhir. Membuat dari nol, dengan teman-teman developer yang dapat _scale_ sampai sekarang. Mungkin sekarang sudah digunakan oleh jutaan pengguna :satisfied:. Dalam membangun aplikasi React terkadang jika tidak memulai dengan struktur folder yang baik, ini dapat mengalami kesulitan dalam menjaga code anda tetap terorganisir.

Seperti artikel yang ditulis dari [Nathanel Beisigel](http://engineering.kapost.com/author/nathanaelbeisiegel/) dimana dia menjelaskan strategi dalam mengatur aplikasi react yang besar, tapi saya masih belum puas dengan pendekatan yang dilakukan. Jadi saya memutuskan untuk meluangkan waktu untuk mencari tahu cara terbaik untuk project aplikasi react saya suatu saat kedepan.

Nb : Saya menggunakan Redux dalam semua contoh pada artikel ini. Jika kamu tidak tahu dengan Redux, kamu dapat mengetahui pada [dokumentasi resminya](https://redux.js.org/). Semua contoh berbasis pada ReactJS, tapi kamu dapat menggunakan. Strukturnya sama persis dengan aplikasi React Native.


### Tantangan apa ketika kamu membangun sebuah aplikasi ?

Ini merupakan apa yang telah terjadi atau yang akan terjadi pada hampir semua _developer_ selama masa karirnya :

- Kamu membangun sebuah aplikasi untuk _client_ dengan tim _developer_, semuanya berjalan dengan lancar :smiley:
- _Client_ kamu membutuhkan fitur baru sama dengan _task_ baru :open_mouth:
- _Client_ kamu bertanya untuk menghilangkan beberapa fitur dan ditambahkan fitur baru, hal ini menjadi rumit. Kamu tidak memikirkannya, tapi kamu membuatya berfungsi meskipun tidak sempurna :relieved:
- _Client_ kamu menginginkan kamu untuk merubah fitur, menghapus fitur yang lain dan menambahkan fitur baru yang tidak diharapkan. Ini seperti layaknya tambal sulam kode :confounded:
- Beberapa lama kamudian setelah iterasi, _code_ aplikasi setelah dilihat kembali sulit dibaca dan dimengerti. Semuanya terlihat seperti _spaghetti code_ :anguished:

Ketika _client_ kamu memutuskan untuk membuat versi baru dari aplikasi, dengan beberapa _code_ baru dan fitur baru. Ada kasus, jika kamu akhirnya menyimpan _legacy code_ yang tetap berjalan dengan _code baru_, ini menjadi lebih sulit untuk di _maintain_. Dan semua ini terjadi karena aplikasi kamu tidak didesain dengan benar dari awal.

Ketika saya memulai untuk belajar React, saya menemukan beberapa artikel menarik yang menjelaskan bagaimana membuat _Todo list_ atau _Game simple_. Artikel-artikel itu sangat berguna untuk memahami dasar-dasar _React_, tapi saya dengan cepat sampai pada titik dimana saya tidak menemukan lebih banyak tentang bagaimana saya dapat menggunakan _React_ untuk membangun aplikasi besar, dengan beberapa puluhan halaman dan ratusan _components_.

Setelah sekian mencari-cari, saya belajar bahwa setiap _React boilerplate project_ pada github menghasilkan struktur yang mirip, mereka mengatur semua file berdasarkan type. Ini mungkin terlihat familiar seperti berikut :

```
 /src
  /actions
    /notifications.js
      
 /components 
	/Header
	/Footer
	/Notifications
	  /index.js

  /containers
    /Home
    /Login
    /Notifications
      /index.js

  /images
    /logo.png

  /reducers 
    /login.js
    /notifications.js

  /styles 
    /app.scss
    /header.scss 
    /home.scss
    /footer.scss
    /notifications.scss

  /utils

  index.js  

 ```

 
Struktur folder ini mungkin baik untuk membangun website atau aplikasi, tapi saya percaya, itu bukan yang terbaik struktur folder. Ketika kamu mengatur file berdasarkan type, sebagai tanda meningkatnya aplikasimu, itu sering mendatangkan kesulitan pada saat _maintain_. Pada saat kamu menerapkan ini, ini sudah terlambat dan kamu akan menginvestasikan waktu lebih lama dan uang untuk merubah segalanya, atau mendukung apa yang kamu punya untuk beberapa tahun kedepan. Hal yang baik dengan React adalah kamu dapat merancang aplikasi mu dengan cara apapun yang kamu sukai. Kamu tidak terpaksa untuk mengikuti struktur folder tertentu, React hanya sebuah library javascript.

### Bagaimana cara yang lebih baik dalam mengatur aplikasi ?

Dalam beberapa tahun terakhir, saya telah mencoba di beberapa project menggunakan Ember.JS sebagai _javascript framework_ utama untuk membuat aplikasi web. Satu hal yang menarik dengan Ember.JS yaitu kemampuan untuk menyusun proyek kamu berdasarkan fitur, bukan berdasarkan type. Dan ini mengubah segalanya. Pods dalam Ember.JS memang sangat bagus tapi masih terbatas, dan saya menginginkan sesuatu yang jauh lebih fleksibel. Setelah beberapa kali percobaan, mencoba menemukan struktur terbaik, saya sampai pada titik dimana saya memutuskan untuk mengelompokan semua fitur menjadi satu, dan mengumpulkannya sesuai kebutuhan. Ini yang saya gunakan sekarang :

```
/src
  /components 
    /Button 
    /Notifications
      /components
        /ButtonDismiss  
          /images
          /locales
          /specs 
          /index.js
          /styles.scss
      /index.js
      /styles.scss
  /scenes
    /Home 
      /components 
        /ButtonLike
      /services
        /processData
      /index.js
      /styles.scss
    /Sign 
      /components 
        /FormField
      /scenes
        /Login
        /Register 
          /locales
          /specs
          /index.js
          /styles.scss
  /services
    /api
    /geolocation
    /session
      /actions.js
      /index.js
      /reducer.js
    /users
      /actions.js
      /api.js
      /reducer.js
  index.js 
  store.js
  ```

Setiap _component_, _scene_ atau _service_ memiliki segala kebutuhan untuk dapat bekerja sendirinya, seperti _style_, _image_, _translation_, _action_, serta _unit testing_ atau _integration testing_. Kamu dapat melihat fitur seperti potongan _code_ yang akan digunakan di aplikasi mu (sedikit mirip node module). Agar berfungsi dengan benar, ikutilah aturan berikut ini :

- Sebuah _component_ dapat menentukan _component_ lainnya atau _service_. Dan ini tidak bisa digunakan untuk menentukan _scene_.
- Sebuah _scene_ dapat menentukan _component_, _scene_ lainya atau _service_.
- Sebuah _Service_ dapat menentukan _service_ lainnya. Ini tidak digunakan atau mendefinisikac _component_ atau _scene_.
- Fitur _child_ / turunan hanya dapat digunakan dari _parent_ nya

Bedasarkan aturan-aturan tersebut, mari kita ulas lebih dalam.

#### _Components_

Kamu pasti tahu apa itu _component_, tapi ada satu hal penting yang kamu harus dalam penyusunan ini yaitu kemampuan untuk menurunkan _component_ ke _component_ lain.


