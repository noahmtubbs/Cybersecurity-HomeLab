\# ---------------- SPLUNK FORWARDER CONFIG ----------------

\# Description: Configuration to ingest system logs and Snort alerts.

\# Path: /opt/splunkforwarder/etc/system/local/inputs.conf



\[default]

host = ubuntu-target



\# 1. Monitor Linux Authentication Logs (SSH Brute Force Detection)

\[monitor:///var/log/auth.log]

disabled = 0

index = linux\_logs

sourcetype = linux\_secure



\# 2. Monitor Snort IDS Alerts

\[monitor:///var/log/snort/alert]

disabled = 0

index = network\_ids

sourcetype = snort\_alert\_full

