# Jarkom-Modul-3-IT13-2024

## Anggota Kelompok IT 13
| Nama Lengkap          | NRP        |
| --------------------- | ---------- |
| Muhammad Dzaky Ahnaf  | 5027231039 |
| Muhammad Nafi Firdaus | 5027231045 |

### Buat Topologi

# Konfigurasi 

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
