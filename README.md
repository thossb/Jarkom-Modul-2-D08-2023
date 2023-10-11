# Jarkom-Modul-2-D08-2023

## Setup dan nomer (1,2,3)
1.	Susun topologi dan koneksi
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/15934f35-f4cd-4149-9285-3bd004a9f58d)
```
eth 1

auto eth0
iface eth0 inet static
	address 192.195.1.2
	netmask 255.255.255.0
	gateway 192.195.1.1

auto eth0
iface eth0 inet static
	address 192.195.1.3
	netmask 255.255.255.0
	gateway 192.195.1.1
----------------------------------------

eth 2

auto eth0
iface eth0 inet static
	address 192.195.2.2
	netmask 255.255.255.0
	gateway 192.195.2.1

auto eth0
iface eth0 inet static
	address 192.195.2.3
	netmask 255.255.255.0
	gateway 192.195.2.1

auto eth0
iface eth0 inet static
	address 192.195.2.4
	netmask 255.255.255.0
	gateway 192.195.2.1

auto eth0
iface eth0 inet static
	address 192.195.2.5
	netmask 255.255.255.0
	gateway 192.195.2.1

------------------------------------------
eth 3

auto eth0
iface eth0 inet static
	address 192.195.3.2
	netmask 255.255.255.0
	gateway 192.195.3.1

auto eth0
iface eth0 inet static
	address 192.195.3.3
	netmask 255.255.255.0
	gateway 192.195.3.1


-------------------------------------------
```
3.	Set iptable di router (**iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.195.0.0/16**), dan taruh di bash script agar tidak hilang
4.	Echo nameserver di node terluar, untuk connect dengan router agar terconnect dengan internet, dan taro di bashscript agar tidak perlu diulang (**echo nameserver 192.168.122.1 > /etc/resolv.conf**)
5.	Instalasi bind di dns master
6.	Buat domain di nano /etc/bind/named.conf.local
7.	Buat directory untuk menyimpan konfigurasi domain yaitu prak1
8.	Copy db.local pada path /etc/bind ke dalam folder prak1 yang baru saja dibuat dan ubah namanya menjadi arjuna.d08.com dan abimanyu.d08.com
9.	Kemudian konfigurasi arjuna.d08.com dan abimanyu.d08.com
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/a4956d84-253c-42d5-a71e-5ae268c25e42)
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/928bffe2-9309-4d95-a0bf-267988dfc54e)

10.	Restart bind9 dengan perintah service bind9 restart
11.	Arahkan client nakula dan sadewa dengan nano /etc/resolv.conf dan tulis nameserver ip dns master kita, untuk mencoba ping arjuna dan abimanyu

## Lakukan DNS forwarding, agar bisa akses internet
11.	edit file /etc/bind/named.conf.options pada server dns master
	 ![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/0402704f-1553-4bfd-a599-3390133b15bb)
12.	service bind9 restart dan coba ping google.com

## Menyiapkan backup pada .bashrc
13.	Tulis code berikut pada /root/.bashrc (lakukan pada dns server)
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/d6894d53-34c2-4a82-83c3-0fa4d970a5f0)
14.	Lalu kita mkdir /root/prak1/bind lalu lakukan cp -r -f /etc/bind /root/prak1 setiap memodifikasi file bind untuk menyimpan /etc/bind, sehingga konfigurasi bind bisa dipanggil saat kita memulai project di bash script

## Membuat subdomain (nomor 4)
15.	 Membuat parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
16.	Atur di nano /etc/bind/prak1/abimanyu pada dns master (yudhistira) masukan parsekit in a - lalu ip node abimanyu
17.	Save, bind restart, dan coba ping

## Membuat reverse domain abimanyu (nomer 5)
18.	Edit file /etc/bind/named.conf.local pada dns master
19.	Tambah domain reverse dari ip abimanyu </br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/0dee95b3-ec2e-4f80-aa22-5c81910ac9e9)
20.	Copykan file db.local dari path /etc/bind ke dalam folder prak1 yang baru saja dibuat dan ubah namanya menjadi 2.168.192.in-addr.arpa
21.	Edit file 2.168.192.in-addr.arpa menjadi seperti gambar di bawah ini </br>
 ![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/6d24d5df-fdb7-4b44-a360-75e5cc7c678e)
22.	service bind9 restart
23.	Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/bef2af5c-f2dd-4289-97af-23f2ca13830f) 
Hasilnya </br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/4c9678b9-471a-4042-8e73-1378ac972035)
24.	Masukan apt-get update dan install dns utils ke /root/.bashscr client, agar otomatis dimulai saat memulai project.

## Membuat DNS slave untuk (nomor 6)
25.	Pertama kita konfigurasi bind dns master, Edit file /etc/bind/named.conf.local dan sesuaikan dengan syntax berikut
 ![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/48ece8a5-6e0e-4043-86d1-3b48072f917d)
Tambahkan pada domain arjuna dan abimanyu
```
    notify yes;
    also-notify { "IP dns slave"; }; // Masukan IP dns slave tanpa tanda petik
    allow-transfer { "IP dns slave"; }; // Masukan IP dns slave tanpa tanda petik
```
Lalu restart bind service bind9 restart

### konfigurasi dns slave
27. Lalu kita konfigurasi dns slave. Lakukan apt-get update dan apt-get install bind9 -y pada dns slave.
28.	Kemudian buka file /etc/bind/named.conf.local pada Dns slave dan tambahkan syntax berikut
zone "namadomain.com (arjuna dan abimanyu)" {
    type slave;
    masters { "IP DNS master"; }; // Masukan IP DNS master tanpa tanda petik
    file "/var/lib/bind/jarkom2022.com"; // ganti jadi nama domain
};
Lalu restart bind service bind9 restart

### Testing
29.	Testing… matikan server DNS master dengan service bind9 service bind9 stop
30.	Pada client pastikan /etc/resolv.conf mengarah ke ip dns master dan dns slave dengan
Nameserver IP DNS master
Nameserver IP DNS slave
31.	Lakukan ping dari client </br>
*note untuk menyalakan lagi dns server lakukan service bind9 restart , 2 server dns bisa berjalan bersamaan </br>
*note backup konfigurasi dns slave dan master ke root/bind/ seperti cara diatas (termasuk update dan install bind di bash script). </br>
*note masukan echo nameserver dns master dan slave, Juga masukan apt-get update dan install dns utils ke /root/.bashscr client, agar otomatis dimulai saat memulai project. </br>

## Membuat subdomain baratayuda di dns slave setelah diarahkan dari dns master (nomor 7) 










