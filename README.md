# 🌍 The Planets: Earth – VulnHub Walkthrough

This repository contains a detailed walkthrough for the **"The Planets: Earth"** machine available on [VulnHub](https://www.vulnhub.com/entry/the-planets-earth,755/), originally created by **SirFlash**.

## 🎯 Objective

The goal of this machine is to gain **user** and **root** access by leveraging enumeration, weak encryption, and privilege escalation techniques. This challenge is rated **easy** and is ideal for those looking to build a strong foundation in penetration testing.

## 🛠️ Tools & Techniques Used

- `netdiscover` – to discover the target on the network  
- `nmap` – for port and service enumeration  
- `gobuster` – to brute-force directories  
- `CyberChef` – for decoding XOR-encrypted messages  
- `netcat` – to establish reverse shells and transfer files  
- `ltrace` – to analyze dynamic behavior of binaries  
- Linux privilege escalation fundamentals (e.g., SUID bit analysis)

## 🧠 Skills Demonstrated

- Reconnaissance and network enumeration  
- SSL certificate analysis and subdomain discovery  
- Web enumeration and information disclosure exploitation  
- Custom decryption using XOR and dynamic analysis of binaries  
- SUID exploitation and privilege escalation to root

## 📁 Contents

- `planets_earth_walkthrough.md`: Full step-by-step walkthrough of the machine

## ⚠️ Disclaimer

This walkthrough is intended for **educational purposes only**. The target machine is intentionally vulnerable and should only be used in a legal and controlled environment such as a local VM lab.

> Original VM author: [SirFlash](https://www.vulnhub.com/entry/the-planets-earth,755/)

All analysis and writing are my own.

## 📬 Contact

If you have any questions or suggestions, feel free to open an issue or contact me via GitHub.

