## Slack integration with alert manager
- Create workspace in slack
- create channel in slack
- click profile -> integration -> add APPS -> incomming webhook and copy the webhook url.

## put the webhook url in alermanager.yml file
```
global:
 resolve_timeout: 20s
route:
  receiver: 'default-slack'
  group_wait: 10s
  group_interval: 20s
  repeat_interval: 1h
  routes:
  - match:
     severity: "Critical"
    receiver: 'urgent_receiver'

receivers:
- name: 'urgent_receiver'
  slack_configs:
  - api_url: 'https://hooks.slack.com/services/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'
    send_resolved: true


- name: 'default-slack'
  slack_configs:
  - api_url: 'https://hooks.slack.com/services/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'
    channel: '#general-alerts'
    send_resolved: true
```
## testing notification in slack manually.
```
 curl -X POST -H 'Content-type: application/json' \
 --data '{"text":"Test message from Alertmanager"}' \
 https://hooks.slack.com/services/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
```

## check alermanager config file
```
./amtool  check-config /var/lib/alertmanager/alertmanager.yml
journalctl -u alertmanager -n 100
```
