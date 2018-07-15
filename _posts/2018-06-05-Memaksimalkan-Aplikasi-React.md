---
layout:     post
title:      Memaksimalkan Aplikasi React
subtitle:   
date:       2018-05-06
author:     Bambang S
header-img: img/post-2-header.jpg
catalog: true
tags:
    - react
    - reactnative
    - javascript
---


Saya telah mengerjakan aplikasi web yang cukup besar beberapa tahun terakhir. Membuat dari nol, dengan teman-teman _developer_ yang dapat _scale_ sampai sekarang. Mungkin sekarang sudah digunakan oleh jutaan pengguna :satisfied:. Dalam membangun aplikasi React terkadang jika tidak memulai dengan struktur folder yang baik, ini dapat mengalami kesulitan dalam menjaga _code_ anda tetap terorganisir.

Seperti artikel yang ditulis dari [Nathanel Beisigel](http://engineering.kapost.com/2016/01/organizing-large-react-applications/) dimana dia menjelaskan strategi dalam mengatur aplikasi React yang besar, tapi saya masih belum puas dengan pendekatan yang dilakukan. Jadi saya memutuskan untuk meluangkan waktu untuk mencari tahu cara terbaik untuk project aplikasi react saya suatu saat kedepan.

Nb: Saya menggunakan Redux dalam semua contoh pada artikel ini. Jika kamu tidak tahu dengan Redux, kamu dapat mengetahui pada [dokumentasi resminya](https://redux.js.org/). Semua contoh berbasis pada ReactJS, tapi kamu dapat menggunakan. Strukturnya sama persis dengan aplikasi React Native.


### Tantangan apa ketika kamu membangun sebuah aplikasi ?

Ini merupakan apa yang telah terjadi atau yang akan terjadi pada hampir semua _developer_ selama masa karirnya :

- Kamu membangun sebuah aplikasi untuk _client_ dengan tim kamu, semuanya berjalan dengan lancar :smiley:
- _Client_ kamu membutuhkan fitur baru sewaktu-waktu :open_mouth:
- _Client_ kamu menginginkan untuk menghilangkan beberapa fitur dan penambahan fitur baru. Hal ini menjadi rumit, dan kamu tidak memikirkannya. Alhasil kamu dapat melakukanya tapi tidak sempurna :relieved:
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

Bedasarkan aturan-aturan tersebut, mari kita ulas lebih lanjut.

#### _Components_

Kamu pasti tahu apa itu _component_, tapi ada satu hal penting yang kamu harus dalam penyusunan ini yaitu kemampuan untuk menurunkan _component_ ke _component_ lain. _Component_ didefinisikan di tingkat _root_ proyek kamu, dalam folder _component_, atau didefinisikan secara global yang dapat digunakan dimanapun dalam aplikasi. Tapi jika kamu memutuskan untuk mendefinisikan _component_ baru didalam _component_ lain (_nesting_), _component_ baru ini hanya dapat digunakan melalui _parentnya_. 

##### Mengapa kamu harus melakukan ini ?

Ketika kamu mengembangkan aplikasi yang besar, ini cukup sering terjadi ketika bahwa kamu perlu membuat _component_ yang anda tahu pasti tidak akan digunakan kembali dimanapun, tapi kamu membutuhkanya. Jika kamu menambahkannya ke tingkat _root_ folder _componentmu_, ini akan menghilangkan ratusan _component_.
Tentu, kamu dapat mengelompokan itu, tapi ketika kamu mau melakukan penghapusan, kamu tidak akan ingat apakah semuanya atau ada yang masih ada digunakan di tempat lain. Meskipun, jika kamu mendefinisikan pada di tingkat _root component_ utama aplikasimu, seperti _button_, _form field_, _thumbnail_, atau yang lebih kompleks seperti _listComment_, _formComposer dengan kepunyaan _child component_ sendiri, ini akan jauh lebih mudah untuk menemukan apa yang kamu butuhkan.

Contoh:

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

- _Button_ dapat digunakan dimana saja di dalam aplikasi.
- _Notification_ dapat juga digunakan dimana saja. _Component_ ini mendeklrasikan _component ButtonDismiss_. Kamu tidak bisa menggunakan _ButtonDismiss_ dimanapun selain didalam _component Notifications_.
- _Component ButtonDismiss_ menggunakan _Button_ secara internal, ini diotorisasi, karena _Button_ didefinisikan pada tingkat _root component_.

#### _Scenes_

_Scene_ merupakan halaman aplikasi. Kamu dapat meletakan _scene_ sama seperti _component_, tapi saya lebih suka memisahkan itu kedalam folder. Jika kamu menggunakan [React Router](https://github.com/reactjs/react-router) atau [React Native Router](https://github.com/aksonov/react-native-router-flux), kamu dapat mengimpor semua scene di file index.js utama dan persiapkan file _route_.

Dengan prinsip yang sama _component_ dapat juga diturunkan (_nested_), kamu juga dapat menurukan _scene_ kedalam _scene_, dan juga dapat mendefinisikan _component_ atau _service_ kedalam _scene_. Kamu harus ingat, jika kamu memutuskan untuk mendefinisikan sesuatu kedalam _scene_, kamu hanya dapat menggunakannya di dalam folder _scene_ itu sendiri.

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

Tidak semuanya dapat menjadi _component_, dan kamu perlu membuat modul tersendiri yang dapat digunakan di _component_ atau _scene_. Kamu dapat melihat _service_ seperti modul tersendiri, dimana kamu akan mendifinisikan _core business logic_ dari aplikasi. Ini pada akhirnya dapat dibagi di antara beberapa _scene_ atau bahkan aplikasi, seperti web dan versi aplikasi _native_.

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

Saya merekomendasikan untuk membuat _service_ untuk mengatur semua API _request_. Kamu dapat melihat itu sebagai _adapter_ antara server API dan _view layer_ (_scene dan component_) aplikasi. Itu dapat menjaga _call network_ aplikasi yang dibuat, _get_ dan _post_ konten, dan mengubah _payload_ yang dibutuhkan sebelum dikirim atau disimpan kedalam media penyimpanan aplikasi (seperti Redux). _Scene_ dan Komponen hanya akan mengirimkan _action_, membaca _store_ dan melakukan _update_ ketika ada perubahan.

#### Kesimpulan

Saya telah mengimplementasikan denga struktur folder selam beberapa bulan terakhir pada proyek pribadi yang dibuat dengan React Native, dan membuat saya menghemat waktu. Dengan ini jauh lebih mudah untuk mendapatkan semua entitas terkait yang dikelompokan bersama, dan itu membuat semuanya lebih mudah dikerjakan. Struktur folder ini adalah salah satu dari banyak cara untuk mengatur proyek kamu. Dan ini, saya menyukainya sampai sekarang. Saya harap dengan ini dapat meningkatkan kinerjamu :blush:




