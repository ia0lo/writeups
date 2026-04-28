# Introduction

**Kioptrix Level 1** is a beginner-friendly vulnerable virtual machine designed for penetration testing practice. The objective of this challenge is to gain initial access to the system and ultimately escalate privileges to root in order to obtain full control over the machine, including the ability to modify sensitive system data such as user passwords.

This lab simulates real-world misconfigurations and outdated services commonly found in legacy systems, making it an excellent environment for learning basic reconnaissance, exploitation, and privilege escalation techniques.

In this write-up, I will demonstrate the step-by-step methodology used to compromise the target machine using common penetration testing tools such as **Nmap**, **ARP-scan**,**searchsploit** and the **Metasploit Framework**, following a structured approach from network discovery to root access.
![[0.png]]

# Network Discovery

To confirm the presence of the target machine within the local network, an initial reconnaissance phase was performed using network scanning tools.

First, **Nmap** was used to identify live hosts on the subnet without performing a port scan. This was done using the following command:

`nmap 10.0.2.0/24 -sn`

The `-sn`  was used to perform a **ping sweep only**, allowing us to discover active devices on the network while avoiding unnecessary port enumeration at this stage.

In addition, **ARP scanning** was also used as an alternative and more reliable method for local network discovery:

`arp-scan -l`

This command helps identify all devices connected to the same Layer 2 network by directly reading ARP responses.

To ensure accuracy, the scan was performed twice:
- Before starting the vulnerable machine to establish a baseline of connected hosts.
- After powering on the Kioptrix target machine to detect any new active IP addresses.

This comparison helped successfully identify the target machine within the same network segment.

![[2.png]]

![[02.png]]![[1.png]]


##  Web Service Verification

To confirm that the target machine was fully operational and reachable, the identified IP address was accessed via HTTP using a web browser.

Upon visiting the web service, a default web page was successfully displayed, indicating that the machine was running a web server and properly responding to HTTP requests.

This step confirmed two important points:
- The target host was active and accessible over the network.
- A web service was exposed, which could potentially be a primary attack surface for further enumeration and exploitation.

The successful loading of the web page marked the transition from basic network discovery to service-level enumeration.

![[0002.png]]

#  Port Scanning & Service Enumeration

After confirming the target’s availability via HTTP, a deeper reconnaissance phase was conducted to identify open ports and gather information about the underlying operating system.
An **Nmap** scan was performed using OS detection and default probing:

`nmap 10.0.2.3 -O`

The results revealed several open services, with particular attention drawn to **port 139**, which was found to be open. This port is associated with **NetBIOS Session Service**, a core component of the **SMB (Server Message Block)** protocol stack used for file and resource sharing in Windows and legacy systems.
![[3.png]]

Given the importance of this service, a more focused scan was performed to identify the exact service version running on this port:

`nmap 10.0.2.3 -p139 -sV`

However, the scan did not return detailed version information. It only indicated that the service running on this port was **Samba**, an open-source implementation of the SMB protocol commonly used on Linux/Unix systems to provide Windows-compatible file sharing services.

This discovery highlighted SMB/Samba as a potential attack vector for further enumeration and exploitation.
![[4.png]]


## SMB Version Enumeration Using Metasploit

To gather more detailed information about the SMB service running on the target, the **Metasploit Framework** was used, as it provides specialized modules for service enumeration and vulnerability analysis.

The framework was launched in quiet mode to reduce startup output noise:

`msfconsole -q`

The `-q`  was used to start Metasploit in **quiet mode**, allowing for a cleaner and more focused working environment.
![[5.png]]


### Metasploit Module Selection for SMB Enumeration

After launching the Metasploit Framework, the next step was to locate an appropriate module for SMB version enumeration.

A search was performed within Metasploit using the following command:

`search smb_version`

This command returned a list of available modules related to SMB version detection. From the results, the relevant module was selected based on its index in the list.

The module was then loaded using its index number:

`use 0`

Here, `0` refers to the position of the desired module in the search results.

Alternatively, the module could also be loaded by specifying its full path instead of using the index number, which is considered a more precise and reliable method, especially in environments where module listings may change.

![[6.png]]
## SMB Module Configuration and Version Discovery
 SMB Module Configuration and Version Discovery

After loading the SMB enumeration module in Metasploit, the next step was to inspect the required parameters for proper execution.

The module options were displayed using:

`options`

This revealed the necessary configuration fields required for the scan, most importantly the RHOSTS parameter, which defines the target IP address.

The target IP was then assigned as follows:

`set RHOSTS 10.0.2.3`

Once the required parameters were configured, the module was executed using:

`run`

The scan successfully completed and returned detailed information about the SMB service running on the target machine. It was identified that the system was running:

`Samba version 2.2.1a`

This version is known to be outdated and potentially vulnerable, making it a strong candidate for further exploitation and privilege escalation in the following phases of the attack.
After loading the SMB enumeration module in Metasploit, the next step was to inspect the required parameters for proper execution.

The module options were displayed using:

`options`

This revealed the necessary configuration fields required for the scan, most importantly the **RHOSTS** parameter, which defines the target IP address.

The target IP was then assigned as follows:

`set RHOSTS 10.0.2.3`

Once the required parameters were configured, the module was executed using:

`run`

The scan successfully completed and returned detailed information about the SMB service running on the target machine. It was identified that the system was running:

> **Samba version 2.2.1a**

This version is known to be outdated and potentially vulnerable, making it a strong candidate for further exploitation and privilege escalation in the following phases of the attack.
![[7.png]]

# Vulnerability Research Using Searchsploit

After identifying the Samba version running on the target system (**Samba 2.2.1a**), the next step was to investigate known vulnerabilities and available exploits for this specific version.

This was done using the **Searchsploit** tool:

`searchsploit samba 2.2.1a`

The purpose of this step was to search the Exploit-DB database for any publicly available exploits targeting the identified Samba version.

The results revealed the existence of known exploits associated with this vulnerable version, including modules that could potentially be leveraged for remote code execution or privilege escalation.

Most importantly, it was observed that a relevant exploit module was also available within the Metasploit Framework, making it easier to integrate directly into the attack workflow.

![[8.png]]


# Exploitation Phase – Samba Trans2Open Vulnerability

After confirming the presence of a vulnerable Samba version (**2.2.1a**), the next step was to identify a suitable exploit within the Metasploit Framework.

A search was performed to locate the relevant exploitation module:

`search trans2open`

The results returned multiple entries related to the **Trans2Open buffer overflow vulnerability**, which affects older Samba versions running on Linux systems.

From the available options, the Linux-specific exploit module was selected, as the target machine was identified as a Linux-based system running Samba services.

This module is known for exploiting a buffer overflow vulnerability in the Samba **Trans2Open** request handling mechanism, which can lead to remote code execution and potentially full system compromise.

The appropriate module was then selected for further configuration and exploitation against the target machine.
![[9.png]]

After selecting the appropriate **Trans2Open exploit module**, the next step was to review the required parameters to properly configure the attack.
This was done using the following command:
`options`
![[10.png]]

## Exploitation Execution and Reverse Shell Access

After configuring the exploit module, the next step involved setting up the target and selecting an appropriate payload for successful exploitation.

The target IP address was defined using:

`set RHOSTS 10.0.2.3`

Next, the available payloads were listed in order to identify a compatible option for the target system:

`show payloads`

From the list of available payloads, a suitable one was selected based on compatibility with the target environment. The payload was then configured using its corresponding index:

`set payload 34`

This payload was chosen due to its compatibility with the target machine and its ability to establish a reverse connection successfully.

Once all required parameters were set, the exploit was executed:

`exploit`

The exploitation was successful, resulting in multiple reverse shell sessions being established in the background. These sessions provided remote access to the target system.

To manage the active sessions and stabilize the environment, the ongoing payload processes were interrupted using:

`Ctrl + C`

![[11.png]]
![[12.png]]
![[13.png]]

# Session Handling & Privilege Confirmation
After successfully exploiting the target and obtaining multiple reverse shell connections, the next step was to manage and interact with the active sessions.
All available sessions were listed using:
`sessions`

This command displayed the active reverse connections along with their corresponding session IDs. One of the stable sessions was selected for interaction:

`sessions -i 2`

This allowed direct interaction with the compromised system through the selected session.

To verify the current user context on the target machine, the following command was executed:

`whoami`

The output confirmed that the session was running with **root privileges**, indicating a full system compromise had already been achieved.

Since root-level access was successfully obtained, the final step involved modifying the system password as required by the challenge objective.

This was done using:

`passwd`

A new password was set for the root user, demonstrating full administrative control over the target machine.
![[13.png]]
# Conclusion
The Kioptrix Level 1 machine was successfully compromised by leveraging an outdated Samba service vulnerability (Trans2Open buffer overflow). Through systematic reconnaissance, vulnerability identification, exploitation, and session management, full root access was achieved.

This exercise highlights the importance of:
- Proper service version management
- Regular patching of legacy systems
- Secure configuration of network services

The machine was fully taken over, achieving the final objective of gaining root-level access and modifying system credentials.
![[14.png]]