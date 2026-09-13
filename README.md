<div align="center">

# ⚡ Sing-Box Config & Subscription Converter
### Universal Protocol to Sing-Box JSON Converter & Rule Engine

[![GitHub Repository](https://img.shields.io/badge/Repository-amiercassanova--21%2Fsingbox--converter-38bdf8?style=for-the-badge&logo=github&logoColor=white)](https://github.com/amiercassanova-21/singbox-converter)
[![Sing-Box Version](https://img.shields.io/badge/Sing--Box-v1.8%2B%20%2F%20v1.9%2B-eab308?style=for-the-badge&logo=sing-box&logoColor=white)](https://sing-box.sagernet.org/)
[![Telegram Contact](https://img.shields.io/badge/Telegram-@kang__rebahan-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/kang_rebahan)
[![WhatsApp Contact](https://img.shields.io/badge/WhatsApp-Chat_Owner-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.link/3yhb8f)

<p align="center">
  <b>Konverter Universal Protokol VLESS, VMess, Trojan, Shadowsocks, Hysteria 2, dan TUIC ke Format JSON Sing-Box Siap Pakai untuk Nekobox, Sing-Box Android/iOS/Windows, SFI, dan SFA.</b>
</p>

---

</div>

## 📌 Tentang Proyek

**Sing-Box Converter** adalah alat konversi konfigurasi dan berlangganan (*subscription*) modern yang dikembangkan untuk mengubah berbagai format link akun proxy (*VLESS, VMess, Trojan, Shadowsocks, Hysteria 2, TUIC*) serta tautan *subscription* (Base64/Clash) menjadi format JSON standar **Sing-Box Core**.

Dengan alat ini, Anda dapat menghasilkan berkas konfigurasi Sing-Box yang sudah teroptimasi dengan aturan *Routing (GeoIP/Geosite)*, *Anti-DNS Leak (FakeIP/DoH)*, serta opsi *TUN / Mixed Inbound* secara otomatis tanpa perlu menyusun file JSON secara manual.

🌐 **GitHub Repository:** [https://github.com/amiercassanova-21/singbox-converter](https://github.com/amiercassanova-21/singbox-converter)

---

## ✨ Fitur Unggulan

- 🚀 **Dukungan Multiproto-kol**:
  - `VLESS` (WS, gRPC, TLS, REALITY, HTTP)
  - `VMess` (WS, gRPC, TCP, TLS)
  - `Trojan` (WS, gRPC, TLS)
  - `Shadowsocks` (2022 / Legacy AEAD)
  - `Hysteria 2` / `TUIC v5`
- 🛡️ **Pencegahan DNS Leak Super Ketat**:
  - Konfigurasi *FakeIP* dan *DoH (DNS over HTTPS)* bawaan (`1.1.1.1`, `8.8.8.8`, `Cloudflare DoH`).
  - Fitur *DNS Hijack* bawaan untuk menangkap seluruh lalu lintas query sistem.
- ⚡ **Opsi Mode Inbound Ganda**:
  - **TUN Mode**: Auto-route sistem penuh untuk perangkat seluler & desktop.
  - **Mixed Mode**: Kombinasi SOCKS5 & HTTP Proxy lokal (Port 2080 / 10808).
- 🔀 **Smart Routing Rules Engine**:
  - Automatic Bypass IP Privat (`geoip:private`) & Local Domain (`geosite:private`).
  - Blokir Otomatis Iklan (`geosite:category-ads-all`) & Torrent (`bittorrent`).
- 📱 **Kompatibilitas Lintas Platform**:
  - Sing-Box Android (SFA) & Sing-Box iOS (SFI)
  - NekoBox / NekoRay
  - Clash Meta / GUI For Sing-Box

---

## 🛰️ Alur Kerja Konversi

```
+------------------------------------+
|  Input Link / Subscription URL     |
| (VLESS / VMess / Trojan / Hy2 / SS)|
+------------------------------------+
                  |
                  v
+------------------------------------+
|     Sing-Box Converter Engine      |
|  - Parse Protocol & Parameters     |
|  - Inject Outbounds, DNS & Route   |
|  - Build Optimized JSON Structure  |
+------------------------------------+
                  |
                  v
+------------------------------------+
|      Sing-Box JSON Output          |
|  (Import to Sing-Box Client / SFA) |
+------------------------------------+
```

---

## 🛠️ Persyaratan Sistem (Prerequisites)

- **Node.js**: v18.0.0 atau lebih baru (jika berbasis Node.js/CLI/Express)
- **Python**: 3.10+ (jika berbasis Python script)
- **Sing-Box Core**: Versi `v1.8.0` / `v1.9.0` ke atas.

---

## 🚀 Cara Instalasi & Penggunaan

### 1. Clone Repository
```bash
git clone https://github.com/amiercassanova-21/singbox-converter.git
cd singbox-converter
```

### 2. Install Dependensi
```bash
# Untuk Node.js / NPM
npm install

# Atau jika menggunakan Python
pip install -r requirements.txt
```

### 3. Jalankan Converter
```bash
# Menjalankan aplikasi web / server konversi
npm start

# Atau menjalankan via CLI
node index.js --input "vless://uuid@example.com:443?type=ws&security=tls#VLESS-Node" -o config.json
```

---

## 📄 Contoh Output JSON Sing-Box

Hasil konversi secara otomatis membentuk struktur JSON Sing-Box standar berikut:

```json
{
  "log": {
    "disabled": false,
    "level": "warn",
    "timestamp": true
  },
  "dns": {
    "servers": [
      {
        "tag": "dns-remote",
        "address": "https://1.1.1.1/dns-query",
        "detour": "proxy"
      },
      {
        "tag": "dns-direct",
        "address": "223.5.5.5",
        "detour": "direct"
      }
    ],
    "rules": [
      {
        "outbound": "any",
        "server": "dns-direct"
      }
    ],
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "tag": "tun-in",
      "interface_name": "tun0",
      "inet4_address": "172.19.0.1/30",
      "auto_route": true,
      "strict_route": true,
      "stack": "mixed"
    }
  ],
  "outbounds": [
    {
      "type": "vless",
      "tag": "proxy",
      "server": "example.com",
      "server_port": 443,
      "uuid": "00000000-0000-0000-0000-000000000000",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "insecure": false
      },
      "transport": {
        "type": "ws",
        "path": "/vless",
        "headers": {
          "Host": "example.com"
        }
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    }
  ],
  "route": {
    "rules": [
      {
        "protocol": "dns",
        "action": "hijack-dns"
      },
      {
        "ip_is_private": true,
        "outbound": "direct"
      },
      {
        "clash_mode": "Direct",
        "outbound": "direct"
      },
      {
        "clash_mode": "Global",
        "outbound": "proxy"
      }
    ],
    "auto_detect_interface": true
  }
}
```

---

## 👤 Pemilik & Pengembang

Dikembangkan dan dipelihara secara independen oleh **amiercassanova**.

- 🐙 **GitHub Repository:** [amiercassanova-21/singbox-converter](https://github.com/amiercassanova-21/singbox-converter)
- 💬 **Telegram:** [@kang_rebahan](https://t.me/kang_rebahan)
- 📱 **WhatsApp:** [Hubungi Pengembang](https://wa.link/3yhb8f)

---

<div align="center">
  <sub>Developed with ❤️ by <b>amiercassanova</b> • Powered by Sing-Box Core Engine</sub>
</div>