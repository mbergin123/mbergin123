  🛡️ Mythic & Kali Linux RDP Brute Force Attack Simulation
This project simulates a real-world Red Team engagement using Kali Linux, Hydra, and the Mythic C2 framework to gain and maintain access to a Windows Server hosted on Vultr. It demonstrates offensive security techniques including brute-force attacks, payload deployment, and command-and-control (C2) operations.

---

🖥️ Lab Environment
  * Attacker Machine: Kali Linux

  * Target Machine: Windows Server on Vultr

  * C2 Platform: Mythic

  * Tools Used:

  * Mythic C2

  * Hydra (for RDP brute force)

  * Windows PowerShell

  * MITRE ATT&CK framework

  * RDP & Windows command line utilities

---

🔧 Phase 1: Server Setup
Deployed a Windows Server using Vultr cloud hosting

Enabled RDP access and configured basic firewall rules


![Mythic Vultr Instance](https://github.com/mbergin123/mbergin123/raw/main/images/image1.png)



---

⚙️ Phase 2: Mythic Installation
Installed Mythic on the server using PowerShell and Git

![Mythic Installed](https://github.com/mbergin123/mbergin123/raw/main/images/image5.png)

Set up the Mythic GUI and verified connectivity

![Mythic GUI](https://github.com/mbergin123/mbergin123/raw/main/images/image6.png)

Reviewed MITRE ATT&CK mappings within the Mythic platform

![MITRE ATT&CK](https://github.com/mbergin123/mbergin123/raw/main/images/image7.png)

---

🛠️ Phase 3: Pre-Attack Configuration
  * Created a fake sensitive file on the Windows Server: passwords.txt

Prepared the environment for brute-force attack simulation

![Kali Linux](https://github.com/mbergin123/mbergin123/raw/main/images/image9.png)

---

🚨 Phase 4: RDP Brute Force Attack
Used Hydra on Kali Linux with a default wordlist to brute-force RDP credentials

![List of default passwords](https://github.com/mbergin123/mbergin123/raw/main/images/image10.png)

Successfully gained access to the Windows Server via RDP

![Hydra](https://github.com/mbergin123/mbergin123/raw/main/images/image11.png)

Executed initial commands:

whoami

ipconfig

net user, net group

Disabled Windows Defender to ensure payload execution

![Disabled Windows Defender](https://github.com/mbergin123/mbergin123/raw/main/images/image14.png)

---


💣 Phase 5: Payload Deployment & C2 Connection
Created and configured a Mythic agent and C2 profile

![Agent & C2](https://github.com/mbergin123/mbergin123/raw/main/images/image16.png)

Generated a custom payload in Mythic

![Payload](https://github.com/mbergin123/mbergin123/raw/main/images/image17.png)

Transferred the payload to the target server over RDP

Got a successful callback from the agent to the Mythic GUI

![Payload](https://github.com/mbergin123/mbergin123/raw/main/images/image19.png)

Ran remote commands on the victim machine from the Mythic interface

![GUI Command](https://github.com/mbergin123/mbergin123/raw/main/images/command.png)

Retrieved the fake sensitive file as proof of access

![File Captured](https://github.com/mbergin123/mbergin123/raw/main/images/filecapture.png)

---

✅ Project Takeaways
🔧 Built & configured a vulnerable Windows Server in the cloud

🧠 Demonstrated Red Team techniques including:

  * Brute-force credential attacks

  * C2 framework configuration

  * Payload generation and execution

  * Windows post-exploitation and defense evasion

🎯 Simulated real-world attacker behavior in a safe, controlled lab
