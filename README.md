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
