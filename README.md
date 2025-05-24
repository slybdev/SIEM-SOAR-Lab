# 🛡️ SIEM & SOAR Lab: Wazuh + TheHive + Shuffle.io Detection Pipeline

A hands-on cybersecurity project that simulates a real-world detection and response environment using **Wazuh** as the SIEM, **TheHive** for incident response, **Shuffle.io** for SOAR automation, and **pfSense** for network segmentation — all integrated to detect malicious tools like **Mimikatz** and respond automatically.

---

## 🎯 Objective

To build and simulate a full-stack SOC environment that detects endpoint threats (like credential dumping with Mimikatz) and automates incident handling using integrated SIEM, SOAR, and IR platforms.

---

## 🧠 Skills Learned

- SIEM & SOAR Integration  
- Wazuh Agent Deployment & Rule Customization  
- Sysmon Logging with Olaf’s Configuration  
- pfSense Network Segmentation  
- TheHive Incident Handling  
- Shuffle.io Workflow Automation  
- Threat Intelligence via VirusTotal API  
- Creating Custom Detection Rules (MITRE Mapped)

---

## 🛠️ Tools Used

- [Wazuh](https://wazuh.com/)  
- [TheHive](https://thehive-project.org/)  
- [Shuffle.io](https://shuffler.io/)  
- [pfSense](https://www.pfsense.org/)  
- Sysmon + [Olaf Config](https://github.com/olafhartong/sysmon-modular)  
- [VirusTotal](https://www.virustotal.com)  
- PowerShell  
- DigitalOcean (Cloud Hosting)

---

## 🔬 Lab Walkthrough

### 1. 🏗️ Environment Setup

- **Windows 10 endpoint** with Sysmon configured via Olaf’s config:
  ```bash
  sysmon.exe -i sysmonolaf.xml
pfSense Firewall installed with:

Adapter 1: NAT

Adapter 2: Internal Network

Configured WAN/LAN IPs, DNS, and gateway routing

![Screenshot SOAR 10](https://github.com/user-attachments/assets/75664d67-0ff5-4c0d-8bae-ed19ffcee8d9)
![Screenshot SOAR 12](https://github.com/user-attachments/assets/52ed571f-3d65-4b27-afd5-9ef942df9d29)
![Screenshot SOAR 16](https://github.com/user-attachments/assets/e13504a2-b635-4bcb-bf42-e14afe3e560f)
![Screenshot SOAR 1](https://github.com/user-attachments/assets/2becbbc8-9a6c-485e-972f-c3cfbe2023ab)

![Screenshot SOAR 35](https://github.com/user-attachments/assets/9ba99624-4609-4f0c-8fb0-10282c8b4430)
![Screenshot SOAR 43](https://github.com/user-attachments/assets/5bf210dc-9857-4155-8f41-7d7be9dca07a)

2. ☁️ Cloud Deployment (Wazuh + TheHive)
A. TheHive Server

Deployed on Ubuntu via Docker

Installed Java (Corretto 11), Cassandra, Elasticsearch

Installed TheHive:

bash
Copy
Edit
apt install thehive
Web UI: http://<VM-IP>:9000
Login: admin@thehive.local / secret

B. Wazuh Server

Deployed following official docs

Configured dashboard & agent enrollment

Enabled archive logs to retain full event details

![Screenshot SOAR 18](https://github.com/user-attachments/assets/ec65f346-00b2-4dae-8a2e-063c6e0ba841)
![Screenshot SOAR 20](https://github.com/user-attachments/assets/099e0ae4-fdeb-4a07-9a51-7e24b3098886)

![Screenshot SOAR 8](https://github.com/user-attachments/assets/3a947869-7c5c-435d-a156-926e5dde4f60)
![Screenshot 2025-05-22 122014](https://github.com/user-attachments/assets/3e34f062-83a5-4403-ae23-5d5b6c56c977)


3. ⚙️ Wazuh Agent (Windows 10)
Installed and registered agent

Collected Sysmon logs via ossec.conf

Verified alert generation and log flow

4. 🧪 Attack Simulation: Mimikatz
Disabled Defender

Executed Mimikatz manually

Wazuh initially missed detection

Solution:

Tuned ossec.conf to collect full logs

Enabled archive indexing

Detection success confirmed:

plaintext
Copy
Edit
OriginalFileName: mimikatz.exe
![Screenshot SOAR 38](https://github.com/user-attachments/assets/49d7cfa5-3a72-4e67-b4fa-5b2d2d623324)

![Screenshot 2025-05-21 115011](https://github.com/user-attachments/assets/995978ca-25ff-4d70-9a7a-5d3c332cd1d8)
![Screenshot 2025-05-21 123323](https://github.com/user-attachments/assets/150eb4f3-5e8b-48fa-8b90-7360acccbb94)


5. 🛡️ Custom Rule Creation
To detect scripting interpreters:

xml
Copy
Edit
<rule id="92000" level="4">
  <if_group>sysmon_event1</if_group>
  <field name="win.eventdata.OriginalFileName" type="pcre2">(?i)\\(c|w)script\.exe</field>
  <options>no_full_log</options>
  <description>Scripting interpreter spawned a new process</description>
  <mitre>
    <id>T1059.005</id>
  </mitre>
</rule>
✅ Tested by renaming Mimikatz — still detected.

6. 🔁 SOAR Integration: Shuffle.io
Signed up at shuffler.io

Configured alert forwarding in ossec.conf

Built workflow to:

Receive alert from Wazuh

Enrich with VirusTotal

Create case in TheHive

Notify security team via email

📸 Screenshots
![Screenshot 2025-05-21 144926](https://github.com/user-attachments/assets/593cbcbc-0824-48c6-8360-1088665155d5)
![Screenshot SOAR 41](https://github.com/user-attachments/assets/85246037-9524-4f32-a1a5-77380fb28889)
![Screenshot SOAR 42](https://github.com/user-attachments/assets/a5670792-d6b7-4801-ac1a-6ec07ae866f3)

![Screenshot 2025-05-22 121818](https://github.com/user-attachments/assets/1d33518f-9518-421e-9571-2276d4def505)

🔐 Key Findings
SIEM requires tuning to detect stealthy malware

Archive logs give visibility into advanced attacker tools

Sysmon + Wazuh is a powerful detection stack

TheHive enhances analyst collaboration

Shuffle.io adds automated response with minimal setup

MITRE ATT&CK mapping brings structured threat visibility



🗣️ Conclusion

This lab demonstrates how to build a functional SOC pipeline using free and open-source tools. From endpoint telemetry to automated enrichment and collaborative case management, every layer plays a vital role in defending against real-world threats like Mimikatz.

💬 Feedback
Got ideas or feedback?
Let’s connect on LinkedIn or open an issue!
