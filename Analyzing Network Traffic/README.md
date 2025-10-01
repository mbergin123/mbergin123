# Analyzing Network Traffic

Use this section as we go. I’ll append each step and screenshot you send so everything stays in order for the final README.

---

## Intake — Lab Materials

### 📖 Introduction

* Corporate networks remain vulnerable to internal and external threats despite firewalls and security appliances.
* Network professionals use protocol capture/analyzer tools to troubleshoot, enforce policies, and detect rogue activity.
* Establishing a **baseline definition** helps identify anomalies, malware, or unusual traffic.
* Baseline creation is an ongoing process, not a one-time task.
* This lab focuses on capturing and analyzing network traffic using tools like **TCPdump, Wireshark, and NetWitness Investigator**.


### 🎯 Learning Objectives

1. Capture live network traffic with Wireshark and TCPdump.
2. Analyze packet capture data in NetWitness Investigator.
3. Use Wireshark statistics to identify baseline definitions.
4. Identify common network protocols (HTTP, Telnet, FTP, TFTP, SSH) in packet captures.
5. Discuss how baseline definitions are created and maintained.


### 📋 Lab Overview

**Section 1:** Guided exercises

1. Capture traffic with TCPdump.
2. Use Wireshark to capture & analyze TCP/IP.
3. Use Tftpd64 and FileZilla for file transfer traffic.
4. Apply Wireshark filters.
5. View capture in NetWitness Investigator.

**Section 2:** Apply knowledge with less guidance, expanded tasks, `grep` with TCPdump files, and NetWitness Request/Response view.

**Section 3:** Independent exploration of the virtual environment, answering questions and simulating real-world scenarios.

**Screenshots provided:**

* `lab_overview_1.png`
* `lab_overview_2.png`
* `lab_overview_3.png`

### 🗺️ Topology

Virtual lab environment includes:

* vWorkstation (Windows Server 2016)
* TargetWindows02 (Windows Server 2016)
* DVWA (Debian Linux)
* Cisco IOS Emulator devices:

  * Router
  * Norfolk 2811
  * Tampa 2811
  * LAN Switch 1
  * LAN Switch 2

**Screenshot provided:** `topology.png`

### 🧰 Tools & Software

* Damn Vulnerable Web Application (DVWA)
* FileZilla
* NetWitness Investigator
* PuTTY
* TCPdump
* Tftpd64
* Wireshark

**Screenshot provided:** `tools_software.png`

---

## Activity Log (chronological)

|  # | Timestamp  | Tool                                    | Action / Command                                                    | Key Settings / Filters          | Expected Result                                   | Observations                                                                                                                                                    | Screenshot                                                            |                  |               |                    |                                                            |                      |
| -: | ---------- | --------------------------------------- | ------------------------------------------------------------------- | ------------------------------- | ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- | ---------------- | ------------- | ------------------ | ---------------------------------------------------------- | -------------------- |
|  1 | 2025-10-01 | PuTTY (DVWA 172.30.0.13)                | Opened session; `man tcpdump`                                       | —                               | Review usage/options                              | Connected to DVWA; tcpdump available                                                                                                                            | FirstPutty.png                                                        |                  |               |                    |                                                            |                      |
|  2 | 2025-10-01 | PuTTY (DVWA)                            | Reviewed tcpdump manual output                                      | —                               | Understand capture/interface/file flags           | Noted `-i`, `-w`, `-c`, filters                                                                                                                                 | ResultsOfFirstPutty.png                                               |                  |               |                    |                                                            |                      |
|  3 | 2025-10-01 | PuTTY (DVWA)                            | `cd /etc/network && ls && cat interfaces`                           | —                               | Show available interfaces                         | `eth0` static 172.30.0.13; `eth1` dhcp                                                                                                                          | ResultsOfCatInter.png                                                 |                  |               |                    |                                                            |                      |
|  4 | 2025-10-01 | tcpdump                                 | `tcpdump -i eth0 -n -w tcpdumpcapturefile`                          | `eth0`, no name‑res, write file | Start baseline capture                            | Listening on eth0 (EN10MB), 262144B                                                                                                                             | tcpdumpCapStart.png                                                   |                  |               |                    |                                                            |                      |
|  5 | 2025-10-01 | Browser → DVWA                          | Open DVWA homepage                                                  | HTTP                            | Generate normal web traffic                       | GET/200 OK to 172.30.0.13                                                                                                                                       | DVWAStart.png                                                         |                  |               |                    |                                                            |                      |
|  6 | 2025-10-01 | Browser → DVWA Security                 | Set security level **low**                                          | HTTP POST `/security.php`       | Adjust env for tests                              | Setting applied                                                                                                                                                 | LevelLow.png                                                          |                  |               |                    |                                                            |                      |
|  7 | 2025-10-01 | Browser → DVWA (XSS reflected)          | Submit name `Simon`                                                 | HTTP                            | Create reflected request/response                 | Page shows "Hello Simon"                                                                                                                                        | XXSReflected.png                                                      |                  |               |                    |                                                            |                      |
|  8 | 2025-10-01 | Browser → DVWA (XSS reflected)          | Submit `<script>alert('this is a vulnerability');</script>`         | HTTP                            | Demonstrate reflected XSS                         | JS alert executed                                                                                                                                               | ThisIsVul.png                                                         |                  |               |                    |                                                            |                      |
|  9 | 2025-10-01 | PuTTY (DVWA)                            | Stop capture; `tcpdump -n -r tcpdumpcapturefile                     | less`                           | read‑only                                         | Verify contents                                                                                                                                                 | 172.30.0.2 ↔ 172.30.0.13, port 80; GET `/DVWA/login.php`, CSS, images | ResultsOfCap.png |               |                    |                                                            |                      |
| 10 | 2025-10-01 | PuTTY (DVWA)                            | Search payload: `tcpdump -A -s0 -r tcpdumpcapturefile 'tcp port 80' | grep -i -n -E '(<script         | </script>                                         | %3Cscript                                                                                                                                                       | alert(                                                                | alert%28)'`      | ASCII payload | Locate XSS request | Found encoded `%3Cscript%3Ealert%28...` with Referer lines | AlertScriptFound.png |
| 11 | 2025-10-01 | Wireshark (172.30.0.10)                 | Start GUI capture on `vmxnet3`                                      | live capture                    | Begin analysis in Wireshark                       | TCP/UDP flows 172.30.0.10 ↔ 172.30.0.2 visible                                                                                                                  | WSstart.png                                                           |                  |               |                    |                                                            |                      |
| 12 | 2025-10-01 | Console (LanSwitch1 172.16.8.5)         | `show interface`                                                    | —                               | Interface status/counters                         | Vlan1 admin‑down; no I/O; ARPA; no errors                                                                                                                       | showinter.png                                                         |                  |               |                    |                                                            |                      |
| 13 | 2025-10-01 | Console (LanSwitch1 172.16.8.5)         | `show vlan`                                                         | —                               | VLAN config / ports                               | VLANs 1,100,200,300,400,500 active; Fa0/* mapped                                                                                                                | showvlan.png                                                          |                  |               |                    |                                                            |                      |
| 14 | 2025-10-01 | Console (LanSwitch2 172.16.20.5)        | `show interface`, `show vlan`                                       | —                               | Verify counters/VLANs                             | Commands executed; results consistent (no screenshot)                                                                                                           | —                                                                     |                  |               |                    |                                                            |                      |
| 15 | 2025-10-01 | Console (Tampa 2811 172.17.8.1)         | `show interface`, `show vlan`                                       | —                               | Verify router interfaces/VLANs                    | Commands executed successfully; results observed (no screenshot)                                                                                                | —                                                                     |                  |               |                    |                                                            |                      |
| 16 | 2025-10-01 | PuTTY (172.16.8.1 via SSH)              | `show interface`                                                    | —                               | Display interface details                         | FastEthernet0/0 is up; IP 172.16.8.1/24; Full-duplex, 100Mb/s; 0 errors; 98 input packets logged                                                                | showint2.png                                                          |                  |               |                    |                                                            |                      |
| 17 | 2025-10-01 | PuTTY (DVWA 172.30.0.13)                | SSH login as `student`                                              | —                               | Test SSH connectivity                             | Logged into DVWA server successfully and then exited session                                                                                                    | DVWAputtyssh.png                                                      |                  |               |                    |                                                            |                      |
| 18 | 2025-10-01 | FileZilla (client)                      | Opened FileZilla dashboard                                          | —                               | Prepare for FTP/SFTP transfer tasks               | Local pane shows C:\ISSA_TOOLS\Documentation; remote site not yet connected                                                                                     | FileZillaDash.png                                                     |                  |               |                    |                                                            |                      |
| 19 | 2025-10-01 | FileZilla (client → server 172.30.0.10) | Uploaded AnyConnect_adminguide.pdf                                  | FTP (student@172.30.0.10)       | Verify file transfer to remote server             | Transfer successful; 4,826,866 bytes uploaded and confirmed in remote directory listing                                                                         | FileUpload.png                                                        |                  |               |                    |                                                            |                      |
| 20 | 2025-10-01 | Server 172.30.0.10 (desktop)            | Verified file present on server desktop                             | —                               | Confirm artifact landed on destination            | `AnyConnect_adminguide.pdf` visible on desktop; upload complete                                                                                                 | loadedonServer.png                                                    |                  |               |                    |                                                            |                      |
| 21 | 2025-10-01 | Tftpd64 (server 172.30.0.10)            | Launched Tftpd64 dashboard                                          | —                               | Prepare for TFTP transfer                         | Dashboard running with current directory set to `C:\Program Files\Tftpd64`; interface bound to 172.30.0.10                                                      | tftp64Dash.png                                                        |                  |               |                    |                                                            |                      |
| 22 | 2025-10-01 | Tftpd64 (172.30.0.10)                   | TFTP **Put** PDF from `172.30.0.2` → `172.30.0.10`                  | Port 69                         | Verify TFTP file transfer                         | Success dialog: `9428 blocks transferred in 1 second`, 0 retransmits; MD5 `6553637110e8e53f55634d2b7c8e870`                                                     | tftp64Transfer.png                                                    |                  |               |                    |                                                            |                      |
| 23 | 2025-10-01 | Tftpd64 (172.30.0.2)                    | Verified received file on server                                    | —                               | Confirm presence of transferred file              | `AnyConnect_adminguide.pdf` listed in Tftpd64 directory viewer on 172.30.0.2                                                                                    | tftp64Proof.png                                                       |                  |               |                    |                                                            |                      |
| 24 | 2025-10-01 | Wireshark (172.30.0.10)                 | Stopped capture session                                             | —                               | Save pcap for analysis                            | Capture shows ARP, TCP, Telnet, and other traffic between 172.16.8.5, 172.16.8.99, and broadcasts; ready for filtering and statistics review                    | WiresharkStop.png                                                     |                  |               |                    |                                                            |                      |
| 25 | 2025-10-01 | Wireshark (172.30.0.10)                 | Viewed Protocol Hierarchy Statistics                                | —                               | Analyze distribution of traffic types             | Results: IPv4 dominant; UDP (84.5% packets, 56.1% bytes), TCP (15.4% packets, 31.9% bytes). Within TCP: Telnet (1.5%), SSH (0.8%), FTP Data (8.2%), FTP (0.2%). | ProtocolH.png                                                         |                  |               |                    |                                                            |                      |
| 26 | 2025-10-01 | Wireshark (172.30.0.10)                 | Viewed Packet Length Statistics                                     | —                               | Assess distribution of packet sizes               | Results: majority packets 40–79 bytes (48%), and 320–639 bytes (42%); some larger 1280–1514 bytes (8.2%).                                                       | PacketL.png                                                           |                  |               |                    |                                                            |                      |
| 27 | 2025-10-01 | NetWitness Investigator (172.30.0.10)   | Opened NetWitness Investigator dashboard                            | —                               | Begin analysis of pcap file                       | Dashboard active with Demo Collection loaded; ready to import capture for analysis                                                                              | NetWisDash.png                                                        |                  |               |                    |                                                            |                      |
| 28 | 2025-10-01 | NetWitness Investigator (172.30.0.10)   | Uploaded Wireshark packet capture into Investigator                 | —                               | Review traffic by service, IPs, events            | Analysis shows service types (SSH, FTP, TFTP, NETBIOS), source/destination IPs, action events (`put`, `login`), and user account `student`; PDF file detected.  | NetWissPacket.png                                                     |                  |               |                    |                                                            |                      |
| 29 | 2025-10-01 | NetWitness Investigator (172.30.0.10)   | Viewed FTP details in Investigator                                  | —                               | Extract filename and credentials from FTP session | Found `anyconnect_adminguide.pdf` as filename and password `p@ssw0rd!` associated with the FTP transfer.                                                        | Filename&Pass.png                                                     |                  |               |                    |                                                            |                      |
| 17 | 2025-10-01 | PuTTY (DVWA 172.30.0.13)                | SSH login as `student`                                              | —                               | Test SSH connectivity                             | Logged into DVWA server successfully and then exited session                                                                                                    | DVWAputtyssh.png                                                      |                  |               |                    |                                                            |                      |

---

## README.md (GitHub Draft)

### 🧭 Analyzing Network Traffic to Create a Baseline Definition

**Course/Lab:** Jones & Bartlett Learning — *Analyzing Network Traffic to Create a Baseline Definition*
**Author:** Marilyn Bergin

### 📌 Overview

This project demonstrates capturing, transferring, and analyzing network traffic to establish a **network baseline**. The lab simulates a real-world enterprise environment with servers, routers, switches, and vulnerable applications. Using tools such as **TCPdump, Wireshark, FileZilla, Tftpd64, and NetWitness Investigator**, I captured normal and malicious traffic, analyzed protocol distributions, and verified file transfers across the environment.

Employers reviewing this project will see a full end-to-end demonstration of practical skills in:

* Packet capture and command-line analysis
* Using Wireshark for deep inspection
* File transfers with FTP/TFTP and validation
* Detecting vulnerabilities (e.g., XSS in DVWA)
* Using NetWitness Investigator for forensic investigation

This project highlights my ability to not only use security tools, but also to interpret results, validate findings, and document them in a professional manner.

### 🎯 Learning Objectives

* Capture live traffic with TCPdump and Wireshark
* Identify baseline network protocols and normal traffic flows
* Generate traffic with FTP, TFTP, SSH, and Telnet
* Detect insecure applications (DVWA XSS reflected attack)
* Transfer and verify files with FileZilla and Tftpd64
* Use NetWitness Investigator for forensic packet analysis

### 🗺️ Topology

Lab environment included:

* **Servers/Hosts:** DVWA (Debian Linux), vWorkstation (Windows Server 2016), Target Windows 2016
* **Network Devices:** Cisco Routers (Norfolk 2811, Tampa 2811), Cisco Switches (LAN Switch1 & Switch2)
* **Applications/Tools:** TCPdump, Wireshark, PuTTY, FileZilla, Tftpd64, NetWitness Investigator

(*Screenshot: topology.png*)

### 🧰 Tools & Software

* **Capture/Analysis:** TCPdump, Wireshark, NetWitness Investigator
* **File Transfer:** FileZilla (FTP), Tftpd64 (TFTP)
* **Access/Connectivity:** PuTTY (SSH/Telnet)
* **Target Application:** Damn Vulnerable Web App (DVWA)

(*Screenshot: tools_software.png*)

---

### 🔹 Section 1 — Capturing and Analyzing Network Traffic

#### Step 1: Launch PuTTY on DVWA server & confirm TCPdump

Connected to **172.30.0.13** and confirmed `tcpdump` availability using `man tcpdump`.
(*Screenshot: FirstPutty.png, ResultsOfFirstPutty.png*)

#### Step 2: Identify Interfaces

Inspected `/etc/network/interfaces` to confirm **eth0** (static 172.30.0.13) was the capture interface.
(*Screenshot: ResultsOfCatInter.png*)

#### Step 3: Start Capture

Ran `tcpdump -i eth0 -n -w tcpdumpcapturefile` to capture packets directly to file.
(*Screenshot: tcpdumpCapStart.png*)

#### Step 4: Generate HTTP Traffic via DVWA

Accessed DVWA homepage and modules to create baseline HTTP traffic.

* Opened homepage
* Set DVWA security level to *low*
* Tested XSS reflected input with name *Simon*
* Confirmed XSS vulnerability with `<script>alert('this is a vulnerability');</script>`
  (*Screenshots: DVWAStart.png, LevelLow.png, XXSReflected.png, ThisIsVul.png*)

#### Step 5: Validate Capture Results

Stopped capture and verified with `tcpdump -n -r tcpdumpcapturefile`. Observed HTTP GET/200 OK requests between **172.30.0.2** and **172.30.0.13**.
(*Screenshot: ResultsOfCap.png*)

#### Step 6: Locate XSS Payload in Capture

Filtered capture with grep to locate encoded `<script>` payload within HTTP stream.
(*Screenshot: AlertScriptFound.png*)

---

### 🔹 Section 2 — Wireshark Analysis

#### Step 7: Launch Wireshark on Remote Server (172.30.0.10)

Captured live traffic (TCP, UDP, Telnet, SSH, ARP).
(*Screenshot: WSstart.png*)

#### Step 8: Switch & Router Checks

Ran `show interface` and `show vlan` on **LanSwitch1 (172.16.8.5)**, **LanSwitch2 (172.16.20.5)**, and **Tampa 2811 (172.17.8.1)** to verify interface status and VLAN mapping.
(*Screenshots: showinter.png, showvlan.png*)

#### Step 9: Additional SSH Checks

Ran `show interface` on **172.16.8.1**, confirming FastEthernet0/0 up and configured.
(*Screenshot: showint2.png*)

#### Step 10: SSH into DVWA Server

Logged in as `student` to **172.30.0.13** and confirmed session access.
(*Screenshot: DVWAputtyssh.png*)

#### Step 11: Protocol Hierarchy

Analyzed protocol distribution: 84% UDP, 15% TCP; breakdown included Telnet, SSH, FTP, TFTP.
(*Screenshot: ProtocolH.png*)

#### Step 12: Packet Length Statistics

Observed most packets in **40–79 bytes (48%)** and **320–639 bytes (42%)** range.
(*Screenshot: PacketL.png*)

---

### 🔹 Section 3 — File Transfers (FTP & TFTP)

#### Step 13: FileZilla Transfers

* Opened FileZilla dashboard
* Uploaded `AnyConnect_adminguide.pdf` to **172.30.0.10**
* Verified file present on remote desktop
  (*Screenshots: FileZillaDash.png, FileUpload.png, loadedonServer.png*)

#### Step 14: Tftpd64 Transfers

* Launched Tftpd64 dashboard
* PUT `AnyConnect_adminguide.pdf` from **172.30.0.2 → 172.30.0.10** (9428 blocks transferred)
* Verified file present on **172.30.0.2** via directory listing
  (*Screenshots: tftp64Dash.png, tftp64Transfer.png, tftp64Proof.png*)

---

### 🔹 Section 4 — NetWitness Investigator

#### Step 15: Launch NetWitness Investigator

Opened dashboard on **172.30.0.10**.
(*Screenshot: NetWisDash.png*)

#### Step 16: Import Wireshark Capture

Uploaded saved `.pcap` file into Investigator, creating a collection view of services and IPs.
(*Screenshot: NetWissPacket.png*)

#### Step 17: Deep Dive on FTP Evidence

Located FTP details revealing:

* Filename: `anyconnect_adminguide.pdf`
* Password used: `p@ssw0rd!`
  (*Screenshot: Filename&Pass.png*)

---

### ✅ Key Results & Takeaways

* **Baseline traffic**: Normal patterns included HTTP web browsing, ARP broadcasts, Telnet/SSH sessions, and file transfers.
* **Vulnerability identified**: DVWA’s reflected XSS allowed arbitrary script execution; confirmed in both browser and captured traffic.
* **File transfer validation**: Demonstrated FTP and TFTP transfers, confirmed by presence on remote servers.
* **Forensic investigation**: NetWitness highlighted service use, user accounts, filenames, and clear-text credentials.

### 🚀 Why This Matters for Employers

This project shows I can move beyond running tools — I:

* Captured and validated real network traffic
* Reproduced vulnerable traffic for testing
* Used **multiple analysis tools** to corroborate findings
* Documented results in a professional, reproducible way

Employers can trust that I understand not just **how** to use these tools, but also **why** they matter in network security and incident response.

---

### 📸 Screenshots Placement Guide

* **Topology/Setup:** topology.png, tools_software.png
* **TCPdump/DVWA:** FirstPutty.png → ThisIsVul.png
* **tcpdump validation:** ResultsOfCap.png, AlertScriptFound.png
* **Wireshark:** WSstart.png, ProtocolH.png, PacketL.png
* **Switch/Router commands:** showinter.png, showvlan.png, showint2.png
* **DVWA SSH:** DVWAputtyssh.png
* **FileZilla:** FileZillaDash.png, FileUpload.png, loadedonServer.png
* **Tftpd64:** tftp64Dash.png, tftp64Transfer.png, tftp64Proof.png
* **NetWitness:** NetWisDash.png, NetWissPacket.png, Filename&Pass.png

---

**End of Section 1 Project Compilation**

