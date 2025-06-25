
# 🔐 DNS Over TLS Configuration Guide 🚀

Welcome to the ultimate guide for enabling **DNS over TLS (DoT)** on your system! This guide will walk you through every step to secure your DNS traffic with TLS encryption, ensuring privacy and security for your network.

> 📚 For a complete Linux server setup, check out these all-in-one guides:
- [Ultimate Server Setup: Admin Panel, SSL, Proxy, Security & More 🔗](https://github.com/mertdogan00/ultimate-server-setup)
- [Complete Linux Server Setup Guide 🔗](https://github.com/mertdogan00/server-manual-setup)

---

## 📚 Table of Contents
- [Introduction 🔍](#introduction-)
- [Prerequisites ⚙️](#prerequisites-)
- [Configuration Steps 🛠️](#configuration-steps-)
- [Firewall Setup 🔥](#firewall-setup-)
  - [UFW (Default Firewall) 🚪](#ufw-default-firewall-)
  - [Firewalld (Alternative) 🔒](#firewalld-alternative-)
- [Testing DNS Over TLS ✅](#testing-dns-over-tls-)
- [References 🌐](#references-)
- [Contributing 🤝](#contributing-)
- [License 📜](#license-)

---

## Introduction 🔍

DNS over TLS (DoT) encrypts your DNS queries to prevent eavesdropping and tampering. By default, DNS queries are sent in plaintext, which poses privacy risks. This guide demonstrates how to enable DoT using **systemd-resolved**.

---

## Prerequisites ⚙

Before you begin, ensure:
- You are using a Linux distribution with **systemd-resolved** (e.g., Debian, Ubuntu, Arch).
- Your system is connected to the internet.
- You have root privileges.

---

## Configuration Steps 🛠

1. **Edit systemd-resolved configuration:**
```bash
sudo nano /etc/systemd/resolved.conf
```

2. **Update the configuration file:**
```ini
[Resolve]
DNS=1.1.1.1#cloudflare-dns.com 1.0.0.1#cloudflare-dns.com
FallbackDNS=8.8.8.8#dns.google 1.1.1.1#cloudflare-dns.com
DNSOverTLS=yes
```

3. **Restart systemd-resolved:**
```bash
sudo systemctl restart systemd-resolved
```

4. **Ensure `/etc/resolv.conf` is linked correctly:**
```bash
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

---

## Firewall Setup 🔥

You must ensure **TCP port 853** is open for **outbound traffic** to allow encrypted DNS queries.

### UFW (Default Firewall) 🚪
UFW is the default firewall on most Linux distributions.

1. **Check UFW status:**
```bash
sudo ufw status
```

2. **Allow outbound DNS over TLS traffic:**
```bash
# Allow outbound TCP connections to port 853 (DNS over TLS)
sudo ufw allow out to any port 853 proto tcp comment 'Allow outbound DNS over TLS'
```

3. **Reload UFW:**
```bash
sudo ufw reload
```

---

### Firewalld (Alternative) 🔒
If you are using firewalld instead of UFW:

1. **Check firewalld status:**
```bash
sudo firewall-cmd --state
```

2. **Allow outbound DNS over TLS traffic:**
```bash
# Allow outbound TCP connections to port 853 (DNS over TLS)
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" destination port="853" protocol="tcp" accept'
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv6" destination port="853" protocol="tcp" accept'
```

3. **Reload firewalld:**
```bash
sudo firewall-cmd --reload
```

---

## Testing DNS Over TLS ✅

1. **Check DNS resolver status:**
```bash
resolvectl status
```
✅ Look for: `+DNSOverTLS` in protocols.

2. **Test DNS query via systemd:**
```bash
resolvectl query example.com
```
✅ Check for: `Data was acquired via local or encrypted transport: yes`

3. **(Optional) DNS Leak Test:**
Visit: [https://www.dnsleaktest.com/](https://www.dnsleaktest.com/)

---

## References 🌐

- 👉 [Ultimate Server Setup: Admin Panel, SSL, Proxy, Security & More](https://github.com/mertdogan00/ultimate-server-setup)
- 👉 [Complete Linux Server Setup Guide](https://github.com/mertdogan00/server-manual-setup)

For a detailed and production-grade Linux server configuration, check out the above repositories. They offer step-by-step tutorials for securing and optimizing your server. 🚀

---

## Contributing 🤝

Contributions are welcome! If you find errors or want to improve this guide, feel free to submit a pull request. Let’s make DNS security accessible for everyone! 🚀

---

## License 📜

This project is licensed under the [MIT License](LICENSE). Feel free to use and modify with attribution.

