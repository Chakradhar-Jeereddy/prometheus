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






