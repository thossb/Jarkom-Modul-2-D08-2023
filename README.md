# Jarkom-Modul-2-D08-2023

Nama Anggota | NRP
------------------- | --------------		
Timothy Hosia Budianto | 5025211098
Arif Nugraha Santosa | 5025211048

## Soal
1. Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 
2. Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.
3. Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.
4. Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
5. Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)
6. Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.
7. Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
8. Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.
9. Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.
10. Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
11. Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy
12. Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.
13. Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
14. Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).
15. Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
16. Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 
17. Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
18. Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
19. Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)
20. Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

PS:
yyy pada url adalah kode kelompok anda
File requirement dapat diakses melalui drive berikut.

## Langkah - langkah penyelesaian
### Setup dan nomer (1,2,3)
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

### Lakukan DNS forwarding, agar bisa akses internet
11.	edit file /etc/bind/named.conf.options pada server dns master
	 ![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/0402704f-1553-4bfd-a599-3390133b15bb)
12.	service bind9 restart dan coba ping google.com

### Menyiapkan backup pada .bashrc
13.	Tulis code berikut pada /root/.bashrc (lakukan pada dns server)
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/d6894d53-34c2-4a82-83c3-0fa4d970a5f0)
14.	Lalu kita mkdir /root/prak1/bind lalu lakukan cp -r -f /etc/bind /root/prak1 setiap memodifikasi file bind untuk menyimpan /etc/bind, sehingga konfigurasi bind bisa dipanggil saat kita memulai project di bash script

### Membuat subdomain (nomor 4)
15.	 Membuat parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
16.	Atur di nano /etc/bind/prak1/abimanyu pada dns master (yudhistira) masukan parsekit in a - lalu ip node abimanyu
17.	Save, bind restart, dan coba ping

### Membuat reverse domain abimanyu (nomer 5)
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

### Membuat DNS slave untuk (nomor 6)
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

### Membuat subdomain baratayuda di dns slave setelah diarahkan dari dns master (nomor 7 dan nomor 8) 
32. pertama modifikasikonfigurasi abimanyu.d08.com pada /etc/bind/prak1 di dnsmaster untuk prefix baratayuda diarahkan ke dns slave menjadi
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/7c900e77-2749-4a48-94f8-c56689414c67)
33. jangan lupa untuk modifikasi named.conf.options comment dnssec-validation auto; dan tambahkan allow-query{any;}; // pada dns master dan slave
34. juga pastikan named.nonf.local domain abimanyu di beri allow transfer ke dns slave lalu restart bind9.
35. Lalu modifikasi domain baratayuda.abimanyu.d08.com pada dns slave menjadi seperti berikut
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/9b290749-7259-43e3-92cf-ded0afb89da4)
36. Lalu buat direktori mkdir /etc/bind/delegasi dan buat file untuk mengkonfigurasi bind baratayuda dengan cp /etc/bind/db.local ke direktori didalam delegasi.
37. kemudian edit file baratayuda... menjadi seperti berikut 
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/f9095378-7bba-4270-a5ed-c7da54c08b5b)
38. Lakukan restart bind9 llalu testing dengan ping ke domain domain yang baru dibuat tersebut

### Nomor 9
### Nomor 10
### Nomor 11
### Nomor 12
### Nomor 13
### Nomor 14
### Nomor 15
### Nomor 16
### Nomor 17
### Nomor 18
### Nomor 19
### Nomor 20










