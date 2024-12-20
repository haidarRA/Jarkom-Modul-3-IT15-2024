# Jarkom-Modul-3-IT15-2024

## ***KELOMPOK IT15***
| Nama      | NRP         |
|-----------|-------------|
| M. Abhinaya Al Faruqi | 5027231011  |  
| Haidar Rafi Aqyla | 5027231029   |

## TOPOLOGI
![image](https://github.com/user-attachments/assets/a086aec0-4163-4c8f-ba12-7a9d6dbca5c3)

## IP Address Tiap Node
| Node           | Jenis Node              | IP Address         |
|----------------|-------------------------|--------------------|
| Paradis        | Router (DHCP Relay)     | 192.168.122.1/24   |
| Annie          | Laravel Worker          | 10.71.1.2          |
| Bertholdt      | Laravel Worker          | 10.71.1.3          |
| Reiner         | Laravel Worker          | 10.71.1.4          |
| Armin          | PHP Worker              | 10.71.2.2          |
| Eren           | PHP Worker              | 10.71.2.3          |
| Mikasa         | PHP Worker              | 10.71.2.4          |
| Zeke           | Client                  | DHCP               |
| Erwin          | Client                  | DHCP               |
| Beast          | Load Balancer (Laravel) | 10.71.3.2          |
| Colossal       | Load Balancer (PHP)     | 10.71.3.3          |
| Warhammer      | Database Server         | 10.71.3.4          |
| Fritz          | DNS Server              | 10.71.4.2          |
| Tybur          | DHCP Server             | 10.71.4.3          |

Semua node menggunakan netmask 255.255.255.0.

# No. 0
Soal:
> Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP (0) mengarah pada Armin.

Jalankan script berikut pada DNS Server Fritz.
```
apt-get update
apt-get install bind9 -y

echo 'zone "marley.it15.com" {
	type master;
	file "/etc/bind/jarkom/marley.it15.com";
};
zone "eldia.it15.com" {
	type master;
	file "/etc/bind/jarkom/eldia.it15.com";
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/marley.it15.com
cp /etc/bind/db.local /etc/bind/jarkom/eldia.it15.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it15.com. root.marley.it15.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it15.com.
@       IN      A       10.71.1.2
www     IN      CNAME   marley.it15.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom/marley.it15.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it15.com. root.eldia.it15.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it15.com.
@       IN      A       10.71.2.2
www     IN      CNAME   eldia.it15.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom/eldia.it15.com

service bind9 restart
```
Setelah menjalankan script di atas, kedua domain dapat dites dengan ping dari node lain.
![image](https://github.com/user-attachments/assets/a6b29c51-15b8-41bb-94a0-d47401c4061e)

# No. 1
Soal:
> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan. Semua Client harus menggunakan konfigurasi ip address dari keluarga Tybur (dhcp).

Network configuration tiap node:
# Router / DHCP Relay
## 1. Paradis
```
auto eth0
iface eth0 inet dhcp

auto eth1 #Marley
iface eth1 inet static
    address 10.71.1.1
    netmask 255.255.255.0

auto eth2 #Eldia
iface eth2 inet static
    address 10.71.2.1
    netmask 255.255.255.0

auto eth3 #Kyojin
iface eth3 inet static
    address 10.71.3.1
    netmask 255.255.255.0

auto eth4 #Noble
iface eth4 inet static
    address 10.71.4.1
    netmask 255.255.255.0
```

# Laravel Worker
## 1. Annie
```
auto eth0
iface eth0 inet static
    address 10.71.1.2
    netmask 255.255.255.0
    gateway 10.71.1.1
```

## 2. Bertholdt
```
auto eth0
iface eth0 inet static
    address 10.71.1.3
    netmask 255.255.255.0
    gateway 10.71.1.1
```

## 3. Reiner
```
auto eth0
iface eth0 inet static
    address 10.71.1.4
    netmask 255.255.255.0
    gateway 10.71.1.1
```

# PHP Worker
## 1. Armin
```
auto eth0
iface eth0 inet static
    address 10.71.2.2
    netmask 255.255.255.0
    gateway 10.71.2.1
```

## 2. Eren
```
auto eth0
iface eth0 inet static
    address 10.71.2.3
    netmask 255.255.255.0
    gateway 10.71.2.1
```

## 3. Mikasa
```
auto eth0
iface eth0 inet static
    address 10.71.2.4
    netmask 255.255.255.0
    gateway 10.71.2.1
```

# Client
## 1. Zeke
```
auto eth0
iface eth0 inet dhcp
```

## 2. Erwin
```
auto eth0
iface eth0 inet dhcp
```

# Load Balancer (Laravel)
## 1. Beast
```
auto eth0
iface eth0 inet static
    address 10.71.3.2
    netmask 255.255.255.0
    gateway 10.71.3.1
```

# Load Balancer (PHP)
## 1. Colossal
```
auto eth0
iface eth0 inet static
    address 10.71.3.3
    netmask 255.255.255.0
    gateway 10.71.3.1
```

# Database Server
## 1. Warhammer
```
auto eth0
iface eth0 inet static
    address 10.71.3.4
    netmask 255.255.255.0
    gateway 10.71.3.1
```

# DNS Server
## 1. Fritz
```
auto eth0
iface eth0 inet static
    address 10.71.4.2
    netmask 255.255.255.0
    gateway 10.71.4.1
```

# DHCP Server
## 1. Tybur
```
auto eth0
iface eth0 inet static
    address 10.71.4.3
    netmask 255.255.255.0
    gateway 10.71.4.1
```

# No. 2 - 5
Soal:
> Client yang melalui bangsa marley mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100 (2)

>Client yang melalui bangsa eldia mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243 (3)

>Client mendapatkan DNS dari keluarga Fritz dan dapat terhubung dengan internet melalui DNS tersebut (4)

>Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)


Jalankan script berikut pada DHCP Relay Paradis.
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

echo 'SERVERS="10.71.4.3"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay restart
```
 
 
Kemudian, jalankan script berikut pada DHCP Server Tybur.
```
apt-get update
apt-get install isc-dhcp-server

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo 'subnet 10.71.1.0 netmask 255.255.255.0 {
    range 10.71.1.5 10.71.1.25;
    range 10.71.1.50 10.71.1.100;
    option routers 10.71.1.1;
    option broadcast-address 10.71.1.255;
    option domain-name-servers 10.71.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 10.71.2.0 netmask 255.255.255.0 {
    range 10.71.2.9 10.71.2.27;
    range 10.71.2.81 10.71.2.243;
    option routers 10.71.2.1;
    option broadcast-address 10.71.2.255;
    option domain-name-servers 10.71.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 10.71.4.0 netmask 255.255.255.0 {
}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
 
 
Kemudian, jalankan script berikut pada DNS Server Fritz.
```
echo 'options {
        directory "/var/cache/bind";

        forwarders {
            192.168.122.1;
        };

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

Setelah melakukan konfigurasi pada DHCP Relay Paradis, DHCP Server Tybur, dan DNS Server Fritz, langsung dapat dites konfigurasinya apakah bisa berjalan atau tidak dengan mengecek IP dari masing - masing client dan mencoba untuk mengakses internet dari kedua client.
![image](https://github.com/user-attachments/assets/24689773-576f-41fa-bdb3-5678a1e6b007)
![image](https://github.com/user-attachments/assets/b44cfaad-8e63-4a96-9657-85523c9ff902)

Dari kedua gambar di atas, dapat dilihat bahwa client Zeke mendapatkan IP address 10.71.1.5 dan client Erwin mendapatkan IP address 10.71.2.9.

![image](https://github.com/user-attachments/assets/162d11ea-a960-44f6-86f6-1c5741d305c8)
![image](https://github.com/user-attachments/assets/141fbc84-12f6-40d6-974e-814d1861e140)

Dari kedua gambar di atas, dapat dilihat bahwa kedua client dapat mengakses internet melalui DNS Server Tybur.

# No. 6
Soal:
> Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 (6)

Jalankan script berikut pada setiap worker PHP (Armin, Eren, dan Mikasa).
```
apt-get update
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y
apt-get install nginx -y
apt-get install wget -y

service nginx start
service php7.3-fpm start

mkdir -p /var/www/eldia.it15.com
mkdir /var/www/eldia.it15.com/css
mkdir /var/www/eldia.it15.com/js

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Z36ci3i-k-sHjyFUnTzx98pVcdkO-SpS' -O /var/www/eldia.it15.com/index.php
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1G4yZZzFKjLKeFt-dgVVKvmLUeVKylYpb' -O /var/www/eldia.it15.com/info.php
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=14qthc0e_7O4LVMq824Ww35bom3NIQ5FZ' -O /var/www/eldia.it15.com/css/styles.css
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=139dJixAsQaAiNfQOaGGQjqTZrFSZnSGl' -O /var/www/eldia.it15.com/js/script.js

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/eldia.it15.com
ln -s /etc/nginx/sites-available/eldia.it15.com /etc/nginx/sites-enabled/eldia.it15.com
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/eldia.it15.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/eldia.it15.com

service nginx restart
service php7.3-fpm restart
```

Setelah menjalankan script di atas pada masing - masing worker, dapat dites menggunakan Lynx dari salah satu client dengan menggunakan command ```lynx [IP Worker PHP]```.
![image](https://github.com/user-attachments/assets/8a269076-bc66-408c-b8a7-80b301380a08)
![image](https://github.com/user-attachments/assets/3bc4ea75-ef0b-419f-b726-f40ac23ebbe1)
![image](https://github.com/user-attachments/assets/b1c5705f-d6f7-4019-a81b-34b508ecccec)

# No. 7
Soal:
> Dikarenakan Armin sudah mendapatkan kekuatan titan colossal, maka bantulah kaum eldia menggunakan colossal agar dapat bekerja sama dengan baik. Kemudian lakukan testing dengan 6000 request dan 200 request/second. (7)

Jalankan script berikut di DNS Server Fritz untuk mengubah IP dari domain eldia.it15.com.
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it15.com. root.eldia.it15.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it15.com.
@       IN      A       10.71.3.3
www     IN      CNAME   eldia.it15.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom/eldia.it15.com

service bind9 restart
```

Kemudian, jalankan script berikut di load balancer PHP Colossal untuk setup load balancer untuk setiap PHP worker.
```
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y

service nginx start

mkdir -p /var/www/eldia.it15.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/eldia.it15.com

echo 'upstream node {
    server 10.71.2.2;
    server 10.71.2.3;
    server 10.71.2.4;
}

server {
    listen 80;
    server_name _;

    root /var/www/eldia.it15.com;
    index index.php index.html index.htm;

    location / {
        proxy_pass http://node;
    }
}' > /etc/nginx/sites-available/eldia.it15.com

ln -s /etc/nginx/sites-available/eldia.it15.com /etc/nginx/sites-enabled/eldia.it15.com
rm /etc/nginx/sites-enabled/default

service nginx restart
```

Setelah menjalankan kedua script di atas, dapat langsung dites dengan mengakses IP load balancer Colossal (10.71.3.3) maupun domain eldia.it15.com di salah satu client dengan menggunakan Lynx.
![image](https://github.com/user-attachments/assets/30f348fb-1172-4344-8709-83721115e6db)
![image](https://github.com/user-attachments/assets/a818212a-e43b-41be-a571-94f840e8bde4)
![image](https://github.com/user-attachments/assets/de6547b0-5f94-43ff-8b77-591742a9aa1a)

Karena load balancer Colossal sudah berjalan dengan baik, maka untuk selanjutnya adalah melakukan load testing pada load balancer Colossal.
Sebelum melakukan load testing, install apache2-utils terlebih dahulu.
```
apt install apache2-utils
```

Setelah itu, bisa dilakukan load testing dengan command ```ab -n 6000 -c 200 http://10.71.3.3/```
![image](https://github.com/user-attachments/assets/d2e667d6-87bf-4884-8f78-b4f0f162b9a7)

# No. 8
Soal:
> Karena Erwin meminta “laporan kerja Armin”, maka dari itu buatlah analisis hasil testing dengan 1000 request dan 75 request/second untuk masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
>
> a. Nama Algoritma Load Balancer
>
> b. Report hasil testing pada Apache Benchmark
>
> c. Grafik request per second untuk masing masing algoritma.
>
> d. Analisis (8)

Untuk mengetes Load Balancer Colossal, dapat menggunakan algoritma load balancing mulai dari Round Robin, Least Connection, IP Hash, sampai Generic Hash.
![image](https://github.com/user-attachments/assets/a38702bf-4e48-4f3e-947d-7f0d64f4b876)

Untuk load testingnya sendiri dapat menggunakan command ```ab -n 1000 -c 75 http://10.71.3.3/``` dari salah satu client.

## 1. Round Robin
![image](https://github.com/user-attachments/assets/8bc2e9b0-5a2e-4367-8d19-c302089941c2)

## 2. Least Connection
![image](https://github.com/user-attachments/assets/1a6b300d-f3de-451e-9e95-ef6631b35a8c)

## 3. IP Hash
![image](https://github.com/user-attachments/assets/5bbd3a0e-e2ff-4ea4-9ef4-5d2d67f4a35b)

## 4. Generic Hash
![image](https://github.com/user-attachments/assets/317ff309-e7df-4389-bc24-1fa1121a38d4)

## Grafik
![image](https://github.com/user-attachments/assets/f397e8c2-2152-4212-bdda-f6787e545192)

## Analisis
Dari hasil yang didapat, dapat disimpulkan bahwa algoritma load balancing IP Hash memiliki performa yang paling baik, diikuti oleh Generic Hash, Least Connection, dan Round Robin. Hal ini juga dilihat dari dari time per request dari tiap algoritma load balancing, di mana algoritma IP Hash memiliki time per request paling kecil yang artinya kecepatannya sangat tinggi.

# No. 9
Soal:
> Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada “laporan kerja Armin”. (9)

Sebelum load testing, jumlah worker pada load balancer Colossal diubah jumlahnya.
![image](https://github.com/user-attachments/assets/871c4dc1-c36a-40a1-8d54-0f6d0acf49ec)

Setelah mengubah jumlah worker pada load balancer, bisa dilakukan load testing dengan command ```ab -n 1000 -c 10 http://10.71.3.3/``` dari salah satu client.

## 3 Worker
![image](https://github.com/user-attachments/assets/95f70c2e-5806-4920-bb1d-72b852575b27)

## 2 Worker
![image](https://github.com/user-attachments/assets/3217d61e-d603-47f0-9d73-0a31e1750bed)

## 1 Worker
![image](https://github.com/user-attachments/assets/100e9202-071d-47f0-b50f-e2829820c8d4)

## Grafik
![image](https://github.com/user-attachments/assets/14a49713-96c6-47c5-997f-6548b86cc5ab)

Dari hasil yang didapat, dapat disimpulkan bahwa performa load balancer yang menggunakan 1 worker lebih baik dibandingkan dengan performa load balancer yang menggunakan 2 ataupun 3 worker. Hal ini karena time per request load balancer yang menggunakan 1 worker lebih cepat dibandingkan dengan 2 worker maupun 3 worker serta jumlah worker yang harus di-handle oleh load balancer lebih sedikit. Selain itu, dapat disimpulkan juga bahwa jumlah worker pada load balancer berbanding lurus dengan jumlah failed request. Semakin banyak worker pada load balancer, maka semakin tinggi jumlah failed request.

# No. 10
Soal:
> Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di Colossal dengan dengan kombinasi username: “arminannie” dan password: “jrkmyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/ (10)

Sebelum melakukan konfigurasi autentikasi di Load Balancer Colossal, ubah algoritma load balancing yang digunakan menjadi Round Robin dan ubah jumlah worker yang ada pada konfigurasi load balancer menjadi 3 terlebih dahulu.
![image](https://github.com/user-attachments/assets/d7a1a239-7b64-4ef9-b568-58a77d800e75)

Setelah itu, jalankan script ini di load balancer Colossal.
```
mkdir /etc/nginx/supersecret
htpasswd -cb /etc/nginx/supersecret/htpasswd arminannie jrkmit15

echo 'upstream node {
    server 10.71.2.2;
    server 10.71.2.3;
    server 10.71.2.4;
}

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://node;

        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/supersecret/htpasswd;
    }
}' > /etc/nginx/sites-available/eldia.it15.com

service nginx restart
```

Setelah setup autentikasi pada load balancer Colossal, bisa mencoba untuk akses IP address 10.71.3.3 maupun domain eldia.it15.com dari salah satu client untuk menguji apakah autentikasi dapat bekerja dengan baik atau belum.
![image](https://github.com/user-attachments/assets/5ed71818-e93d-4034-96af-828a47170414)
![image](https://github.com/user-attachments/assets/f818a674-aba8-4d39-9736-e3e7dd64c9b7)
![image](https://github.com/user-attachments/assets/30a112fa-5e17-4ca6-8245-398c7aec360f)
![image](https://github.com/user-attachments/assets/5fcd4ab8-5bda-4c12-8c57-13208c47b774)

# No. 11
Soal:
> Lalu buat untuk setiap request yang mengandung /titan akan di proxy passing menuju halaman https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki (11) 
hint: (proxy_pass) (11)

jalankan script ini di load balancer colossal
```
echo 'upstream node {
    server 10.71.2.2;
    server 10.71.2.3;
    server 10.71.2.4;
}

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://node;

        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/supersecret/htpasswd;
    }
    location /titan {
        rewrite ^/titan(.*)$ https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki/$1 permanent;
    }
}' > /etc/nginx/sites-available/eldia.it15.com

service nginx restart
```
![Screenshot 2024-10-27 045716](https://github.com/user-attachments/assets/0f2b18db-a2b2-4ba9-b6db-de63ba2e5213)

# No. 12
> Selanjutnya Colossal ini hanya boleh diakses oleh client dengan IP [Prefix IP].1.77, [Prefix IP].1.88, [Prefix IP].2.144, dan [Prefix IP].2.156. (12) 
hint: (fixed in dulu clientnya) (12)

jalankan script ini untuk membuat client Zeke menggunakan ip address tetap
```
echo 'subnet 10.71.1.0 netmask 255.255.255.0 {
    range 10.71.1.5 10.71.1.25;
    range 10.71.1.50 10.71.1.100;
    option routers 10.71.1.1;
    option broadcast-address 10.71.1.255;
    option domain-name-servers 10.71.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 10.71.2.0 netmask 255.255.255.0 {
    range 10.71.2.9 10.71.2.27;
    range 10.71.2.81 10.71.2.243;
    option routers 10.71.2.1;
    option broadcast-address 10.71.2.255;
    option domain-name-servers 10.71.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 10.71.4.0 netmask 255.255.255.0 {
}
host Zeke {
    hardware ethernet 22:5d:5c:a6:9e:8b;
    fixed-address 10.71.1.77;
}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
jalankan script ini agar server hanya dapat di akses dengan ip address tertentu
```
echo 'upstream node {
    server 10.71.2.2;
    server 10.71.2.3;
    server 10.71.2.4;
}

server {
    listen 80;
    server_name _;

    location / {
	allow 10.71.1.77;
        allow 10.71.1.88;
        allow 10.71.2.144;
        allow 10.71.2.156;
        deny all;

        proxy_pass http://node;

        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/supersecret/htpasswd;
    }
    location /titan {
        rewrite ^/titan(.*)$ https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki/$1 permanent;
    }
}' > /etc/nginx/sites-available/eldia.it15.com

service nginx restart
```

dan yang terakhir jalankan script ini di client Zeke lalu restart Zeke
```
#!/bin/bash

echo "auto eth0
iface eth0 inet dhcp
hwaddress ether 22:5d:5c:a6:9e:8b
" > /etc/network/interfaces
```
![Screenshot 2024-10-27 060830](https://github.com/user-attachments/assets/ade6d7e1-e331-4116-979e-18864aa4b7ad)
![Screenshot 2024-10-27 060859](https://github.com/user-attachments/assets/2c81547c-40b2-4142-807a-a76f35d13730)


# No. 13
> Karena mengetahui bahwa ada keturunan marley yang mewarisi kekuatan titan, Zeke pun berinisiatif untuk menyimpan data data penting di Warhammer, dan semua data tersebut harus dapat diakses oleh anak buah kesayangannya, Annie, Reiner, dan Berthold.  (13)

pertama install mariadb server di warhammer untuk menyimpan data-data penting
```
#!/bin/bash

apt-get update
apt-get install mariadb-server -y
service mysql start

mysql -e "CREATE USER 'kelompokit15'@'%' IDENTIFIED BY 'passwordit15';"
mysql -e "CREATE USER 'kelompokit15'@'eldia.it15.com' IDENTIFIED BY 'passwordit23';"
mysql -e "CREATE DATABASE dbkelompokit15;"
mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'kelompokit15'@'%';"
mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'kelompokit23'@'eldia.it15.com';"
mysql -e "FLUSH PRIVILEGES;"

mysql="[mysqld]
skip-networking=0
skip-bind-address
"
echo "$mysql" > /etc/mysql/my.cnf

service mysql restart
```

install maria clien di laravel worker
```
apt-get update
apt-get install mariadb-client -y
```

setelah menginstal cobalah akses mariadb di laravel worker nya dengan perintah 
```
mariadb --host=10.71.3.4 --port=3306 --user=kelompokit15 --password
```
![Screenshot 2024-10-27 055859](https://github.com/user-attachments/assets/03895e4d-7c15-4b97-a4a1-6b959547bacf)

