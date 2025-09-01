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

#################################################################
## Enabling https for prometheus
- Genarate cert and key under /etc/prometheus and change ownership to prem user
```
  openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout node_exporter.key -out node_exporter.crt -subj "/C=BE/ST=Antwerp/L=Brasschaat/O=Inuits/CN=localhost"
```
- Update web.yml file with tls configuration
```
tls_server_config:
  cert_file: prom.crt
  key_file: prom.key
basic_auth_users:
  admin: $2y$10$LTEVXpevnuTGT9Dtc18on.fQsqBhi.qtfGTbrxxWzIOyD511NbACm
```

- Restart prometheus.
  systemctl restart prometheus
#########################################################################################
## Enabling HTTPS communication between prometheus and exporter.
- create cert and key files using openssl, create web.yml file and put the tls entries.
- Update the nodeexporter service file with the web.yml
```
root@prometh:/etc/node# ls
prom.crt  prom.key  web.yml
root@prometh:/etc/node# cat web.yml
tls_server_config:
  cert_file: prom.crt
  key_file: prom.key

[Unit]
Description=Prometheus Node Exporter
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prem
Group=prem
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/var/lib/node/node_exporter  --web.config.file=/etc/node/web.yml

SyslogIdentifier=prometheus_node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

## Enabling SSL for alert manager
- Follow the same procedure as nodeexporter.

