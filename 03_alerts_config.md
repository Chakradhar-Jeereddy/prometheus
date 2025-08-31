# Alerts
=> https://samber.github.io/awesome-prometheus-alerts/
- all alerts are in above URL, keep it handy.

- Alerts in prometheus are defined in yaml files in promql format.
- We declare threshold close to the point of choas or point of unprediction.
- The yaml files should be on premetheus server.
- The promsql is constantly beign evavluated and when value returns one, non-empty result. We get alert.
- Once the alert is generated, it will appear on UI under alerts tab, but no notification is sent through ticket or email.
- For notification, we need a component calld alert manager.
- Alert manager gets the signal from prometheus and converts into format suitable for target (email, sclak, webhook,pagerduty).
- We normally configure two premetheus servers for HA. Both gets alters from node exporter.
- Alert manger when it gets two signals, it will aggregate and send only once.

```
/etc/prometheus/prometheus.yaml file
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
  #    rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

  - job_name: 'application_server'
    static_configs:
    - targets: ['138.197.21.170:9100']

rule_files:
  - "rule/alert_01.yaml"  #When we restart prometheus its going to load this file.
```

- alert file in yaml format
```
root@prometh:/etc/prometheus# cat rules/alerts_01.yaml
groups:
  - name: Alerts  #name of the rule
    rules:
     - alert: Is Node Exported U p #name of the alert
       expr: up{job="application_server"} == 0               #promsql exporession raise alert when expression return zero results
                                                             #take the above expression from status->targer health tabs.
                                                             #checking whether node exported is running.
```

- Restart prometheus to load the alert file.
```
systemctl status prometheus
systemctl stop prometheus
systemctl start prometheus
systemctl status prometheus
```

## Test case 1
```
stop exported
systemctl stop node
systemctl status node
```
- Now you should get alert.
- Start exporter
```
systemctl start node
systemctl stus node
Note - The alert gets cleared.
```

## Check config, rule and logs 
```
promtool check config /etc/prometheus/prometheus.yml
Checking /etc/prometheus/prometheus.yml
  SUCCESS: 1 rule files found
  SUCCESS: /etc/prometheus/prometheus.yml is valid prometheus config file syntax
Checking /etc/prometheus/rules/alerts_01.yml
  SUCCESS: 1 rules found

journalctl -u prometheus -n 100

promtool check rules rules/alerts_01.yml
Checking rules/alerts_01.yml
  SUCCESS: 1 rules found

```
## examples of promptool
```
root@prometh:/etc/prometheus# promtool
usage: promtool [<flags>] <command> [<args> ...]

Tooling for the Prometheus monitoring system.


Flags:
  -h, --[no-]help            Show context-sensitive help (also try --help-long and --help-man).
      --[no-]version         Show application version.
      --[no-]experimental    Enable experimental commands.
      --enable-feature= ...  Comma separated feature names to enable. Valid options: promql-experimental-functions,
                             promql-delayed-name-removal. See https://prometheus.io/docs/prometheus/latest/feature_flags/ for more
                             details

Commands:
help [<command>...]
    Show help.

check service-discovery [<flags>] <config-file> <job>
    Perform service discovery for the given job name and report the results, including relabeling.

check config [<flags>] <config-files>...
    Check if the config files are valid or not.

check web-config <web-config-files>...
    Check if the web config files are valid or not.

check healthy [<flags>]
    Check if the Prometheus server is healthy.

check ready [<flags>]
    Check if the Prometheus server is ready.

check rules [<flags>] [<rule-files>...]
    Check if the rule files are valid or not.

check metrics
    Pass Prometheus metrics over stdin to lint them for consistency and correctness.

    examples:

    $ cat metrics.prom | promtool check metrics

    $ curl -s http://localhost:9090/metrics | promtool check metrics

query instant [<flags>] <server> <expr>
    Run instant query.

query range [<flags>] <server> <expr>
    Run range query.

query series --match=MATCH [<flags>] <server>
    Run series query.

query labels [<flags>] <server> <name>
    Run labels query.

query analyze --server=SERVER --type=TYPE --match=MATCH [<flags>]
    Run queries against your Prometheus to analyze the usage pattern of certain metrics.

debug pprof <server>
    Fetch profiling debug information.

debug metrics <server>
    Fetch metrics debug information.

debug all <server>
    Fetch all debug information.

push metrics [<flags>] <remote-write-url> [<metric-files>...]
    Push metrics to a prometheus remote write (for testing purpose only).

test rules [<flags>] <test-rule-file>...
    Unit tests for rules.

tsdb bench write [<flags>] [<file>]
    Run a write performance benchmark.

tsdb analyze [<flags>] [<db path>] [<block id>]
    Analyze churn, label pair cardinality and compaction efficiency.

tsdb list [<flags>] [<db path>]
    List tsdb blocks.

tsdb dump [<flags>] [<db path>]
    Dump samples from a TSDB.

tsdb dump-openmetrics [<flags>] [<db path>]
    [Experimental] Dump samples from a TSDB into OpenMetrics text format, excluding native histograms and staleness markers, which
    are not representable in OpenMetrics.

tsdb create-blocks-from openmetrics [<flags>] <input file> [<output directory>]
    Import samples from OpenMetrics input and produce TSDB blocks. Please refer to the storage docs for more details.

tsdb create-blocks-from rules --start=START [<flags>] <rule-files>...
    Create blocks of data for new recording rules.

promql format <query>
    Format PromQL query to pretty printed form.

promql label-matchers set [<flags>] <query> <name> <value>
    Set a label matcher in the query.

promql label-matchers delete <query> <name>
    Delete a label from the query.
```



