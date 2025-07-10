# Lab Report: Metasploitable2 – Nmap Service Version Scan

* **Date:** July 10, 2025
* **Author:** Noah Tubbs
* **Objective:** Identify open ports and running service versions on a Metasploitable2 VM using Nmap, as part of an initial reconnaissance and vulnerability assessment.

---

##  Tools Used

* Kali Linux (Attacker VM)
* Nmap (Version 7.95)

---
##  Target Information

* **Target:** Metasploitable2 VM
* **IP Address:** `192.168.100.10`

---

## Steps Performed

1. Started both Kali Linux and Metasploitable2 VMs on the same Hyper-V network.
2. Verified Kali’s network connectivity (IP: `172.18.154.112`) to the target.
3. Ran an Nmap service version scan with the following command:

```bash
nmap -sV 192.168.100.10
```

---

## Key Findings

The scan revealed a large number of open ports and outdated services. Below are selected highlights:

| Port    | Service        | Version / Notes                                                                |
| ------- | -------------- | ------------------------------------------------------------------------------ |
| 21      | FTP            | vsftpd 2.3.4 — Known backdoor vulnerability (CVE-2011-2523)                    |
| 22      | SSH            | OpenSSH 4.7p1 — Potentially vulnerable to brute force or outdated crypto       |
| 23      | Telnet         | Linux telnetd — Sends credentials in plaintext                                 |
| 80      | HTTP           | Apache 2.2.8 — Outdated and misconfigurable web server                         |
| 139/445 | Samba          | Samba smbd 3.X–4.X — Old versions vulnerable to RCE (e.g., MS08-067)           |
| 3306    | MySQL          | MySQL 5.0.51a — Vulnerable to auth bypass and weak configurations              |
| 5432    | PostgreSQL     | PostgreSQL 8.3.x — Obsolete version, weak auth or default credentials possible |
| 5900    | VNC            | Protocol 3.3 — Often unencrypted, brute-forceable                              |
| 1524    | Backdoor Shell | Intentional root shell running on TCP/1524                                     |

---

## Full Nmap Output

<details>
<summary>Click to expand full scan result</summary>

```text
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
Service Info: Hosts: metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/.
Nmap done: 1 IP address (1 host up) scanned in 52.61 seconds
```

</details>

---

