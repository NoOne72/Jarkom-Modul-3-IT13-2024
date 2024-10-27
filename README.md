# Jarkom-Modul-3-IT13-2024

## Anggota Kelompok IT 13
| Nama Lengkap          | NRP        |
| --------------------- | ---------- |
| Muhammad Dzaky Ahnaf  | 5027231039 |
| Muhammad Nafi Firdaus | 5027231045 |

### Buat Topologi

# 0 

## Fritz (DNS Server) 
- Mengaktifkan Akses Internet
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
```
- Lakukan Install untuk dependensi yang akan dibutuhkan
```
apt-get update -y
apt-get install bind9 -y
```
- Letakaan Konfiguras Domain pada `/etc/bind/named.conf.local`
```
echo '
zone "eldia.it13.com" {
	type master;
	file "/etc/bind/it13/eldia.it13.com";
};' > /etc/bind/named.conf.local

echo '
zone "marley.it13.com" {
	type master;
	file "/etc/bind/it13/marley.it13.com";
};' >> /etc/bind/named.conf.local
```
- Buat Direktori `/etc/bind/it13 directory`
```
mkdir -p /etc/bind/it22
```
- Lakukan Perubahan Pada DNS Record
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it13.com. root.eldia.it13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it13.com.
@       IN      A       10.70.2.2
@       IN      AAAA    ::1' > /etc/bind/it13/eldia.it13.com

echo "Konfigurasi DNS untuk eldia.it13.com sudah berhasil."

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it13.com. root.marley.it13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it13.com.
@       IN      A       10.70.1.2
@       IN      AAAA    ::1' > /etc/bind/it13/marley.it13.com

echo "Konfigurasi DNS untuk marley.it13.com sudah berhasil."
```
- Lakukan Restart Bind9
```
service bind9 restart
```
# Nomer 1
## Konfigurasi 

## Paradis (Router/DHCP Relay)

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.70.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.70.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.70.3.1
	netmask 255.255.255.0
	
auto eth4
iface eth4 inet static
	address 10.70.4.1
	netmask 255.255.255.0
```
## Tybur (DHCP Server) 
```
auto eth0
iface eth0 inet static
	address 10.70.4.2
	netmask 255.255.255.0
	gateway 10.70.4.1
```
## Fritz (DNS Server) 
```
auto eth0
iface eth0 inet static
	address 10.70.4.3
	netmask 255.255.255.0
	gateway 10.70.4.1
```
## Warhammer (Database Server) 
```
auto eth0
iface eth0 inet static
	address 10.70.3.2
	netmask 255.255.255.0
	gateway 10.70.3.1
```
## Colossal (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 10.70.3.3
	netmask 255.255.255.0
	gateway 10.70.3.1
```
## Beast (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 10.70.3.4
	netmask 255.255.255.0
	gateway 10.70.3.1
```
## Annie (Laravel Worker) 
```
auto eth0
iface eth0 inet static
	address 10.70.1.2
	netmask 255.255.255.0
	gateway 10.70.1.1
```
## Bertholdt (Laravel Worker) 
```
auto eth0
iface eth0 inet static
	address 10.70.1.3
	netmask 255.255.255.0
	gateway 10.70.1.1
```
## Reiner (Laravel Worker) 
```
auto eth0
iface eth0 inet static
	address 10.70.1.4
	netmask 255.255.255.0
	gateway 10.70.1.1
```
## Armin (PHP Worker) 
```
auto eth0
iface eth0 inet static
	address 10.70.2.2
	netmask 255.255.255.0
	gateway 10.70.2.1
```
## Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.70.2.3
	netmask 255.255.255.0
	gateway 10.70.2.1
```
## Mikasa (PHP Worker) 
```
auto eth0
iface eth0 inet static
	address 10.70.2.4
	netmask 255.255.255.0
	gateway 10.70.2.1
```
## Erwin (Client)
```
auto eth0
iface eth0 inet dhcp
```
## Zeke (Client) 
```
auto eth0
iface eth0 inet dhcp
```
# 1-5 (Cerita Pertama) 

## Tybur (DHCP Server) 
- Lakukan Update dan Install DHCP Server
```
apt-get update -y
apt-get install isc-dhcp-server -y
dhcpd --version
```
- Untuk memastikan install berhasil, lakukan cek status
- Lakukan pengecekan dan pastikan Network Interface untuk DHCP Server
```
echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server
```
Disini Kami menggunakan eth0

- Lakukan Konfigurasi DHCP Server
```
echo 'subnet 10.70.1.0 netmask 255.255.255.0 {
    range 10.70.1.05 10.70.1.25;
    range 10.70.1.50 10.70.1.100;
    option routers 10.70.1.1;
    option broadcast-address 10.70.1.255;
    option domain-name-servers 10.70.4.3;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 10.70.2.0 netmask 255.255.255.0 {
    range 10.70.2.09 10.70.2.27;
    range 10.70.2.81 10.70.2.243;
    option routers 10.70.2.1;
    option broadcast-address 10.70.2.255;
    option domain-name-servers 10.70.4.3;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 10.70.3.0 netmask 255.255.255.0 {
}' > /etc/dhcp/dhcpd.conf
```
- Lalu lakukan restart pada DHCP Server dan cek status
```
service isc-dhcp-server restart
service isc-dhcp-server status
```
## Fritz (DNS Server) 
- Agar Client bisa terhubung ke Fritz (DNS Server), Tambahkan Forwarder
```
echo '
options {
        directory "/var/cache/bind";

        forwarders {
            192.168.122.1;
        };

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```
