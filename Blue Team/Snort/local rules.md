\# ---------------- LOCAL RULES ----------------

\# Title: Custom Detection Rules for Home Lab

\# Author: Noah Tubbs

\# Updated: Feb 2026



\# RULE 1: Detect ICMP Pings (Testing Connectivity \& Potential Sweeps)

\# Rationale: Basic visibility test to ensure Snort is seeing traffic between VMs.

alert icmp any any -> $HOME\_NET any (msg:"\[LAB] ICMP Ping Detected - Potential Sweep"; sid:100001; rev:1;)



\# RULE 2: Detect Potential Command \& Control (C2) User-Agent

\# Rationale: Detecting a specific User-Agent string often used by script kiddie tools.

alert tcp $HOME\_NET any -> $EXTERNAL\_NET $HTTP\_PORTS (msg:"\[LAB] Suspicious User-Agent Detected"; flow:to\_server,established; content:"User-Agent|3a| BadBot/1.0"; http\_header; classtype:trojan-activity; sid:100002; rev:1;)



\# RULE 3: Detect SSH Root Login Attempt

\# Rationale: Monitoring for unauthorized administrative access attempts.

alert tcp any any -> $HOME\_NET 22 (msg:"\[LAB] SSH Root Login Attempt"; content:"SSH-2.0"; flow:to\_server,established; sid:100003; rev:1;)



\# RULE 4: Detect Nmap TCP Scan

\# Rationale: Identifies stealth scanning activity against the Ubuntu target.

alert tcp any any -> $HOME\_NET 22 (msg:"\[LAB] Nmap TCP Scan Detected"; flags:S; threshold: type both, track by\_src, count 5, seconds 60; sid:100005; rev:1;)

