# Troubleshooting

## Problem 1: MySQL Service Stopped

### Symptoms

Unable to connect using:

mysql

### Investigation

- Checked service status
- Reviewed logs
- Restarted VM

### Resolution

After rebooting the VM, MySQL started correctly.

---

## Problem 2: Windows Agent Unavailable

### Symptoms

Host appeared unavailable in Zabbix.

### Investigation

Agent logs showed:

connection rejected, allowed hosts...

### Resolution

Updated the Server parameter to include the Windows host IP.

---

## Problem 3: NAT vs Bridged Networking

### Symptoms

Lubuntu agent could not reach Zabbix Server.

### Investigation

- Ping failed
- Port 10051 unreachable
- Hosts were on different subnets

### Resolution

Changed VMware network mode from NAT to Bridged.
