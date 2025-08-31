## Used to define evaluation time of promql expression
```
Default is 1 min.
The for expression should come right after the expression at same indentation.

root@prometh:/etc/prometheus# cat rules/alerts_01.yml
groups:
  - name: Alerts
    rules:
    - alert: Is Node Exported Up
      expr: up{job="application_server"} == 0
      for: 0s    # evaluate in less then a second
      labels:
        severity: warning
      annotations:
        summary: Exporter node is missing (instance {{ $labels.instance }})   # it will display the IP of server in UI.
```


