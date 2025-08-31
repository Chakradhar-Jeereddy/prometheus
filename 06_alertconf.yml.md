## alertmanager.yml
- We define matchers
- We use lables to filter or match

## These are deprecated.
- Two types of matches
   - match: equity matcher serverity="high"
   - match_re: regualar expression (regex) matcher  system=~"billing.*"
- serverity="high" is label

## New
- matchers: Matachers belong to route

## alert manager config file
```
global:
  smtp_smarthost: 'smtp.gmail.com:587' # Change to your SMTP server and port
  smtp_from: 'chakradharreddy473@gmail.com' # Change @gmail.com to your email address
  smtp_auth_username: 'chakradharreddy473@gmail.com' # Change @gmail.com to your email address
  smtp_auth_password: 'Reddy@8527'
  smtp_require_tls: false #disable if you don't want to use TLS e.g., for local testing

route:
  receiver: 'main_receiver'
  routes:
  - receiver: 'urgent_receiver'
    matchers:
    - severity="Critical"

receivers:
- name: 'main_receiver'
  email_configs:
  - to: 'chakradharreddy473@gmail.com' # Change to your receiver email address
    send_resolved: true

- name: 'urgent_receiver'
  email_configs:
  - to: 'chakradharreddy473@gmail.com' # Change to your receiver email address
    send_resolved: true
    headers:
      subject: 'Urgent: Critical Alert'
```

# prometheus.yml
```
alerting:
  alertmanagers:
    - static_configs:
        - targets:
           - alertmanager:9093  #uncomment this line.
```       








