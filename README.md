# Laporan Praktikum Modul 4

## Anggota  Kelompok D15 :

## Richard Asmarakandi (05111940000017)

## Nurul Izzatil Ulum (05111940000058)



Pertama untuk menyambungkan node2 lainnya ke internet, isikan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.192.0.0/16` pada roter "Foosha"
Isikan `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada server2 lainnya agar nameservernya terganti dan bisa menerima internet. dan atur konfigurasi nodenya sesuai dengan modul sebelumnya.

## VLSM di GNS3

* Pertama menentukan nama dan panjang atau area untuk setiap subnetnya
![nyoba tree subnetting_page-0001](https://user-images.githubusercontent.com/76694068/143686688-91b23e9c-e40c-46ee-8526-e180285333dd.jpg)

* Setelah mendapatkan nama dan alokasi subnet, daftar nama nama semua subnet, jumlah hostnya serta lenghtnya
![image](https://user-images.githubusercontent.com/76694068/143686735-2709fc63-e667-45c9-916b-96d0dc99142c.png)

* Kemudian, cari NID yang dibutuhkan melalui metode tree
![PohFak-Page-2 drawio](https://user-images.githubusercontent.com/76694068/143686764-b67cdd55-7e39-496f-b6d8-d99702185a99.png)
      Keterangan :
      
     Contoh cara menghitung setiap branch IP tree :

     contoh pada NID 192.199.0.0 /19 akan diubah ke /20
     pada tabel di modul akan diperlihatkan jika wildcard yang dimiliki /20 adalah 0.0.15.255.

     Berarti untuk tree bagian kiri range nilainya akan ada diantara
     Batas bawah 	: 192.199.0.0 /20 sampai
     Batas atas 	: 192.199.(0+15).(0+255) = 192.199.15.255

     Untuk tree bagian kanan, batas bawahnya ditambah dengan 0.0.0.1 untuk IP dari subnet itu sendiri dan karena telah dipakai oleh batas atas cabang kanan maka total menjadi  
     192.199.15.256.

     Perlu diingat jika batas max dari bit ipv4 adalah 255, maka alokasi bit yang berlebihan akan dipindahkan ke depannya sehingga menjadi 192.199.16.0

     Sedang untuk tree bagian kanan range nilainya akan ada diantara
     Batas bawah	: 192.199.16.0 sampai
     Batas atas 	: 192.199.(16+15).(0+255) = 192.199.31.255

     Sama seperti untuk sisi kiri tree, jika bagian ini diturunkan, maka bagian ini juga ditambah 0.0.0.1 dan akan berubah menjadi 192.199.32.0
     Perlu diingat pula, bahwa yang ditulis dalam tree adalah batas bawah dari masing masing cabang

```
Lakukan langkah diatas sampai seluruh lenght di dalam daftar terpenuhi.
```

* Jika telah menemukan NID, maka NID, Netmask, dan Broadcastnya
![list_page-0001](https://user-images.githubusercontent.com/76694068/143686902-5061b6f2-1c5e-4909-b98a-00a2c9406600.jpg)


### Routing
* Untuk routing, pertama masukkan konfigurasi kedalam semua node. sertakan `gateway` untuk tiap `eth0` dalam tiap node. `gateway` digunakan untuk memberikan akses internet pada node, agar ada jalan untuk terhubung dengan internet.
* Gateaway ditentukan dengan melihat IP mana yang dituju oleh node tersebut, atau tempat dimana node tersebut akan keluar.
```
FOOSHA
# Foosha (Router)
auto eth0
iface eth0 inet dhcp

# Mengarah ke Blueno A1
auto eth1
iface eth1 inet static
  address 192.199.8.1
  netmask 255.255.252.0
  
# Mengarah ke Doriki A14
auto eth2
iface eth2 inet static
  address 192.199.27.161
  netmask 255.255.255.252

# Mengarah ke Guanhao A7
auto eth4
iface eth4 inet static
  address 192.199.27.153
  netmask 255.255.255.252
  
# Mengarah ke Water7 A3
auto eth3
iface eth3 inet static
  address 192.199.27.145
  netmask 255.255.255.252

```
```
WATER7
Water7 (Router)
# Mengarah ke Foosha A3
auto eth0
iface eth 0 inet static
  address 192.199.27.146
  netmask 255.255.255.252
  gateway 192.199.27.145

#mengarah ke chiper A2
auto eth2
iface eth2 inet static
  address 192.199.16.1
  netmask 255.255.252.0

#mengarah ke pucci A4
auto eth1
iface eth1 inet static
address 192.199.27.149
netmask 255.255.255.252

```
```
GUANHAO
#mengarah ke foosha A7
auto eth0
iface eth0 inet static
address 192.199.27.154
netmask 255.255.255.252
gateway 192.199.27.153

#subnet ke jabra A8
auto eth1
iface eth1 inet static
address 192.199.20.1
netmask 255.255.252.0

#subnet ke oimo A11
auto eth3
iface eth3 inet static
address 192.199.27.157
netmask 255.255.255.252

#subnet ke maingate/alabasta A6
auto eth2
iface eth2 inet static
address 192.199.24.1
netmask 255.255.254.0
```
```
PUCCI
#subnet kembali ke water7 A4
auto eth0
iface eth0 inet static
address 192.199.27.150
netmask 255.255.255.252
gateway 192.199.27.149

#subnet ke courtyard/calmbelt A9
auto eth2
iface eth2 inet static
address 192.199.0.1
netmask 255.255.248.0

#subnet ke Jipangu A5
auto eth1
iface eth1 inet static
address 192.199.27.1
netmask 255.255.255.128

```
```
ALABASTA
#subnet ke maingate A6
auto eth0
iface eth0 inet static
address 192.199.24.2
netmask 255.255.254.0
gateway 192.199.24.1

#subnet ke jorge A10
auto eth1
iface eth1 inet static
address 192.199.27.129
netmask 255.255.255.240

```
```
OIMO
#subnet ke guanhao A11
auto eth0
iface eth0 inet static
address 192.199.27.158
netmask 255.255.255.252
gateway 192.199.27.157

#subnet ke fukurou A15
auto eth1
iface eth1 inet static
address 192.199.27.165
netmask 255.255.255.252

#subnet ke enieslobby/seastone A12
auto eth2
iface eth2 inet static
address 192.199.26.1
netmask 255.255.255.0

```
```
SEASTONE
#subnet ke enieslobby A12
auto eth0
iface eth0 inet static
address 192.199.26.2
netmask 255.255.255.0
gateway 192.199.26.1

#subnet ke elena A13
auto eth1
iface eth1 inet static
address 192.199.12.1
netmask 255.255.252.0

```
Sedangkan untuk konfigurasi client :
```
BLUENO
# Blueno (1000 Host) A6
auto eth0
iface eth0 inet static
  address 192.199.8.2
  netmask 255.255.252.0
  gateway 192.199.8.1

DORIKI
#Doriki (client)
auto eth0
iface eth0 inet static
address 192.199.27.162
netmask 255.255.255.252
gateway 192.199.27.161

CHIPER
#subnet ke water7 A2
auto eth0
iface eth0 inet static
address 192.199.16.2
netmask 255.255.252.0
gateway 192.199.16.1

CALMBELT
#subnet ke pucci A9
auto eth0
iface eth0 inet static
address 192.199.0.2
netmask 255.255.248.0
gateway 192.199.0.1

COURTYARD
#subnet ke pucci A9
auto eth0
iface eth0 inet static
address 192.199.0.3
netmask 255.255.248.0
gateway 192.199.0.1

JIPANGU
#subnet ke pucci A5
auto eth0
iface eth0 inet static
address 192.199.27.2
netmask 255.255.255.128
gateway 192.199.27.1

JABRA
#subnet ke guanhao A8
auto eth0
iface eth0 inet static
address 192.199.20.2
netmask 255.255.252.0
gateway 192.199.20.1

MAINGATE
#subnet A6
auto eth0
iface eth0 inet static
address 192.199.24.3
netmask 255.255.254.0
gateway 192.199.24.1

JORGE
#subnet A10
auto eth0
iface eth0 inet static
address 192.199.27.130
netmask 255.255.255.240
gateway 192.199.27.129

ENIESLOBBY
#subnet A12
auto eth0
iface eth0 inet static
address 192.199.26.3
netmask 255.255.255.0
gateway 192.199.26.1

ELENA
#subnet A13
auto eth0
iface eth0 inet static
address 192.199.12.2
netmask 255.255.252.0
gateway 192.199.12.1

FUKUROU
#subnet A15
auto eth0
iface eth0 inet static
address 192.199.27.166
netmask 255.255.255.252
gateway 192.199.27.165

```

* Kemudian untuk routing sendiri, dimasukkan pada router yang mempunyai node yang tidak terhubung langsung dengannya. Maka dari pertimbangan tersebut, didapatkan :
```
**FOOSHA**
#A2 CHIPER
route add -net 192.199.16.0 netmask 255.255.252.0 gw 192.199.27.146
#A4 PUCCI
route add -net 192.199.27.148 netmask 255.255.255.252 gw 192.199.27.146
#A5 JIPANGU
route add -net 192.199.27.148 netmask 255.255.255.252 gw 192.199.27.146
#A9 CALMBELT/COURTYARD
route add -net 192.199.0.0 netmask 255.255.248.0 gw 192.199.27.146

#A6 MAINGATE
route add -net 192.199.24.0 netmask 255.255.254.0 gw 192.199.27.154
#A8 JABRA
route add -net 192.199.20.0 netmask 255.255.252.0 gw 192.199.27.154
#A12 ENIESLOBBY
route add -net 192.199.26.0 netmask 255.255.255.0 gw 192.199.27.154
#A13 ELENA
route add -net 192.199.12.0 netmask 255.255.252.0 gw 192.199.27.154
#A15 FUKUROU
route add -net 192.199.27.164 netmask 255.255.255.252 gw 192.199.27.154
#A10 JORGE
route add -net 192.199.27.128 netmask 255.255.255.240 gw 192.199.27.154
#A11 OIMO
route add -net 192.199.27.156 netmask 255.255.255.252 gw 192.199.27.154

**WATER 7**
#A9 CALMBELT/COURTYARD
route add -net 192.199.0.0 netmask 255.255.248.0 gw 192.199.27.150
#A5 JIPANGU
route add -net 192.199.27.148 netmask 255.255.255.252 gw 192.199.27.150
#A3 FOOSHA
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.199.27.145

**GUANHAO**
#A10 JORGE
route add -net 192.199.27.128 netmask 255.255.255.240 gw 192.199.24.2
#A12 ENIESLOBBY
route add -net 192.199.26.0 netmask 255.255.255.0 gw 192.199.27.158
#A13 ELENA
route add -net 192.199.12.0 netmask 255.255.252.0 gw 192.199.27.158
#A15 FUKUROU
route add -net 192.199.27.164 netmask 255.255.255.252 gw 192.199.27.158
#A7 FOOSHA
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.199.27.153 

**OIMO**
#A13 ELENA
route add -net 192.199.12.0 netmask 255.255.252.0 gw 192.199.26.2
#A11 GUANHAO
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.199.27.157 

```
Maka dikatakan brhasil jika bisa ping pada internet.


## CIDR di CPT

Pertama-tama kita tentukan nama dan panjang subnet untuk setiap subnetnya.
![0](https://cdn.discordapp.com/attachments/804405775988555776/914141419193634826/unknown.png)

Lalu kita gabungkan subnet yang terluar. Dalam kasus ini adalah subnet A15 dengan jarak 4 Subnet terhadap Foosha. Maka kita gabungkan dengan Subnet A14 dengan jarak 3 Subnet terhadap Foosha karena Subnet A15 merupakan sambungan dari Subnet A14. Kita namai Subnet B1 dengan panjang subnet /21. Caranya dengan kita ambil yang terkecil dari yang digabungkan (A14/24 dan A15/22), yaitu /22 lalu dikurang 1 menjadi /21
![1](https://cdn.discordapp.com/attachments/804405775988555776/914142704240296026/unknown.png)

Setelah itu kita cari subnet dengan jarak 3 Subnet terhadap Foosha, yaitu ada A1, A2, A11, B1 dan A13. Kita Gabungkan A1 A2 karena sama-sama berjarak 3 subnet dari Foosha, dan keduanya merupakan lanjutan dari subnet A3. Lalu A13 dan B1, alasannya sama dengan penggabungan A1 dan A2. Lalu untuk A11 kita gabungkan dengan A10, karena tidak ada yang sama-sama berjarak 3 subnet dari Foosha yang merupakan lanjutan dari subnet A10 juga. Hal ini dilakukan terus menerus hingga semua subnet berhasil dihubungkan menjadi 1 subnet
![1](https://cdn.discordapp.com/attachments/804405775988555776/914144869985951744/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914145692862255125/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914145850358394911/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914145900677464075/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914145930150834207/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914145968239304704/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914146003282714644/unknown.png)
![1](https://cdn.discordapp.com/attachments/804405775988555776/914146049499758702/unknown.png)

Sehingga didapatkan subnet yang didapatkan adalah subnet dengan panjang /15 atau sekitar 131070 Usable IP Address. Berikutnya kita gambarkan Pohon Faktornya.
![1](https://cdn.discordapp.com/attachments/804405775988555776/914148781690323015/unknown.png)

Lalu dari pohon faktor tersebut, kita dapat mendapatkan NID, Netmask dari panjang subnet, serta Broadcast addressnya.
![1](https://cdn.discordapp.com/attachments/804405775988555776/914150641591521300/unknown.png)

Lalu kita buat Topologinya sesuai dengan permintaan soal, lalu menghubungkannya dengan logo kabel yang bertuliskan "Automatically Choose Connection Type".
![1](https://cdn.discordapp.com/attachments/804405775988555776/914109094175080458/unknown.png)

Setelah topologi sesuai dengan yang diminta, maka kita perlu mengatur IP pada setiap nodenya (Router, Client, Server). untuk mengaturnya kita perlu mengecek sambungannya. Misalnya Subnet A5
![2](https://media.discordapp.net/attachments/804405775988555776/914112265651904512/unknown.png?width=842&height=473)

Jika dilihat pada topologi Cisco Packet Tracer, A5 akan terletak seperti pada gambar :
![3](https://cdn.discordapp.com/attachments/804405775988555776/914111443538944030/unknown.png).

Karena itu pembagian IP Subnet A5 kita gunakan pada Foosha Fast ethernet 1/1 dan  Water7 Fast ethernet 0/0. Kita dapatkan Network ID 192.199.27.0 dengan subnet 255.255.255.128 pada subnet A5. Maka kita masukkan salah satu IP dengan 192.199.27.1 dan satu lainnya dengan 192.199.27.2 agar tidak bentrok. Contohnya 192.199.27.1 pada Foosha, Maka Fast Ethernet 1/1 untuk Foosha confignya akan terlihat seperti berikut :
![4](https://cdn.discordapp.com/attachments/804405775988555776/914114776269987880/unknown.png)
Lalu Fast Ethernet 0/0 untuk Water7 confignya akan terlihat seperti berikut :
![5](https://media.discordapp.net/attachments/804405775988555776/914115409546997850/unknown.png?width=842&height=473)

Dan Cara tersebut dilakukan di setiap sambungan pada setiap node. Lalu Untuk Client atau PC, kita perlu mengatur IP Configurationnya. IPv4 Address tinggal menyamakan dengan IP PC yang tersambung. Lalu untuk gateawaynya, berikan IP address dari router yang memberikan PC tersebut IP. Misalnya pada Blueno, maka yang memberikan Blueno IP adalah Router Foosha, tepatnya pada Foosha sambungan FastEthernet 0/1. Sehingga gateway blueno sama dengan IP Foosha FastEthernet 0/1.
![6](https://cdn.discordapp.com/attachments/804405775988555776/914118994947108914/unknown.png)
![7](https://cdn.discordapp.com/attachments/804405775988555776/914119012089204786/unknown.png)
![8](https://cdn.discordapp.com/attachments/804405775988555776/914119624793141248/unknown.png)

Lalu untuk Routing, perlu dilakukan pada subnet yang diturunkan pada Router, serta yang tidak langsung tersambung dengan router yang sedang dikonfigurasikan. Misalnya kita mengkonfigurasikan router Foosha. Foosha mengepalai ke hampir semua subnet. Maka routing dilakukan pada semua subnet kecuali subnet A5, A6, A7, A8 karena telah secara langsung tersambung ke router Foosha. Sisanya kita perlu memasukkannya.
![9](https://cdn.discordapp.com/attachments/804405775988555776/914120148602986506/unknown.png)

```
Jadi Routing pada Foosha akan berisi :\
Network 192.199.16.0/30 Next Hop 192.199.64.2\
Network 192.199.0.0/21 Next Hop 192.199.64.2\
Network 192.199.8.0/25 Next Hop 192.199.64.2\
Network 192.199.32.0/22 Next Hop 192.199.64.2\
Network 192.200.36.0/22 Next Hop 192.200.64.2\
Network 192.200.32.0/23 Next Hop 192.200.64.2\
Network 192.200.34.0/28 Next Hop 192.200.64.2\
Network 192.200.16.0/30 Next Hop 192.200.64.2\
Network 192.200.8.0/30 Next Hop 192.200.64.2\
Network 192.200.4.0/24 Next Hop 192.200.64.2\
Network 192.200.0.0/22 Next Hop 192.200.64.2\
```

Untuk cara memasukkannya, klik tab static pada menu ROUTING. untuk network kita isikan NID, Mask berarti Netmask, lalu next hop merupakan IP terdekat dari Router yang sedang dikonfigurasikan. Dalam kasus ini kita mengkonfigurasikan Foosha. Maka untuk Subnet A3, A1, A2 dan A4 kita masukkan IP Water7 yang mengarah ke Foosha, karena subnet2 tersebut diturunkan dari Water7 dan Water7 terhubung langsung dengan Foosha. Lalu untuk subnet A9, A10, A11, A12, A13, A14, A15 kita masukkan IP GuanHao yhang mengarah ke Foosha, karena subnet2 tersebut diturunkan dari GuanHao dan GuanHao terhubung langsung dengan Foosha. Karena pada Foosha A5, A6, A7, A8 tidak dimasukkan routing, seharusnya pada Foosha terdapat 11 Subnet yang perlu di routing.
![10](https://cdn.discordapp.com/attachments/804405775988555776/914129410242777119/unknown.png)

Contoh yang lain misal kita mencoba melakukan konfigurasi routing pada router Water7. Water7 mengepalai subnet A3, A1, A2, A4. Namun karena A3 dan A4 telah terhubung langsung ke router Water7, maka tidak perlu dilakukan routing pada subnet tersebut. Sehingga seharusnya pada router Water7 terdapat 2 routing, yaitu A1 dan A2, ditambah 1 route untuk gateway dari Water7 kembali ke parentnya, yaitu Foosha.

```
Jadi Routing pada Water7 akan berisi :
Network 192.199.8.0/25 Next Hop 192.199.16.2
Network 192.199.0.0/21 Next Hop 192.199.16.2
Network 0.0.0.0/0 Next Hop 192.199.64.1
```

Begitu seterusnya untuk semua Node.
