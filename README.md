# Jarkom_Modul5_Lapres_T15

Nama Anggota: 
  - Widyantari Febiyanti 05311840000030
  - Nabella Desyawulansari 05311840000039
  

  
## Nomor 1

Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.
----------------------

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
--------------------

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
--------------------------------

jalankan  di MALANG dan MOJOKERTO

`iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP`

KETERANGAN :
 connlimit-above untuk menempatkan pembatasan
–connlimit-mask 0 : Kelompok host berasal dari alamat mana saja.

## Nomor 4

kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: (4) Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. Selain itu paket akan di REJECT.
------------------------------

Jalankan di MALANG

    iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
    iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 17.01 --timestop 23.59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
    iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 00:00 --timestop 23.59 --weekdays Sat,Sun -j REJECT
    
## Nomor 5 

kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: (5) Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Selain itu paket akan di REJECT.
------------------------------

Jalankan di MALANG

`iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 07:01 --timestop 16:59 -j REJECT`

## Nomor 6 

Karena kita memiliki 2 buah WEB Server, (6) Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.
----------------

## Nomor 7

Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.
------------------------

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

