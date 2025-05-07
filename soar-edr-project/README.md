# SOAR EDR Project

This project demonstrates an automated SOAR-EDR integration using VMware, LimaCharlie, Tines, and Slack for real-time detection and response.

## Project Summary
- Configured a Windows machine using VMware with LimaCharlie as the EDR.
- Connected LimaCharlie to Tines for automated incident response.
- Detected LaZagne malware on the Windows machine.
- Generated alerts via Slack and email.
- Provided a user prompt to decide whether to isolate the machine.
- Automated isolation via LimaCharlie upon user confirmation.

## Technologies Used
- VMware
- Windows VM
- LimaCharlie EDR
- Tines Automation
- Slack for notifications

## Workflow Overview
1. Detection of LaZagne malware on Windows VM.
2. Tines playbook triggers an alert via Slack and email.
3. User decides whether to isolate the machine.
4. If yes, LimaCharlie isolates the machine and notifies the user.
5. If no, a notification is sent indicating no isolation.

## Project Files
- [SOAR EDR Project Presentation](link_to_pptx) (add the link after uploading)
