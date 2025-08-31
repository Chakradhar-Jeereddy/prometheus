```
Download software - https://prometheus.io/download/

groupadd --system prometheus
useradd -s /sbin/nologin --system -g prometheus prometheus
mkdir /var/lib/prometheus
mkdir -p /etc/prometheus/rules
mkdir -p /etc/prometheus/rules.s
mkdir -p /etc/prometheus/files.sd

tar -xzvf prometheus.tar.gz
cd swoftware
mv prometheus /usr/local/bin
prometheus --version

vi /etc/systemd/system/prometheus.service
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.external-url=

SyslogIdentifier=prometheus
Restart=always

[Install]
WantedBy=multi-user.target
EOF

https://github.com/aussiearef/Prometheus/blob/main/prometheus.service

chown -R prometheus:prometheus /etc/prometheus
chown -R prometheus:prometheus /etc/prometheus/*
chmod -R 775 /etc/prometheus
chmod -R 775 /etc/prometheus/*
chown -R prometheus:prometheus /var/lib/prometheusls 
chown -R prometheus:prometheus /var/lib/prometheus/*

ls /var/lib/prometheus
system daemon-reload
systemctl start prometheus
systemctl enable service
systemctl status service
```

```
Installing prometheus
Enable port 9100 in droplet firewal rules

Download node exporter -
https://prometheus.io/download/#node_exporter
tar -xzvf node_exporter.tar.gz

cd software
./node_exporter

vi /etc/prometheus/prometheus.yaml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

   - job_name: 'application_server'

    static_configs:
    - targets: ['138.197.21.170:9100']

systemctl start prometheus
systemctl stop prometheus

###########################################################

Creating service for node-exporter
vi /etc/systemd/system/node.service

[Unit]
Description=Prometheus Node Exporter
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/var/lib/node/node_exporter

SyslogIdentifier=prometheus_node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
##############################################
mkdir /var/lib/node
mv node_exporter /var/lib/node
chown -R prometheus:prometheus /var/lib/node
chown -R prometheus:prometheus /var/lib/node*
chmod -R 775 /var/lib/node/*
systemctl daemon-reload
systemctl start node
systemctl status node






