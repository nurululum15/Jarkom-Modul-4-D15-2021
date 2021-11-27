# Laporan Praktikum Modul 4

## Anggota  Kelompok D15 :

## Richard Asmarakandi (05111940000017)

## Nurul Izzatil Ulum (05111940000058)



Pertama untuk menyambungkan node2 lainnya ke internet, isikan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.192.0.0/16` pada roter "Foosha"
Isikan `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada server2 lainnya agar nameservernya terganti dan bisa menerima internet. dan atur konfigurasi nodenya sesuai dengan modul sebelumnya.


## CIDR

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