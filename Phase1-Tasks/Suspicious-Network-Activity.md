
#  Suspicious Network Activity Detection

This task is about detecting suspicious network activity, such as port scanning, unusual connection attempts, or odd DNS queries using tools like Wireshark/tcpdump, and correlating this with logs in Wazuh SIEM.

# Objective
Detect reconnaissance activity (e.g., port scan) targeting Ubuntu using:

-> Packet capture tools (tcpdump, Wireshark)

-> Wazuh alerting based on connection attempts

-> Optionally, auditd or firewalld logs

# Tools Used
-> nmap (on Kali)

-> Suricata Intigration For Ntework Log Collection

-> Wazuh with network/connection logging enabled

**Install Suricata on the Ubuntu endpoint. We tested this process with version 6.0.8 and it can take some time:**

    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo apt-get install suricata -y

**Download and extract the Emerging Threats Suricata ruleset:**

    cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
    sudo tar -xvzf emerging.rules.tar.gz && sudo mv rules/*.rules /etc/suricata/rules/
    sudo chmod 640 /etc/suricata/rules/*.rules

Modify Suricata settings in the /etc/suricata/suricata.yaml file and set the following variables:

    HOME_NET: "<UBUNTU_IP>"
    EXTERNAL_NET: "any"
    
    default-rule-path: /etc/suricata/rules
    rule-files:
    - "*.rules"
    
    # Global stats configuration
    stats:
    enabled: Yes
    
    # Linux high speed capture support
    af-packet:
      - interface: eth0
      
**Restart the Suricata service:**

    sudo systemctl restart suricata

 # Step-by-Step Guide
**Step 1: Simulate a Port Scan (on Kali)**
Run a stealth SYN scan against Ubuntu:

    nmap -sS <ubuntu-ip> -p 1-1000

You can also add -T4 for faster scan:

    nmap -sS -T4 <ubuntu-ip> -p 1-1000

This will send SYN packets without completing the TCP handshake — typical reconnaissance.

Watch for many SYN packets from Kali:

**In /var/ossec/etc/ossec.conf:**

    xml
    
    <localfile>
      <log_format>syslog</log_format>
      <location>/var/log/suricata/eve.json</location>
    </localfile>
    
**Restart Wazuh agent:**

    sudo systemctl restart wazuh-agent


# Proof-Of-Concept


![Network-detection](https://github.com/Gagancybersec01/SIEM-Internship-Phase-1/blob/49c31291b07d0d34f7c7ff0817d15c818273c7d5/Screenshots/nmap1.png)

![Network-detection](https://github.com/Gagancybersec01/SIEM-Internship-Phase-1/blob/49c31291b07d0d34f7c7ff0817d15c818273c7d5/Screenshots/nmap2.png)

