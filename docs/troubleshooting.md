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

The Windows host appeared as **Unavailable** in the Zabbix frontend.

The Zabbix Agent log reported messages similar to:

```
connection from "192.168.1.77" rejected, allowed hosts: "192.168.146.132"
```

### Investigation

Network connectivity tests confirmed that the Zabbix Server could reach the Windows host on port **10050**, indicating that the agent was running correctly.

The log message revealed that the connection was being rejected due to the **Server** parameter in the agent configuration. The agent only accepted incoming connections from the IP address specified in the `Server` directive.

### Resolution

1. Reviewed the `zabbix_agent2.conf` configuration file.
2. Updated the `Server` parameter to allow connections from the correct Zabbix Server IP address.
3. Restarted the Zabbix Agent service.
4. Verified that the host became available in the Zabbix frontend.

Expected result:

```
Host availability: Available
```

### Outcome

After updating the allowed server address, the Zabbix Agent accepted incoming requests and monitoring resumed successfully.

---

## Problem 3: NAT vs Bridged Networking

### Symptoms

The Lubuntu host could not communicate with the Zabbix Server.

Connectivity tests failed:

```bash
ping 192.168.146.132
```

```bash
nc -zv 192.168.146.132 10051
```

Both commands timed out.

### Investigation

Although the Zabbix Server and the agent were configured correctly, the virtual machine was connected using VMware **NAT** networking.

The monitored devices were connected to the physical LAN (`192.168.1.0/24`), while the virtual machine was placed on a different virtual subnet (`192.168.146.0/24`).

As a result, the physical hosts could not establish direct communication with the server.

### Resolution

1. Changed the VMware network adapter from **NAT** to **Bridged** mode.
2. Restarted the virtual machine.
3. Verified that the Ubuntu Server obtained an IP address on the same subnet as the monitored hosts.
4. Updated the Zabbix Agent configuration with the new server IP address.
5. Restarted the Zabbix Agent service.

Expected result:

```
Successful communication between agents and Zabbix Server.
```

### Outcome

Once the virtual machine joined the same network as the monitored hosts, communication was restored and both agents successfully reported to the Zabbix Server.

---

## Problem 4: Recovering a Locked Zabbix User via MySQL

### Symptoms

The default **Admin** account became locked after multiple failed login attempts, preventing access to the Zabbix web interface.

### Investigation

Since the account could not be unlocked through the web interface, direct access to the MySQL database was required to reset the failed login counters and update the account credentials.

### Resolution

1. Connected to the Zabbix MySQL database.

2. Reset the failed login attempts.

```sql
UPDATE users
SET attempt_clock = 0,
    attempt_failed = 0
WHERE username = 'Admin';
```

For legacy Zabbix versions:

```sql
UPDATE users
SET attempt_clock = 0,
    attempt_failed = 0
WHERE alias = 'Admin';
```

3. Generated a new password hash using the **bcrypt** algorithm.

4. Updated the administrator password directly in the database.

```sql
UPDATE zabbix.users
SET passwd = 'BCRYPT_HASH'
WHERE username = 'Admin';
```

Legacy versions:

```sql
UPDATE zabbix.users
SET passwd = 'BCRYPT_HASH'
WHERE alias = 'Admin';
```

5. Logged into the Zabbix frontend using the new credentials.

Expected result:

```
Admin account unlocked and accessible.

### Outcome

The administrator account was successfully recovered without reinstalling the application or modifying configuration files. This exercise also provided practical experience managing Zabbix users directly through MySQL and understanding how password hashes are stored using the bcrypt algorithm.

Note: The password value is a bcrypt hash generated before executing the SQL statement. Plain-text passwords cannot be stored directly in the passwd column.
