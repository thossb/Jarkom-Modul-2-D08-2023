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

## IP ADDRESS KELOMPOK D08
### DNS Server:
Werkudara (DNS Slave):
```
auto eth0
iface eth0 inet static
	address 192.195.1.2
	netmask 255.255.255.0
	gateway 192.195.1.1
```
Yudhistira (DNS Master):
```
auto eth0
iface eth0 inet static
	address 192.195.1.3
	netmask 255.255.255.0
	gateway 192.195.1.1
```

### Web Server
Arjuna (LB):
```
auto eth0
iface eth0 inet static
	address 192.195.2.2
	netmask 255.255.255.0
	gateway 192.195.2.1
```
Wisanggeni:
```
auto eth0
iface eth0 inet static
	address 192.195.2.3
	netmask 255.255.255.0
	gateway 192.195.2.1
```
Prabukusuma:
```
auto eth0
iface eth0 inet static
	address 192.195.2.4
	netmask 255.255.255.0
	gateway 192.195.2.1
```
Abimanyu:
```
auto eth0
iface eth0 inet static
	address 192.195.2.5
	netmask 255.255.255.0
	gateway 192.195.2.1
```

### Client
__NOTE: SETIAP CLIENT NAMESERVER PADA FILE "/etc/resolv.conf" MENGARAH KE IP DNS MASTER DAN DNS SLAVE"__

Nakula:
```
auto eth0
iface eth0 inet static
	address 192.195.3.2
	netmask 255.255.255.0
	gateway 192.195.3.1
```
Sadewa:
```
auto eth0
iface eth0 inet static
	address 192.195.3.3
	netmask 255.255.255.0
	gateway 192.195.3.1
```

### Router
Pandudewanata:
```
eth0: DHCP dari NAT
eth1: 192.195.1.1 (ke DNS server)
eth2: 192.195.2.1 (ke Web server)
eth3: 192.195.3.1 (ke client)
```

## PENYELESAIAN
### Setup nomor 1, 2, dan 3
1.	Susun topologi dan koneksi seperti pada gambar di bawah.
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/87381d4c-5abb-48f8-b2be-799ae314beaf)

3.	Set iptable di router (**iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.195.0.0/16**), dan taruh di bash script agar tidak hilang
4.	Echo nameserver di node terluar, untuk connect dengan router agar terconnect dengan internet, dan taro di bashscript agar tidak perlu diulang (**echo nameserver 192.168.122.1 > /etc/resolv.conf**)
5.	Instalasi bind di dns master
6.	Buat domain di nano /etc/bind/named.conf.local
7.	Buat directory untuk menyimpan konfigurasi domain yaitu prak1
8.	Copy db.local pada path /etc/bind ke dalam folder prak1 yang baru saja dibuat dan ubah namanya menjadi arjuna.d08.com dan abimanyu.d08.com
9.	Kemudian konfigurasi arjuna.d08.com dan abimanyu.d08.com
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/d3e426a5-0437-441d-93d9-7a656f61d8f3)
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/2689aab4-6abe-4bc1-a339-50fdc7b2655b)

10.	Restart bind9 dengan perintah service bind9 restart
11.	Arahkan client nakula dan sadewa dengan nano /etc/resolv.conf dan tulis nameserver ip dns master kita, untuk mencoba ping arjuna dan abimanyu

### Lakukan DNS forwarding, agar bisa akses internet
11.	edit file /etc/bind/named.conf.options pada server dns master
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/7ed74e10-421f-4d05-b126-6f9fec1ca860)
12.	service bind9 restart dan coba ping google.com

### Menyiapkan backup pada .bashrc
13.	Tulis code berikut pada /root/.bashrc (lakukan pada dns server)
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/ded58241-08c3-4c99-9c31-e443e2a02e05)

14.	Lalu kita mkdir /root/prak1/bind lalu lakukan cp -r -f /etc/bind /root/prak1 setiap memodifikasi file bind untuk menyimpan /etc/bind, sehingga konfigurasi bind bisa dipanggil saat kita memulai project di bash script

### Membuat subdomain (nomor 4)
15.	 Membuat parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
16.	Atur di nano /etc/bind/prak1/abimanyu pada dns master (yudhistira) masukan parsekit in a - lalu ip node abimanyu
17.	Save, bind restart, dan coba ping

### Membuat reverse domain abimanyu (nomer 5)
18.	Edit file /etc/bind/named.conf.local pada dns master
19.	Tambah domain reverse dari ip abimanyu 
20.	Copykan file db.local dari path /etc/bind ke dalam folder prak1 yang baru saja dibuat dan ubah namanya menjadi 2.168.192.in-addr.arpa
21.	Edit file 2.168.192.in-addr.arpa menjadi seperti gambar di bawah ini </br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/3b366553-e330-43a9-ad45-2f264ff54d53)

22.	service bind9 restart
23.	Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/f772b833-06f3-4a6f-a76f-6c24f2295ee7)

24.	Masukan apt-get update dan install dns utils ke /root/.bashscr client, agar otomatis dimulai saat memulai project.

### Membuat DNS slave untuk (nomor 6)
25.	Pertama kita konfigurasi bind dns master, Edit file /etc/bind/named.conf.local dan sesuaikan dengan syntax berikut </br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/ce8a30d7-333d-413a-92b6-80645349b121) </br>
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
32. pertama modifikasikonfigurasi abimanyu.d08.com pada /etc/bind/prak1 di dnsmaster untuk prefix baratayuda diarahkan ke dns slave menjadi </br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/dc2c0983-94e4-437f-ae35-5747a0a00a37) </br>

33. jangan lupa untuk modifikasi named.conf.options comment dnssec-validation auto; dan tambahkan allow-query{any;}; // pada dns master dan slave
34. juga pastikan named.nonf.local domain abimanyu di beri allow transfer ke dns slave lalu restart bind9.
35. Lalu modifikasi domain baratayuda.abimanyu.d08.com pada dns slave menjadi seperti berikut
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/aa2feb3c-5420-4cc9-bf0a-908d16f83ac6)

36. Lalu buat direktori mkdir /etc/bind/delegasi dan buat file untuk mengkonfigurasi bind baratayuda dengan cp /etc/bind/db.local ke direktori didalam delegasi.
37. kemudian edit file baratayuda... menjadi seperti berikut 
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/fe918a81-3922-4169-b815-9cfa99ee5dc7)
38. Lakukan restart bind9 llalu testing dengan ping ke domain domain yang baru dibuat tersebut

### ⭕ Nomor 9 ➡️ Nomor 10
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
- Prabakusuma:8001
- Abimanyu:8002
- Wisanggeni:8003

### 🟢 Jawaban Nomor 9 ➡️ Nomor 10
### 9️⃣&🔟 Setting Worker
- Langkah pertama yang harus dilakukan adalah dengan melakukan instalasi `nginx` dan keperluan lainnya pada setiap worker dan load balancernya dengan perintah:
```
apt-get update && apt install nginx php php-fpm -y
apt-get install libapache2-mod-php7.0 wget unzip -y
```
- Kedua, pada setiap worker dan load balancer masukan perintah
```
service nginx start
```
- Ketiga, kita masuk pada direktori `/var/www`. Langkah ini dilakukan pada setiap worker saja.
```
cd /var/www
```
- Keempat, pada folder `/var/www` kita masukan file yang berisi asset pada file yang diberikan. Untuk mendownload assetnya kita akan menggunakan wget yaitu dengan command:
```
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=17tAM_XDKYWDvF-JJix1x7txvTBEax7vX' -O arjuna.d08.zip
```
- Kelima, file tersebut akan disimpan pada folder `/var/www`. Kemudian kita akan melakukan unzip dan menghapus file .zip dengan command: <br>
```
unzip arjuna.d08.zip && rm arjuna.d08.zip
```
__NOTE: JANGAN LUPA UNTUK MENGINSTALL UNZIP__
- Keenam, kita unzip file `arjuna.d08.zip`, seharusnya kita akan mendapatkan folder bernama `arjuna.yyy.com`. Kita akan merename folder tersebut menjadi `jarkom` dengan command:
```
mv arjuna.yyy.com jarkom
```

- di dalam folder `jarkom` yang telah kita buat akan berisi sebuah file `index.php` yang berisi:
```
<?php
$hostname = gethostname();
$date = date('Y-m-d H:i:s');
$php_version = phpversion();
$username = get_current_user();

echo "Hello World!<br>";
echo "Saya adalah: $username<br>";
echo "Saat ini berada di: $hostname<br>";
echo "Versi PHP yang saya gunakan: $php_version<br>";
echo "Tanggal saat ini: $date<br>";
?>
```
__⬆️File tersebut akan memberitahukan kita berada di server mana.__ <br>

- Ketujuh, kita akan masuk ke direktori `/etc/nginx/sites-available` dengan command:
```
cd /etc/nginx/sites-available
```

- Kedelapan, kita akan membuat file baru yang bernama `jarkom` dengan command:
```
nano jarkom
```

- Kesembilan, kita akan memasukan konfigurasi berikut ke dalam file `jarkom`:

👉🏻 Pada Prabukusuma:
```
 server {

 	listen 8001;

 	root /var/www/jarkom;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/jarkom_error.log;
 	access_log /var/log/nginx/jarkom_access.log;
 }
```
👉🏻 Pada Abimanyu:
```
 server {

 	listen 8002;

 	root /var/www/jarkom;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/jarkom_error.log;
 	access_log /var/log/nginx/jarkom_access.log;
 }
```
👉🏻 Pada Wisanggeni:
```
 server {

 	listen 8003;

 	root /var/www/jarkom;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/jarkom_error.log;
 	access_log /var/log/nginx/jarkom_access.log;
 }
```

 - Kesepuluh, kita akan menyimpan file tersebut dan akan membuat `symlink` dengan command:
 ```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
 ```

 - Kesebelas, kita melakukan reload pada service `nginx` dengan command:
 ```
 service nginx restart
 ```
#### Penting!!!
```
📝📝 Perlu dicatat bahwa langkah ketiga hingga kesebelas dilakukan pada tiap worker yaitu Abimanyu, Prabukusuma, dan Wisanggeni. 📝📝
```

### 9️⃣&🔟 Setting Load Balancer
- Untuk melakukan setting pada load balancer, kita buka terminal pada load balancer kita yaitu `arjuna`.
- Pertama, kita akan masuk ke direktori `/etc/nginx/sites-available` dengan command:
```
cd /etc/nginx/sites-available
```

- Kedua, kita akan membuat file baru yang bernama `lb-arjuna` dengan command:
```
nano lb-arjuna
```

- Ketiga, kita akan memasukan konfigurasi berikut ke dalam file `lb-arjuna`:

👉🏻 Pada lb-arjuna:
```
 # Default menggunakan Round Robin
 upstream myweb {
 	server 192.195.2.4:8001; #Prabukusuma
	server 192.195.2.5:8002; #Abimanyu
	server 192.195.2.3:8003; #Wisanggeni
 }
 server {
 	listen 80;
 	server_name arjuna.d08.com;

 	location / {
 	proxy_pass http://myweb;
 	}
 }
```
- Keempat, simpan file tersebut dan buat `symlink` dengan command:
```
ln -s /etc/nginx/sites-available/lb-arjuna /etc/nginx/sites-enabled
```
 - Kelima, kita melakukan reload pada service `nginx` dengan command:
 ```
 service nginx restart
 ```

### 9️⃣&🔟 Testing Nomor 9 & 10
- Buka console salah satu client.
- Pastikan setiap client telah terinstall `lynx`. Apabila belum terinstall, gunakan command:
```
apt-get install lynx -y
```
- Kemudian masukan command:
```
lynx arjuna.d08.com
```
- Setelah menjalankan command tersebut, seharusnya kita akan mendapat tampilan web seperti di bawah ini.

![image9a](./assets/images/NO9A.png)
![image9b](./assets/images/NO9B.png)
![image9c](./assets/images/NO9C.png)

__⬆️Setiap kali kita melakukan lynx ke domain arjuna, maka load balancer akan menunjuk salah satu worker untuk menampilkan webnya.__ <br>

### ⭕ Nomor 11
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### 🟢 Jawaban Nomor 11
### 1️⃣1️⃣ Setting Apache2 Pada Abimanyu

- Pada server `Abimanyu` lakukan instalasi berikut untuk memenuhi persyaratan.
```
apt-get install apache2 wget unzip -y
apt-get install libapache2-mod-php7.0 -y
```

- Setelah melakukan instalasi, kita masuk ke direktori `/etc/apache2/sites-available` dengan command:
```
cd /etc/apache2/sites-available
```

- Setelah itu kita akan mencopy file `000-default.conf` dengan command:
```
cp 000-default.conf abimanyu.d08.com.conf
```

- Di dalam file `abimanyu.d08.com.conf` kita tambahkan beberapa konfigurasi sehingga konfigurasinya menjadi:
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.d08
        ServerName abimanyu.d08.com
        ServerAlias www.abimanyu.d08.com
		
		ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Kita hanya menambahkan `ServerName` dan `ServerAlias` serta mengubah DocumentRoot-nya menjadi `/var/www/abimanyu.d08`
```
 ServerName abimanyu.d08.com
 ServerAlias www.abimanyu.d08.com
```

- Setelah itu kita akan menghapus file `000-default.conf` yang telah dienable dengan command:
```
rm /etc/apache2/sites-enabled/000-default.conf
```

- Lalu kita akan mengenable konfigurasi file `abimanyu.d08.com.conf` dengan command:
```
a2ensite abimanyu.d08.com.conf
```

- Kemudian kita akan pergi ke direktori `var/www` dengan command:
```
cd /var/www
```

- Pada direktori tersebut, kita akan mendownload file yang berisi konten `abimanyu.d08.com` dengan wget, berikut adalah commandnya:
```
wget --no-check-certificate "https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc" -O abimanyu.d08.com.zip
```

- Setelah itu unzip file yang telah kita download dengan command:
```
unzip abimanyu.d08.com.zip
```

- Kemudian kita akan merename folder yang telah di-unzip menjadi `abimanyu.d08` dengan command:
```
mv abimanyu.yyy.com abimanyu.d08
```

- Dan hapus file hasil download zipnya dengan command:
```
rm -r abimanyu.d08.com.zip
```

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣1️⃣ Testing Nomor 11
- Buka client dan lakukan `lynx` pada `abimanyu.d08.com`:
```
lynx abimanyu.d08.com
```
atau
```
lynx www.abimanyu.d08.com
```
- Hasilnya akan menampilkan page di bawah:

![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/1056702f-b2cb-4509-874d-3a65eafa2c82)

### ⭕ Nomor 12
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### 🟢 Jawaban Nomor 12
### 1️⃣2️⃣ Setting Apache2 Pada Abimanyu

- Tambahkan konfigurasi pada file `/etc/apache2/sites-available/abimanyu.d08.com.conf` menjadi seperti berikut:
```
Alias "/home" "/var/www/abimanyu.d08/index.php/home"
```
Atau seperti gambar di bawah: <br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/e92c8e15-d222-45f5-9f1e-f35fc31509b9)

### 1️⃣2️⃣ Testing Nomor 12
- Hasil `lynx www.abimanyu.d08.com/home` pada client:

![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/7b551372-6f01-48a6-b3f1-e7b3a53587f4)


### ⭕ Nomor 13
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### 🟢 Jawaban Nomor 13
### 1️⃣3️⃣ Setting Apache2 Pada Abimanyu
- Langkahnya sangat mirip dengan nomor 11.
- Pertama, kita masuk ke direktori `/etc/apache2/sites-available` dengan command:
```
cd /etc/apache2/sites-available
```

- Setelah itu kita akan mencopy file `abimanyu.d08.com.conf` dengan command:
```
cp abimanyu.d08.com.conf parikesit.abimanyu.d08.com.conf
```

- Di dalam file `parikesit.abimanyu.d08.com.conf` kita tambahkan beberapa konfigurasi sehingga konfigurasinya menjadi:
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.d08
        ServerName parikesit.abimanyu.d08.com
        ServerAlias www.parikesit.abimanyu.d08.com
		
		ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Kita hanya menambahkan `ServerName` dan `ServerAlias` serta mengubah DocumentRoot-nya menjadi `/var/www/parikesit.abimanyu.d08`

- Lalu kita akan mengenable konfigurasi file `parikesit.abimanyu.d08.com.conf` dengan command:
```
a2ensite parikesit.abimanyu.d08.com.conf
```

- Kemudian kita akan pergi ke direktori `var/www` dengan command:
```
cd /var/www
```

- Pada direktori tersebut, kita akan mendownload file yang berisi konten `parikesit.abimanyu.d08.com` dengan wget, berikut adalah commandnya:
```
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS' -O parikesit.abimanyu.d08.com.zip
```

- Setelah itu unzip file yang telah kita download dengan command:
```
unzip parikesit.abimanyu.d08.com.zip
```

- Kemudian kita akan merename folder yang telah di-unzip menjadi `parikesit.abimanyu.d08` dengan command:
```
mv parikesit.abimanyu.yyy.com parikesit.abimanyu.d08
```

- Dan hapus file hasil download zipnya dengan command:
```
rm -r parikesit.abimanyu.d08.com.zip
```

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣3️⃣ Testing Nomor 13
- Hasil `lynx www.parikesit.abimanyu.d08.com` pada client:

![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/21dc6265-e64e-42f3-8a14-554ae3df824b)

Akan muncul tampilan diatas, dimana kita bisa melihat directory listing sesuai isinya.

### ⭕ Nomor 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### 🟢 Jawaban Nomor 14
### 1️⃣4️⃣ Setting Apache2 Pada Abimanyu

- Untuk nomor 14, tambahkan konfigurasi pada file `/etc/apache2/sites-available/parikesit.abimanyu.d08.com.conf` seperti berikut:
```
<Directory /var/www/parikesit.abimanyu.d08/public>
	Options +Indexes
</Directory>

<Directory /var/www/parikesit.abimanyu.d08/secret>
    Options -Indexes
</Directory>
```
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/15b9d06d-b367-44d2-8560-9eb16f028040)

- Code tersebut memberi perintah agar folder secret tidak dapat diakses (403 forbidden).

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣4️⃣ Testing Nomor 14

- `lynx www.parikesit.abimanyu.d08.com/public`

![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/3a73a105-4652-4b5f-a2c9-270aeb3fc948)

- `lynx www.parikesit.abimanyu.d08.com/secret` 

![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/b02b2055-0e89-4a22-8b49-e28d814deda2)

### ⭕ Nomor 15
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### 🟢 Jawaban Nomor 15
### 1️⃣5️⃣ Setting .htaccess pada Abimanyu

- Pertama, kita jalankan perintah berikut untuk mengaktifkan module rewrite:
```
a2enmod rewrite
```

- Kemudian kita perlu membuat sebuah file `.htaccess` pada direktori `/var/www/parikesit.abimanyu.d08` dengan command:
```
nano /var/www/parikesit.abimanyu.d08/.htaccess
```

- Lalu kita isikan file `.htaccess`nya dengan kode berikut:
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^([^\.]+)$ $1.php [NC,L]
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
```
![Image15](./assets/images/NO15C.png)

- Pada file `/etc/apache2/sites-available/parikesit.abimanyu.d08.com.conf` tambahkan konfigurasi berikut:
```
<Directory /var/www/parikesit.abimanyu.d08>
    Options +FollowSymLinks -Multiviews
    AllowOverride All
</Directory>
```
![Image15](./assets/images/NO15B.png)

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣5️⃣ Testing Nomor 15

- `lynx www.parikesit.abimanyu.d08.com/secret`

__Folder /secret sudah diatur agar tidak dapat menampilkan direktori listing (403 Forbidden)__

![Image15](./assets/images/NO15D.png)

- `lynx www.parikesit.abimanyu.d08.com/xixixixi`

__Folder /xixixixi tidak ada, maka errornya adalah 404 Not Found__

![Image15](./assets/images/NO15E.png)

### ⭕ Nomor 16
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi www.parikesit.abimanyu.yyy.com/js 

### 🟢 Jawaban Nomor 16
### 1️⃣6️⃣ Setting Apache2 Pada Abimanyu

- Pada file `/etc/apache2/sites-available/parikesit.abimanyu.d08.com.conf` tambahkan konfigurasi berikut:
```
Alias "/js" "/var/www/parikesit.abimanyu.d08/public/js"
```
![Image16](./assets/images/NO16A.png)

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣6️⃣ Testing Nomor 16

- `lynx www.parikesit.abimanyu.d08.com/js`

![Image16](./assets/images/NO16B.png)

### ⭕ Nomor 17 ➡️ Nomor 18
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

### 🟢 Jawaban Nomor 17 ➡️ Nomor 18
### 1️⃣7️⃣&1️⃣8️⃣Setting Apache2, .htaccess, .htpasswd Pada Abimanyu

- Untuk nomor 17, kita perlu melakukan deployment yang sama seperti pada nomor 11, dan 13. Disini, kita asumsikan kita sudah melakukan setting pada folder `/etc/apache2/sites-available` dan melakukan `a2ensite rjp.baratayuda.abimanyu.d08.com.conf` serta melakukan download asset pada folder `/var/www` yang kemudian foldernya diberi nama `rjp.baratayuda.abimanyu.d08`

- Berikut adalah isi file `rjp.baratayuda.abimanyu.d08.com.conf` pada folder `/etc/apache2/sites-available`:
```
<VirtualHost *:14000 *:14400>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.d08
        ServerName rjp.baratayuda.abimanyu.d08.com
        ServerAlias www.rjp.baratayuda.abimanyu.d08.com

		<Directory /var/www/rjp.baratayuda.abimanyu.d08>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
                AuthType Basic
                AuthName "Restricted Content"
                AuthUserFile /var/www/rjp.baratayuda.abimanyu.d08/.htpasswd
                Require valid-user
        </Directory>

		ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
__Berikut isi file rjp.baratayuda.abimanyu.d08.com.conf:__

![Image17](./assets/images/NO17A.png)
![Image17](./assets/images/NO17B.png)
![Image17](./assets/images/NO17C.png)

- Lalu, kita akan masuk ke direktori `/var/www/rjp.baratayuda.abimanyu.d08` yang di sana kita akan membuat file `.htaccess` dan `.htpasswd`. Berikut adalah isinya:

- `.htaccess`
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^([^\.]+)$ $1.php [NC,L]
```

- Dan untuk membuat file `.htpasswd`, dapat melalui langkah berikut:
	- gunakan command `htpasswd -c /var/www/rjp.baratayuda.abimanyu.d08 Wayang`
	- Setelah itu kita akan diminta untuk memasukan passwordnya. Ketikan passwordnya dan isi `baratayudad08`.
- `.htpasswd`
```
Wayang:$apr1$aejmbDtH$QByjsGPIg4FzPWKmJS4VE0
```

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣7️⃣&1️⃣8️⃣ Testing Nomor 17 ➡️ Nomor 18

- `lynx www.rjp.baratayuda.abimanyu.d08.com:14000` tanpa menggunakan auth Wayang.

__NOTE: HANYA ALERT__

![Image18](./assets/images/NO18A.png)

__DAPAT DILIHAT BAHWA MUNCUL "RESTRICTED CONTENT"__
![Image18](./assets/images/NO18B.png)

- `lynx -auth=Wayang:baratayudad08 www.rjp.baratayuda.abimanyu.d08.com:14000` menggunakan auth Wayang.

![Image18](./assets/images/NO18C.png)

### ⭕ Nomor 19
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

### 🟢 Jawaban Nomor 19
### 1️⃣9️⃣ Setting Apache2 Pada Abimanyu

- Pada file `/etc/apache2/sites-available/abimanyu.d08.com.conf` tambahkan konfigurasi berikut:
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.d08
    ServerName 192.195.2.5
    ServerAlias 192.195.2.5
    Redirect permanent / http://www.abimanyu.d08.com/
</VirtualHost>
```
![Image19](./assets/images/NO19A.png)

- Setelah itu kita restart apache2-nya dengan command:
```
service apache2 restart
```

### 1️⃣9️⃣ Testing Nomor 19

- `lynx 192.195.2.5`

![Image19](./assets/images/NO19C.png) <br>
⬆️Pada gambar di atas sudah menggunakan `www.abimanyu.d08.com`

![Image19](./assets/images/NO19D.png)
