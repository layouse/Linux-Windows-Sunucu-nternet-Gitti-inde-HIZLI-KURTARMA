<p align="center">
  <img src="https://img.shields.io/badge/OS-Linux%20%7C%20Windows-blue?style=for-the-badge&logo=linux" alt="OS Support">
  <img src="https://img.shields.io/badge/Status-Emergency%20Rescue-red?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License">
</p>

# Linux-Windows-Sunucu İnternet-Gittiginde-HIZLI-KURTARMA
> Bu rehber, elektrik kesintisi / modem reseti sonrası   **sunucu IP alamadığında** direkt kurtarma sağlar.
## 💀 Belirti



- `destination host unreachable`

- `ip neigh` → `INCOMPLETE`

- `dhclient: command not found`

- internet yok


---


### ⚡ 1. Acil Ağ Yapılandırması (Geçici Statik IP) **LINUX**

Ağ arayüzüne elle IP atayarak ağ erişimini anında aktif hale getirmek için aşağıdaki komut zincirini yürütün:

> 💡 **Not:** `<HEDEF_IP>` alanına ağ bloğunuzda boşta olan bir IP adresini (örnek: `192.168.1.50`), `enp2s0` alanına ise ilgili ağ arayüz adınızı (interface) giriniz.

```bash
sudo ip addr flush dev enp2s0
sudo ip link set enp2s0 up
sudo ip addr add 192.168.1.<HEDEF_IP>/24 dev enp2s0
sudo ip route add default via 192.168.1.1
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```
### ⚡ 2.ACİL AĞ KURTARMA (WINDOWS STATIK IP)

:: <INTERFACE> yerine ağ adını yaz (Ethernet / Wi-Fi)
:: <HEDEF_IP> yerine boş IP yaz (örn: 192.168.1.50)

:: 1. Mevcut IP ve DNS temizle (reset gibi düşün)
netsh interface ip set address name="<INTERFACE>" dhcp
netsh interface ip set dns name="<INTERFACE>" dhcp

:: 2. Statik IP ata
netsh interface ip set address name="<INTERFACE>" static 192.168.1.<HEDEF_IP> 255.255.255.0 192.168.1.1

:: 3. DNS ata (internet için şart)
netsh interface ip set dns name="<INTERFACE>" static 8.8.8.8
netsh interface ip add dns name="<INTERFACE>" 1.1.1.1 index=2

:: 4. DNS cache temizle (çökmüş ağları diriltir)
ipconfig /flushdns

:: 5. Test (opsiyonel ama önerilir)
ping 192.168.1.1
ping 8.8.8.8
ping google.com
