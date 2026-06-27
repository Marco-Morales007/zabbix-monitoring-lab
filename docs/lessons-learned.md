# Lessons Learned

This project provided practical experience deploying, configuring, and troubleshooting a monitoring infrastructure using Zabbix. Beyond the installation process, it reinforced several concepts commonly encountered in IT operations and infrastructure administration.

## Key Takeaways

### Monitoring Architecture

* Understood how Zabbix Server, MySQL, Apache, and Zabbix Agents interact to provide a complete monitoring solution.
* Learned how monitored endpoints communicate with the server using active agent checks.

### Network Troubleshooting

* Identified how VMware networking modes (NAT vs. Bridged) directly affect communication between virtual machines and physical hosts.
* Applied connectivity troubleshooting techniques using tools such as `ping`, `nc`, and `telnet` to isolate network-related issues before modifying application configurations.

### Agent Configuration

* Configured Windows and Linux agents.
* Learned the purpose of the `Server`, `ServerActive`, and `Hostname` parameters and how incorrect values can prevent successful communication.

### Monitoring Configuration

* Configured hosts using official Zabbix templates.
* Validated monitoring by analyzing collected metrics, generated graphs, dashboards, and built-in triggers.
* Gained practical understanding of how templates, items, graphs, and triggers work together.

### System Administration

* Managed Linux services using `systemctl`.
* Diagnosed service failures using log files and service status information.
* Verified network services and listening ports during troubleshooting.

### Database Administration

* Managed Zabbix users directly through MySQL.
* Reset locked administrator accounts.
* Updated user credentials using bcrypt password hashes.
* Gained a better understanding of how Zabbix stores authentication data.

### Troubleshooting Methodology

One of the most valuable lessons from this project was adopting a structured troubleshooting process instead of relying on trial and error.

For every issue encountered, the workflow consisted of:

1. Identifying the symptoms.
2. Verifying service status.
3. Reviewing system and application logs.
4. Testing network connectivity.
5. Validating configuration files.
6. Applying corrective actions.
7. Confirming that the monitoring environment operated as expected.

This systematic approach significantly reduced troubleshooting time and improved confidence when diagnosing infrastructure-related issues.

