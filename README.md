Tentang Repositori
==================
Repositori ini berisi tentang tutorial yang berkaitan dengan opendaylight. Ada beberapa tutorial yang akan dibahas pada repositori ini.

## Requirement
- Lab akan menggunakan GNS3 dan Docker,
- Minimal 8GB RAM,
- Host OS base on Ubuntu.

## Daftar Isi
1. Pengenalan Software Defined Network
1. Pengenalan OpenDayLight
1. Instalasi OpenDayLight
1. Instalasi OpenVSwitch
1. Instalasi Docker
1. Menghubungkan OpenVSwitch ke OpenDayLight (OpenFlow)
1. Menghubungkan perangkat ke OpenDayLight dengan NETCONF
1. OpenDayLight Clustering
1. OpenDayLight Virtual Tenant Network

## File yang diperlukan
- [GNS3](https://gns3.com/)
- [Ubuntu](https://www.ubuntu.com/)
- [Docker](https://www.docker.com/)
- [OpenDayLight Docker](https://hub.docker.com/r/zufardhiyaulhaq/opendaylight/)
- [OpenVSwitch](https://hub.docker.com/r/gns3/openvswitch/)


OpenDayLight
============
OpenDayLight adalah sebuah perangkat lunak sumber terbuka yang berfungsi sebagai controller SDN (Software Defined Network).

OpenDayLight mendukung protokol SDN OpenFlow dan juga protokol SDN terbuka yang lain seperti NetConf ataupun OSVDB.

## Arsitektur OpenDayLight
![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight/master/Images/OpenDayLight%20Architecture.png)


Pengenalan Software Defined Network
===================================
Software Defined Network atau yang biasa disingkat dengan SDN adalah suatu paradigma baru dalam dunia jaringan dimana secara garis besar dapat memisahkan control plane dan data plane.

![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight/master/Images/OpenDayLight1.jpg)

Didalam SDN ada yang dinamakan dengan Controller, dimana controller ini berfungsi sebagai control plane untuk setiap forwarding device yang terhubung dengan controller.

Instalasi OpenDayLight
======================
Direkomendasikan untuk menginstall OpenDayLight di ubuntu karena tutorial instalasi ini akan dilakukan di Ubuntu, begitu juga images docker OpenDayLight yang akan dipakai.

Tutorial ini tidak berkaitan dengan tutorial yang lain, karena kita akan menggunakan OpenDayLight base on Docker. Tetapi kemampuan instalasi OpenDayLight harus dibutuhkan untuk implementasi di dunia nyata.

## Panduan Instalasi
- Install Ubuntu
- Install unzip dan Wget
```
sudo apt-get install unzip wget
```
- Install Java

```
sudo apt-get install default-jre-headless
nano ~/.bashrc
 
tambahkan didalam bashrc
export JAVA_HOME=/usr/lib/jvm/default-java

jalankan 
source ~/.bashrc
```
- Unduh OpenDayLight
```
wget https://nexus.opendaylight.org/content/repositories/public/org/opendaylight/integration/distribution-karaf/0.5.4-Boron-SR4/distribution-karaf-0.5.4-Boron-SR4.zip
```
- Unzip OpenDayLight
```
unzip distribution-karaf-0.5.4-Boron-SR4.zip
```
- Jalankan OpenDayLight
```
cd distribution-karaf-0.5.4-Boron-SR4
./bin/karaf
```
- Install Feature DLUX

DLUX atau OpenDayLight User Interface adalah salah satu fitur penting dimana menyediakan interface atau antarmuka untuk mengkonfigurasi OpenDayLight

Berikut dokumentasi terkait dengan [DLUX](http://docs.opendaylight.org/en/stable-carbon/getting-started-guide/common-features/dlux.html)
```
feature:install odl-dlux-all
```
- Buka OpenDayLight dengan Browser

```
http://<your-opendaylight-ip>:8181/index.html
```
- Login dengan username : admin dan password : admin

![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight/master/Images/odl-dlux.png)

Instalasi OpenVSwitch
=====================
Tutorial ini tidak berkaitan dengan tutorial yang lain, karena kita akan menggunakan OpenVSwitch base on Docker. Tetapi kemampuan instalasi OpenVSwitch harus dibutuhkan untuk implementasi di dunia nyata.

Silahkan lihat repositori saya yang lain untuk instalasi OpenVSwitch.

- [OpenVSwitch](https://github.com/zufardhiyaulhaq/OpenVSwitch)

Instalasi Docker
================

## Pengenalan Docker
Docker adalah sebuah teknologi dimana sebuah aplikasi dapat berjalan didalam sebuah container. Container sendiri merupakan cara dimana aplikasi tersebut disimpan dan diikatkan dengan librarynya sehingga dapat berjalan dengan sempurna.

Perbedaan mendasar antara konsep container dengan virtual machine adalah didalam container, kita tidak perlu menginstall seluruh sistem operasi seperti virtual machine.

![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight/master/Images/docker-vm-container.png)

## Catatan Instalasi
- Docker pada repositori ini berguna untuk menyimpan container OpenVSwitch dan juga OpenDayLight sehingga tidak perlu menginstallnya didalam sebuah virtual machine. Dimana docker akan dikombinasikan dengan GNS3 sehingga container dapat dimasukan secara langsung kedalam Lab di GNS3.
- Kita akan menginstall docker pada Host OS sehingga tidak diperlukan sebuah GNS3 VM.

## Instalasi

### Set Up Repository

- Install apt agar mengizinkan untuk menggunakan repositori melalui https

```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
- Tambahkan Repository

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### instalasi Docker

```
$ sudo apt-get update
$ sudo apt-get install docker-ce
```

### Pull Docker Images yang diperlukan
Kita akan melakukan pull docker images OpenDayLight dan OpenVSwitch

```
$ docker pull zufardhiyaulhaq/opendaylight
$ docker pull gns3/openvswitch
```

Sampai tahap ini, kita sudah siap untuk melakukan Lab OpenDayLight.

Menghubungkan OpenVSwitch ke OpenDayLight
=========================================

Kasus ini hanyalah contoh bagaimana cara untuk menghubungkan OpenVSwitch ke OpenDayLight.

## Topologi
![alt text](https://github.com/zufardhiyaulhaq/OpenVSwitch/blob/master/Images/Topology.png)

## Konfigurasi OpenVSWitch

- Buat sebuah Bridge

```
$ ovs-vsctl add-br br0
```

- Masukan interface kedalam Bridge

```
$ ovs-vsctl add-port br0 eth0
$ ovs-vsctl add-port br0 eth1
$ ovs-vsctl add-port br0 eth2
$ ovs-vsctl add-port br0 eth3
```

- Set Controller ke OpenDayLight

```
$ ovs-vsctl set-controller br0 "tcp:192.168.122.254:6633"
```

## Konfigurasi OpenDayLight

- Jalankan OpenDayLight dengan Karaf

```
$ ./bin/karaf
```

- Install OpenDayLight DLUX dan L2Switch feature

```
feature:install odl-dlux-all odl-l2switch-all
```

## Verifikasi

Lihat apakah OpenVSwitch terkoneksi dengan Controller
```
$ ovs-vsctl list controller
_uuid               : dd619d79-33f8-4efb-89c6-bda14799b40e
connection_mode     : []
controller_burst_limit: []
controller_rate_limit: []
enable_async_messages: []
external_ids        : {}
inactivity_probe    : []
is_connected        : true
local_gateway       : []
local_ip            : []
local_netmask       : []
max_backoff         : []
other_config        : {}
role                : master
status              : {last_error="Connection refused", sec_since_connect="11", sec_since_disconnect="19", state=ACTIVE}
target              : "tcp:192.168.122.254:6633"
```

## Troubleshooting
- Jika OpenVSwitch dan OpenDayLight sudah terkoneksi tetapi tidak dapat terhubung ke controller, nyalakan interface bridge pada OpenVSwitch

```
$ ifconfig br0 up
```

Menghubungkan perangkat ke OpenDayLight dengan NETCONF
======================================================

NETCONF adalah protokol northbound pada SDN (Software Defined Network) yang berfungsi sama seperti OpenFlow. 

Kita akan mensimulasikan NETCONF dengan sebuah NETCONF Testtool yang dibuat oleh OpenDayLight. Untuk versi perangkat aslinya, Masih dalam tahap riset.

### Topologi
![alt text](https://github.com/zufardhiyaulhaq/OpenDayLight/blob/master/Images/NETCONF-Topology-1.png)

### Konfigurasi pada OpenDayLight

- Jalankan OpenDayLight

```
./bin/karaf
```

- Install Feature OpenDayLight

```
feature:install odl-netconf-topology odl-restconf
```

### Konfigurasi pada Perangkat

- Jalankan Perangkat NETCONF dan lakukan instalasi wget dan maven (dalam hal ini menggunakan ubuntu).

```
$ apt-get install maven wget
```

- Unduh OpenDayLight NETCONF Testtool

```
$ wget https://nexus.opendaylight.org/content/repositories/public/org/opendaylight/netconf/netconf-testtool/1.1.4-Boron-SR4/netconf-testtool-1.1.4-Boron-SR4-executable.jar
```

- Jalankan NETCONF Testtool

```
java -jar netconf-testtool-1.1.4-Boron-SR4-executable.jar
```

### Tambahkan Perangkat didalam OpenDayLight dengan Menggunakan RESTCONF

Dengan menggunakan postman, akan lebih mudah memasukan perintah RESTCONF.
- Type : PUT
- URL : _http://192.168.122.254:8181/restconf/config/network-
topology:network-topology/topology/topology-
netconf/node/**new-netconf-device**_
- Payload atau Body :
```
<node xmlns="urn:TBD:params:xml:ns:yang:network-topology">
<node-id>new-netconf-device</node-id>
<host xmlns="urn:opendaylight:netconf-node-topology">192.168.122.2</host>
<port xmlns="urn:opendaylight:netconf-node-topology">17830</port>
<username xmlns="urn:opendaylight:netconf-node-topology">admin</username>
<password xmlns="urn:opendaylight:netconf-node-topology">admin</password>
<tcp-only xmlns="urn:opendaylight:netconf-node-topology">false</tcp-only>
</node>
```

### Verifikasi

Lakukan verifikasi apakah perangakt berhasil terhubung dengan OpenDayLight.

Dengan menggunakan postman.
- Type : GET
- URL :_http://192.168.122.254:8181/restconf/operational/network-
topology:network-topology/topology/topology-
netconf/node/**new-netconf-device**_

![alt test](https://github.com/zufardhiyaulhaq/OpenDayLight/blob/master/Images/NETCONF-POSTMAN-2.png)

### Menghapus Perangkat NETCONF

Dengan menggunakan postman.
- Type : DELETE
- URL : _http://192.168.122.254:8181/restconf/config/network-topology:network-topology/topology/topology-
netconf/node/**new-netconf-device**_
 
![alt text](https://github.com/zufardhiyaulhaq/OpenDayLight/blob/master/Images/NETCONF-Postman-1.png)

OpenDayLight Virtual Tenant Network
===================================
Virtual Tenant Network (VTN) adalah sebuah paradigma dimana kita dapat mendesain sebuah jaringan virtual  
diatas sebuah jaringan fisik yang asli.

Virtual Tenant Network memisahkan antara jaringan virtual dan jaringan fisiknya, kita dapat mendesain sebuah jaringan virtual tanpa memperhatikan topologi jaringan fisik itu sendiri.

## Keuntungan
1. Pemisahan jaringan virtual dan fisik akan menyembunyikan kompleksnya jaringan fisik tersebut,
1. Managemen perangkat yang lebih baik,
1. Memilimalisir kesalahan konfigurasi.


![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight-VTN/master/Images/VTN_Overview.jpg)

Setelah kita mendesain sebuah jaringan virtual didalam VTN, jaringan virtual tersebut akan otomatis termapping kedalam jaringan fisik.
