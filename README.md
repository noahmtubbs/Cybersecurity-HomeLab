# Cybersecurity Home Lab

This repository documents my personal cybersecurity lab environment using Hyper-V to host Kali Linux and Metasploitable2. It focuses on vulnerability scanning, exploitation, and post-exploitation techniques using real-world tools and scenarios.

## Tools Used

- Host: Windows 11 Pro with Hyper-V
- Attacker: Kali Linux 2024.1
- Target: Metasploitable2
- Tools: Nmap, Metasploit Framework, netcat, searchsploit

## Exploits Completed

### 1. VSFTPD v2.3.4 Backdoor
- Vulnerability: Hardcoded backdoor triggered by a specially crafted username.
- Exploit Module: `exploit/unix/ftp/vsftpd_234_backdoor`
- Result: Root shell opened on port 6200 after login attempt with `:)`.
- Post-Exploitation: Accessed `/etc/passwd` and attempted user password cracking.

### 2. Samba Remote Command Execution (usermap script)
- Vulnerability: CVE-2007-2447, command injection via usermap script.
- Exploit Module: `exploit/multi/samba/usermap_script`
- Result: Shell access on Metasploitable2.
- Post-Exploitation: Created proof-of-concept file in `/root/` to verify access.

### 3. UnrealIRCd 3.2.8.1 Backdoor
- Vulnerability: CVE-2010-2075, intentional backdoor in UnrealIRCd source code.
- Exploit Module: `exploit/unix/irc/unreal_ircd_3281_backdoor`
- Result: Remote root shell through command injection.
- Post-Exploitation: Established persistence by adding a new root user.

### 4. PostgreSQL 8.3.7 Default Credentials and Payload Delivery
- Vulnerability: Default credentials and exposed PostgreSQL service on port 5432.
- Enumeration: Service and version identified using `nmap -sV -p 5432`.
- Exploitation:
  - Used `auxiliary/scanner/postgres/postgres_login` to authenticate with `postgres:postgres`.
  - Deployed `exploit/linux/postgres/postgres_payload` with a Meterpreter reverse shell payload.
- Post-Exploitation: Verified session with `sysinfo`, `id`, and accessed system files. Planning privilege escalation and data exfiltration steps.
