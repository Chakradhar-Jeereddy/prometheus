## PushGateway
- In cloud the VMs can scale when load increases and scale down when load decreases.
- Also serverless functions like lamda has no IP or DNS.
- In those cases it is not easy to export metrics.
- Pushgateway acts as endpoint or cache where cloud resources can send metrics.
- Prometheus will scrap the metrics from pushgateway.

## Install pushgateway
```
https://prometheus.io/download/#pushgateway
wget https://github.com/prometheus/pushgateway/releases/download/v1.11.1/pushgateway-1.11.1.linux-amd64.tar.gz
tar -xzvf pushgateway-1.11.1.linux-amd64.tar.gz
cp pushgateway /usr/local/bin/
chown prem:prem /usr/local/bin/pushgateway

- Create a service file
cat /etc/systemd/system/pushgateway.service
[Unit]
Description=Prometheus Pushgateway
Wants=network-online.target
After=network-online.target

[Service]
User=prem
Group=prem
Type=simple
ExecStart=/usr/local/bin/pushgateway
```
- Reload daemon
- start pushgateway
- browse pushgateway http://138.197.21.170:9091/metrics

[Install]
WantedBy=multi-user.target
