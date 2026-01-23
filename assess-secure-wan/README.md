# Assessing & Securing Systems on a WAN — Lab Project

[![Lab Status](https://img.shields.io/badge/status-complete-success)](./)
[![Tools: nmap](https://img.shields.io/badge/tools-nmap-blue)](https://nmap.org)
[![Tools: tcpdump](https://img.shields.io/badge/tools-tcpdump-orange)](https://www.tcpdump.org)
[![Tools: ClamWin/AVG](https://img.shields.io/badge/tools-AVs-lightgrey)](#)

**Short summary**  
Hands-on lab demonstrating reconnaissance, malware removal, and host hardening across a small lab WAN. This repo contains commands, findings, verification steps, and evidence screenshots.

---

## Table of contents
1. [Project overview](#project-overview)  
2. [Lab parts (what I did)](#lab-parts-what-i-did)  
3. [Key commands (copy/paste-ready)](#key-commands-copypaste-ready)  
4. [Findings (concise)](#findings-concise)  
5. [Before / after comparison](#before--after--comparison)  
6. [Evidence images (ordered)](#evidence-images-ordered)  
7. [How I verified remediation](#how-i-verified-remediation)  
8. [Recommendations & next steps](#recommendations--next-steps)  
9. [License & contact](#license--contact)

---

## Project overview
Four lab parts performed in an isolated WAN environment:
- Part 1 — Scan the WAN (discovery, OS detection, NSE checks).
- Part 2 — Clean vulnerable systems (ClamWin / AVG).
- Part 3 — Reduce attack surface on Windows 2003 (enable firewall, scope exceptions).
- Part 4 — Reduce attack surface on Windows 2008 R2 (Advanced Security rules, logging).

**Targets (lab examples)**  
- `100.16.16.50` — Windows Server 2003  
- `100.20.9.25` — Windows Server 2008 R2  
- `100.30.10.238` — Debian Linux (scan/monitor host)
- `172.30.0.31` — Windows Server 2012 R2

---

## Lab parts (what I did)
- Reconnaissance with `nmap` (OS detection + NSE scripts).  
- Packet capture on the Debian host using `tcpdump` to capture decoy activity.  
- Cleaned infected systems with ClamWin and AVG; quarantined EICAR/B.O. test files.  
- Hardened Windows hosts: enabled firewall, set scope, logging, ICMP rules, and limited inbound services.

---

## Key commands (copy/paste-ready)

### Discovery & OS detection
```bash
nmap -O -v 100.16.16.50
nmap -O -v 100.20.9.25
nmap -O -v 100.30.10.238
```

### Vulnerability NSE check (MS08-067 example)
```bash
nmap --script=smb-vuln-ms08-067 -p445 100.16.16.50
```

### SSH hostkey enumeration (example)
```bash
nmap --script=ssh-hostkey -p22 100.30.10.238
```

### Packet capture (on Debian monitor host)
```bash
tcpdump -i eth0 -w marilyn_capture.pcap host 100.30.10.238
# analyze with tcpdump -r marilyn_capture.pcap
```

---

## Findings (concise)

**Summary:** During reconnaissance and scanning I found multiple exposed services and several high-risk findings:

- **100.16.16.50 (Windows Server 2003)** — I performed nmpa -O -v and found multiple ports open such as: SMB (445), NetBIOS (139), RPC/LSA (1026), RDP (3389) open. At the command prompt I entered `smb-vuln-ms08-067` to check for SMB vulnerability, specifically the MS08-067 exploit

   ![NSE MS08-067 result](https://github.com/mbergin123/mbergin123/blob/main/images/10.30.10.238.OS.Open.Ports.png)
  
  ![NSE-MS08-067-Vulnerable](https://github.com/mbergin123/mbergin123/blob/main/images/nampScript100.16.16.50P1.png)
  
- **100.20.9.25 (Windows Server 2008 R2)** — SMB (445), NetBIOS (139), multiple ephemeral ports. SMB script output indicated possible service issues prior to firewall hardening.  

  ![100.20 before](https://github.com/mbergin123/mbergin123/blob/main/images/nmap100.20.9.25openPortsP1.2.png)
  
  ![100.20_vulnerable](https://github.com/mbergin123/mbergin123/blob/main/images/nmapScript100.20.9.25Part1.2.png)

- **172.30.0.31 (Windows Server 2012 R2)** — Scanned server for open ports. I foudn 12 open ports. Port 135 could be vulnerable to MS03=026 (overflow attack). Port 139 could be vulnerable to MS03-067 (Remote Code Execution). Port 445 could be vulnerable to MS08-67 ^ MS17-010 (Remote Coed Execution) and (Wannacry).

  ![172.30 nmap](https://github.com/mbergin123/mbergin123/blob/main/images/nmap173.30.0.31.png)
  

- **100.30.10.238 (Debian)** — SSH (22), HTTP (80), telnet (23), rpcbind (111). SSH hostkeys enumerated; tcpdump confirmed decoy IPs during stealth scan.
  ![nmap.100.30](https://github.com/mbergin123/mbergin123/blob/main/images/nmap100.30.10.238Part1.2.png)
  
  ![ssh hostkeys](https://github.com/mbergin123/mbergin123/blob/main/images/ssh-hoskey.3encrypt.footprint.keys.png)
  
  ![tcpdump decoys](https://github.com/mbergin123/mbergin123/blob/main/images/Screenshot%202025-09-23%20213734.png)

---

## Before / after (hardening & malware removal)

### Malware removal evidence
- On remote server 100.20.9.25 ClamWin scan (infections found). The virus found is a live Trojan.BO which would allow a BOclient to control the remote system if they are logged in locally
  ![ClamWin scan](https://github.com/mbergin123/mbergin123/blob/main/images/Screenshot%202025-09-21%20104329.png)

- ClamWin removal confirmation & logs.  
  ![ClamWin removed files](https://github.com/mbergin123/mbergin123/blob/main/images/clamwinsvirusremoved.png)

- On remote server 172.30.0.31 AVGnfor Business quarantine confirmation (evidence: EICAR test files moved to quarantine). AVG was not orginally turned on, after careful configuration the AVG anti virus was turned on.
  ![AVG quarantine](https://github.com/mbergin123/mbergin123/blob/main/images/avgTurnedOn.png)  
  ![AVG dashboard](https://github.com/mbergin123/mbergin123/blob/main/images/avg_malware_removed.png)

### Firewall hardening evidence
- On remote server 100.16.16.50 Firewall initially off / unprotected (before).  

- Firewall turned on, scoped exceptions added, logging enabled.  
  ![Firewall on](https://github.com/mbergin123/mbergin123/blob/main/images/firewall_windows_turned_on.png)  
  ![Firewall scope](https://github.com/mbergin123/mbergin123/blob/main/images/firewall_scope.png)  
  ![Firewall logging](https://github.com/mbergin123/mbergin123/blob/main/images/firewall_changed_log_settings.png)  
  ![ICMP rule: echo request limited](https://github.com/mbergin123/mbergin123/blob/main/images/firewall_configured_echo_request_limit.png)

- Export of rules / documented inbound rules (evidence).  
  ![Exported rules screenshot](https://github.com/mbergin123/mbergin123/blob/main/images/Screenshot%202025-09-24%20152615.png)

---

## How I verified remediation
- Follow-up `nmap` scans showed required ports filtered or limited after hardening.
- For the remote server 100.16.16.50 I wanted to make sure only port 3389 was open
  ![100.16 post-hardening scan](https://github.com/mbergin123/mbergin123/blob/main/images/Screenshot%202025-09-24%20151913.png)
- For the remote server 100.20.9.25 I wanted to make sure only port 3389 was open
  ![100.20 post-hardening scan](https://github.com/mbergin123/mbergin123/blob/main/images/Screenshot%202025-09-23%20211946.png)

- For servers where full hardening was not required in the scope of the lab (100.30.10.238 and 172.30.0.31), remediation consisted of malware detection and removal only. I verified remediation by:

- Running antivirus scans (ClamWin / AVG) and capturing results showing infected files detected and moved to quarantine (screenshots and ClamWin/AVG logs included in the images/ folder).

- Re-scanning the host with nmap where appropriate to verify that services matched expectations after removal (e.g. ports/services remained the same when no firewall changes were requested).

- Hashing or noting filenames and paths of quarantined files and including the AV log output in the evidence folder.

- Where hardening was performed (Windows Server 2003 and 2008 targets) I documented firewall rule changes, ICMP scope restrictions, and logged verification scans showing reduced attack surface. For hosts without hardening, the primary verification required is AV removal + re-scan and retention of logs/screenshots as evidence.


---


## Recommendations & next steps
- Apply vendor patches (where available) or isolate legacy systems (e.g., Windows 2003).  
- Implement centralized AV and update signature sets.  
- Use host-based intrusion detection and scheduled scans.  
- Replace or isolate obsolete services (Telnet, rpcbind) and use least-privilege firewall rules.

---

## License
This repository is a lab demo for assessment & remediation techniques — not production configuration. This lab was produced by Jones & Barlett Learning.




