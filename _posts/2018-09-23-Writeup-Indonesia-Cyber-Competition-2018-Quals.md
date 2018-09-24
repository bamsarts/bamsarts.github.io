---
layout:     post
title:      Writeup Indonesia Cyber Competition 2018 - Quals
subtitle:   
date:       2018-09-23
author:     Bambang S
header-img: img/background.jpg
catalog: false
tags:
    - Crypto
    - Cyber
    - Competition
---

Berikut ini saya akan berbagi kualifikasi soal dan pembahasan _Capture The Flag Indonesia Cyber Competition 2018_ yang berhasil saya kerjakan. Kategori soal yang diberikan sama pada umumnya CTF yaitu _Binary Exploitation, Cryptography, Forensic, Reverse Engineering, Steganography, dan Web Hacking. Berikut ulasannya ...

#### DecryptME - Crypto

![Decrypt Me](https://bamsarts.github.io/img/decryptme.png)

Diberikan file [decryptme.py](https://github.com/bamsarts/Writeup-IDCC/blob/master/DecryptMe/decryptme.py)

<script src="https://gist.github.com/bamsarts/b0d48959196cf4a0924ba6d61b08c56a.js"></script>

dan file hasil [enkripsi](https://github.com/bamsarts/Writeup-IDCC/blob/master/DecryptMe/enkripsi).

```
F7=&D_6@9YU&9HA) MK9HL=RMSY3(
```

Dalam penyelesaian soal ini, isi file enkripsi yang berupa string perlu didekripsi untuk mendapatkan flagnya. Dilihat dari line 9 pada file decryptme.py merupakan proses pengubahan nilai integer menjadi char. Langkah pertama yang harus dilakukan mengubah setiap char dari string yang didapat menjadi bentuk integer. Nilai integer didapat dari nilai variabel teks dan kunci yang di modulo 127. 
Modulo yang diterapkan bisa juga diartikan sebagi pengurangan, karena nilai dikurang maka untuk penyelesainya dengan proses kebalikannya yaitu ditambah. Jadi setiap integer yang didapat ditambah dengan 127. Berikut kode penyelesainnya

```python
enkripsi = open('enkripsi','r').read()

integer = []

for i in range(len(enkripsi)):
	integer.append(ord(enkripsi[i]) + 127)

print integer
```

output program

```
[197, 182, 188, 165, 195, 148, 222, 181, 191, 184, 155, 216, 212, 165, 184, 199, 192, 168, 159, 204, 202, 148, 184, 145, 199, 203, 188, 209, 204, 147, 210, 145, 216, 178, 167, 158, 137]
```

Dari hasil programm, selanjutnya dilakukan proses _bruteforce_ untuk bisa mendapatkan nilai dari kunci, sedangkan nilai dari variabel teks akan menjadi parameter pengecekan yang bisa tebak dari format flag yang diawali dengan string 'IDCC{'. Proses yang dilakukan melakukan pengecekan key yang dimasukkan dengan param yang sudah diencode base64 apakah senilai atau tidak. Berikut kode penyelesainnya

```python
from base64 import *

enkripsi = open('enkripsi','r').read()

integer = []

for i in range(len(enkripsi)):
	integer.append(ord(enkripsi[i]) + 127)

param = b64encode('IDCC{')
key = []

for i in range(4):
	for j in range(0,127):
		cek = integer[i] - j
		if chr(cek) == param[i]:
			key.append(j)

print key

```

output program

```
[114, 97, 106, 97]
```

Dari output program, berhasil mendapatkan nilai _key_ nya. Tahap berikutnya melakukan dekripsi dari string flag yang sudah dienkripsi dengan mengurangi setiap nilai integer dari string enkripsi dengan _key_ secara berulang. Kemudian dirubah menjadi huruf dan didecode base64. Berikut kode penyelesainnya 

```python
from base64 import *

enkripsi = open('enkripsi','r').read()

integer = []

for i in range(len(enkripsi)):
	integer.append(ord(enkripsi[i]) + 127)


param = b64encode('IDCC{')
key = []

for i in range(4):
	for j in range(0,127):
		cek = integer[i] - j
		if chr(cek) == param[i]:
			key.append(j)


flag = ''
for i in range(len(integer)):
	tmp = integer[i] - key[i % len(key)]
	flag += chr(tmp)

print b64decode(flag)
```

output program

```
IDCC{S1mpl3_4nd_stR4ight}
```

dapat flagnya : **IDCC{S1mpl3_4nd_stR4ight}**

#### Do Not Cheat! - WEB

![Do Not Cheat](https://bamsarts.github.io/img/donotcheat.png)

Diberikan url [http://206.189.88.9:6301/](http://206.189.88.9:6301/) jika diaksed web nya ada hal yang menarik pada kode javascript nya yaitu terdapat method post menuju flag.php 

![Flag.php](https://bamsarts.github.io/img/flag.php.png)

jika dicoba melalui console pada _browser_ dengan memasukkan 

```javascript
$.post('flag.php',function(t){alert(t)})
```

voila muncul alert

![Alert Flag](https://bamsarts.github.io/img/alertFlag.png)

Flag : **IDCC{0nlY_th3_we4K_che4T}**

#### EzPz - Reversing

![EzPz Challenge](https://bamsarts.github.io/img/EzPz-challenge.png)

Diberikan file [EzPz](https://github.com/bamsarts/Writeup-IDCC/blob/master/Ezpz/EzPz) yang merupakan file binary _executable_ 64 bit

![Ezpz](https://bamsarts.github.io/img/EzPz.png)

ketika saya mencoba mengganti nama file tersebut, hasil keluaran dari program tersebut berubah sebagai berikut :

![Padding](https://bamsarts.github.io/img/padding.png)

Hasil keluaran dari program seperti base64, yaitu ketika input berjumlah 3 byte hasil keluaran berjumlah 4 byte. Tapi, charset yang digunakan berbeda, seperti angka 5 sebagai _padding_, bukan '=' seperti layaknya base64. Untuk mendapatkan flag, dilakukan _bruteforce_ setiap _output_ 4 byte. _Bruteforce_ dilakukan per karakter, pada byte pertama akan dibandingkan input[0] == output[0], byte kedua dibandingkan input[:2] == output[:2], sedangkan pada byte ketiga dibandingkan input = output. Berikut kode penyelesainnya

```python
import os
from pwn import *

mapping = {}
data = '_0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ{}'
file = open('base','rb')
binary = file.read()
file.close()

def solve(z):
    file = open('output/' + z, 'wb')
    os.system('chmod +x output/'+z)

    file.write(binary)
    file.close()
    r = process('output/' + z)
    hasil = r.recvline()[1:-2]
    r.close()

    return hasil

def crack(z):
    first = []
    second = []
    for c in data:
        out = solve(c)
        if (out == z):
            return c
        elif (out[0] == z[0]):
            first.append(c)
    for c in data:
        for d in first:
            out = solve(d+c)
            if (out == z):
                return d+c
            elif (out[:2] == z[:2]):
                second.append(d+c)
    for c in data:
        for d in second:
            if (solve(d+c) == z):
                return d+c

flag = 'c=/2HsfweAeTCz]!V@alV@pz9??$eYjQVz&ln<z5'
decrypted = ''
for i in range(10):
    decrypted += crack(flag[i*4:i*4+4])
    file = open('flag', 'w')
    file.write(decrypted)
    file.close()

```

Flagnya : 

![flag](https://bamsarts.github.io/img/flag-ezpz.png)










