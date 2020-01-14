---
layout:     post
title:      Writeup Capture The Flag COMPFEST 11 - Qualification
subtitle:   
date:       2019-08-05
author:     Bambang S
header-img: img/photo-flag.jpg
catalog: false
tags:
    - Reversing
    - Web
    - Hacking
---

Kurang lebih sekitar 1/2 tahun tidak bermain tangkap bendera dan akhirnya bisa juga meluangkan waktu menyelesaikan beberapa soal tantangan pada kompetisi CTF kali ini. Berikut ulasannya..

#### File Separation - Forensic

![File Separation](https://bamsarts.github.io/img/file-separation.png)

[Download Files](https://github.com/bamsarts/Writeup-IDCC/blob/master/DecryptMe/enkripsi)

Diberikan beberapa potongan file terpisah berjumlah 8, diantara file tersebut ada 1 gambar .jpg yang tidak bisa dibuka. Kemudian file gambar tersebut dilakukan analisis awal dengan mengecek _signature_ pada file FUONSYBYZZFIZRO24CR7TUIJUOMMPWNL 

![File Signature](https://bamsarts.github.io/img/signature.png)

Pada gambar diatas terdapat _header signature_ dengan kode ff d8 ff e1. yang menandakan _signature_ tersebut berekstensi .jpg. Untuk menggabungkan potongan file tersebut dilakukan percobaan dengan menebak urutan yang benar menggunakan permutasi. berikut script penyelesaiannya

```python
from itertools import permutations as p

first = open('FUONSYBYZZFIZRO24CR7TUIJUOMMPWNL').read()

a = [
        open('5TJKUQMMHVSH6N26OVGYSEML7MR7XYMV').read(), 
        open('7UEIMXOSXHB7QMAK7ESUJFZ6W4S73L2Y').read(), 
        open('MXXRYR7KVHCQYQCCPTC4YTTZI4CVRHEB').read(), 
        open('O2KA5QQJO7SADZKP3REYQADUB7MR3CT6').read(), 
        open('L6MX2CYFKDEYXEZ5QWHHM4Q57H6WSJQK').read(), 
        open('YUAE3MNDTWG67BGF4BKXLFR2XNXWWGCV').read(),
        open('LVF5LK4BNHVW2K5DBT4J7KIQJD4MQDQH').read()
    ]

res = list(p(a, len(a)))
print len(res)

c = 1
for i in res:
    t = first + ''.join(i)
    q = open("{}.jpg".format(c), 'w')
    q.write(t)
    q.close()
    c += 1
```

ketemu flag pada file 4975.jpg

FLAG : **COMFPEST11{l1TTL3_t1R3D_hUH_9e153ed8}**

###
