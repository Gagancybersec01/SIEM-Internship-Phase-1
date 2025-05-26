# Simulate Log Tampering (T1562.003)

**Lab Setup**

**Component	Role**

Ubuntu	Target/victim machine

Kali Linux	Attacker machine

Wazuh	Monitoring system (manager + dashboard)

# Step-by-Step Simulation
**Step 1: Simulate SSH Brute Force from Kali**
Use hydra to simulate brute force from Kali:

    hydra -l ubuntu -P /usr/share/wordlists/rockyou.txt ssh://<ubuntu_ip>

Log Location on Ubuntu: **/var/log/auth.log**

Wazuh will monitor this file for failed SSH attempts.
________________________________________
**Step 2: SSH Access After "Attack"**
After guessing the password (or simulating access), log into Ubuntu from Kali:

    ssh ubuntu@<ubuntu_ip>

This login is also logged in /var/log/auth.log.
________________________________________
**Step 3: Simulate Log Tampering (T1562.002)**

On Ubuntu (as attacker), run any of the following to clear or delete logs:
sudo truncate -s 0 /var/log/auth.log
# or
**sudo rm /var/log/auth.log**
# or
**echo "" | sudo tee /var/log/auth.log**

Each method is a common technique used to hide activity.
________________________________________
**Step 4: Wazuh Detection Configuration**

**1. Enable File Integrity Monitoring (FIM) on Ubuntu**
Check Wazuh agent config (/var/ossec/etc/ossec.conf) and make sure /var/log/ is being monitored:

    <syscheck>
      <directories check_all="yes">/var/log</directories>
    </syscheck>

Restart Wazuh agent:

    sudo systemctl restart wazuh-agent
________________________________________
**2. Add a Custom Rule on Wazuh Manager**

On the Wazuh Manager (not the agent), create a new custom rule in /var/ossec/etc/rules/local_rules.xml:

    <rule id="100200" level="10">
      <if_sid>550</if_sid> <!-- FIM event -->
      <match>/var/log/auth.log</match>
      <description>Possible log tampering: auth.log modified or cleared</description>
      <group>log_tampering, suspicious</group>
    </rule>

Then restart the manager:

    sudo systemctl restart wazuh-manager
________________________________________


# Proof-Of-Concept 

![Log-Tampering](https://github.com/Gagancybersec01/SIEM-Internship-Phase-1/blob/6cacabe3625203b744defefe46b2d06df79b01e1/Screenshots/log-Tampring1.png)

![Log-Tampering](https://github.com/Gagancybersec01/SIEM-Internship-Phase-1/blob/6cacabe3625203b744defefe46b2d06df79b01e1/Screenshots/Log-tamparing2.png)


