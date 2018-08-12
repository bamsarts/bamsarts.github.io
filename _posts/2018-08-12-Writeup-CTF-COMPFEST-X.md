---
layout:     post
title:      Writeup CTF COMPFEST X
subtitle:   
date:       2018-08-12
author:     Bambang S
header-img: img/ctf.jpg
catalog: false
tags:
    - Pwn
    - Cryptography
    - Reversing
---

#### Hash Math (Crypto)

![Hash Math](https://bamsarts.github.io/img/hash.png)


Diberikan file [contestant.py]('https://ctf.compfest.web.id/files/53ec223d4fb964f055ce624d5fc9e830/contestant.py') yang diberi integer N dan M dan harus mencari _hash collision_ nya dengan string N dan modulo M. Pada file yang diberikan terdapat hal menarik pada fungsi hatching() yaitu 

```python
ret = 26 * ret + ord(x) - ord('a')
ret %= mod
```

pada baris code pertama hashnya akan dibuat menjadi 0, dan baris code kedua hanya perlu merubah modulo M menjadi basis 26 dengan digit 'a' - 'z' dengan batas kiri menggunakan 'a' sampai panjang string menjadi N. Berikut code penyelesaiannya :

```python
from pwn import *
import string

digs = 'abcdefghijklmnopqrstuvwxyz'

def int2base(x, base):
    if x < 0:
        sign = -1
    elif x == 0:
        return digs[0]
    else:
        sign = 1
    x *= sign
    digits = []
    while x:
        digits.append(digs[int(x % base)])
        x = int(x / base)
    if sign < 0:
        digits.append('-')
    digits.reverse()
    return ''.join(digits)

def main():
    r = remote('103.252.50.113', 8361)
    i = 0

    while True:
        try:
            r.recvuntil('N = ')
        except:
            r.interactive()
            exit()

        N = int(r.recvuntil('M = ', drop=True))
        M = int(r.recv())

        print N, M

        payload = 'a' * N
        r.sendline(payload)

        payload = int2base(M, 26)
        payload = payload.rjust(N, 'a')
        r.sendline(payload)
        i+=1

if __name__ == "__main__":
    main()

```

setelah di run

![flag](https://bamsarts.github.io/img/flag_hash.png)

dapat flagnya : **CTFX{FwP_adalah_orang_ganteng}**

### Munch (Web)

![Munch](https://bamsarts.github.io/img/munch.png)

Pada soal tersebut terdapat link [http://103.252.50.113:8364/]('http://103.252.50.113:8364/'), jika diakses tampilan website hanya menampilkan kata **hey**. Setelah mengulik website tersebut, ditemukan banyak _cookies_ yang tersimpan dalam bentuk base64. 

![cookies](https://bamsarts.github.io/img/cookies.png)

Untuk mengotomatisasi pengambilan _cookies_ berikut scriptnya :

```python

import requests
import httplib
import base64
import re

httplib._MAXHEADERS = 1000

url = "http://103.252.50.113:8364/"
req = requests.get(url)
cookies = req.headers['Set-Cookie']
cookies = cookies.split("; Path=/, ")

result = ""
for cookie in cookies:
	tmp = re.findall("[a-zA-Z0-9]{4}=(.*)",cookie)[0].replace("\"","")
	try:
		result+=base64.b64decode(tmp)
	except Exception as e:
		pass

print result

```

lalu jalankan

```
python solver.py > teks

```

dan hasilnya

![munch flag](https://bamsarts.github.io/img/munch_flag.png)

dapat flagnya : **CTFX{t00_many_c00k1es_h3r3}**

### Firmware (Forensic)

![firmware](https://bamsarts.github.io/img/firmware.png)

Dalam challenge ini diberikan sebuah file [firmware.bin](https://drive.google.com/open?id=1X8gccvWkIQ2PMckLGeCE10T8qT73vJ_e). Kemudian saya ekstrak file tersebut dengan [binwalk](https://github.com/ReFirmLabs/binwalk). Setelah diekstrak terdapat banyak file maupun folder didalamnya setelah mencari2 saya menemukan gambar dengan nama file vpnfilter yang terdapat pada folder 
/firmware.bin.extracted/squashfs-root/var. Kemudian saya melihat metadata dari file tersebut dengan bantuan **exiftool**, berikut hasil metadatanya :

![exiftool](https://bamsarts.github.io/img/exiftool.png)

Pada metadata file tersebut ada yang mencurigakan pada baris comment yaitu berisi hint beserta URL [pastebin](https://pastebin.com/yu5WpX9U). Setelah dilihat pada link tersebut berisikan

>Very cool. Huh? Have you found the Stage 2 C&C IP adress yet? Cuz flag format is:
>Flag: CTFX{1mag3_GPS_iS_th3_k3y_[IP address of C&C server]}
>Example: CTFX{1mag3_GPS_iS_th3_k3y_127.0.0.1}
>Good Luck.

Dalam link tersebut terdapat potongan flag **CTFX{1mag3_GPS_iS_th3_k3y_[IP address of C&C server]}** serta sebuah hint **IP address of C&C server**. Setelah mencari-cari akhirnya saya menemukan referensi artikel terkait [VPNfilter](https://blog.securityevaluators.com/vpnfilter-threat-how-to-prevent-detect-and-mitigate-9cdf74fee92a) saya menemukan cara untuk menemukan IP address of C&C server dengan script sederhana

```python

o1p2 = 12 #latitude
o1p1 = 3  #latitude
o2p1 = 22 #latitude

o3p2 = 21 #longitude
o3p1 = 11 #longitude
o4p1 = 21 #longitude

octets = []

octets.append(o1p1 + (o1p2 + 0x5A))
octets.append(o2p1 + (o1p2 + 0x5A))
octets.append(o3p2 + (o3p2 + 0xB4))
octets.append(o4p1 + (o3p2 + 0xB4))

print(octets)

```

nilai variabel didapat dari GPS location latitude dan longitude pada gambar vpnfilter. Ketikan dijalankan scriptnya terdapat sebuah angka 

```
[105, 124, 222, 222]

```

dan ketika disubmit dengan format flag CTFX{1mag3_GPS_iS_th3_k3y_105.124.222.222} dan berhasil :tada:

### Crypto Matrix (Crypto)


![Crypto Matrix](https://bamsarts.github.io/img/crypto_matrix.png)

Diberikan file [encrypted](https://ctf.compfest.web.id/files/44dcec6fe919348c4882c173e95e1768/encrypted) dan [ecrypt.py](https://ctf.compfest.web.id/files/35b8addc521e26c1ad66f697634f68c5/encrypt.py). File encrypted berisi matriks 8X8 yang merupakan flag yang terenkripsi menggunakan encrypted.py. Karena matriks di encrypted berukuran 8X8, saya asumsikan nilai variabel size bernilai 8

```python
size = int(math.ceil(math.sqrt(len(flag))))

def pad(s):
    return s + (size - (len(s) % size)) * chr(size - len(s) % size)
```

flag masuk melalui fungsi pad() sehingga len(flag) bernilai 64, kemungkinan panjang flag asli sekitar 50-63.

```python
 arr = [flag[size*x:size*(x+1)] for x in range(len(flag)/size)]

    num = []
    for z in range(size):
        while(True):
            ch = random.choice(string.digits)
            if ch != '0':
                break
        num.append(z * '0' + ch + (size-1-z)*'0')
```

Array arr berisi variabel flag yang sudah dipisah menjadi beberapa bagian yang panjangnya 8. Sementara array num merupakan matriks diagonal (matriks yang semua elemen diluar diagonal utamanya nol).

```python

res = []
    for p in range(size):
        temp = []
        for q in range(size):
            cnt = 0
            for r in range(size):
                cnt += ord(arr[p][r]) * ord(num[r][q])
            temp.append(cnt)
        res.append(temp)

```

Setelah dipahami, matriks[0][0] di file encrypted dihasilkan dari looping variabel

```python
cnt += ord(arr[p][r]) * ord(num[r][q])

```

dan begitu seterusnya. Saya langsung generate semua constraint agar memudahkan pengerjaan. Karena variabel flag dan diagonal utama matriks num tidak diketahui, maka saya buat script dengan variabel baru dengan persamaan arr = flag + diagonal_utama_num sehingga len(arr) = 64 + 8 = 72. Berikut scriptnya.

```python

enc = open('encrypted').read()
enc = eval(enc)
x = 0
y = 0
size = 8

for p in range(size):
	for q in range(size):
		cnt = "s.add( "
		for r in range(size):
			if r != q:
				cnt += "(arr[{}] * 48) + ".format(p*8 + r)
			else:
				cnt += "(arr[{}] * arr[{}]) + ".format(p*8 + r, 64+r)

    	cnt = cnt.strip(" + ")
    	cnt += " == {} )".format(enc[x][y])
    	y += 1

    	if y == 8:
   		 y = 0
   		 x += 1

    	print cnt

```

Setelah mendapatkan semua constraint, langsung saja menuju menu utamanya menggunakan z3-solver.

```python
from z3 import *

s = Solver()
arr = [BitVec(i, 32) for i in range(72)]

for i in range(64):
    s.add(arr[i] >= 0)
    s.add(arr[i] < 256)

for i in range(64, 72):
    s.add(arr[i] > 48)
    s.add(arr[i] < 58)    

# hasil eksekusi script sebelumnya
s.add( (arr[0] * arr[64]) + (arr[1] * 48) + (arr[2] * 48) + (arr[3] * 48) + (arr[4] * 48) + (arr[5] * 48) + (arr[6] * 48) + (arr[7] * 48) == 36307 )
s.add( (arr[0] * 48) + (arr[1] * arr[65]) + (arr[2] * 48) + (arr[3] * 48) + (arr[4] * 48) + (arr[5] * 48) + (arr[6] * 48) + (arr[7] * 48) == 36912 )
s.add( (arr[0] * 48) + (arr[1] * 48) + (arr[2] * arr[66]) + (arr[3] * 48) + (arr[4] * 48) + (arr[5] * 48) + (arr[6] * 48) + (arr[7] * 48) == 36380 )
s.add( (arr[0] * 48) + (arr[1] * 48) + (arr[2] * 48) + (arr[3] * arr[67]) + (arr[4] * 48) + (arr[5] * 48) + (arr[6] * 48) + (arr[7] * 48) == 36504 )
s.add( (arr[0] * 48) + (arr[1] * 48) + (arr[2] * 48) + (arr[3] * 48) + (arr[4] * arr[68]) + (arr[5] * 48) + (arr[6] * 48) + (arr[7] * 48) == 37224 )
s.add( (arr[0] * 48) + (arr[1] * 48) + (arr[2] * 48) + (arr[3] * 48) + (arr[4] * 48) + (arr[5] * arr[69]) + (arr[6] * 48) + (arr[7] * 48) == 36564 )
s.add( (arr[0] * 48) + (arr[1] * 48) + (arr[2] * 48) + (arr[3] * 48) + (arr[4] * 48) + (arr[5] * 48) + (arr[6] * arr[70]) + (arr[7] * 48) == 36975 )
s.add( (arr[0] * 48) + (arr[1] * 48) + (arr[2] * 48) + (arr[3] * 48) + (arr[4] * 48) + (arr[5] * 48) + (arr[6] * 48) + (arr[7] * arr[71]) == 37010 )
s.add( (arr[8] * arr[64]) + (arr[9] * 48) + (arr[10] * 48) + (arr[11] * 48) + (arr[12] * 48) + (arr[13] * 48) + (arr[14] * 48) + (arr[15] * 48) == 39269 )
s.add( (arr[8] * 48) + (arr[9] * arr[65]) + (arr[10] * 48) + (arr[11] * 48) + (arr[12] * 48) + (arr[13] * 48) + (arr[14] * 48) + (arr[15] * 48) == 39944 )
s.add( (arr[8] * 48) + (arr[9] * 48) + (arr[10] * arr[66]) + (arr[11] * 48) + (arr[12] * 48) + (arr[13] * 48) + (arr[14] * 48) + (arr[15] * 48) == 39396 )
s.add( (arr[8] * 48) + (arr[9] * 48) + (arr[10] * 48) + (arr[11] * arr[67]) + (arr[12] * 48) + (arr[13] * 48) + (arr[14] * 48) + (arr[15] * 48) == 39453 )
s.add( (arr[8] * 48) + (arr[9] * 48) + (arr[10] * 48) + (arr[11] * 48) + (arr[12] * arr[68]) + (arr[13] * 48) + (arr[14] * 48) + (arr[15] * 48) == 39944 )
s.add( (arr[8] * 48) + (arr[9] * 48) + (arr[10] * 48) + (arr[11] * 48) + (arr[12] * 48) + (arr[13] * arr[69]) + (arr[14] * 48) + (arr[15] * 48) == 39492 )
s.add( (arr[8] * 48) + (arr[9] * 48) + (arr[10] * 48) + (arr[11] * 48) + (arr[12] * 48) + (arr[13] * 48) + (arr[14] * arr[70]) + (arr[15] * 48) == 39889 )
s.add( (arr[8] * 48) + (arr[9] * 48) + (arr[10] * 48) + (arr[11] * 48) + (arr[12] * 48) + (arr[13] * 48) + (arr[14] * 48) + (arr[15] * arr[71]) == 39875 )
s.add( (arr[16] * arr[64]) + (arr[17] * 48) + (arr[18] * 48) + (arr[19] * 48) + (arr[20] * 48) + (arr[21] * 48) + (arr[22] * 48) + (arr[23] * 48) == 34274 )
s.add( (arr[16] * 48) + (arr[17] * arr[65]) + (arr[18] * 48) + (arr[19] * 48) + (arr[20] * 48) + (arr[21] * 48) + (arr[22] * 48) + (arr[23] * 48) == 35088 )
s.add( (arr[16] * 48) + (arr[17] * 48) + (arr[18] * arr[66]) + (arr[19] * 48) + (arr[20] * 48) + (arr[21] * 48) + (arr[22] * 48) + (arr[23] * 48) == 34370 )
s.add( (arr[16] * 48) + (arr[17] * 48) + (arr[18] * 48) + (arr[19] * arr[67]) + (arr[20] * 48) + (arr[21] * 48) + (arr[22] * 48) + (arr[23] * 48) == 34461 )
s.add( (arr[16] * 48) + (arr[17] * 48) + (arr[18] * 48) + (arr[19] * 48) + (arr[20] * arr[68]) + (arr[21] * 48) + (arr[22] * 48) + (arr[23] * 48) == 34568 )
s.add( (arr[16] * 48) + (arr[17] * 48) + (arr[18] * 48) + (arr[19] * 48) + (arr[20] * 48) + (arr[21] * arr[69]) + (arr[22] * 48) + (arr[23] * 48) == 34521 )
s.add( (arr[16] * 48) + (arr[17] * 48) + (arr[18] * 48) + (arr[19] * 48) + (arr[20] * 48) + (arr[21] * 48) + (arr[22] * arr[70]) + (arr[23] * 48) == 34841 )
s.add( (arr[16] * 48) + (arr[17] * 48) + (arr[18] * 48) + (arr[19] * 48) + (arr[20] * 48) + (arr[21] * 48) + (arr[22] * 48) + (arr[23] * arr[71]) == 34519 )
s.add( (arr[24] * arr[64]) + (arr[25] * 48) + (arr[26] * 48) + (arr[27] * 48) + (arr[28] * 48) + (arr[29] * 48) + (arr[30] * 48) + (arr[31] * 48) == 40429 )
s.add( (arr[24] * 48) + (arr[25] * arr[65]) + (arr[26] * 48) + (arr[27] * 48) + (arr[28] * 48) + (arr[29] * 48) + (arr[30] * 48) + (arr[31] * 48) == 41216 )
s.add( (arr[24] * 48) + (arr[25] * 48) + (arr[26] * arr[66]) + (arr[27] * 48) + (arr[28] * 48) + (arr[29] * 48) + (arr[30] * 48) + (arr[31] * 48) == 40542 )
s.add( (arr[24] * 48) + (arr[25] * 48) + (arr[26] * 48) + (arr[27] * arr[67]) + (arr[28] * 48) + (arr[29] * 48) + (arr[30] * 48) + (arr[31] * 48) == 40662 )
s.add( (arr[24] * 48) + (arr[25] * 48) + (arr[26] * 48) + (arr[27] * 48) + (arr[28] * arr[68]) + (arr[29] * 48) + (arr[30] * 48) + (arr[31] * 48) == 41248 )
s.add( (arr[24] * 48) + (arr[25] * 48) + (arr[26] * 48) + (arr[27] * 48) + (arr[28] * 48) + (arr[29] * arr[69]) + (arr[30] * 48) + (arr[31] * 48) == 40476 )
s.add( (arr[24] * 48) + (arr[25] * 48) + (arr[26] * 48) + (arr[27] * 48) + (arr[28] * 48) + (arr[29] * 48) + (arr[30] * arr[70]) + (arr[31] * 48) == 41090 )
s.add( (arr[24] * 48) + (arr[25] * 48) + (arr[26] * 48) + (arr[27] * 48) + (arr[28] * 48) + (arr[29] * 48) + (arr[30] * 48) + (arr[31] * arr[71]) == 41132 )
s.add( (arr[32] * arr[64]) + (arr[33] * 48) + (arr[34] * 48) + (arr[35] * 48) + (arr[36] * 48) + (arr[37] * 48) + (arr[38] * 48) + (arr[39] * 48) == 41663 )
s.add( (arr[32] * 48) + (arr[33] * arr[65]) + (arr[34] * 48) + (arr[35] * 48) + (arr[36] * 48) + (arr[37] * 48) + (arr[38] * 48) + (arr[39] * 48) == 42384 )
s.add( (arr[32] * 48) + (arr[33] * 48) + (arr[34] * arr[66]) + (arr[35] * 48) + (arr[36] * 48) + (arr[37] * 48) + (arr[38] * 48) + (arr[39] * 48) == 41790 )
s.add( (arr[32] * 48) + (arr[33] * 48) + (arr[34] * 48) + (arr[35] * arr[67]) + (arr[36] * 48) + (arr[37] * 48) + (arr[38] * 48) + (arr[39] * 48) == 41910 )
s.add( (arr[32] * 48) + (arr[33] * 48) + (arr[34] * 48) + (arr[35] * 48) + (arr[36] * arr[68]) + (arr[37] * 48) + (arr[38] * 48) + (arr[39] * 48) == 42328 )
s.add( (arr[32] * 48) + (arr[33] * 48) + (arr[34] * 48) + (arr[35] * 48) + (arr[36] * 48) + (arr[37] * arr[69]) + (arr[38] * 48) + (arr[39] * 48) == 41931 )
s.add( (arr[32] * 48) + (arr[33] * 48) + (arr[34] * 48) + (arr[35] * 48) + (arr[36] * 48) + (arr[37] * 48) + (arr[38] * arr[70]) + (arr[39] * 48) == 42345 )
s.add( (arr[32] * 48) + (arr[33] * 48) + (arr[34] * 48) + (arr[35] * 48) + (arr[36] * 48) + (arr[37] * 48) + (arr[38] * 48) + (arr[39] * arr[71]) == 42387 )
s.add( (arr[40] * arr[64]) + (arr[41] * 48) + (arr[42] * 48) + (arr[43] * 48) + (arr[44] * 48) + (arr[45] * 48) + (arr[46] * 48) + (arr[47] * 48) == 39330 )
s.add( (arr[40] * 48) + (arr[41] * arr[65]) + (arr[42] * 48) + (arr[43] * 48) + (arr[44] * 48) + (arr[45] * 48) + (arr[46] * 48) + (arr[47] * 48) == 39976 )
s.add( (arr[40] * 48) + (arr[41] * 48) + (arr[42] * arr[66]) + (arr[43] * 48) + (arr[44] * 48) + (arr[45] * 48) + (arr[46] * 48) + (arr[47] * 48) == 39432 )
s.add( (arr[40] * 48) + (arr[41] * 48) + (arr[42] * 48) + (arr[43] * arr[67]) + (arr[44] * 48) + (arr[45] * 48) + (arr[46] * 48) + (arr[47] * 48) == 39531 )
s.add( (arr[40] * 48) + (arr[41] * 48) + (arr[42] * 48) + (arr[43] * 48) + (arr[44] * arr[68]) + (arr[45] * 48) + (arr[46] * 48) + (arr[47] * 48) == 40032 )
s.add( (arr[40] * 48) + (arr[41] * 48) + (arr[42] * 48) + (arr[43] * 48) + (arr[44] * 48) + (arr[45] * arr[69]) + (arr[46] * 48) + (arr[47] * 48) == 39519 )
s.add( (arr[40] * 48) + (arr[41] * 48) + (arr[42] * 48) + (arr[43] * 48) + (arr[44] * 48) + (arr[45] * 48) + (arr[46] * arr[70]) + (arr[47] * 48) == 39881 )
s.add( (arr[40] * 48) + (arr[41] * 48) + (arr[42] * 48) + (arr[43] * 48) + (arr[44] * 48) + (arr[45] * 48) + (arr[46] * 48) + (arr[47] * arr[71]) == 39895 )
s.add( (arr[48] * arr[64]) + (arr[49] * 48) + (arr[50] * 48) + (arr[51] * 48) + (arr[52] * 48) + (arr[53] * 48) + (arr[54] * 48) + (arr[55] * 48) == 40046 )
s.add( (arr[48] * 48) + (arr[49] * arr[65]) + (arr[50] * 48) + (arr[51] * 48) + (arr[52] * 48) + (arr[53] * 48) + (arr[54] * 48) + (arr[55] * 48) == 40736 )
s.add( (arr[48] * 48) + (arr[49] * 48) + (arr[50] * arr[66]) + (arr[51] * 48) + (arr[52] * 48) + (arr[53] * 48) + (arr[54] * 48) + (arr[55] * 48) == 40126 )
s.add( (arr[48] * 48) + (arr[49] * 48) + (arr[50] * 48) + (arr[51] * arr[67]) + (arr[52] * 48) + (arr[53] * 48) + (arr[54] * 48) + (arr[55] * 48) == 40233 )
s.add( (arr[48] * 48) + (arr[49] * 48) + (arr[50] * 48) + (arr[51] * 48) + (arr[52] * arr[68]) + (arr[53] * 48) + (arr[54] * 48) + (arr[55] * 48) == 40824 )
s.add( (arr[48] * 48) + (arr[49] * 48) + (arr[50] * 48) + (arr[51] * 48) + (arr[52] * 48) + (arr[53] * arr[69]) + (arr[54] * 48) + (arr[55] * 48) == 40260 )
s.add( (arr[48] * 48) + (arr[49] * 48) + (arr[50] * 48) + (arr[51] * 48) + (arr[52] * 48) + (arr[53] * 48) + (arr[54] * arr[70]) + (arr[55] * 48) == 40692 )
s.add( (arr[48] * 48) + (arr[49] * 48) + (arr[50] * 48) + (arr[51] * 48) + (arr[52] * 48) + (arr[53] * 48) + (arr[54] * 48) + (arr[55] * arr[71]) == 40643 )
s.add( (arr[56] * arr[64]) + (arr[57] * 48) + (arr[58] * 48) + (arr[59] * 48) + (arr[60] * 48) + (arr[61] * 48) + (arr[62] * 48) + (arr[63] * 48) == 21223 )
s.add( (arr[56] * 48) + (arr[57] * arr[65]) + (arr[58] * 48) + (arr[59] * 48) + (arr[60] * 48) + (arr[61] * 48) + (arr[62] * 48) + (arr[63] * 48) == 21928 )
s.add( (arr[56] * 48) + (arr[57] * 48) + (arr[58] * arr[66]) + (arr[59] * 48) + (arr[60] * 48) + (arr[61] * 48) + (arr[62] * 48) + (arr[63] * 48) == 21310 )
s.add( (arr[56] * 48) + (arr[57] * 48) + (arr[58] * 48) + (arr[59] * arr[67]) + (arr[60] * 48) + (arr[61] * 48) + (arr[62] * 48) + (arr[63] * 48) == 21495 )
s.add( (arr[56] * 48) + (arr[57] * 48) + (arr[58] * 48) + (arr[59] * 48) + (arr[60] * arr[68]) + (arr[61] * 48) + (arr[62] * 48) + (arr[63] * 48) == 21152 )
s.add( (arr[56] * 48) + (arr[57] * 48) + (arr[58] * 48) + (arr[59] * 48) + (arr[60] * 48) + (arr[61] * arr[69]) + (arr[62] * 48) + (arr[63] * 48) == 21132 )
s.add( (arr[56] * 48) + (arr[57] * 48) + (arr[58] * 48) + (arr[59] * 48) + (arr[60] * 48) + (arr[61] * 48) + (arr[62] * arr[70]) + (arr[63] * 48) == 21148 )
s.add( (arr[56] * 48) + (arr[57] * 48) + (arr[58] * 48) + (arr[59] * 48) + (arr[60] * 48) + (arr[61] * 48) + (arr[62] * 48) + (arr[63] * arr[71]) == 21148 )

www = s.check()
model = s.model()
fff = [0 for i in range(72)]
# print model

for i in range(72):
	index = eval(str(model[i])[2:])
	fff[index] = eval(str(model[model[i]]))

flag = "".join([chr(fff[i]) for i in range(72)])
print "Flag: {}".format(flag)

```

kemudian running program 

![z3solver](https://bamsarts.github.io/img/z3-solver.png)

Voila :tada: dapat flag : **CTFX{linear_algebra_1s_1mport4nt_for_your_life_and_college_}**