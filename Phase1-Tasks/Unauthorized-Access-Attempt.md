# Unauthorized Access Attempt (SSH brute-force)
 **Simulate from Kali:**

**1. Simulate Unauthorized Access from Kali
From Kali (Attacker):**

**Attempt SSH login to Ubuntu using:**

      ssh wronguser@<ubuntu-ip>
      
       
  **2. Expected Log Events in Ubuntu**
  **In /var/log/auth.log,** you’ll see:

**Failed login:**

Failed password for invalid user wronguser from 192.168.1.X port 22 ssh2
✅ Successful login:

Accepted password for ubuntu from 192.168.1.X port 22 ssh2

**3. Monitor & Detect in Wazuh SIEM
Log into the Wazuh Dashboard, then:**

Filter Ubuntu Agent Logs
Go to Security Events

Select your Ubuntu agent

Search for:

authentication

sshd

Failed password

Accepted password

root

invalid user

for i in {1..10}; do ssh invalid@<ubuntu-ip>; done

Ubuntu logs this in /var/log/auth.log.

Ensure it’s monitored in Wazuh:

    xml
    
    <localfile>
      <log_format>syslog</log_format>
      <location>/var/log/auth.log</location>
    </localfile>
________________________________________
**Wazuh rule:**

        xml
        
        <rule id="101000" level="13">
          <match>Failed password</match>
          <frequency>5</frequency>
          <timeframe>60</timeframe>
          <same_source_ip />
          <description>Multiple failed SSH logins - possible brute-force</description>
          <mitre>
            <id>T1110</id>
          </mitre>
        </rule>


# Proof-Of-Concept

![image alt]()





