## Alermanager
- PORT: 9093
- Alertmanager converts alerts to notifications.
- It can receive alerts from multiple prometheus servers and de-duplicate alerts.
- It Can silent alerts
- It has web user interface
- The web user interface is accessed via port 9093
- Is configured via alertmnager.yml file.

## Install Alermanager
- Install pre-release package
  ```
  mkdir /root/alertmanager
  cd /root/alertmanager
  wget https://github.com/prometheus/alertmanager/releases/download/v0.28.1/alertmanager-0.28.1.linux-amd64.tar.gz
  tar -xvzf alertmanager-0.28.1.linux-amd64.tar.gz
  mkdir /var/lib/alertmanager
  cp * /var/lib/alertmanager

  Create a service file
  vi  /etc/systemd/system/alertmanager.service
###################################################################
[Unit]
Description=Prometheus Alert Manager
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prem
Group=prem
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/var/lib/alertmanager/alertmanager --storage.path="/var/lib/alertmanager/data" --config.file="/var/lib/alertmanager/alertmanager.yml"

SyslogIdentifier=prometheus_alert_manager
Restart=always

[Install]
WantedBy=multi-user.target

#########################################################################
chown -R prem:prem /var/lib/alertmanager
chown -R prem:prem /var/lib/alertmanager/*
chmod 755 -R /var/lib/alertmanager
chown -R prem:prem /var/lib/alertmanager/*

systemctl daemon-reload
systemctl start alertmanager
systemctl status alertmanager
```
- Alert manager web brower
http://138.197.21.170:9093/
