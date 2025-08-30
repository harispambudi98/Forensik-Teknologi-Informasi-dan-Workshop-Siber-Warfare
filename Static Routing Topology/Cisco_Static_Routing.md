# 🌐 Cisco Packet Tracer – Static Routing Topology  

Dokumentasi ini berisi simulasi jaringan menggunakan **Cisco Packet Tracer** dengan topologi **2 Router, 2 Switch, dan 4 PC**.  
Tujuan utama adalah memahami konsep **Static Routing**, yaitu menghubungkan dua jaringan berbeda melalui router.  

---

## 🖼️ 1. Topologi Jaringan  

📌 **Struktur Koneksi**  

PC1 --- Switch1 --- Router1 === Router2 --- Switch2 --- PC3
PC2 ---/ --- PC4


![Topologi](images/image1.png)  

**Gambar 1.** Topologi Jaringan Static Routing  

---

## ⚙️ 2. Spesifikasi Perangkat  

| Perangkat | Model       | Jumlah | Keterangan                      |
|-----------|-------------|--------|---------------------------------|
| Router    | Cisco 1941  | 2      | Router penghubung antar subnet  |
| Switch    | Cisco 2960  | 2      | Switch distribusi host PC       |
| PC        | PC-PT       | 4      | End-user host                   |
| Kabel     | Straight    | –      | PC ↔ Switch ↔ Router            |
| Kabel     | Cross-Over  | –      | Router ↔ Router                 |
| Kabel     | Serial (opsional) | – | Untuk modul **WIC-2T**         |

---

## 💻 3. Konfigurasi IP Address  

📍 **Subnet 1 – 192.168.1.0/24**  
| Perangkat | IP Address   | Subnet Mask     | Default Gateway |
|-----------|--------------|-----------------|-----------------|
| PC1       | 192.168.1.2  | 255.255.255.0   | 192.168.1.1     |
| PC2       | 192.168.1.3  | 255.255.255.0   | 192.168.1.1     |

![Konfigurasi PC1](images/image2.png)  
**Gambar 2.** Konfigurasi IP Address pada PC1  

![Konfigurasi PC2](images/image3.png)  
**Gambar 3.** Konfigurasi IP Address pada PC2  

📍 **Subnet 2 – 192.168.3.0/24**  
| Perangkat | IP Address   | Subnet Mask     | Default Gateway |
|-----------|--------------|-----------------|-----------------|
| PC3       | 192.168.3.2  | 255.255.255.0   | 192.168.3.1     |
| PC4       | 192.168.3.3  | 255.255.255.0   | 192.168.3.1     |

![Konfigurasi PC3](images/image4.png)  
**Gambar 4.** Konfigurasi IP Address pada PC3  

![Konfigurasi PC4](images/image5.png)  
**Gambar 5.** Konfigurasi IP Address pada PC4  

---

## 🛠️ 4. Konfigurasi Router  

### 📡 Router1  
- **Gi0/0** → ke Switch1 (`192.168.1.1/24`)  
- **Gi0/1** → ke Router2 (`10.10.10.1/30`)  
- **Static Route** → mengenali jaringan `192.168.3.0/24`  

![Router1 Gi0/0](images/image6.png)  
**Gambar 6.** Konfigurasi Router1 Gi0/0  

![Router1 Gi0/1](images/image7.png)  
**Gambar 7.** Konfigurasi Router1 Gi0/1  

```bash
Router> enable
Router# configure terminal
Router(config)# interface gig0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface gig0/1
Router(config-if)# ip address 10.10.10.1 255.255.255.252
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# ip route 192.168.3.0 255.255.255.0 10.10.10.2
Router(config)# end
Router# write memory

📡 Router2

    Gi0/0 → ke Switch2 (192.168.3.1/24)

    Gi0/1 → ke Router1 (10.10.10.2/30)

    Static Route → mengenali jaringan 192.168.1.0/24


Gambar 8. Konfigurasi Router2 Gi0/0


Gambar 9. Konfigurasi Router2 Gi0/1

Router> enable
Router# configure terminal

! Konfigurasi interface Gi0/0
Router(config)# interface gig0/0
Router(config-if)# ip address 192.168.3.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Konfigurasi interface Gi0/1
Router(config)# interface gig0/1
Router(config-if)# ip address 10.10.10.2 255.255.255.252
Router(config-if)# no shutdown
Router(config-if)# exit

! Static Routing menuju jaringan 192.168.1.0/24 via Router1 (10.10.10.1)
Router(config)# ip route 192.168.1.0 255.255.255.0 10.10.10.1

Router(config)# end
Router# write memory

🔍 5. Pengujian Koneksi

✅ PC1 ↔ PC2 (dalam satu subnet)

PC1> ping 192.168.1.3
Reply from 192.168.1.3: bytes=32 time<1ms TTL=128


Gambar 10. Hasil uji ping PC1 ke PC2

✅ PC2 ↔ PC4 (antar subnet, via 2 router)

PC2> ping 192.168.3.3
Reply from 192.168.3.3: bytes=32 time<1ms TTL=128


Gambar 11. Hasil uji ping PC2 ke PC4
📌 7. Kesimpulan

    Jaringan berhasil menghubungkan dua subnet berbeda: 192.168.1.0/24 dan 192.168.3.0/24.

    Konfigurasi IP Address, Gateway, dan Router sudah benar sehingga komunikasi antar host berjalan.

    Static Routing memungkinkan jalur komunikasi ditentukan secara manual, sederhana, dan stabil.

    Uji konektivitas (ping) antar PC menunjukkan hasil Reply sukses.

    Topologi ini memperlihatkan konsep dasar bahwa Router adalah penghubung utama antar jaringan berbeda.

✍️ Author: Dokumentasi Jaringan Cisco – Static Routing Topology Haris Pambudi