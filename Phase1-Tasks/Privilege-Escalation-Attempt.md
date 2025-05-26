# Privilege Escalation Attempt

**Simulate on Ubuntu:**

Try adding user to sudoers:

    sudo useradd escalated
    sudo usermod -aG sudo escalated

Or create SUID shell:

    cp /bin/bash /tmp/rootbash
    chmod +s /tmp/rootbash
________________________________________
🧠 Wazuh rule:

           <rule id="101200" level="15">
               <match>sudo</match>
               <match>usermod</match>
            <description>Possible privilege escalation - user added to sudo</description>
             <mitre>
              <id>T1068</id>
             </mitre>
             </rule>
          <rule id="101201" level="15">
            <match>chmod 4..5</match>
            <description>SUID binary created - privilege escalation</description>
          </rule>


# Proof-Of-Concept 

![Privilege-Attempt]()

![Privilege-Attempt]()
