\# RDP Brute Force Attack: Analysis \& Detection

\### Project Overview

In this lab, I set up a SOC environment to simulate and detect a Brute Force attack on a Windows endpoint.



\### Environment

\* \*\*Attacker:\*\* Kali Linux (Hydra)

\* \*\*Victim:\*\* Windows 11 (Configured with Sysmon \& Splunk Universal Forwarder)

\* \*\*SIEM:\*\* Splunk Enterprise



\### The Attack

I used Hydra to simulate an adversary attempting to brute force RDP credentials:

`hydra -l administrator -P /usr/share/wordlists/rockyou.txt.gz rdp://\[Target-IP]`



\### The Detection

I configured Splunk to ingest Windows Security Event ID \*\*4625\*\* (Failed Logon).

Query used to visualize the attack intensity:

```splunk

index=main EventCode=4625 | timechart span=1m count by IpAddress

