<p align="center">
  <img src="https://img.shields.io/badge/OS-Linux%20%7C%20Windows-blue?style=for-the-badge&logo=linux" />
  <img src="https://img.shields.io/badge/Status-Emergency%20Rescue-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" />
</p>

# ⚡ Internet Gittiğinde Hızlı Kurtarma (Linux + Windows)

> Elektrik kesintisi / modem reseti sonrası **IP alamayan cihazları** anında ayağa kaldırmak için hızlı çözüm rehberi.

---

## 💀 Belirtiler

- `destination host unreachable`
- `ip neigh` → `INCOMPLETE`
- `dhclient: command not found`
- Wi-Fi bağlı ama internet yok
- Panel / server erişilemiyor

---

# 🐧 ⚡ 1. ACİL AĞ KURTARMA (LINUX - STATIK IP)

> 💡 `<HEDEF_IP>` → boş IP (örn: `192.168.1.50`)  
> 💡 `enp2s0` → kendi interface’in

```bash
sudo ip addr flush dev enp2s0
sudo ip link set enp2s0 up
sudo ip addr add 192.168.1.<HEDEF_IP>/24 dev enp2s0
sudo ip route add default via 192.168.1.1
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf 
```
🪟 ⚡ 2. ACİL AĞ KURTARMA (WINDOWS - STATIK IP)
-------------------------------------
💡 <INTERFACE> → "Ethernet" veya "Wi-Fi"
💡 <HEDEF_IP> → boş IP (örn: 192.168.1.50)
-------------------------------------
:: 1. Reset (DHCP'e dön)
netsh interface ip set address name="<INTERFACE>" dhcp
netsh interface ip set dns name="<INTERFACE>" dhcp

:: 2. Statik IP ata
netsh interface ip set address name="<INTERFACE>" static 192.168.1.<HEDEF_IP> 255.255.255.0 192.168.1.1

:: 3. DNS ata
netsh interface ip set dns name="<INTERFACE>" static 8.8.8.8
netsh interface ip add dns name="<INTERFACE>" 1.1.1.1 index=2

:: 4. Cache temizle
ipconfig /flushdns

:: 5. Test
ping 192.168.1.1
ping 8.8.8.8
ping google.com
