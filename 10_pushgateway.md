## PushGateway
- In cloud the VMs can scale when load increases and scale down when load decreases.
- Also serverless functions like lamda has no IP or DNS.
- In those cases it is not easy to export metrics.
- Pushgateway acts as endpoint or cache where cloud resources can send metrics.
- Prometheus will scrap the metrics from pushgateway.

