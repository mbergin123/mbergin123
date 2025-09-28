# Assessing & Securing Systems on a WAN — Lab Project

[![Lab Status](https://img.shields.io/badge/status-complete-success)](./)
[![Tools: nmap](https://img.shields.io/badge/tools-nmap-blue)](https://nmap.org)
[![Tools: tcpdump](https://img.shields.io/badge/tools-tcpdump-orange)](https://www.tcpdump.org)
[![Tools: ClamWin/AVG](https://img.shields.io/badge/tools-AVs-lightgrey)](#)
[![License](https://img.shields.io/badge/license-lab--demo-lightgrey)](#license--contact)

---

## Short summary
Hands-on lab demonstrating reconnaissance, malware removal, and host hardening across a small isolated WAN. Tools used: `nmap` (discovery + NSE scripts), `tcpdump` (pcap capture), ClamWin / AVG (malware removal), and Windows Firewall / `netsh` (host hardening). This repository contains the commands I ran, concise findings, verification steps, and evidence artifacts (screenshots & pcaps) showing before/after results.

---

## Table of contents
1. [Project overview](#project-overview)  
2. [Lab parts (what I did)](#lab-parts-what-i-did)  
3. [Key commands (copy/paste-ready)](#key-commands-copypaste-ready)  
4. [Findings (concise)](#findings-concise)  
5. [Before / after comparison (how I showed hardening)](#before--after-comparison-how-i-showed-hardening)  
6. [Evidence layout (what I saved)](#evidence-layout-what-i-saved)  
7. [How I verified remediation](#how-i-verified-remediation)  
8. [Recommendations & next steps](#recommendations--next-steps)  
9. [Skills demonstrated](#skills-demonstrated)  
10. [How to reproduce (notes & safety)](#how-to-reproduce-notes--safety)  
11. [License & contact](#license--contact)

---

## Project overview
This project documents four lab parts performed in an isolated WAN environment:

- **Part 1 — Scan the WAN:** discovery, OS detection, NSE checks, and decoy scans.  
- **Part 2 — Clean vulnerable systems:** use ClamWin and AVG to detect and remove malware (EICAR/BO test files).  
- **Part 3 — Reduce the attack surface on Windows 2003:** enable Windows Firewall, restrict exception scope to specific IPs, configure logging, create ICMP rules, verify via `nmap`.  
- **Part 4 — Reduce the attack surface on Windows 2008:** Windows Firewall with Advanced Security — disable unnecessary inbound rules, create scoped rules (RDP + ICMP), verify.

**Targets used (lab examples)**  
- `100.16.16.50` — Windows Server 2003  
- `100.20.9.25` — Windows Server 2008 R2  
- `100.30.10.238` — Debian Linux (scanner/monitor host)

---

## Lab parts (what I did)
- Reconnaissance & scanning using `nmap` (OS detection & NSE scripts).  
- Packet capture on the Debian host using `tcpdump` to capture decoy activity during stealth scans.  
- Detected and removed test trojans using ClamWin and AVG (EICAR / B.O. test artifacts).  
- Hardened Windows hosts by enabling Windows Firewall across Domain/Private/Public profiles, restricting exception scopes to allowed IPs, creating ICMP inbound rules, enabling firewall logging, and leaving only required inbound services (RDP).  
- Verified changes with follow-up `nmap` scans and pcap review.

---

## Key commands (copy/paste-ready)

### Discovery & OS detection
```bash
# discovery + OS detection (verbose)
nmap -O -v 100.16.16.50
nmap -O -v 100.20.9.25
nmap -O -v 100.30.10.238
```

## Findings (concise)

**Summary:**  
During reconnaissance and vulnerability scanning I discovered multiple exposed services and several high-risk findings across the lab hosts:

- **100.16.16.50 (Windows Server 2003)** — SMB (445), NetBIOS (139), RPC/LSA (1026), RDP (3389) open. NSE script `smb-vuln-ms08-067` reported **VULNERABLE** (CVE-2008-4250).  
  Screenshot: ![nmap ms08 result](images/namp100.16.16.50.P1.png)

- **100.20.9.25 (Windows Server 2008 R2)** — SMB (445), NetBIOS (139), multiple ephemeral ports open prior to firewall changes; after hardening only RDP remained reachable from allowed IPs.  
  Before screenshot: `images/nmap_2008_before.png`  
  After screenshot: `images/nmap_2008_after.png`

- **100.30.10.238 (Debian Linux - monitoring host)** — SSH (22), HTTP (80), telnet (23), rpcbind (111). SSH hostkeys enumerated using the `ssh-hostkey` NSE script; tcpdump capture confirmed decoy IPs appearing in the SYN stream during a stealth scan.  
  SSH hostkeys screenshot: `images/ssh_hostkeys.png`  
  tcpdump capture: `evidence/marilyn_capture_s2.pcap` (packet capture)

---

## Before / after comparison (how to show)

**What to show:** include two nmap outputs (or screenshots) per target — one from **before** remediation and one **after** remediation — and then a short bullet explanation of why the results changed.

**Example (for 100.20.9.25):**
- Before: many ephemeral ports + SMB exposed (screenshot `images/nmap_2008_before.png`).
- After: firewall enabled for Domain/Private/Public profiles, exception rules narrowed to specific IPs, ICMP allowed only via a named inbound rule — result: only port 3389 visible from the scanner IP (screenshot `images/nmap_2008_after.png`).
- Possible reasons for differences:
  - Windows Firewall was previously off or permissive → now set to Block (inbound) with explicit allowed rules.
  - Scoped rules reduced the attack surface (exceptions limited to allowed IPs).
  - Logging & service changes (stopping unnecessary services) removed listening sockets.

---

## Evidence layout (what I saved in the repo)

Place these files in the indicated folders so reviewers can quickly inspect evidence:



