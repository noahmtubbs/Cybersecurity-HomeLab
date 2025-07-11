# Network Connection Setup and Verification

---

## Checking Network Configuration on Metasploitable

First, verify the network interfaces and IP addresses on Kali using:

```bash
ifconfig
```

This shows all active interfaces, their IPs, and statuses.

![Metasploitable ifconfig output](images/ifconfig.png)

---

## Verifying Connectivity Between Kali and Metasploitable

To confirm that Kali can reach the Metasploitable VM, ping its IP address:

```bash
ping 192.168.100.10
```

Successful replies indicate network connectivity and proper routing.

![Kali ping Metasploitable](images/Kali_Meta_Ping.png)

---

