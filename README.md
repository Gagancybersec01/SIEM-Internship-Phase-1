# SIEM-Internship-Phase-1

**Objective**: Build your own lab environment, understand log sources, and implements basic detection use cases using real tools. 

**Phase Goals**

•	Set up a personal cybersecurity lab (your virtual SOC).
•	Configure log generation, forwarding, and collection. 
•	Detect and respond to basic brute-force and login anomalies and some other 

**Step 1: Setup Our Lab Environment**

1. **Attacking Machine:** Install VMware with kali linux the kali is the attack machine
2. **Target Machine:** Ubuntu is Target Machine for logs collection
3. **SIEM:** Wazuh For Monitors logs, file integrity, rootkits, and system calls.
   Detects malware, unauthorized access, and suspicious behavior.
5. **Monitoring Add-Ons:** AuditD and NXLog Tools

-> We Download and install All Machines and Tools. Then Configure The Wazuh and Add Endpoint is Ubuntu With Monirotoing Tools AuditD and NXLog. Here we detailed Explaind in **SIEM-LAB-SETUP File** This File You Can Find the **Phase1-Tasks Folder** that How To Install, Steup and Configure the SIEM Lab Setup. Go Through The File and Setup Lab. 

**What Is A SIEM and Why it Matters?**

SIEM stands for Security Information and Event Management. It refers to a set of technologies and practices used by organizations to monitor, detect, analyze, and respond to security events across their IT infrastructure. A SIEM system provides a centralized platform that collects, normalizes, and analyzes data from various sources to help security teams detect malicious activity, ensure compliance, and improve overall security posture.
-> You Can Find a Detailed Documentation in the **Phase1-Tasks** Folder the file is **What-Is-SIEM** in this file explained detailed what is siem and why it is matters. 

_________________________________________________________________________________________________

# Scenarios PHASE-1 TASKS

**1. Brute Force Detection**

**2. Malware Detection**

**3. Suspicious Network Activity**

**4. Unauthorized Access Attempt**

**5. Privilege Escalation Attempt**

**6. Latral Movement Detection**

**7. Coomannd And Control (C2)**

**8. Simulate Log Tampering**

# Reflection & Evaluation Quesions 

**✅ 1. What is the role of SIEM in Modern Cybersecurity**

**A SIEM (Security Information and Event Management) system helps organizations:**

-> Collect security data from across the network (logs, events).

-> Analyze that data to detect suspicious or malicious activity.

-> Alert security teams about threats in real time.

-> Store logs for investigations and compliance.

**✅ 2. What challenges did you face while setting up your lab**

   I am facing so many problems while build our SIEM lab. The first and foremost is system requirements because we build our SIEM lab with windows VM the windows is lagging issues that's why I am not able to do tasks then we change our machine that ubuntu is out target machine and kali linux is our attack machine and then the wazuh is our logs monitoring SIEM Solution. I am successfully steup lab that we move to our real-world scenarios/Tasks we are facing error that is configuration error. Then we solve the configuration errors and successfully completed all Tasks in the phase 1. 

**✅ 3. What is the difference between Sysmon logs and Windows Security logs?**

Windows Security logs track user activity like logins, logoffs, and access to resources.
Sysmon logs give detailed system-level activity, like process creation, network connections, and file changes — very useful for detecting advanced threats.

**✅ 4. How does a brute-force attack appear in logs? Mention specific Event ID.**

A brute-force attack shows as many failed login attempts. In Windows logs, you’ll see Event ID 4625 (Failed Logon) repeated many times for the same account.

**✅ 5. How would you detect a login outside normal business hours?**

By monitoring Event ID 4624 (Successful Logon) and checking the timestamp, we can compare it with normal working hours (e.g., 9 AM – 5 PM) to detect unusual logins.

**✅ 6. Describe how RDP lateral movement is tracked in event logs?**

You can track RDP use with Event ID 4624 (Logon) with Logon Type 10, which means a remote interactive session.
Also watch for Event ID 1149 in the TerminalServices-RemoteConnectionManager log — it shows successful RDP connection.

**✅ 7. What is the risk of log tampering and how can we detect it?**

   Log tampering hides attacker activity and makes investigation hard.
We can detect it by using File Integrity Monitoring (FIM) tools like Wazuh or Tripwire, which alert if log files are deleted, cleared, or changed unexpectedly.

**✅ 8. What improvements would you make in you lab setup if give more time**

   As of now We Build a decent and solid SIEM Lab and We take so many days to build this SIEM lab because of we face some issues and error and some other reasons. As of now we build a solid SIEM lab. 

**✅ 9. How will this phase help you in real-world interviews or jobs**

   Phase 1 provided me with practical, hands-on experience that closely mirrors the responsibilities of a real-world SOC analyst or cybersecurity engineer. By setting up a SIEM (Wazuh), simulating real cyberattacks, and analyzing logs for threats like brute force, malware, and privilege escalation, I learned how to detect and investigate security incidents end-to-end. This not only strengthened my technical skills in Linux, log analysis, and security tooling but also prepared me to confidently discuss detection strategies, threat patterns, and incident response workflows during interviews. Employers value candidates who can bridge the gap between theory and actual security operations—and this phase has helped me build exactly that bridge.

**✅ 10. what was your biggest takeaway from phase 1?**

   My biggest takeaway from Phase 1 was gaining hands-on experience with how real-world cyber threats are detected using SIEM tools like Wazuh. By simulating attack scenarios such as brute force, malware execution, and data exfiltration, I developed a deeper understanding of how these activities appear in system logs and how detection rules are crafted based on patterns and behaviors. This phase taught me not just how to generate logs, but how to interpret them effectively—an essential skill for any SOC analyst or cybersecurity engineer.



