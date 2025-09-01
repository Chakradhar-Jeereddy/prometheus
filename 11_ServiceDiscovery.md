## Service Discovery
- Configured in prometheus.yml file
- <ec2_sd_config>,<dbs_sd_config>,<file_sd_config>,<kubernetes_sd_config>,<azure_sd_config>,<gce_sd_config>

## Service Discovery and AWS.
- In AWS service discovery is available only for EC2 and lightsail(comes with software preinstalled, easy to launch)
- <ec2_sd_config> & <lighsail_sd_config>
- Provide port, region, access_key, secret_key.
- If prometheus is in AWS, you need IAM role.
- you need filters, refresh_interval and source lables to discover instances.
- Source label example -> __meta_ec2_ami:the EC2 Amazon Machine Image
-                         __meta_ec2_availability_zone
-                         __meta_ec2_availability_zone_id
-                         __meta_ec2_instance_id
-                         __meta_ec2_instance_state
-                         __meta_ec2_instance_type
-                         __meta_ec2_private_ip
-                         __meta_ec2_private_ip_dns_name
-                         __meta_ec2_public_ip
-                         __meta_ec2_public_ip_dns_name
-                         __meta_ec2_vpc_id
-                         __meta_ec2_tag_<tag_type>     __meta_ec2_tag_environment="production"

```
scrape_configs:
 - job_name: "My EC2"
    ec2_sd_configs:
    - port: 8000
```

## Configurting service discovery for EC2 instances in prometheus.yml file.
```
# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets: ['138.197.21.170:9097']

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "aws sd"
    ec2_sd_configs:
       - port: 9090
         region: ap-southeast-2
         access_key:
         secret_key:
         filters:
           - name: tag:Name
             values:
               - dev-.*
    relable_configs:
     - source_labels: [__meta_ec2_tag_Name,__meta_ec2_private_ip]
       target_label: instance
     - source_labels: [__meta_ec2_tag_Name]
       regex: dev-test-.*
       action: drop
     - source_labels: [__meta_ec2_public_ip]
       replacement: ${1}:9090
       target_label: __address__

  - job_name: 'application_server'
    static_configs:
    - targets: ['138.197.21.170:9100']

rule_files:
  - "rules/alerts_01.yml"
```

## File based service discovery for IBM and alibaba cloud , which does not have inbuilt service discovery
```
```
# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets: ['138.197.21.170:9097']

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "aws sd"
    ec2_sd_configs:
       - port: 9090
         region: ap-southeast-2
         access_key:
         secret_key:
         filters:
           - name: tag:Name
             values:
               - dev-.*
  - job_name: "file sd"
    file_sd_configs:
      - files:
          - /usr/local/etc/file_sd/*.yml 
    relable_configs:
     - source_labels: [__meta_ec2_tag_Name,__meta_ec2_private_ip]
       target_label: instance
     - source_labels: [__meta_ec2_tag_Name]
       regex: dev-test-.*
       action: drop
     - source_labels: [__meta_ec2_public_ip]
       replacement: ${1}:9090
       target_label: __address__

  - job_name: 'application_server'
    static_configs:
    - targets: ['138.197.21.170:9100']

rule_files:
  - "rules/alerts_01.yml"
