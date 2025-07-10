# Lab Report: Metasploitable2  - Nmap Service Version Scan

**Date:** July 10, 2025

**Objective:** To perform active network scan on the Metasploitable2 target VM (192.168.100.10) using Nmap from Kali Linux, identifying open ports, running services, and their versions for initial vulnerability assessment.

**Tools Used:**
* Kali Linux
* Nmap

**Target:**
* Metasploitable2 (IP: 192.168.100.10)

**Steps Performed:**
* Ensured SSH connectivity between Windows Host and Kali Linux (172.18.154.112).
* Executed Nmap with service version detection from Kali Linux via SSH: `nmap -sV 192.168.100.10`

**Results/Findings:**

The Nmap service version scan revealed numerous open ports and running services on the Metasploitable2 target, indicating a wide attack surface for penetration testing. Some of the key findings include:

* **FTP (vsftpd 2.3.4) on Port 21/tcp**: This specific version is known to have a backdoor vulnerability.
* **SSH (OpenSSH 4.7p1 Debian 8ubuntu1) on Port 22/tcp**: An older version that may be susceptible to various exploits.
* **Telnet (Linux telnetd) on Port 23/tcp**: This service typically sends credentials in plaintext.
* **HTTP (Apache httpd 2.2.8) on Port 80/tcp**: An outdated web server version.
* **Samba (Samba smbd 3.X - 4.X) on Ports 139/tcp and 445/tcp**: Older Samba versions often contain critical vulnerabilities.
* **MySQL (MySQL 5.0.51a-3ubuntu5) on Port 3306/tcp**: A significantly old database version, likely vulnerable to common database attacks.
* **PostgreSQL (PostgreSQL DB 8.3.0 - 8.3.7) on Port 5432/tcp**: Another outdated database system.
* **VNC (protocol 3.3) on Port 5900/tcp**: Often a target for brute-force attacks or default credential exploits.
* **Metasploitable root shell on Port 1524/tcp**: An intentional backdoor for administrative access.

**Full Nmap Scan Output:**
┌──(noah㉿kali)-[~]
└─$ nmap -sV 192.168.100.10
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-10 15:46 CDT
Nmap scan report for 192.168.100.10
Host is up (0.0048s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login?
514/tcp  open  shell       Netkit rshd
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
MAC Address: 00:15:5D:00:28:02 (Microsoft)
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 52.61 seconds
