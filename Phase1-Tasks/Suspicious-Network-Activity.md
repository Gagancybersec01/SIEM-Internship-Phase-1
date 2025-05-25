
#  Suspicious Network Activity Detection

This task is about detecting suspicious network activity, such as port scanning, unusual connection attempts, or odd DNS queries using tools like Wireshark/tcpdump, and correlating this with logs in Wazuh SIEM.

# Objective
Detect reconnaissance activity (e.g., port scan) targeting Ubuntu using:

-> Packet capture tools (tcpdump, Wireshark)

-> Wazuh alerting based on connection attempts

-> Optionally, auditd or firewalld logs

# Tools Used
-> nmap (on Kali)

-> tcpdump / Wireshark (on Ubuntu)

-> Wazuh with network/connection logging enabled

 # Step-by-Step Guide
**Step 1: Simulate a Port Scan (on Kali)**
Run a stealth SYN scan against Ubuntu:

    nmap -sS <ubuntu-ip> -p 1-1000

You can also add -T4 for faster scan:

    nmap -sS -T4 <ubuntu-ip> -p 1-1000

This will send SYN packets without completing the TCP handshake — typical reconnaissance.

**Step 2: Monitor with tcpdump or Wireshark (on Ubuntu)**

Use tcpdump on Ubuntu:

    sudo tcpdump -i eth0 port 1-1000

Watch for many SYN packets from Kali:

Open Wireshark and Apply Filter:

    tcp.flags.syn == 1 and tcp.flags.ack == 0

This will show pure SYN packets (common in -sS scan).

**Step 3: Configure Logging for Detection**

Ensure Ubuntu firewall is logging (for dropped scans)If using ufw:

sudo ufw logging on

Then view logs:

    sudo tail -f /var/log/ufw.log

Make sure /var/log/ufw.log is monitored by Wazuh:

**In /var/ossec/etc/ossec.conf:**

    xml
    
    Copy code
    <localfile>
      <log_format>syslog</log_format>
      <location>/var/log/ufw.log</location>
    </localfile>
    
**Restart Wazuh agent:**

    sudo systemctl restart wazuh-agent

**Step 4: Create Wazuh Alert Rule for Scan Detection**

On the Wazuh Manager, create a custom rule to detect port scans.

    sudo nano /var/ossec/etc/rules/local_rules.xml
    
  **Example Rule for Nmap SYN Scan**
  
    xml
    
    Copy code
    <group name="network,suspicious,portscan,nmap">
      <rule id="100300" level="10">
        <if_sid>5710</if_sid> <!-- base Wazuh rule for connection attempts -->
        <match>UFW BLOCK</match>
        <description>Possible port scan detected (UFW blocked connection)</description>
        <frequency>10</frequency>
        < timeframe>60</timeframe>
        <same_source_ip />
        <mitre>
          <id>T1046</id> <!-- Network Service Scanning -->
        </mitre>
      </rule>
    </group>

After configuration the alert rules we need to restart the wazuh manager. Then we will see all the suspicious network activity logs in the wazuh dashboard. 

# Proof-Of-Concept




