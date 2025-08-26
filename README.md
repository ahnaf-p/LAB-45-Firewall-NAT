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
     
