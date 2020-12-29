# Jarkom_Modul5_Lapres_T15

Nama Anggota: 
  - Widyantari Febiyanti 05311840000030
  - Nabella Desyawulansari 05311840000039
  
  
## Soal Topologi
![1](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/topologi%20asli.png)

Diperintahkan membuat topologi seperti diatas dengan ketentuan: 
- __SURABAYA__ diberikan IP TUNTAP
- __MALANG__ merupakan DNS Server diberikan IP DMZ
- __MOJOKERTO__ merupakan DHCP Server diberikan IP DMZ
- __MADIUN__ dan __PROBOLINGGO__ merupakan WEB Server
- Setiap Server diberikan memory sebesar 128M
- Client dan Router diberikan memori sebesar 96M
- Jumlah host pada subnet __SIDOARJO__ 200 Host
- Jumlah host pada subnet __GRESIK__ 210 Host
  
## Perhitungan Subnetting
Yang dilakukan adalah: 
1. Melakukan pembagian subnet seperti dibawah ini:

![2](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/TOPOLOGI(1).jpg)

2. Setelah itu, buat pengelompokan tabel seperti dibawah ini:

![3](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/TABEL(1).jpg)

3. Untuk saat ini, digunakan perhitungan pembagian IP menggunakan cara VLSM. Dan didapatkan seperti dibawah ini:

![4](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/VLSM(1).jpg)

## Pembuatan Topologi di UML
Setelah mendapatkan IP untuk masing-masing kelompok, bisa langsung diaplikasikan dalam pembuatan topologi di PuTTY. Seperti dibawah: 

![5](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20putty.jpg)

## Setting interfaces antar UML 
Perlu dilakukan setting interfaces untuk setiap UML yang telah dibuat. Berikut akan dilampirkan setting interface untuk setiap UML, (untuk router): 

- __SURABAYA__

![6](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20router%202.png)

- __BATU__ dan __KEDIRI__

![7](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20router%201.png)

Untuk server: 

- __MADIUN__

![8](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20server%202.png)

- __MALANG__, __MOJOKERTO__, dan __PROBOLINGGO__

![9](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20server.png)

Untuk client: 

- __GRESIK__ dan __SIDOARJO__

![10](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20client.png)

## Setting Routing disetiap UML Router
Diperlukan routing agar setiap UML bisa saling berhubungan. Routing ini diberikan untuk setiap router yang menjadi penghubung antar server maupun client. Pemasangan routing seperti dibawah: 

Untuk router __SURABAYA__, __BATU__, dan __KEDIRI__: 

![11](https://github.com/belladewusa/Jarkom_Modul5_Lapres_T15/blob/main/JARKOM%20MODUL%205/uml%20routing%20setiap%20router.png)




## Nomor 1

Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.

Jalankan di SURABAYA

`iptables -t nat -A POSTROUTING -s -192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.76.85`

KETERANGAN :
POSTROUTING : Digunakan untuk mentranslasi address setelah proses routing. Dilakukan dengan merubah source IP Address dari paket data
-s, --source address 	mendefinisikan opsi alamat asal dari paket
-o, --out-interface name 	mendefinisikan opsi interface yang dilihat keluar paketnya
-j, --jump targetname 	mendefinisikan rule akan menggunakan target yang mana
SNAT (Source NAT), yaitu ketika anda mengubah alamat asal dari paket pertama dengan kata lain anda mengubah dari mana koneksi terjadi.


## Nomor 2

Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

Jalankan di SURABAYA

`iptables -A FORWARD -p tcp --dport 22 -d 10.151.77.168/29 -i eth0 -j DROP`

KETERANGAN : 
-A, --append chain rule-specification 	menambahkan rules pada chain
FORWARD : Untuk menyaring paket yang menuju ke NIC lain dalam sever atau host lain (hanya diteruskan/melewati firewall).
-d, --destination address 	mendefinisikan opsi alamat tujuan dari paket
-i, --in-interface name 	mendefinisikan opsi interface yang dilihat masuk paketnya
DROP – Firewall akan menolak paket data.

## Nomor 3

Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server , selebihnya akan di DROP.

jalankan  di MALANG dan MOJOKERTO

`iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP`

KETERANGAN :
 connlimit-above untuk menempatkan pembatasan
–connlimit-mask 0 : Kelompok host berasal dari alamat mana saja.

## Nomor 4

kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: (4) Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. Selain itu paket akan di REJECT.

Jalankan di MALANG

    iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
    iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 17.01 --timestop 23.59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
    iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 00:00 --timestop 23.59 --weekdays Sat,Sun -j REJECT
    
## Nomor 5 

kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: (5) Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Selain itu paket akan di REJECT.

Jalankan di MALANG

`iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 07:01 --timestop 16:59 -j REJECT`

## Nomor 6 

Karena kita memiliki 2 buah WEB Server, (6) Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.

    iptables -t nat -A PREROUTING -p tcp --dport 80 -m state --state NEW -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.0.18:80

    iptables -t nat -A PREROUTING -p tcp --dport 80 -m state --state NEW -m statistic --mode nth --every 1 --packet 0 -j DNAT --to-destination 192.168.0.19:80

KETERANGAN : 

atas : ip probolinggo
bawah : ip madiun

## Nomor 7

Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.

jalankan iptables di UML SURABAYA

    iptables -N LOGGING 
    iptables -A FORWARD -j LOGGING 
    iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 
    iptables -A LOGGING -j DROP

jalankan iptables di UML MALANG

    iptables -N LOGGING 
    iptables -A INPUT -j LOGGING 
    iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 
    iptables -A LOGGING -j DROP

jalankan iptables di UML MOJOKERTO 

    iptables -N LOGGING 
    iptables -A INPUT -j LOGGING 
    iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4 
    iptables -A LOGGING -j DROP
    
KETERANGAN :
iptables -N LOGGING: Buat rantai baru bernama LOGGING
-m batas: Ini menggunakan modul pencocokan batas. Dengan menggunakan ini, Anda dapat membatasi logging menggunakan opsi –limit.
–Limit 2 / min: Ini menunjukkan rata-rata tingkat kecocokan maksimum . Dalam contoh ini, untuk paket serupa itu akan membatasi logging menjadi 2 per menit. Anda juga dapat menentukan 2 / detik, 2 / menit, 2 / jam, 2 / hari.
-j LOG: Ini menunjukkan bahwa target untuk paket ini adalah LOG. yaitu menulis ke file log.
–Log-prefix “IPTables-Dropped:” Anda dapat menentukan awalan log apa pun, yang akan ditambahkan ke pesan log yang akan ditulis ke file / var / log / messages
–Log-level 4 Ini adalah level syslog standar. 4 adalah peringatan. Anda dapat menggunakan nomor dari kisaran 0 hingga 7. 0 adalah darurat dan 7 adalah debug.

