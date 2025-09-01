## Basic authentication
- Choose username and password
- Create bcrypt hash of you password
- Create web configuration file
- Launch prometheus with the web configuration file

- Install apache utilits to bcrpt password
```
htpasswd -nBC 10 "admin"
New password:
Re-type new password:
admin:$2y$10$LTEVXpevnuTGT9Dtc18on.fQsqBhi.qtfGTbrxxWzIOyD511NbACm
```

## now create web.yml file under folder where you have prometheus.yaml file and put the credentials
```
basic_auth_users:
  admin: $2y$10$LTEVXpevnuTGT9Dtc18on.fQsqBhi.qtfGTbrxxWzIOyD511NbACm

 /usr/local/bin/prometheus --help
--web.config.file=""

- Supply this web.yml file to service file

vi /etc/systemd/system/prometheus.yaml
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prem
Group=prem
ExecReload=/bin/kill -HUP
ExecStart=/usr/local/bin/prometheus   --config.file=/etc/prometheus/prometheus.yml   --storage.tsdb.path=/var/lib/prometheus   --web.console.templates=/etc/prometheus/consoles   --web.console.libraries=/etc/prometheus/console_libraries   --web.listen-address=0.0.0.0:9090  --web.config.file=/etc/prometheus/web.yml

SyslogIdentifier=prometheus
Restart=always

[Install]
WantedBy=multi-user.target
```
- Reload and restart prometheus
  ```
  systemctl daemon-reload
  systemctl restart prometheus
  systemctl status prometheus
  ```
- Access the WEB UI and supply the password
http://138.197.21.170:9090
Username:
Password:

