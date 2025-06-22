🚀 SOAR EDR Project
This project demonstrates a full SOAR (Security Orchestration, Automation, and Response) pipeline using LimaCharlie for detection, Tines for automation and playbooks, Slack for alerts, and VMware to simulate the environment. The goal: automate detection, response, and machine isolation decisions.

---

🔍 Overview
This simulation uses:

  * A Windows VM as the target system.

  * LaZagne malware to generate detectable events.

  * LimaCharlie to detect those events.

  * Tines to automate the workflow.

  * Slack & Email to notify users and capture decisions.

---

🧰 Technology Stack
Component	Purpose
  * VMware	Windows test environment
  * LimaCharlie	EDR agent and rule engine
  * Tines	SOAR platform and playbooks
  * Slack	Alert notifications
  * Email	Secondary alert method

---

🔄 Project Workflow

1. 💻 Setup Windows Machine in VMware

  * Installed LimaCharlie agent on Windows VM
  * Verified telemetry and communication

![LimaCharlie Agent Installed](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image2.png)

---

2. 🧪 Deploy LaZagne Malware Simulation

Downloaded & ran LaZagne using PowerShell

![Downloaded LaZagne](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image5.png)

LimaCharlie captured "new process" telemetry

![New Process](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image6.png)

---

3. 🔐 Create & Test Custom Detection Rule in LimaCharlie

Started from a template

![Template](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image7.png)


Modified to match LaZagne behavior

![New Rule](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image8.png)
![Action Part of RUle](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image9.png)


Successfully triggered on re-execution

![Re-execution Triggered](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image11.png)
![Re-execution Triggered](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image12.png)

---

4. 🤝 Connect LimaCharlie to Tines

Tines receives events from LimaCharlie

![Tines Recievees Event](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image14.png)

Detection triggers start playbooks

---

5. 💬 Integrate Slack for Notifications

Successfully sent test and event alerts

![Slack notification](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image21.png)
![Slack Email](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image20.png)


Slack displays detection + decision prompt

![Decision Prompt](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image22.png)


---

6. 📋 Build Full Tines Playbook

  * Slack & email alerts

  * Slack user prompt: “Isolate this machine?”

  * If-else decision branch

![Playbook](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image25.png)

---

7. 🔄 User Decision: Do Not Isolate

Prompt returned “No”

Tines branch sent Slack message:

“This computer was not isolated”

![Descision on Isolate or NOt](https://github.com/mbergin123/mbergin123/raw/main/soar-edr-project/imageSOAR/image23.png)

---

📌 Key Takeaways
  * Built end-to-end SOAR workflow from scratch

  * Created and tested custom detection rules

  * Automated Slack & email alerts

  * Included user-driven isolation response

  * Successfully executed a live system isolation


