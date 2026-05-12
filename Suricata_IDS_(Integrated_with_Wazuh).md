🚨 Detecting Network Intrusion Using Suricata IDS (Integrated with Wazuh)
=========================================================================

📌 Overview
-----------

This project demonstrates how to detect network intrusions using **Suricata IDS** and integrate its logs with **Wazuh SIEM** for centralized monitoring, alerting, and analysis.

The lab simulates real-world attack detection (e.g., Nmap scan) and visualizes alerts in Wazuh.

🎯 Objectives
-------------

*   Detect network intrusions using Suricata IDS
    
*   Integrate Suricata logs with Wazuh Agent
    
*   Monitor and analyze alerts in real-time via Wazuh Dashboard
    

## 🧪 Lab Environment

| Component        | Details                          |
|------------------|----------------------------------|
| Wazuh-agent      | Ubuntu Linux                    |
| IDS              | Suricata                        |
| SIEM             | Wazuh Manager & Agent           |
| Log File         | /var/log/suricata/eve.json      |
| Attack Machine   | Kali Linux                      |

🛠️ Tools Used
--------------

*   Suricata (IDS/IPS)
    
*   Wazuh (SIEM)
    
*   Nmap (Attack simulation)
    
*   Emerging Threats Ruleset
    

⚙️ Installation & Configuration
-------------------------------

### 🔹 Step 1: Install Suricata

```xml
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata -y 
```

### 🔹 Step 2: Setup Rules Directory
```xml
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 640 /etc/suricata/rules/*.rules
```

### 🔹 Step 3: Configure Suricata

```xml
HOME_NET: "<UBUNTU_IP>"
EXTERNAL_NET: "any"

default-rule-path: /etc/suricata/rules
rule-files:
- "*.rules"

# Global stats configuration
stats:
enabled: Yes

# Linux high-speed capture support
af-packet:
  - interface: eth0
```

### 🔹 Step 4: Restart Suricata
```bash
   sudo systemctl restart suricata 
```

🔗 Wazuh Integration
--------------------

### 🔹 Step 5: Configure Wazuh Agent

open:
```bash
sudo nano /var/ossec/etc/ossec.conf 
```
Add:
```xml
<ossec_config>  
<localfile>    
<log_format>json</log_format>    
<location>/var/log/suricata/eve.json</location>  
</localfile>
</ossec_config>  
```
### 🔹 Step 6: Restart Wazuh Agent
```bash
sudo systemctl restart wazuh-agent
```

⚔️ Attack Simulation
--------------------

From Kali Linux:
```bash
nmap -sS <UBUNTU_IP>
```

This performs a **TCP SYN scan**, which should trigger Suricata alerts.

📊 Monitoring & Detection
-------------------------

1.  Open **Wazuh Dashboard**
    
2.  Navigate to **Discover**
    
4.  Observe alerts: Suricata: ET-Alerts
        
        
<img width="940" height="573" alt="image" src="https://github.com/user-attachments/assets/cd472328-2cf5-45c7-b360-6bac2cfa72d7" />
