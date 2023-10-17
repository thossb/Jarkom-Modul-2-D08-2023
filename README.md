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

### Nomor 9 ➡️ Nomor 10
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
- Prabakusuma:8001
- Abimanyu:8002
- Wisanggeni:8003

### Jawaban  Nomor 9 ➡️ Nomor 10
### 9️⃣🔟 Setting Worker
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

### 9️⃣🔟 Setting Load Balancer
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

### 9️⃣🔟 Testing
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

### Nomor 11
konfigurasi apache server untuk abimanyu
```
answer :
apt-get install apache2 wget unzip -y
apt-get install libapache2-mod-php7.0 -y

cd /etc/apache2/sites-available
cp 000-default.conf abimanyu.d08.com.conf

----
 tambahin
 ServerName abimanyu.d08.com
 ServerAlias www.abimanyu.d08.com
----

rm -r 000-default.conf
a2ensite abimanyu.d08.com.conf

cd /var/www/
wget --no-check-certificate "https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc" -O abimanyu.d08.com.zip
unzip abimanyu.d08.com.zip
mv abimanyu.yyy.com abimanyu.d08
rm -r abimanyu.d08.com.zip
service apache2 restart

```
Menginstal Apache dan Modul PHP:

apt-get install apache2 wget unzip -y: Ini adalah perintah untuk menginstal Apache2, wget, dan unzip dengan opsi "-y" untuk mengonfirmasi pemasangan tanpa interaksi pengguna.
apt-get install libapache2-mod-php7.0 -y: Ini menginstal modul PHP versi 7.0 yang diperlukan untuk menjalankan skrip PHP di server.
Membuat Konfigurasi Situs Baru:
cd /etc/apache2/sites-available: Anda berpindah ke direktori konfigurasi situs Apache.
cp 000-default.conf abimanyu.d08.com.conf: Anda membuat salinan file konfigurasi default dengan nama domain baru "abimanyu.d08.com.conf".
Mengedit Konfigurasi Situs:
Anda menambahkan informasi ServerName dan ServerAlias ke dalam file konfigurasi situs baru "abimanyu.d08.com.conf" untuk menentukan nama domain dan alias yang akan digunakan oleh situs.
Menghapus Konfigurasi Situs Default:
rm -r 000-default.conf: Anda menghapus konfigurasi situs default yang tidak diperlukan lagi.
Mengaktifkan Konfigurasi Situs Baru:
a2ensite abimanyu.d08.com.conf: Anda mengaktifkan konfigurasi situs baru dengan perintah ini.
Mengunduh dan Menginstal Konten Situs Web:
cd /var/www/: Anda berpindah ke direktori root web server.
wget --no-check-certificate "https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc" -O abimanyu.d08.com.zip: Anda mengunduh file ZIP yang berisi konten situs web.
unzip abimanyu.d08.com.zip: Anda mengekstrak isi ZIP.
mv abimanyu.yyy.com abimanyu.d08: Anda mengubah nama direktori yang telah diekstrak menjadi "abimanyu.d08" (sepertinya ini adalah kesalahan ketik, seharusnya "abimanyu.d08.com" sesuai dengan nama domain).
Menghapus File ZIP:
rm -r abimanyu.d08.com.zip: Anda menghapus file ZIP yang telah diekstrak sebelumnya.
Merestart Apache:
service apache2 restart: Anda merestart layanan Apache untuk menerapkan konfigurasi baru. hasilnya jika di lynx akan menghasilkan
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/1056702f-b2cb-4509-874d-3a65eafa2c82)

### Nomor 12
konfigurasi /etc/apache2 menjadi seperti berikut</br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/e92c8e15-d222-45f5-9f1e-f35fc31509b9)
</br>
hasilnya lynx www.abimanyu.d08.com/home di client</br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/7b551372-6f01-48a6-b3f1-e7b3a53587f4)
</br>

### Nomor 13
lakukan hal yang sama dengan nomer 11, tetapi sekarang untuk parikesit abimanyu.. isinya di dapat dari wget ke drive yang sudah tertera.
hasilnya jika kita lynx www.parikesit.abimanyu.d08.com di client akan menghasilkan</br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/21dc6265-e64e-42f3-8a14-554ae3df824b)
</br>
akan muncul tampilan diatas, dimana kita bisa melihat directory listing sesuai isinya.

### Nomor 14
Untuk nomor 14, konfigurasi parikesit.abimanyu seperti berikut</br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/15b9d06d-b367-44d2-8560-9eb16f028040)
</br>
code tersebut memberi perintah agar folder secret tidak dapat diakses (403 forbidden).
hasilnya jika kita coba buka adalah sebagai berikut</br>
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/3a73a105-4652-4b5f-a2c9-270aeb3fc948)
![image](https://github.com/thossb/Jarkom-Modul-2-D08-2023/assets/90438426/b02b2055-0e89-4a22-8b49-e28d814deda2)



### Nomor 15
### Nomor 16
### Nomor 17
### Nomor 18
### Nomor 19
### Nomor 20










