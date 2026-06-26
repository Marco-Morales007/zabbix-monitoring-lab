# Troubleshooting

## Problem 1: MySQL Service Was Not Running

### Symptoms

- The Zabbix web interface was unavailable.
- The MySQL service appeared as inactive.

```bash
sudo systemctl status mysql.service
```

Output:

```
Active: inactive (dead)
```

### Investigation

The service was installed correctly but was not running after the virtual machine restarted. Since Zabbix depends on MySQL, the monitoring platform could not function properly.

### Resolution

1. Verified the current service status.

```bash
sudo systemctl status mysql.service
```

2. Enabled the service to start automatically at boot.

```bash
sudo systemctl enable mysql.service
```

3. Restarted the service.

```bash
sudo systemctl restart mysql.service
```

4. Verified that the service was running successfully.

```bash
sudo systemctl status mysql.service
```

Expected result:

```
Active: active (running)
```

### Outcome

After the MySQL service was restored, the Zabbix Server was able to access its database and the web interface became operational again.

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
