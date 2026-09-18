# NetworkWalks-B083-Week-2-Footprinting-Network-Scanning-Final-Report
A hands-on cybersecurity project covering Phase 1 (Passive Footprinting &amp; OSINT) and Phase 2 (Active Network Discovery) on networkwalks.com and a virtualized 10.0.0.0/24 LAN using Kali Linux, Nmap/Zenmap, and DNS enumeration tools

# Final Report 👇:
[Week-2-FootPrinting-NetworkScanning-FinalReport.docx.pdf](https://github.com/user-attachments/files/32381802/Week-2-FootPrinting-NetworkScanning-FinalReport.docx.pdf)

# LinkedIn Post 👇:
https://lnkd.in/p/gSPdxRxU

# Penetration Testing Report — Phase 1 & 2
**Passive Footprinting & Active Network Discovery**

---

## 📌 Executive Summary
This repository contains the documentation and findings for **Phase 1 (Footprinting & Reconnaissance)** and **Phase 2 (Scanning & Host Discovery)** executed as part of the Week 2 Cybersecurity Engineering Internship at **Networkwalks** 

The primary objective was to simulate pre-engagement intelligence-gathering methodologies used by threat actors and penetration testers to uncover technical assets, infrastructure dependencies, software version exposures, and active network nodes

---

## 📄 Project Details
* **Author:** Muhammed Ibrahim Muhammed Hussain
* **Program/Batch:** B083-Networkwalks
* **Date:** 18 August 2026
* **Modules Completed:** W2-PM1 (Multiple Kali Tools) & W2-PM5 (Zenmap Scanning)
* **Target Assets:** 
  * `networkwalks.com` (External Domain)
  * `10.0.0.0/24` (Internal Virtual Subnet)

---

## 🛠️ Assessment Arsenal & Commands

| Tool | Target Scope | Executed Command | Purpose |
| :--- | :--- | :--- | :--- |
| **WHOIS** | `networkwalks.com` | `whois networkwalks.com` | Query domain registration records, owner privacy status, and name servers |
| **WhatWeb** | `networkwalks.com` | `whatweb networkwalks.com` | Web technology stack fingerprinting (CMS, web server, scripts, cookies) |
| **Nslookup** | `networkwalks.com` | `nslookup networkwalks.com` | DNS A-record resolution using default upstream recursive resolvers |
| **Curl** | `networkwalks.com` | `curl -1 https://networkwalks.com` | Inspect raw HTTP/2 response headers, server banners, and session cookies |
| **Wafw00f** | `networkwalks.com` | `wafw00f networkwalks.com` | Detect presence and ruleset implementation of Web Application Firewalls (WAF) |
| **DNSRecon** | `networkwalks.com` | `dnsrecon -d networkwalks.com` | Enumerate SOA, NS, MX, A, TXT, and SRV service records |
| **Nmap / Zenmap** | `10.0.0.0/24` | `nmap -sn 10.0.0.0/24` | Execute active ICMP/Ping sweep discovery on the local subnet |

---

## 🔍 Phase 1: Passive Footprinting Findings
* **WHOIS & Privacy:** Privacy protection is enabled via *Domains By Proxy, LLC* Registrar locks are active (`clientTransferProhibited`) Name servers are split across HostGator and GoDaddy
* **Web Technology Stack:** WordPress version 7.1.1, WordPress Download Manager (v3.3.58), Bootstrap 7.1.1, and jQuery 3.7.1 running on Apache
* **WAF Protection:** Detected **ModSecurity (SpiderLabs)** Web Application Firewall actively inspecting inbound payloads
* **DNS Infrastructure:** Revealed **BIND 9.16.23-RH** version disclosure on HostGator name servers
* **SPF Misconfiguration:** SPF record uses `~all` (SoftFail), leaving the domain moderately vulnerable to email spoofing

---

## 🌐 Phase 2: Active Network Discovery
A ping sweep across the `10.0.0.0/24` subnet discovered two live hosts:

| Host IPv4 | Latency | Hardware MAC Address | Vendor / Architecture | Network Role |
| :--- | :--- | :--- | :--- | :--- |
| `10.0.0.1` | 0.0011s | `52:54:00:12:35:00` | QEMU Virtual NIC | Network Gateway / Hypervisor Interface |
| `10.0.0.2` | N/A (Local) | Unexposed | Virtual Machine | Local Scanning Host (Kali Linux) |

---

## ⚠️ Threat Exposure & Risks Identified
1. **Software Version Disclosure:** Exposing explicit WordPress (v7.1.1) and plugin versions
2. **DNS Daemon Version Exposure:** BIND 9.16.23-RH banner exposure enables targeted CVE searches
3. **Permissive SPF Policy:** `~all` softfail allows email spoofing risks
4. **Exposed REST APIs:** Active `/wp-json/` routes allow user and metadata enumeration
5. **Missing Security Headers:** Missing HSTS, CSP, and X-Frame-Options headers
6. **Unsigned DNSSEC:** Leaves domain resolution vulnerable to DNS cache poisoning

---

## 🛡️ Remediation Recommendations
* Change SPF DNS policy from `~all` to `-all` (HardFail)
* Hide WordPress version tags and suppress Apache server headers (`ServerTokens Prod`)
* Implement essential security headers (`Strict-Transport-Security`, `X-Frame-Options`, `Content-Security-Policy`)
* Restrict unauthenticated public access to `/wp-json/` endpoints
* Enable DNSSEC signing at the domain registrar level

---

> **Disclaimer:** *All operations detailed in this report were performed strictly within authorized scope and on designated laboratory infrastructure for educational and portfolio evaluation purposes*
