## 🚀 SOAR EDR Project

### 🔍 Overview:

This project showcases the integration of **LimaCharlie**, **Tines**, **Slack**, and **VMware** to create a real-time SOAR (Security Orchestration, Automation, and Response) workflow. It leverages LimaCharlie for endpoint detection and response (EDR), Tines for automation and decision-making, and Slack for alert notifications.

---

### 🛠️ Technology Stack:

* **VMware**: Virtualized Windows environment
* **LimaCharlie**: EDR for detection and response
* **Tines**: SOAR platform for automated response
* **Slack**: Real-time communication for alerting
* **Email Alerts**: Automated email notifications

---

### 🔄 Project Workflow:

1. **🖥️ Setup Windows Machine in VMware**

   * Installed LimaCharlie agent on the Windows VM.
   * Established successful communication with LimaCharlie.

2. **💾 Deploy LaZagne Malware Simulation**

   * Downloaded and executed LaZagne on the Windows machine.
   * LimaCharlie successfully detected the new process created by LaZagne.

3. **🔐 Detection and Response Rules in LimaCharlie**

   * Created a custom detection rule to identify LaZagne process activities.
   * Tested and verified detection in real-time.

4. **🤝 Tines Integration and Playbook Creation**

   * Integrated LimaCharlie with Tines for automated workflows.
   * Developed a playbook to:

     * Send alerts to Slack and Email when a detection occurs.
     * Prompt the user if they would like to isolate the machine.

5. **🛡️ User Isolation Decision**

   * If the user selects **No**, a Slack and Email notification is sent indicating that the machine is not isolated.
   * If the user selects **Yes**, Tines sends an isolation request to LimaCharlie, successfully isolating the machine.

---

### 📸 Project Screenshots:

(Screenshots from the project slides will be added here for visual reference)

1. 🖥️ Windows Machine Setup
2. 🔎 LimaCharlie Detection Rules
3. ⚙️ Tines Playbook Setup
4. 🔔 Slack Alert Notification
5. ❓ User Prompt for Isolation
6. 🔒 Confirmation of Isolation in LimaCharlie

---

### ✅ Key Takeaways:

* Seamless integration of SOAR and EDR technologies.
* Real-time detection and automated response capabilities.
* Multi-platform communication through Slack and Email.
* Enhanced decision-making with user prompts for machine isolation.

---

### 🔄 Future Improvements:

* Expanding the playbook for broader threat detection.
* Integrating additional automation for response enrichment.
* Implementing periodic scans and reporting for improved visibility.

---

### 📁 Repository Structure:

```
├── SOAR_EDR_Project
│   ├── README.md
│   ├── VMware_Setup.md
│   ├── LimaCharlie_Detection.md
│   ├── Tines_Playbook.md
│   ├── Slack_Integration.md
│   ├── Email_Alerts.md
│   └── Screenshots/
```

---

### ⚙️ Installation Steps:

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/SOAR_EDR_Project.git
   ```

2. Navigate to the project directory:

   ```bash
   cd SOAR_EDR_Project
   ```

3. Follow the documentation in each `.md` file for specific configurations and setup.

---

### 📸 Screenshots:

(Attach your images to the `Screenshots/` folder in your repository and reference them here)

---

### ✅ Next Steps:

* [ ] Add your screenshots to the `Screenshots/` folder.
* [ ] Update the README with direct links to each screenshot.
* [ ] Final review before pushing to GitHub.

