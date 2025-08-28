Telemetry in software is a collection of metric data and store and visualise it for purpose of production issues or improving the business.

Metrics -
How many errors and exceptions?
What is the response time?
How many times API is called?
How many servers?
How many users from NewYork?

Time series database stores metrics, value and time.
Prometheus database stores, query and alert.


Exporter - It runs on source and gets the metrics.
Prometheus connects to the exporter and gets the metrics.
Pulling the metrics from exporter is called scraping.


Push gateway is prometheus component, it is temporary storage where application sends data.
Prometheus will pull the data from there.

Node exporter -It is an offical prometheus exporter for collecting metrics that are exposed by Unix based kernels. eg Linux and Ubuntu.
Metrics like CPU, Disk, Memory and Network I/O.
Node exporter can be extended with pluggable metric collectors.







