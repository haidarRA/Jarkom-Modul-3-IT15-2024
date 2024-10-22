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
