# Project Context: Stealth Bridge (Canada-to-Iran Hardware Tunnel)

## 📌 Project Overview
This project establishes a highly resilient, hardware-to-hardware anti-censorship tunnel designed to bypass state-level Deep Packet Inspection (DPI), active firewall probing, and protocol throttling in a highly restricted network environment (Iran), routing back to an open internet gateway (Canada). 

The core implementation strategy shifts 100% of the cryptographic masking and encapsulation to an embedded edge router, providing a completely zero-configuration, plug-and-play Wi-Fi network for the end client.

---

## 🏗️ Architectural Topology

```
[ Client Devices (Phone/Laptop) ]
│
▼ (Local Secure Wi-Fi Broadcast)
┌────────────────────────────────────────┐
│  IRAN NODE: GL.iNet OpenWrt Router     │
│  - Captures Layer 2/3 traffic          │
│  - Wraps payload in VLESS-Reality      │
└──────────────────┬─────────────────────┘
                   │
                   ▼ (Camouflaged TLS 1.3 Stream via Port 443)
[ State Firewall / DPI ] ───► (Probes dropped/mirrored to legitimate SNI)
                   │
                   ▼ (Global Internet)
┌────────────────────────────────────────┐
│  CANADA NODE: Residential Linux Server  │
│  - Retains Private Keys / Validates ID │
│  - Decrypts proxy packets              │
└──────────────────┬─────────────────────┘
                   │
                   ▼
          [ Unrestricted Web ]
```

---

## 🛠️ The Hardware Stack
*   **Canada Node (Gateway Server):** [Insert your hardware here, e.g., Raspberry Pi 4 / Spare Ubuntu x86 PC] hardwired via Ethernet to residential router.
*   **Iran Node (Edge Client):** GL.iNet Travel Router running an OpenWrt-derived kernel with native hardware crypto-acceleration.
*   **Evasion Protocol:** VLESS over XTLS-Reality (Xray Core v1.8+).

---

## ⚙️ Baseline Engineering Specs

### 1. Network Gateway Layer
*   **Inbound Channel:** Port `443` (TCP) forwarded via residential NAT to the internal gateway server.
*   **Asymmetric Tracking:** Dynamic DNS daemon deployed via Cron to sync WAN IP shifts to a fixed domain (`[Your-DDNS-Hostname].duckdns.org`).

### 2. Camouflage Configuration
*   **Protocol Core:** VLESS (Stateless, signature-free application proxy layer).
*   **Security Layer:** REALITY (Identity-stitching masquerade framework).
*   **SNI Target spoofed:** `www.microsoft.com` / `www.apple.com` (TLS 1.3 handshake verification target).
*   **Active Probing Defense:** Unauthorized scanner probes hitting the listening port automatically trigger an internal redirect loop, serving the genuine destination certificates of the spoofed corporate entity back to the firewall auditors.

### 3. Edge Router Execution (OpenWrt)
*   **WAN Ingress (WWAN):** Virtualized radio interface acting as a Station client to pull data from local Iranian Wi-Fi.
*   **LAN Egress (AP):** Simultaneous private Wi-Fi access point broadcasting a secure SSID.
*   **Core Proxy Client:** Deployed `passwall` / `mihomo` packages intercepting the localized network bridge and transparently forcing global proxy rules.

---

## 🎯 Current Objective / Next Engineering Task
I have mapped out the theoretical blueprint, core protocols, and hardware requirements. 

**Right now, I need help with:** 
*   [ ] Writing the specific configuration files (`config.json` for Xray Core).
*   [ ] Walking through the OpenWrt LuCI package setup steps.
*   [ ] Hardening the Linux firewall rules on the home server.
*   [ ] Configuring the Tor Snowflake fallback routine.

*(Delete or update the checklist items above based on what you want Claude to generate next!)*
