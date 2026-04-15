# Ansible-SIEM-Deployment
Grafana SIEM solution deployment using Ansible

**Last Updated:** April 15, 2026

## Grafana Stack Deployment
The following services are deployed on the designated Grafana server:
    - Loki
    - Prometheus
    - Grafana

Loki is used to aggregate logs being sent to the Grafana server. The Loki instance is version 2.9.3. Port 3100 must be open for the service to function. The instance is run as a service named `loki.service`.

Prometheus is used to collect metrics from the client machines. Port 9090 must be open on the server machine, and port 9100 must be open on all machines sending metrics. The instance is run as a service named `prometheus.service`. The web UI can be accessed at http://<server_ip>:9090.

Grafana is used to visualize logs being sent to the server. The web UI is located at http://<server_ip>:3000. The Ansible deployment sets up the default login credientials:
    ```
    Username: admin
    Password: admin
    ```
It is recommended to change these to something more secure. The Grafana instance runs as a service named `grafana-server.service`.  

**The Ansible deployment assumes that the Grafana server is being deployed on an Ubuntu machine.**

## NodeExporter Deployment
Node Exporter is used to ship metrics from the client machine to the server machine. It runs as service named `node_exporter.service`. View the Prometheus web UI shows whether or not the client machine is properly connected and sending metrics.

## Promtail Deployment
Promtail is used to ship logs from the client machine to the server machine. What logs and where the log files are located are all specifed in the `/etc/promtail/config.yml` promtail configuration file. You can edit this file to customize what Promtail is sending to Loki on the server machine.

## Logging Tools Deployment
- **Auditd:** Auditd is installed and setup up with 10 basic rules:
    1. Log commands executed with root privileges
    2. Log permission modifications
    3. Log failed access attempts (permission denied)
    4. Watch /etc/passwd for changes
    5. Watch /etc/shadow for changes
    6. Watch /etc/sudoers for changes
    7. Log system time changes
    8. Log file deletions and renames
    9. Watch updates to login records (lastlog)
    10. Watch su usage

- **Sysmon:** Sysmon is installed to log services, cronjobs, running process, and network connections.
