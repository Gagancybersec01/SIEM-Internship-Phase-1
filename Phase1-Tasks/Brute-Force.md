  # Brute Force Attack and Detection 
**Task 1: Simulating a Brute Force Attack Followed by Privileged 
Login (T1110 + T1078)**

-> This task is intended for educational purposes only. It demonstrates a Brute 
Force attack (T1110) followed by a Privileged Login (T1078) using Hydra, Ubuntu, 
and Wazuh.

-> The purpose of this exercise is to understand the mechanics of these 
    two tactics and learn how to detect such activities using a monitoring system like 
    Wazuh. By completing this task, you'll gain practical experience in ethical hacking 
    and incident detection.
    
**Objective**
--------------

**1. Brute Force Attack (T1110):** Simulate a brute force attack by repeatedly 
attempting to guess a user's password using Hydra.

**2. Privileged Login (T1078):** Simulate a successful login with admin credentials 
after the brute force attack.

**3. Wazuh Monitoring:** Use Wazuh to detect and log brute force attempts and 
privileged logins.

**Task Breakdown**
------------------

This Task involves setting up and running the attack, monitoring the system, and 
detecting any malicious activities with Wazuh. 

**Tools Needed**

**• Hydra:** Tool for brute-force attacks.

**• Ubuntu:** The target machine for login attempts (to be attacked).

**• Kali Linux:** Tha Kali is the attack machine

**• Wazuh:** Log management and monitoring tool to detect brute force and login 
attempts.

**Step-by-Step Practical Guide**

**1. Setup the Target System (Ubuntu)**

The target system must have SSH enabled to allow for login attempts, both 
successful and unsuccessful.

**1.1 Install Ubuntu (Target Machine)**


• You can use VirtualBox, VMware, or a physical machine for Ubuntu.

• Ensure SSH is enabled on the Ubuntu machine to allow login attempts.

-> sudo apt update

-> sudo apt install openssh-server

-> sudo systemctl enable ssh

-> sudo systemctl start ssh




Make sure to note down the passwords of both users for later use.

**1.3 Confirm SSH is Working**

Test SSH to ensure you can log in with the test user:

-> ssh testuser@<Target_IP>

-> You should be prompted for the password and see if you can authenticate.

**2. Set Up the Attacker Machine (Hydra)**

The attacker machine will simulate the brute-force login attempts using Hydra.

**2.1 Install Hydra**

If you're using Kali Linux, Hydra is pre-installed. On Ubuntu (attacker machine), 
you can install Hydra using:

-> sudo apt update

-> sudo apt install hydra

-> Verify the installation with:

-> hydra -h

**2.2 Prepare Wordlist for Hydra**

Hydra uses a wordlist of potential passwords. One common wordlist is RockYou
(rockyou.txt), which you can find pre-installed on Kali Linux or download from 
GitHub.

**2.3 Run the Brute Force Attack**

Use Hydra to brute-force the testuser account on SSH. The following command will 
attempt multiple password guesses based on the wordlist (rockyou.txt):

-> hydra -l testuser -P /path/to/rockyou.txt ssh://<Target_IP> -t 4

**Where:**
• -l testuser: The username for the login attempts.

• -P /path/to/rockyou.txt: The path to the password list (RockYou wordlist in this 
case).

• ssh://<Target_IP>: The IP address of your target machine.

• -t 4: Number of parallel tasks (adjust based on your machine's performance).

Hydra will continue trying various passwords for testuser until a match is found or the 
attack is manually stopped.

**3. Install and Configure Wazuh for Log Monitoring**

Wazuh will be used to monitor failed login attempts and alert us when suspicious 
activities, like a brute-force attack, occur.

**3.2 Verify Wazuh Alerts for Brute Force Attack**

Wazuh is designed to detect brute-force attempts automatically by monitoring 
/var/log/auth.log (the SSH log file). After running the Hydra brute force attack, you 
should see alerts in the Wazuh logs.

-> View the Wazuh logs to confirm detection:

-> sudo tail -f /var/ossec/logs/alerts/alerts.log

-> You should see alerts related to multiple failed login attempts (T1110: Brute Force).

-> You can see the brute force logs in the Wazuh visual dashboard 

**4. Privileged Login (T1078)**

After the brute-force attack, you will use the admin credentials to simulate a 
successful login.

**4.1 Log in with Admin Credentials**

After Hydra has completed, use the admin credentials to log in:

-> ssh admin@<Target_IP>

-> This simulates a successful login with valid credentials (T1078: Valid Accounts).

**4.2 Monitor Wazuh for the Privileged Login**

-> Wazuh will also log the successful login attempt. Check the logs again to see if 
    Wazuh has flagged the privileged login:

-> sudo tail -f /var/ossec/logs/alerts/alerts.log

-> You should see a log entry for the successful login using admin

**Visualize the alerts**

You can visualize the alert data in the Wazuh dashboard. To do this, go to the Threat Hunting module and add the filters in the search bar to query the alerts.

Linux - rule.id:(5551 OR 5712). Other related rules are 5710, 5711, 5716, 5720, 5503, 5504.


# Proof-Of-Concept

![image alt](Screenshots/Brute-Force-2.png)
![image alt](Screenshots/Brute-Force-3.png)
![image alt](Screenshots/Brute-Force-4.png)
![image alt](Screenshots/privilege-Access-1.png)
![image alt](Screenshots/privilege-Access-2.png)






