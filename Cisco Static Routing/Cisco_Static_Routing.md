# 🌐 Cisco Packet Tracer – Static Routing Topology  

Dokumentasi ini berisi simulasi jaringan menggunakan **Cisco Packet Tracer** dengan topologi **2 Router, 2 Switch, dan 4 PC**.  
Tujuan utama adalah memahami konsep **Static Routing**, yaitu menghubungkan dua jaringan berbeda melalui router.  

---

## 🖼️ 1. Topologi Jaringan  

📌 **Struktur Koneksi**  
```
 PC1 --- Switch1 --- Router1 === Router2 --- Switch2 --- PC3
 PC2 ---/                                 \--- PC4
```

<p align="center">
  <img src="images/h1.png" alt="Topologi Jaringan" width="600"/><br>
  <b>Gambar 1.</b> Topologi Jaringan Static Routing
</p>
---

## ⚙️ 2. Spesifikasi Perangkat  
<div align="center">
| Perangkat | Model       | Jumlah | Keterangan                      |
|-----------|-------------|--------|---------------------------------|
| Router    | Cisco 1941  | 2      | Router penghubung antar subnet  |
| Switch    | Cisco 2960  | 2      | Switch distribusi host PC       |
| PC        | PC-PT       | 4      | End-user host                   |
| Kabel     | Straight    | –      | PC ↔ Switch ↔ Router            |
| Kabel     | Cross-Over  | –      | Router ↔ Router                 |
| Kabel     | Serial (opsional) | – | Untuk modul **WIC-2T**         |
</div>
---

## 💻 3. Konfigurasi IP Address  

📍 **Subnet 1 – 192.168.1.0/24**  
<div align="center">
| Perangkat | IP Address   | Subnet Mask     | Default Gateway |
|-----------|--------------|-----------------|-----------------|
| PC1       | 192.168.1.2  | 255.255.255.0   | 192.168.1.1     |
| PC2       | 192.168.1.3  | 255.255.255.0   | 192.168.1.1     |
</div>

<p align="center">
  <img src="images/h2.png" alt="Konfigurasi IP PC1" width="500"/><br>
  <b>Gambar 2.</b> Konfigurasi IP Address pada PC1
</p>

<p align="center">
  <img src="images/h3.png" alt="Konfigurasi IP PC2" width="500"/><br>
  <b>Gambar 3.</b> Konfigurasi IP Address pada PC2
</p>


📍 **Subnet 2 – 192.168.3.0/24**  
<div align="center">
| Perangkat | IP Address   | Subnet Mask     | Default Gateway |
|-----------|--------------|-----------------|-----------------|
| PC3       | 192.168.3.2  | 255.255.255.0   | 192.168.3.1     |
| PC4       | 192.168.3.3  | 255.255.255.0   | 192.168.3.1     |
</div>

<p align="center">
  <img src="images/h4.png" alt="Gambar 4. Konfigurasi IP Address pada PC3" width="500"/><br>
  <b>Gambar 4.</b> Konfigurasi IP Address pada PC3
</p>

<p align="center">
  <img src="images/h5.png" alt="Gambar 5. Konfigurasi IP Address pada PC4" width="500"/><br>
  <b>Gambar 5.</b> Konfigurasi IP Address pada PC4
</p>

---

## 🛠️ 4. Konfigurasi Router  

### 📡 Router1  
- **Gi0/0** → ke Switch1 (`192.168.1.1/24`)  
- **Gi0/1** → ke Router2 (`192.168.2.1/24`)  
- **Static Route** → mengenali jaringan `192.168.2.0/24`  

<p align="center">
  <img src="images/h6.png" alt="Gambar 6. Konfigurasi Router1 Gi0/0" width="500"/><br>
  <b>Gambar 6.</b> Konfigurasi Router1 Gi0/0
</p>

<p align="center">
  <img src="images/h7.png" alt="Gambar 7. Konfigurasi Router1 Gi0/1" width="500"/><br>
  <b>Gambar 7.</b> Konfigurasi Router1 Gi0/1
</p>

```bash
Router>enable
Router#
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface GigabitEthernet0/0
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
ip address 192.168.1.1 255.255.255.0
Router(config-if)#ip address 192.168.1.1 255.255.255.0
Router(config-if)#
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/1
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/0
Router(config-if)#
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/1
Router(config-if)#ip address 192.168.2.1 255.255.255.0
Router(config-if)#ip address 192.168.2.1 255.255.255.0
Router(config-if)#
```
---

### 📡 Router2  
- **Gi0/0** → ke Router1 (`192.168.2.2/24`) 
- **Gi0/1** → ke Switch2 (`192.168.3.1/24`)  
- **Static Route** → mengenali jaringan `192.168.3.0/24`  

<p align="center">
  <img src="images/h8.png" alt="Gambar 8. Konfigurasi Router2 Gi0/0" width="500"/><br>
  <b>Gambar 8.</b> Konfigurasi Router2 Gi0/0
</p>

<p align="center">
  <img src="images/h9.png" alt="Gambar 9. Konfigurasi Router2 Gi0/1" width="500"/><br>
  <b>Gambar 9.</b> Konfigurasi Router2 Gi0/1
</p>

```bash
Router>enable
Router#
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface GigabitEthernet0/0
Router(config-if)#ip address 192.168.2.2 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#interface GigabitEthernet0/1
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up
ip address 192.168.3.1 255.255.255.0
Router(config-if)#ip address 192.168.3.1 255.255.255.0
Router(config-if)#
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/0
Router(config-if)#
Router(config-if)#exit
Router(config)#interface GigabitEthernet0/1
Router(config-if)#
```
---

## 🔍 5. Pengujian Koneksi  

✅ **PC1 ↔ PC2 (dalam satu subnet)**  
```bash
PC1> ping 192.168.1.3
Reply from 192.168.1.3: bytes=32 time<1ms TTL=128
```

<p align="center">
  <img src="images/h10.png" alt="Gambar 10. Hasil uji ping PC1 ke PC2" width="500"/><br>
  <b>Gambar 10.</b> Hasil uji ping PC1 ke PC2
</p>

✅ **PC2 ↔ PC4 (antar subnet, via 2 router)**  
```bash
PC2> ping 192.168.3.3
Reply from 192.168.3.3: bytes=32 time<1ms TTL=128
```

<p align="center">
  <img src="images/h11.png" alt="Gambar 11. Hasil uji ping PC2 ke PC4" width="500"/><br>
  <b>Gambar 11.</b> Hasil uji ping PC2 ke PC4
</p>
---

## 📌 7. Kesimpulan  

1. Jaringan berhasil menghubungkan dua subnet berbeda: `192.168.1.0/24` dan `192.168.3.0/24`.  
2. Konfigurasi **IP Address, Gateway, dan Router** sudah benar sehingga komunikasi antar host berjalan.  
3. **Static Routing** memungkinkan jalur komunikasi ditentukan secara manual, sederhana, dan stabil.  
4. Uji konektivitas (ping) antar PC menunjukkan hasil **Reply sukses**.  
5. Topologi ini memperlihatkan konsep dasar bahwa **Router adalah penghubung utama antar jaringan berbeda**.  

---

✍️ **Author:** Dokumentasi Jaringan Cisco – *Static Routing Topology Haris Pambudi*
