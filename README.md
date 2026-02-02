# Cybersecurity Home Lab: Purple Team Simulation

## 🚩 Project Overview
This repository documents my personal cybersecurity lab designed to simulate a **Purple Team** environment (Attack & Defense).
I use this lab to practice the full lifecycle of a cyber attack:
1.  **Red Team:** Executing real-world exploits using Kali Linux and Metasploit.
2.  **Blue Team:** Detecting those attacks in real-time using Snort IDS and Splunk SIEM.

## 🛠️ Environment & Tools
* **Virtualization:** Hyper-V on Windows 11 Pro
* **Attacker Node:** Kali Linux 2024.1 (Metasploit, Nmap, Hydra)
* **Defensive Node:** Ubuntu Server 22.04 (Snort IDS, Splunk Universal Forwarder)
* **Vulnerable Target:** Metasploitable2 (Intentionally vulnerable Linux VM)
* **SIEM:** Splunk Enterprise (Log Analysis & Dashboarding)

---

## 🛡️ BLUE TEAM: Defensive Operations (Splunk & Snort)
*Focus: Network Intrusion Detection, Log Ingestion, and SIEM Visualization.*

### 1. Intrusion Detection with Snort
* **Configuration:** Deployed Snort in NIDS mode on the Ubuntu target to monitor live interface traffic.
* **Custom Rules:** Authored signatures in `local.rules` to detect reconnaissance activity.
    * **ICMP Sweeps:** Created rules to flag ping sweeps often used in initial discovery.
    * **Port Scanning:** Tuned rules to detect Nmap TCP stealth scans.
* **Result:** Successfully generated alerts in `/var/log/snort` verifying detection of Red Team scans.

### 2. SIEM Log Analysis with Splunk
* **Log Ingestion:** Configured Splunk Universal Forwarder to ship system logs and IDS alerts to the Splunk Indexer.
* **SSH Brute Force Detection:**
    * Ingested `/var/log/auth.log` to monitor authentication attempts.
    * Built a Splunk dashboard to visualize "Failed Password" spikes in real-time.
* **Alert Correlation:** Correlated Snort alerts with system logs to validate successful vs. blocked attack attempts.

---

## ⚔️ RED TEAM: Offensive Operations (Metasploit)
*Focus: Vulnerability Scanning, Exploitation, and Post-Exploitation.*

### 1. PostgreSQL Privilege Escalation
* **Vulnerability:** Default credentials on port 5432 coupled with an outdated SUID-enabled Nmap binary.
* **Exploit Chain:**
    * Used `auxiliary/scanner/postgres/postgres_login` to authenticate (postgres:postgres).
    * Deployed `exploit/linux/postgres/postgres_payload` to gain a Meterpreter shell.
    * **Privilege Escalation:** Abused the SUID bit on `/usr/bin/nmap` using interactive mode (`!sh`) to escalate from `postgres` user to `root`.

### 2. VSFTPD v2.3.4 Backdoor
* **Vulnerability:** Hardcoded backdoor in VSFTPD v2.3.4 source code.
* **Exploit:** `exploit/unix/ftp/vsftpd_234_backdoor`
* **Result:** Triggered the backdoor by sending a smiley face `:)` in the username, opening a root shell on port 6200.
* **Post-Exploitation:** Dumped `/etc/passwd` to harvest user credentials.

### 3. Samba Remote Command Execution
* **Vulnerability:** CVE-2007-2447 (Username Map Script).
* **Exploit:** `exploit/multi/samba/usermap_script`
* **Result:** Gained shell access by injecting shell meta-characters into the username field during authentication.

### 4. UnrealIRCd Backdoor
* **Vulnerability:** CVE-2010-2075 (Intentional backdoor in source archive).
* **Exploit:** `exploit/unix/irc/unreal_ircd_3281_backdoor`
* **Result:** Obtained remote root shell via command injection.
* **Persistence:** Created a new root-level user account to maintain access after the initial session closed.
