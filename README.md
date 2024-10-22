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
