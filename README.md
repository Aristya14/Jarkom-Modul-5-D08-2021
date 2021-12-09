# Jarkom-Modul-5-D08-2021
- 05111940000233 Aristya Vika Wijaya
- 05111940000199 Aprilia Annisa Suryo
- 05111940000188 Riki Wahyu Nur Dianto

## SubNomor A-D
### A-B. Membuat topologi dengan menggunakan teknik VLSM 
![image](https://user-images.githubusercontent.com/72466039/145443495-8ca75b2a-e4b4-4b27-862e-c203bc1ea6aa.png)
 
 Berikut Merupakan Jumlah IP dan Netmask yang Didapat
 | Subnet | Jumlah IP | Netmask |
 | :---: | :-: | :-: |
 | A1 | 3 | /29 |
 | A2 | 101 | /25 |
 | A3 | 701 | /22 |
 | A4 | 2 | /30 |
 | A5 | 2 | /30 |
 | A6 | 301 | /23 |
 | A7 | 201 | /24 |
 | A8 | 3 | /29 |
 | TOTAL | 1314 | /21 |
 
 Perhitungan Alamat IP berdasarkan NID dan juga Netmask Menggunakan TREE Sebagai Berikut:
 ![image](https://user-images.githubusercontent.com/72466039/145445846-2eb16aee-02a9-427b-bc90-3de75af0ed58.png)

 Dari tree tersebut didapatkan pembagian IP sebagia berikut:
 ![image](https://user-images.githubusercontent.com/72466039/145446318-0521f169-0425-42e8-8b41-9478106b1ab9.png)

  Setting interface pada GNS3 Foosha
  **water** 7
``` bash 
  auto eth0
  iface eth0 inet static
  address 10.25.0.18
  netmask 255.255.255.252
         	gateway 10.25.0.17
 
  auto eth1
  iface eth1 inet static
  address 10.25.0.129
  netmask 255.255.255.128
 
  auto eth2
  iface eth2 inet static
  address 10.25.4.1
  netmask 255.255.252.0
 
  auto eth3
  iface eth3 inet static
  address 10.25.0.1
  netmask 255.255.255.248
  ```
 
**DORIKI**
``` bash
  auto eth0
  iface eth0 inet static
  address 10.25.0.2
  netmask 255.255.255.248
  gateway 10.25.0.1
```
**JIPANGU**
``` bash 
  auto eth0
  iface eth0 inet static
  address 10.25.0.3
  netmask 255.255.255.248
  gateway 10.25.0.1
```

**FOOSHA**
``` bash
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
  address 10.25.0.21
  netmask 255.255.255.252

  auto eth2
  iface eth2 inet static
  address 10.25.0.17
  netmask 255.255.255.252
```

**GUANHAO**
``` bash 
  auto eth0
  iface eth0 inet static
    address 10.25.0.22
    netmask 255.255.255.252
    gateway 10.25.0.21

  auto eth1
  iface eth1 inet static
  address 10.25.2.1
  netmask 255.255.254.0

  auto eth2
  iface eth2 inet static
  address 10.25.1.1
  netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
  address 10.25.0.9
  netmask 255.255.255.248
```

**JORGE**
``` bash 
  auto eth0
  iface eth0 inet static
    address 10.25.0.10
    netmask 255.255.255.248
    gateway 10.25.0.9
```

**MAINGATE**
``` bash 
  auto eth0
  iface eth0 inet static
    address 10.25.0.11
    netmask 255.255.255.248
    gateway 10.25.0.9
```
**Blueno**
  auto eth0
  iface eth0 inet dhcp

**Chiper**
  auto eth0
  iface eth0 inet dhcp

**ELENA**
  auto eth0
  iface eth0 inet dhcp

**fukurou**
  auto eth0
  iface eth0 inet dhcp

### C. Melakukan routing pada Foosha sebagai berikut
``` bash
route add -net 10.25.1.0 netmask 255.255.255.0 gw 10.25.0.6                                     
route add -net 10.25.2.0 netmask 255.255.254.0 gw 10.25.0.22                                  
route add -net 10.25.0.8 netmask 255.255.255.248 gw 10.25.0.22                                    
route add -net 10.25.4.0 netmask 255.255.252.0 gw 10.25.0.18                                         
route add -net 10.25.0.128 netmask 255.255.255.128 gw 10.25.0.18                                 
route add -net 10.25.0.0 netmask 255.255.255.248 gw 10.25.0.18
```

### D. setting DHCP Relay
Pada water7, foosha, dan guanhao menginstall apt-get update, apt-get install isc-dhcp-relay -y
Lalu edit pada file `/etc/default/isc-dhcp-relay` pada bagian server dan interfaces menjadi
**foosha**
SERVERS="10.25.0.3"
INTERFACES="eth1 eth2"

**Water 7**
SERVERS="10.25.0.3"
INTERFACES="eth0 eth1 eth2 eth3"

**Guanhao**
SERVERS="10.25.0.3"
INTERFACES="eth0 eth1 eth2"

**JIPANGU**
sebagai DHCP server, install apt-get install isc-dhcp-server
lalu pada file `/etc/dhcp/dhcpd.conf` edit menjadi

``` bash
subnet 10.25.0.128 netmask 255.255.255.128 {
	range 10.25.0.130 10.25.0.255;
	option routers 10.25.0.129;
	option broadcast-address 10.25.0.255;
	option domain-name-servers 10.25.0.2;
	default-lease-time 360;
	max-lease-time 7200;
}
 
subnet 10.25.0.0 netmask 255.255.255.248 {
}
 
subnet 10.25.4.0 netmask 255.255.252.0 {
	range 10.25.4.2 10.25.7.255;
	option routers 10.25.4.1;
	option broadcast-address 10.25.7.255;
	option domain-name-servers 10.25.0.2;
	default-lease-time 360;
	max-lease-time 7200;
}
 
subnet 10.25.2.0 netmask 255.255.254.0 {
	range 10.25.2.2 10.25.3.255;
	option routers 10.25.2.1;
	option broadcast-address 10.25.3.255;
	option domain-name-servers 10.25.0.2;
	default-lease-time 360;
	max-lease-time 7200;
}
 
subnet 10.25.1.0 netmask 255.255.255.0 {
	range 10.25.1.2 10.25.1.255;
	option routers 10.25.1.1;
	option broadcast-address 10.25.1.255;
	option domain-name-servers 10.25.0.2;
	default-lease-time 360;
	max-lease-time 7200;
}

```

lalu pada file `/etc/default/isc-dhcp-server` isi bagian interfaces dengan `eth0`

## NOMOR 1 Mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.
## NOMOR 2 Mendrop semua akses HTTP dari luar Topologi kalian pada server yang memiliki ip DHCP dan DNS Server demi menjaga keamanan.
## NOMOR 3 Luffy meminta untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.
## NOMOR 4 Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.Selain itu di reject 
## NOMOR 5 Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.Selain itu di reject
## NOMOR 6 Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate








