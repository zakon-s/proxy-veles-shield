# 🛡️ Veles Proxy<img width="146" height="146" alt="1842" src="https://github.com/user-attachments/assets/edd1538c-da31-46e3-8195-c8a190dc26aa" />


> **An intelligent centralized routing and traffic protection platform for routers running OpenWrt / ImmortalWrt.**  
> Seamlessly bypasses censorship and network restrictions across your entire home network: Smart TVs, streaming boxes, smartphones, and PCs work out of the box without installing VPN clients on each device.

---

[![OpenWrt](https://img.shields.io/badge/OpenWrt-23.05%20%7C%2024.10%20%7C%2025.12-blue?logo=openwrt)](https://openwrt.org)
[![ImmortalWrt](https://img.shields.io/badge/ImmortalWrt-Supported-brightgreen)](https://immortalwrt.org)
[![Sing-box](https://img.shields.io/badge/Core-sing--box%20%2F%20hiddify-orange)](https://github.com/SagerNet/sing-box)
[![Zapret 2](https://img.shields.io/badge/DPI-Zapret%202%20%2B%20ByeDPI-red)](#)
[![Package Manager](https://img.shields.io/badge/pkg-opkg%20%26%20apk-lightgrey)](#)
[![License](https://img.shields.io/badge/License-GPL--2.0-blue.svg)](LICENSE)

---

## 💡 Why Veles Proxy?

Instead of manually configuring and turning VPN connections on and off on every mobile device, laptop, or Android TV, **Veles Proxy** manages network filtering directly at the router gateway:

* **Smart Split-Tunneling:** Local services, domestic banking apps, government portals, and local video platforms route directly through your ISP at full line speed.
* **Automated Censorship Bypass:** Blocked websites, restricted services, and AI platforms are transparently directed through an encrypted tunnel.
* **VPS Bandwidth Optimization:** High-bandwidth media streams (such as YouTube 4K and Discord voice/video) leverage integrated on-router DPI evasion techniques (Zapret 2 / ByeDPI) without overloading your remote VPS bandwidth.

---

## ⚡ Quick Installation

Connect to your router via **SSH** (using PuTTY, Termius, or standard terminal `ssh root@192.168.1.1`) and run:

```bash
wget -qO- https://raw.githubusercontent.com/zakon-s/proxy-veles-shield/master/install.sh | sh
```

### What the installer does:
1. Automatically detects CPU architecture (`aarch64`, `mips`, `x86_64`) and package manager format (`opkg` or `apk`).
2. Installs the **Veles Proxy** LuCI web interface with localization support.
3. Downloads, verifies, and configures the chosen routing core.
4. Optionally installs local DPI circumvention modules (**Zapret 2** + **ByeDPI**).
5. Pre-configures smart split-tunnel routing rules.

---

## ⚙️ Core Comparison

The installer allows you to choose between two cores based on your router's available storage:

| Feature | `hiddify-core` (Default) | `sing-box-extended` |
| :--- | :--- | :--- |
| **Disk footprint** | ~15–20 MB (Compact) | ~75–80 MB (Full) |
| **Recommended storage** | Routers with 32–64 MB flash | Routers with 128+ MB / U-Boot mod |
| **Supported protocols** | VLESS (Reality), VMess, Trojan, ShadowTLS, Hysteria 2, TUIC | VLESS, VMess, Trojan, ShadowTLS, Hysteria 2, TUIC + **AmneziaWG**, **WARP** |
| **RAM usage** | Minimal | Standard |

---

## 🚀 Initial Configuration

1. **Access Web Interface:**  
   Open your browser at `http://192.168.1.1` ➔ navigate to **Services** ➔ **Veles Proxy**.

2. **Add Node Subscriptions:**  
   * Go to **Node Settings** ➔ **Subscriptions**.
   * Paste your subscription link (VLESS / Sing-box / Base64 format).
   * Click **Save & Apply**, then click **Update Subscriptions**.

3. **Select Mode & Server:**  
   * Under the **Client Settings** tab, set **Main Node** to a specific location or select the **`URLTest`** group (automatically routes through the server with the lowest latency).
   * Switch the service toggle to **Enabled** and apply changes.

---

## 🛠️ Architecture & Traffic Flow

```text
[ Client Device: TV / Mobile / PC ]
                 │
                 ▼
          [ Veles Proxy ]
                 │
       ┌─────────┴─────────┐
       ▼                   ▼
(Domestic Domain?)   (Blocked Domain?)
       │                   │
       ▼                   ▼
 [ Direct ISP Out ]  [ Veles Tunnel ] ──► (VPS: DE / NL / US)
  (Banks, Streaming)  (VLESS Reality)
```

* **Re:filter Routing Engine:** Direct routing by rule-sets (domestic regional categories bypass the tunnel entirely, while restricted targets are proxied).
* **Zapret 2 Integration:** Uses the system-level `nfqws` queue handler to modify packet signatures, bypassing ISP throttling and DPI inspection for YouTube and Discord without consuming VPS server resources.

---

## 📋 Useful CLI Commands

Check service status:
```bash
/etc/init.d/homeproxy status
```

Restart service:
```bash
/etc/init.d/homeproxy restart
```

Inspect flash storage (`/overlay` partition):
```bash
df -h /overlay
```

View live routing and runtime logs:
```bash
logread -e homeproxy
```

Clean temporary package cache:
```bash
apk cache clean 2>/dev/null || rm -rf /tmp/opkg-lists/*
```

---

## 📌 System Requirements

* **Supported Hardware:** MediaTek Filogic (MT7981, MT7986, MT7988), Qualcomm, Rockchip, x86_64 architectures.
* **Firmware:** OpenWrt 23.05, 24.10, 25.12-SNAPSHOT, or modern ImmortalWrt builds.
* **Storage (`/overlay` partition):**
  * **25+ MB** free space for `hiddify-core`.
  * **90+ MB** free space for `sing-box-extended`.
