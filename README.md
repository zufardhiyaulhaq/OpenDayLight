OpenDayLight
============

OpenDayLight adalah sebuah perangkat lunak sumber terbuka yang berfungsi sebagai controller SDN (Software Defined Network).

OpenDayLight mendukung protokol SDN OpenFlow dan juga protokol SDN terbuka yang lain SEPERTI NetConf ataupun OSVDB.

## Arsitektur OpenDayLight
![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight/master/Images/OpenDayLight%20Architecture.png)

## Tentang Repositori
Repositori ini berisi tentang tutorial yang berkaitan dengan opendaylight.

Pengenalan Software Defined Network
===================================
Software Defined Network atau yang biasa disingkat dengan SDN adalah suatu paradigma baru dalam dunia jaringan dimana secara garis besar dapat memisahkan control plane dan data plane.

![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight/master/Images/OpenDayLight1.jpg)

Didalam SDN ada yang dinamakan dengan Controller, dimana controller ini berfungsi sebagai control plane untuk setiap forwarding device yang terhubung dengan controller.


OpenDayLight Virtual Tenant Network
===================================

Virtual Tenant Network (VTN) adalah sebuah paradigma dimana kita dapat mendesain sebuah jaringan virtual  
diatas sebuah jaringan fisik yang asli.

Virtual Tenant Network memisahkan antara jaringan virtual dan jaringan fisiknya, kita dapat mendesain sebuah jaringan virtual tanpa memperhatikan topologi jaringan fisik itu sendiri.

## Keuntungan
1. Pemisahan jarinagn virtual dan fisik akan menyembunyikan kompleksnya jaringan fisik tersebut,
1. Managemen perangkat yang lebih baik,
1. Memilimalisir kesalahan konfigurasi.


![alt text](https://raw.githubusercontent.com/zufardhiyaulhaq/OpenDayLight-VTN/master/Images/VTN_Overview.jpg)

Setelah kita mendesain sebuah jaringan virtual didalam VTN, jaringan virtual tersebut akan otomatis termapping kedalam jaringan fisik.
