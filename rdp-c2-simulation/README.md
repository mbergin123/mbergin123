# 🔐 RDP C2 Simulation

This project demonstrates a simulated Red Team scenario in which I used multiple tools to gain access to a vulnerable Windows server, establish command & control using Mythic, and create ELK dashboards and alerts for detection and response.

## 🧠 Project Overview

- Deployed a Windows Server on Vultr
- Brute-forced RDP using Hydra from Kali Linux
- Used Mythic C2 to establish access and run commands
- Captured a mock "password" file
- Configured ELK (Elasticsearch, Logstash, Kibana) to monitor events and generate alerts

---

## ⚒️ Tools Used

| Tool          | Purpose                         |
|---------------|---------------------------------|
| Kali Linux (https://www.kali.org/)   | Offensive security workstation  |
| Hydra(https://github.com/vanhauser-thc/thc-hydra)        | Brute force RDP credentials     |
| Mythic C2(https://github.com/its-a-feature/Mythic)     | Command and control             |
| ELK Stack(https://www.elastic.co/what-is/elk-stack)     | Log ingestion, visualization    |
| Vultr(https://www.vultr.com/)         | Cloud hosting                   |

---

## 📸 Screenshots & Walkthrough

### 🔑 Brute-forcing RDP with Hydra
![Hydra Command](https://github.com/mbergin123/mbergin123/raw/main/images/image1.png)
![Kali-Linux](https://github.com/mbergin123/mbergin123/raw/main/images/image16.png)
With Kali_Linux, Hydra was used to target RDP on the vulnerable server. Upon successful login, credentials were obtained.

---

### 👁️ Access with Mythic C2
![Mythic Execution](https://github.com/mbergin123/mbergin123/raw/main/images/image2.png)
![Mythic Execution 2](https://github.com/mbergin123/mbergin123/raw/main/images/image17.png)

Using Mythic C2, I connected to the server and ran commands to exfiltrate a test "password" file. I also used Apollo as my Agent

---

### 📊 ELK Dashboard
![Elastic Dashboard](https://github.com/mbergin123/mbergin123/raw/main/images/dashboard.copy.png)



Kibana dashboards visualized RDP brute-force events and command activity. My RDP is open so it will show brute-force activity not just from me but from anyone.

---

## 🎯 Learning Objectives

- Practice using real-world offensive tools in a safe, controlled environment
- Build detection capability with ELK Stack
- Understand how C2 works and how to monitor for it

---

## 🚀 Future Improvements

- Automate alerting with Elastalert
- Integrate TheHive for case management
