```
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
```
```
Data model
Prometheus stores data as time series
Every time series is identified by a metric name and  label
<metric name> {key=value, key=value,...}
auth_api_hit{count=1,time_taken=800}
```

```
promql
Data types
1 scalar - Float and string

Store
prometheus_http_total{code="200",job="prometheus"}
query
prometheus_http_requests_total{code="2.*",job="prometheus"}

2 Range vectors are similar to instant vectors except they select a range of samples.
label_name[time_spec]
auth_api_hit[5m]
```


```
Operators
+,-,*,/,Modulo,^

Binary operator
==
!=
>
<
>=
<=

Set operators can only applied on instant vectors.
and, or, unless or comma

matches and selectors
Metric{key=value,key2=value)
!=
=~ regular expression
!~ not matching regular expression
Metric{key=~2.*,key2!~3.*)

Aggregation operators
Applicable only on instant vector and retruns instant vector with aggregated value.
sum()
group()
max(), min(),avg(), count(), group, topk(), bottomk()



Time offsets
To check values in past.
Metric{key=~2.*,key2!~3.*) offset 10m
```

```
Function
absent(metric) retuns empty vector if parameter has elements.
abcent_over_time(metric) for range vector.
abs
ceil
floor
clamp(instant vector,min.max)

day_of_month
day_of_week










