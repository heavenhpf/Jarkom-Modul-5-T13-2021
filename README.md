# Jarkom-Modul-5-T13-2021

## Anggota Kelompok

| No. | Nama | NRP |
| ------ | ------ | ------ |
| 1. | Heaven Happyna Putra F. | (05311940000026) |
| 2. | Gavin Bagus Kenzie Narain | (05311940000028) |
| 3. | Tera Nurwahyu Pratama | (05311940000039) |

## Daftar Isi
* [Pre-Soal](#pre-soal)
* [Soal 1](#soal-1)
* [Soal 2](#soal-2)
* [Soal 3](#soal-3)
* [Soal 4](#soal-4)
* [Soal 5](#soal-5)
* [Soal 6](#soal-6)

## Pre-Soal
A. Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy.

B. Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting, 

C. Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

D. Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

### Penyelesaian
Di sini, kami menggunakan metode CIDR untuk melakukan subnetting dan membagi IP yang sesuai untuk setiap subnet. Penggambaran subnetting yang kami buat kurang lebih adalah sebagai berikut:

![image](https://user-images.githubusercontent.com/72731522/145670879-2e49df9f-fead-446f-9da2-2ce8a0e573c0.png)

Kemudian dibuat tree sebagai berikut:

![image](https://user-images.githubusercontent.com/72731522/145670881-1db218e2-97e0-4247-8bfa-1009f49692bd.png)

Setelah subnetting berhasil dilakukan, langkah selanjutnya adalah melakukan routing dan konfigurasi IP dengan detailnya sebagai berikut:

DHCP client (Blueno, Cipher, Fukurou, dan Elena):
```bash
auto eth0
iface eth0 inet dhcp
```

FOOSHA (ROUTER):

Address pada eth0 didapat dengan membuat eth0 sebagai dhcp terlebih dahulu, catat IPnya, kemudian konfigurasi IP staticnya dengan IP yang dicatat tersebut. 
```bash
auto eth0
iface eth0 inet static
address 192.168.122.36
netmask 255.255.255.0
gateway 192.168.122.1

auto eth1
iface eth1 inet static
        address 10.48.48.1
        netmask 255.255.255.252
post-up route add -net 10.48.36.0 netmask 255.255.255.248 gw 10.48.48.2
post-down route del -net 10.48.36.0 netmask 255.255.255.248 gw 10.48.48.2
post-up route add -net 10.48.40.0 netmask 255.255.254.0 gw 10.48.48.2
post-down route del -net 10.48.40.0 netmask 255.255.254.0 gw 10.48.48.2
post-up route add -net 10.48.32.0 netmask 255.255.255.0 gw 10.48.48.2
post-down route del -net 10.48.32.0 netmask 255.255.255.0 gw 10.48.48.2

auto eth2
iface eth2 inet static
        address 10.48.16.1
        netmask 255.255.255.252
post-up route add -net 10.48.4.0 netmask 255.255.255.248 gw 10.48.16.2
post-down route del -net 10.48.4.0 netmask 255.255.255.248 gw 10.48.16.2
post-up route add -net 10.48.8.0 netmask 255.255.255.128 gw 10.48.16.2
post-down route del -net 10.48.8.0 netmask 255.255.255.128 gw 10.48.16.2
post-up route add -net 10.48.0.0 netmask 255.255.252.0 gw 10.48.16.2
post-down route del -net 10.48.0.0 netmask 255.255.252.0 gw 10.48.16.2
```

GUANHAO (ROUTER):
```bash
auto eth0
iface eth0 inet static
        address 10.48.48.2
        netmask 255.255.255.252
gateway 10.48.48.1
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.48.48.1
post-down route del -net 0.0.0.0 netmask 0.0.0.0 gw 10.48.48.1

auto eth1
iface eth1 inet static
        address 10.48.40.1
        netmask 255.255.254.0

auto eth2
iface eth2 inet static
        address 10.48.32.1
        netmask 255.255.255.0

auto eth3
iface eth3 inet static
        address 10.48.36.1
        netmask 255.255.255.248
```

WATER7 (ROUTER)
```bash
auto eth0
iface eth0 inet static
        address 10.48.16.2
        netmask 255.255.255.252
	gateway 10.48.16.1
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.48.16.1
post-down route del -net 0.0.0.0 netmask 0.0.0.0 gw 10.48.16.1

auto eth1
iface eth1 inet static
        address 10.48.8.1
        netmask 255.255.255.128

auto eth2
iface eth2 inet static
        address 10.48.0.1
        netmask 255.255.252.0

auto eth3
iface eth3 inet static
        address 10.48.4.1
        netmask 255.255.255.248
```

DORIKI (DNS SERVER):
```bash
auto eth0
iface eth0 inet static
        address 10.48.4.3
        netmask 255.255.255.248
gateway 10.48.4.1
```

JIPANGU (DHCP SERVER):
```bash
auto eth0
iface eth0 inet static
        address 10.48.4.2
        netmask 255.255.255.248
gateway 10.48.4.1
```

MAINGATE (WEB SERVER):
```bash
auto eth0
iface eth0 inet static
        address 10.48.36.2
        netmask 255.255.255.248
gateway 10.48.36.1
```

JORGE (WEB SERVER):
```bash
auto eth0
iface eth0 inet static
        address 10.48.36.3
        netmask 255.255.255.248
gateway 10.48.36.1
```

setting DHCP, DNS, dan WebServer baru bisa dilakukan ketika topologi bisa mengakses keluar (Soal 1).

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.
### Penyelesaian
Jalankan `iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 192.168.122.10 -s 10.48.0.0/16` agar foosha bisa mengakses keluar. Pada opsi `—to-source`, mengikuti IP address eth0 dari Foosha, dan seperti yang sudah saya jelaskan sebelumnya, IP itu harus diganti jika memulai sesi GNS3 yang baru.

Kalau sudah harusnya semua sudah bisa ping google kecuali client. Kemudian, dilakukan installing package yang dibutuhkan:

1. doriki:

![image](https://user-images.githubusercontent.com/72731522/145670887-84c3b045-2f91-4af2-8434-eb1d7e1c240d.png)

2. jipangu:

![image](https://user-images.githubusercontent.com/72731522/145670893-706d3407-86ab-4ad7-a71b-ed034bd830b2.png)

3. water7:

![image](https://user-images.githubusercontent.com/72731522/145670896-f85232b1-5091-4821-a1e5-4340f8be6e66.png)

4. guanhao:

![image](https://user-images.githubusercontent.com/72731522/145670898-11e21ec4-63af-49be-a17a-1da35b291461.png)

Setelah itu, dilakukan setting pada DHCP Server dan Relay.

1. JIPANGU (DHCP SERVER):

install dhcp server
```bash
apt-get update
apt-get install isc-dhcp-server
```

melakukan setting pada INTERFACES eth0
```bash
echo '#DHCPD_CONF=/etc/dhcp/dhcpd.conf
#DHCPD_PID=/var/run/dhcpd.pid
#OPTIONS=""
INTERFACES="eth0"' > /etc/default/isc-dhcp-server
```

Kemudian, ditambahkan subnet ke dalam dhcpd.conf:
```bash
echo "subnet 10.48.4.0 netmask 255.255.255.248 {
}

#subnet A2
subnet 10.48.0.0 netmask 255.255.252.0 {
    range 10.48.0.2 10.48.2.189;
    option routers 10.48.0.1;
    option broadcast-address 10.48.3.254;
    option domain-name-servers 10.48.4.3, 202.46.129.2;
    default-lease-time 360;
    max-lease-time 7200;
}

#subnet A3
subnet 10.48.8.0 netmask 255.255.255.128 {
    range 10.48.8.2 10.48.8.101;
    option routers 10.48.8.1;
    option broadcast-address 10.48.8.127;
    option domain-name-servers 10.48.4.3, 202.46.129.2;
    default-lease-time 360;
    max-lease-time 7200;
}

#subnet A7
subnet 10.48.32.0 netmask 255.255.255.0 {
    range 10.48.32.2 10.48.32.201;
    option routers 10.48.32.1;
    option broadcast-address 10.48.32.254;
    option domain-name-servers 10.48.4.3, 202.46.129.2;
    default-lease-time 360;
    max-lease-time 7200;
}

#subnet A6
subnet 10.48.40.0 netmask 255.255.254.0 {
    range 10.48.40.2 10.48.41.44;
    option routers 10.48.40.1;
    option broadcast-address 10.48.41.255;
    option domain-name-servers 10.48.4.3, 202.46.129.2;
    default-lease-time 360;
    max-lease-time 7200;
}

" >> /etc/dhcp/dhcpd.conf
```
keterangan untuk konfigurasi:
- `range` di sini kita masukkan range IP sesuai subnetnya. misal pada subnet A2, mempunyai 700 host dengan NID 10.48.0.0 . Maka range IPnya adalah 10.48.0.2 - 10.48.2.189 . Hal ini berlaku untuk subnet lainnya
- `option routers` diisi gateway dari subnet tersebut
- `option broadcast-address` diisi broadcast address. didapat dari operasi OR dari ip dan (netmask')
- `option domain-name-servers` diisi DNS server yaitu IP DORIKI (DNS Server) dan 202.46.129.2 (ns1 its)
- `default-lease-time` diisi lama waktu DHCP server meminjamkan alamat IP kepada klien yaitu 360 detik
- `max-lease-time` diisi waktu maksimal yang di alokasikan untuk peminjaman IP oleh DHCP server ke klien yaitu 7200 detik


Untuk DHCP Relay ada 3, yaitu router WATER7, FOOSHA, dan GUANHAO.

2.  WATER7 (DHCP Relay):
```bash
apt-get update
apt-get install isc-dhcp-relay
echo '# What servers should the DHCP relay forward requests to?
SERVERS=10.48.4.2

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay
```

3. FOOSHA (DHCP Relay):
```bash
apt-get update
apt-get install isc-dhcp-relay
echo '# What servers should the DHCP relay forward requests to?
SERVERS=10.48.4.2

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay
```

4. GUANHAO (DHCP Relay):
```bash
apt-get update
apt-get install isc-dhcp-relay
echo '# What servers should the DHCP relay forward requests to?
SERVERS=10.48.4.2

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay
```

Selanjutnya, dilakukan konfigurasi pada DNS Servernya, yakni dengan menginstall bind9 dan diatur forwardernya:
```bash
apt-get update
apt-get install bind9
echo 'options {
        directory "/var/cache/bind";
        forwarders {
        192.168.122.1;
        };
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p
```

Disetting juga forwarder setiap DHCP Relay (WATER7, FOOSHA, GUANHAO) dengan ditambahkan command berikut:
```bash
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p
```

Jika sudah, setiap klien seharusnya sudah mendapat IP dynamic:
![image](https://user-images.githubusercontent.com/73151823/145667289-e29de4dd-3559-41d2-bdc4-db93c3d17069.png)
![image](https://user-images.githubusercontent.com/73151823/145667301-5343b2f8-4bee-406d-a6f8-423cd787fb34.png)
![image](https://user-images.githubusercontent.com/73151823/145667309-907bfc84-5df1-4046-9a46-cc5296e67118.png)
![image](https://user-images.githubusercontent.com/73151823/145667312-99f7ff5c-75c5-427f-bc98-ac92631ab269.png)

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.
### Penyelesaian
Command berikut dimasukkan pada router FOOSHA:
```bash
iptables -A FORWARD -d 10.48.4.0/29 -i eth0 -p tcp -m tcp --dport 80 -j DROP
```

Menggunakan syntax `-A FORWARD` FORWARD chain untuk menyaring paket dengan syntax `-p tcp -m tcp` protokol TCP dari luar topologi menuju ke JIPANGU (DHCP server) dan DORIKI (DNS Server) (yang berada di satu subnet yang sama yaitu `-d 10.48.4.0/2`), dimana akses HTTP (yang memiliki `--dport 80 port 80`) yang masuk ke JIPANGU (DHCP server) dan DORIKI (DNS Server) melalui `-i eth0` interfaces `eth`0 dari  JIPANGU (DHCP server) danDORIKI (DNS Server) agar di DROP dengan menggunakan syntax `-j DROP`

### Tes
tes menggunakan python http server. Pada foosha, buat http server pada port 80:

![image](https://user-images.githubusercontent.com/73151823/145667395-a89a033d-9ad9-467d-a2d0-1db1376d707b.png)

kemudian lakukan akses pada [localhost:80](http://localhost:80) pada router lain. hasilnya akan refused

![image](https://user-images.githubusercontent.com/73151823/145667402-e2bdc83e-26ca-4a21-a261-3cfbf33b48fa.png)

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.
### Penyelesaian
Ditambahkan command berikut diatur pada JIPANGU (DHCP server) dan DORIKI (DNS Server:
```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```
Menggunakan command `-A INPUT` untuk menyaring paket dengan syntax `-p icmp` protokol ICMP yang masuk agar dibatasi dengan menggunakan command `-m connlimit --connlimit-above 3` hanya sebatas maksimal 3 koneksi saja,  `--connlimit-mask 0` darimana saja, sehingga selebihnya akan di DROP dengan command `-j DROP`.

### Tes
tes menggunakan ping ke ip DORIKI/JIPANGU. Ping ke jipangu saja karena doriki nanti akan direstrict aksesnya sesuai jam pada no 5. Jika benar, maka ping ke 4 tidak akan bisa.

![image](https://user-images.githubusercontent.com/73151823/145667429-c8aa78e6-f410-4cf3-be7e-8dbd780ac17f.png)

![image](https://user-images.githubusercontent.com/73151823/145667433-4e7c9a9f-af38-4398-aef8-07b834477bf5.png)

![image](https://user-images.githubusercontent.com/73151823/145667440-58c3d770-d5ef-4e03-9854-dbeabb7e5db3.png)

![image](https://user-images.githubusercontent.com/73151823/145667446-cb1bac83-a4c2-4527-95e9-d1d5dd690125.png)

tidak bisa ping saat di fukurou

## Soal 4
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut:

Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.
### Penyelesaian
Karena membatasi akses ke Doriki, maka lakukan iptables pada Doriki:

`iptables -A INPUT -s 10.48.8.0/25 -m time --weekdays Fri,Sat,Sun -j REJECT`

`iptables -A INPUT -s 10.48.0.0/22 -m time --weekdays Fri,Sat,Sun -j REJECT`

Syntax di atas menggunakan `-A INPUT` untuk menyaring paket dari Blueno `10.48.8.0/25` dan Cipher `10.48.0.0/22` kemudian dispesifikkan waktunya yaitu `—weekdays Fri,Sat,Sun` untuk direject `-j REJECT`.

Berikutnya untuk menyaring jam, kita gunakan dua syntax untuk setiap subnet. Pertama untuk mereject jam 00:00 hingga 06:59, kedua dari jam 15:01 hingga 23:59. Syntax berikut untuk menyaring jam 00:00 hingga 06:59:

`iptables -A INPUT -s 10.48.8.0/25 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu -j REJECT`

`iptables -A INPUT -s 10.48.0.0/22 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu -j REJECT`

Syntax berikut untuk menyaring jam 15:01 hingga 23:59:

`iptables -A INPUT -s 10.48.8.0/25 -m time --timestart 15:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu -j REJECT`

`iptables -A INPUT -s 10.48.0.0/22 -m time --timestart 15:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu -j REJECT`

### Tes
ping ke google, jika sudah benar harusnya tidak bisa. Sebelum itu ubah date agar masuk ke range ditolak:

![image](https://user-images.githubusercontent.com/73151823/145667525-19e1fd90-6a82-44f4-8961-d8653613fc22.png)

![image](https://user-images.githubusercontent.com/73151823/145667527-d41f1afe-99a5-43bc-b15e-c7868a386d0c.png)

jika masuk rangenya, maka ping berhasil

![image](https://user-images.githubusercontent.com/73151823/145667540-152a2160-3469-4260-acde-0b08e56b33e6.png)

![image](https://user-images.githubusercontent.com/73151823/145667543-572f1611-999b-412d-b671-d51a0fce84bd.png)

jika tidak masuk rangenya, maka ping gagal

## Soal 5
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut:

Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
### Penyelesaian
Syntaxnya hampir sama dengan no 4, hanya saja jamnya beda dan tidak ada batasan hari:

`iptables -A INPUT -s 10.48.40.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT`

`iptables -A INPUT -s 10.48.32.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT`

## Soal 6
Selain itu di reject:

Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate
### Penyelesaian
Di GUANHAO, ditambahkan command/syntax berikut:
`iptables -A PREROUTING -t nat -p tcp -d 10.48.4.3 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.48.36.3`

`iptables -A PREROUTING -t nat -p tcp -d 10.48.4.3 -j DNAT --to-destination 10.48.36.2`

## Kendala yang Dialami
1. Pada setiap boot GNS3, entah kenapa harus mengulangi membuat IP baru pada foosha agar bisa berhasil (yang berarti address yang saya tulis di konfigurasi eth0 router foosha ini hanya work untuk satu sesi GNS3, kemudian tidak work ketika memulai sesi berikutnya)
