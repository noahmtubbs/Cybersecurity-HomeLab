# 🛡️ SOC Lab: Ransomware Detection with Splunk & Sysmon

### 📝 Executive Summary
In this project, I built a home SOC (Security Operations Center) environment to simulate and detect a realistic ransomware attack. I utilized **Splunk Enterprise** as the SIEM and **Sysmon** for granular endpoint telemetry.

The goal was to move beyond signature-based detection and identify ransomware behavior based on **Mass File Modification** and **Malicious Script Execution**.

---

### 🏗️ Network Architecture
The lab consists of a split-network environment hosted on Hyper-V:
* **Victim Endpoint:** Windows 11 (Configured with Sysmon & Splunk Universal Forwarder)
* **Attacker Node:** Kali Linux (Used for C2 and brute force testing)
* **SIEM Server:** Ubuntu Linux (Hosting Splunk Enterprise)

### 🔧 Tools & Technologies
* **Splunk Enterprise:** Log ingestion, indexing, and visualization (SPL).
* **Splunk Universal Forwarder:** Forwarding local Windows event logs to the SIEM.
* **Sysmon:** Advanced Windows event logging (Event IDs 1, 3, 11).
* **PowerShell:** Used to script the ransomware simulation.

---

### 💥 The Attack Simulation
I developed a custom PowerShell script to emulate the behavior of ransomware (e.g., Ryuk, WannaCry). The script performs the following actions:
1.  **Staging:** Creates a target directory with 50 dummy files.
2.  **Encryption:** Loops through files, reads content, and creates an encrypted version with a `.locked` extension.
3.  **Cleanup:** (Optional) Deletes original files to simulate data theft/destruction.

**The "Ransomware" Script:**
```powershell
$targetDir = "C:\Ransomware_Test"
1..50 | ForEach-Object { ("Data " + $_) | Set-Content -Path "$targetDir\file_$_.txt" }
Get-ChildItem -Path $targetDir -Filter *.txt | ForEach-Object {
    $content = Get-Content $_.FullName
    Set-Content -Path "$($_.FullName).locked" -Value $content
}

```

---

### 🕵️ The Detection Strategy

Traditional antivirus often misses custom scripts. I relied on **Behavioral Analysis** using Splunk and Sysmon.

#### 1. Telemetry Configuration

I installed Sysmon on the endpoint and configured `inputs.conf` on the Universal Forwarder to monitor specific Event Channels:

* `Endpoint` -> `Universal Forwarder` (Port 9997) -> `Splunk Indexer`

#### 2. Detection Logic (SPL)

I used the following Splunk Search Processing Language (SPL) queries to identify the attack:

**Query 1: Detecting Mass File Creation (The Smoking Gun)**
Searching for a high volume of file creations with suspicious extensions within a short time window.

```splunk
index=main EventCode=11 TargetFilename="*.locked"

```

**Query 2: Detecting Malicious Execution**
Identifying the process responsible for the file modification.

```splunk
index=main EventCode=1 Image="*powershell.exe" | search "ransomware"

```

---

### 📸 Evidence of Detection

**Visual Proof:** The screenshot below demonstrates the live attack and detection.

* **Left Screen:** The PowerShell script executing the encryption loop.
* **Right Screen:** Splunk capturing the "Process Create" (Event 1) logs in real-time, identifying the execution of `ransomware_sim.ps1`.

![Ransomware Evidence](INSERT YOUR SCREENSHOT LINK HERE)

---

### 🧠 Challenges & Solutions

During the build, I encountered a critical issue where Splunk was receiving **Security** logs but not **Sysmon** logs.

* **Problem:** The default Sysmon configuration filters out file creation events to reduce noise, causing false negatives.
* **Diagnosis:** I verified local logs using Windows Event Viewer and confirmed Sysmon Event ID 11 was missing.
* **Solution:** I reconfigured the Sysmon XML config to explicitly whitelist the testing directory and reset the Universal Forwarder permissions to ensure proper ingestion.

---

### 🏆 Conclusion

This lab demonstrated the importance of **Endpoint Detection & Response (EDR)** visibility. By tuning Sysmon and Splunk, I successfully transformed a "blind" endpoint into a monitored asset capable of detecting live ransomware activity.

```

