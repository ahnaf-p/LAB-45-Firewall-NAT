# LAB-45-Firewall-NAT
Selasa 26 Agustus 2025  
  
# Network Address Translation  
  
  
# Chain  
  **srcnat**  
    
  **dstnat**  
  
# Action
  1. src-nat  
     Tindakan NAT yang mengganti IP sumber paket manjadi IP yang ditentukan di **to address** sebelum paket keluar router. Semua paket yang keluar dari jaringan internal (LAN) ke internat akan selalu memakai IP Publik tertentu yang statis. Berbeda dengan masquerade yang dynamic atau netmap. src-nat ditentukan static. Tujuannya agar paket keluar LAN terlihat berasal dari IP publik tertentu.  
     Contoh pengunaan:  
![](IMAGES/srcaddress.png)  
     chain=srcnat, Paket keluar LAN > WAN  
     src-address=22.6.8.0/24, Semua IP LAN diproses  
![](IMAGES/srcaddresslanjutpartduasoalnyarame.png)  
     action=src-nat, Ganti IP sumber paket  
     to-addresses=192.168.87.7, IP publik(WAN) statis yang digunakan  
![](IMAGES/bisaterkoneksikeinternettanpamasqueradecumanminesnyaipnyaharusjangangantigantisolanyastatisjadinantiharusdiubahlagikaloganti.png)  
     Hasilnya semua perangkat di subnet 192.168.88.0/24 akan terlihat keluar ke internet sebagai IP 192.168.87.7  
       
  2. masquerade  
     NAT otomatis yang mengganti alamat sumber paket dari alamat IP lokal menjadi alamat IP WAN. Untuk menyembunyikan local address dibelik public address yang didapat dari ISP. Dipakai saat semua perangkat di jaringan lokal ingin mengakses internet melalui WAN tanpa konfigurasi IP manual.  
     Contoh penggunaan:  
![](IMAGES/masq1.png)  
     chain=src-nat, untuk mengubah alamat sumber paket yang keluar dari router.  
     out-interface=ether1, interface yang mengarah ke WAN, NAT hanya berkalu untuk paket keluar lewat interface ini.  
![](IMAGES/masq2.png)  
     action=masquerade, NAT otomatis mengganti alamat sumber paket ke IP WAN.  
       
     Pengujian:  
     Lakukan ping di PC client untuk mengecek apakah client bisa mengakses internet.  
![](IMAGES/pingmasq.png)  
  
  3. accept  
     Menerima paket tanpa menjalankan NAT. Paket tidak lanjut ke next firewall rule. Paket dilewati sehingga tidak diubah alamat IP atau portnya. Digunakan jika ada perangkat di jaringan lokal yang harus tetap menggunakan alamat IP asli saat mengakses internet.  
     Contoh penggunaan:  
![](IMAGES/acchashtagizinadmin.png)  
![](IMAGES/accdongmin.png)  
     Pengujian:  
     Untuk pengujian, cara mudahnya kita bisa lihat di kolom **Bytes** dan **Packets**, jika angkanya terus bertambah, itu tandanya paket aktif/berjalan.  
![](IMAGES/natcounters.png)  
  
  4. redirect  
     Replace destinasi port dari IP packet dengan yang spesifik dari **to-ports** parameter dan destinasi address ke address dari virtual atau physical interface. Mengaarhkan paket ke router itu sendiri. Semua paket LAN diarahkan ke router sebelum ke internet.  
     Contoh Penggunaan:  
![](IMAGES/neverssl.png)
dst-address, IP tujuan paket
dst-port, port tujuan paket  
protocol, protokol paket  
![](IMAGES/neverssl2.png)  
action=redirect, paket diarahkan ke router sendiri  
to-ports, port di router yang akan menerima paket (3128 untuk proxy)  
![](IMAGES/neverSSL3.png)  
  5. add-dst-to-address-list  
     Untuk menambah tujuan paket (destination IP) kedalam **address list**. Tidak mengubah paket, hanya mencatat alamat tujuan untuk keperluan firewall, NAT atau monitoring.  
     Contoh penggunaan:  
![](IMAGES/catetdst.png)  
dst-address, jika kosong, semua IP tujuan ditambahkan.  
src-address, jika kosong, semua ip sumber akan ditambahkan.  
![](IMAGES/catetdst2.png)  
![](IMAGES/catetdst3.png)  
  6. add-src-to-address-list  
     Untuk menambahkan alamat sumber ke dalam **address list**. Tidak mengubah paket, hanya mencatat alamat sumber untuk keperluan firewall, NAT, atau monitoring.  
     Contoh penggunaan:  
![](IMAGES/srcnat.png)  
![](IMAGES/srcnat2.png)  
![](IMAGES/srcnat3.png)  
  7. jump
     Digunakan untuk melompat ke chai nlain(custom chain). Tujuannya untuk memisahkan rule NAT yang spesifik agar chain utama tetap rapi dan lebih mudah diatur. Jadi NAT tidak di proses di rule utama lagi, tapi di proses di chain target.
     Contoh Pengunaan:
![](IMAGES/wellwellwell.png)
![](IMAGES/srcnatya.png)
![](IMAGES/srcnatsrcnar.png)  
