---
layout:     post
title:      Memaksimalkan Aplikasi React
subtitle:   
date:       2018-07-23
author:     Bambang S
header-img: img/post-2-header.jpg
catalog: true
tags:
    - react
    - reactnative
    - javascript
---


Dalam membuat aplikasi dengan React terkadang jika tidak memulai dengan struktur folder yang baik, ini dapat mengalami kesulitan dalam menjaga _code_ anda tetap terorganisir. Seperti artikel yang ditulis dari [Nathanel Beisigel](http://engineering.kapost.com/2016/01/organizing-large-react-applications/) dimana dia menjelaskan strategi dalam mengatur aplikasi React yang besar, tapi saya masih belum puas dengan pendekatan yang dilakukan. Jadi saya memutuskan untuk meluangkan waktu untuk mencari tahu cara terbaik untuk project aplikasi react saya suatu saat kedepan.

Nb: Saya menggunakan Redux dalam semua contoh pada artikel ini. Jika kamu tidak tahu dengan Redux, kamu dapat mengetahui pada [dokumentasi resminya](https://redux.js.org/). Semua contoh berbasis pada ReactJS, tapi kamu dapat menggunakan. Strukturnya sama persis dengan aplikasi React Native.


### Tantangan apa ketika kamu membangun sebuah aplikasi ?

Mungkin ini merupakan kejadian yang pernah terjadi pada semua _Software Developer_ selama masa karirnya :

- Kamu membuat sebuah aplikasi untuk _client_ dengan tim kamu, semuanya berjalan dengan lancar :smiley:
- _Client_ kamu membutuhkan fitur baru sewaktu-waktu :open_mouth:
- _Client_ kamu menginginkan untuk menghilangkan beberapa fitur dan menambahkan fitur baru. Hal ini menjadi akan menjadi rumit ketika kamu tidak memikirkannya. :relieved:
- _Client_ kamu menginginkan kamu untuk merubah fitur, menghapus fitur lain dan menambahkan fitur baru yang tidak diharapkan. :confounded:
- Beberapa lama kamudian setelah iterasi _development_, _code_ aplikasi setelah dilihat kembali sulit dibaca dan dimengerti. Semuanya terlihat seperti _spaghetti code_ :anguished:

Ketika _client_ kamu memutuskan untuk membuat versi baru dari aplikasi, dengan beberapa _code_ baru dan fitur baru. Dan pada akhirnya menyimpan _legacy code_ yang tetap berjalan dengan _code baru_, ini menjadi lebih sulit untuk di maintain. Karena aplikasi kamu tidak didesain dengan benar dari awal. Ketika saya memulai untuk belajar React, saya menemukan beberapa artikel menarik yang menjelaskan bagaimana membuat _Todo list_ atau aplikasi permainan sederhana. Artikel-artikel itu sangat berguna untuk memahami dasar-dasar React, tapi saya dengan cepat sampai pada titik dimana saya tidak menemukan lebih banyak tentang penggunan _React_ untuk membangun aplikasi yang besar, dengan beberapa puluhan halaman dan ratusan komponen.

Setelah sekian mencari-cari, saya belajar bahwa setiap _React boilerplate_ pada github menghasilkan struktur yang mirip. _Boilerplate_ tersebut semua file dikelompokan berdasarkan tipe. Ini mungkin terlihat familiar seperti berikut :

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

 
Struktur folder ini bagus untuk membangun website atau aplikasi, tapi saya percaya itu bukan struktur terbaik. Ketika kamu mengatur file berdasarkan tipe, sebagai parameter meningkatnya aplikasimu, hal itu akan sulit pada saat di maintain. Pada saat kamu menerapkan ini, ini akan menghambat ketika kamu ingin merubah file didalamnya untuk keperluan fitur mendatang. Hal yang baik dengan React adalah kamu dapat merancang aplikasi mu dengan cara apapun yang kamu sukai. Kamu tidak terpaksa untuk mengikuti struktur folder tertentu, Karena React hanya sebuah javascrupt library.

### Bagaimana cara yang lebih baik dalam mengatur aplikasi ?

Setahun yang lalu, saya telah mencoba menggunakan Ember.JS sebagai _javascript framework_ utama untuk membuat aplikasi web. Satu hal yang menarik dengan Ember.JS yaitu kemampuan untuk menyusun proyek kamu berdasarkan fitur, bukan berdasarkan tipe. Dan ini mengubah segalanya. Pods dalam Ember.JS memang sangat bagus tapi masih terbatas, dan saya menginginkan sesuatu yang jauh lebih fleksibel. Setelah beberapa kali percobaan, dan akhirnya menemukan struktur terbaik. Saya memutuskan untuk mengelompokan semua fitur menjadi satu, dan mengumpulkannya sesuai kebutuhan. Ini yang saya gunakan sekarang :

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

Setiap _component_, _scene_ atau _service_ memiliki segala kebutuhan untuk dapat bekerja sendirinya, seperti _style_, _image_, _action_, serta _unit testing_ atau _integration testing_. Kamu dapat melihat fitur seperti potongan kode yang akan digunakan di aplikasimu (mirip node module). Agar berfungsi dengan benar, ikutilah aturan berikut ini :

- Sebuah _component_ dapat menentukan _component_ lainnya atau _service_. Dan ini tidak bisa digunakan untuk menentukan _scene_.
- Sebuah _scene_ dapat menentukan _component_, _scene_ lainnya atau _service_.
- Sebuah _Service_ dapat menentukan _service_ lainnya. Ini tidak digunakan atau mendefinisikan _component_ atau _scene_.
- File yang berada di _child_ / turunan hanya dapat digunakan dari _parent_ nya

Bedasarkan aturan-aturan tersebut, mari kita ulas lebih lanjut.

#### _Components_

Kamu pasti tahu apa itu _component_, tapi ada satu hal penting yang kamu harus tahu dalam penyusunan ini yaitu kemampuan untuk menurunkan _component_ ke _component_ lain. _Component_ didefinisikan di _root_ proyek kamu, dalam folder _component_, atau didefinisikan secara global yang dapat digunakan dimanapun dalam aplikasi. Tapi jika kamu memutuskan untuk mendefinisikan _component_ baru didalam _component_ lain (_nesting_), _component_ baru ini hanya dapat digunakan melalui _parentnya_. 

##### Mengapa harus melakukan ini ?

Ketika kamu mengembangkan aplikasi yang besar, ini sering terjadi ketika kamu perlu membuat _component_ yang pasti tidak akan digunakan kembali dimanapun, tapi terkadang sewaktu - waktu dibutuhkan. Jika kamu menambahkan di _root_ folder _componentmu_, ini akan menghilangkan _component_ didalamnya.
Tentu kamu dapat mengelompokan itu, tapi ketika kamu mau menghapus suatu _component_,  mungkin kamu tidak akan ingat apakah _component_ ada yang masih digunakan di tempat lain atau tidak. Meskipun jika kamu mendefinisikan pada di tingkat _root component_ aplikasimu, seperti _Button_ dan _FormField_. Dari kasus tersebut ini akan jauh lebih mudah untuk menentukan apa yang kamu butuhkan.

Contoh :

```
/src
  /components
    /Button
      /index.js
    /Notifications 
      /components 
        /ButtonDismiss 
          /index.js
      /actions.js
      /index.js
      /reducer.js
```

- _Component Button_ dapat digunakan dimana saja di dalam aplikasi.
- _Component Notification_ dapat juga digunakan dimana saja. _Component_ ini mendeklrasikan _component ButtonDismiss_. Kamu tidak bisa menggunakan _ButtonDismiss_ dimanapun selain didalam _component Notifications_.


#### _Scenes_

_Scene_ merupakan halaman aplikasi. Kamu dapat meletakan _scene_ sama seperti _component_, tapi saya lebih suka memisahkan itu kedalam folder. Jika kamu menggunakan [React Router](https://github.com/reactjs/react-router) atau [React Native Router](https://github.com/aksonov/react-native-router-flux), kamu dapat mengimpor semua _scene_ di file index.js utama dan menyiapkan file _routenya_.

Dengan prinsip yang sama seperti _component_, itu juga dapat diturunkan (_nesting_), kamu juga dapat menurukan _scene_ kedalam _scene_, dan juga dapat mendefinisikan _component_ atau _service_ kedalam _scene_. Kamu harus ingat, jika kamu memutuskan untuk mendefinisikan sesuatu kedalam _scene_, kamu hanya dapat menggunakannya di dalam folder _scene_ itu sendiri.

Contoh :

```
/src
  /scenes
    /Home 
      /components
        /ButtonShare
          /index.js
      /index.js
    /Sign
      /components
        /ButtonHelp
          /index.js
      /scenes
        /Login
          /components 
            /Form
              /index.js
            /ButtonFacebookLogin
              /index.js
          /index.js
       
        /Register
          /index.js
      /index.js
```

- _Scene Home_ mempunyai sebuah _component ButtonShare_, itu hanya dapat digunakan di _Home Scene_.
- _Scene Sign_ mempunya sebuah _component ButtonHelp_. _Component_ ini dapat juga digunakan di _scene login_ atau _scene register_, atau _component_ apa pun yang didefinisikan di _scene_ tersebut.
- _Component Form_ menggunakan ButtonHelp secara internal, hal ini diotorisasi karena ButtonHelp didefinisikan di _parent_.
- _Scene Register_ tidak dapat menggunakan salah satu _component_ yang didefinisikan di dalam _scene login_, tapi dapat menggunakan _component ButtonHelp_.

#### _Services_

Tidak semuanya dapat menjadi _component_, dan kamu perlu membuat modul tersendiri yang dapat digunakan di _component_ atau _scene_. Kamu dapat melihat _service_ seperti modul tersendiri, dimana kamu akan mendifinisikan _logic core business_ dari aplikasi. Ini pada akhirnya dapat dibagi di antara beberapa _scene_ atau bahkan aplikasi keseluruhan.

~~~
/src
  /services
    /api
      /services
        /handleError
          /index.js
      /index.js
    /geolocation 
    /session 
      /actions.js
      /index.js
      /reducer.js
~~~

Saya merekomendasikan untuk membuat _service_ untuk mengatur semua API _request_. Kamu dapat melihat itu sebagai _adapter_ antara server API dan _view layer_ (_scene dan component_) aplikasi. Hal itu dapat menjaga _call network_ aplikasi yang dibuat seperti _get_ dan _post_ konten, atau mengubah _payload_ yang dibutuhkan sebelum dikirim atau disimpan kedalam _store_ aplikasi (seperti Redux). _Scene_ dan _Component_ hanya akan mengirimkan _action_, membaca _store_ dan melakukan _update_ ketika ada perubahan.

#### Kesimpulan

Saya telah mengimplementasikan dengan struktur folder ini selama beberapa bulan terakhir pada proyek pribadi yang dibuat dengan React Native, dan saya merasa lebih efisien dalam mengatur kodingan. Dan juga lebih mudah untuk mendapatkan semua entitas terkait yang dikelompokan, dan itu membuat semuanya lebih mudah dikerjakan. Struktur folder ini adalah salah satu dari banyak cara untuk mengatur aplikasi React. Dan ini membuat saya terus mengimplementasikan di proyek-proyek selanjutnya. Saya harap dengan ini kamu juga dapat meningkatkan efisiensi dalam membuat aplikasi menggunakan React :blush:

Jika kamu tertarik, saya memiliki contoh project yang mengikuti struktur folder tersebut

- [https://github.com/bamsarts/Batu-Gunting-Kertas](https://github.com/bamsarts/Batu-Gunting-Kertas)


