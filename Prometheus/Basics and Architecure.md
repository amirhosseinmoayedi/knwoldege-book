#prometheus #monitoring #architecture 

# <font color="#953734">Prometheus</font>
Prometheus is a<font color="#953734"> monitoring platform that collects metrics from monitored targets</font> by scraping metrics HTTP endpoints on these targets.

### <font color="#ffff00">What are metrics</font>?
_metrics_ are **numeric measurements**. 
<font color="#92cddc">Time series</font> means that <font color="#92cddc">changes are recorded over time</font>. **What users want to measure differs from application to application**. For a web server it might be request times, for a database it might be number of active connections or number of active queries etc.
 
<u>Metrics are used to track and analyze the performance and health of various components of a system</u>, such as software and hardware resources or user behavior.

> Metrics are typically collected over time, and can be used to identify trends or patterns in a system's behavior.

## Architecure 
![[Pasted image 20230415135927.png]]

## <font color="#e36c09">When does it not fit</font>?
Prometheus values reliability. You can always view what statistics are available about your system, even under failure conditions. **If you need 100% accuracy**, such as for per-request billing, <u>Prometheus is not a good choice as the collected data will likely not be detailed and complete enough</u>. In such a case you would be best off using some other system to collect and analyze the data for billing, and Prometheus for the rest of your monitoring.


## <font color="#c3d69b">Config</font> 
this is an simple example of Prometheus config: 
```yaml
# controls the Prometheus server's global configuration.
global:  
  scrape_interval:     15s # controls how often Prometheus will scrape targets.
  evaluation_interval: 15s # how often Prometheus will evaluate rules

# specifies the location of any rules we want the Prometheus server to load.
rule_files:
  # - "first.rules"
  # - "second.rules"

#scrape_configs, controls what resources Prometheus monitors.
scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
```

During <font color="#fac08f">each evaluation</font>, Prometheus <font color="#fac08f">checks whether the conditions specified in the rules are met or not</font>. **If the conditions are met, Prometheus will trigger the corresponding alert**.

Prometheus also has an [[API]] which <font color="#b7dde8">allows to query metrics which have been stored by scraping</font>.

### Run the prometheus:
`./prometheus --config.file=prometheus.yml`

## Some Key Words
- <font color="#ffff00">Alert</font>: The outcome of an alerting rule in Prometheus that is actively firing.

- <font color="#ffff00">Alertmanager</font>: A component that receives alerts from Prometheus, groups them, applies various actions (such as silencing, throttling), and sends notifications to various channels.

- <font color="#ffff00">Collector</font>: A part of an exporter that represents a set of metrics, either a single metric or many metrics.

- <font color="#ffff00">Endpoint</font>: A source of metrics that can be scraped, typically corresponding to a single process.

- <font color="#ffff00">Exporter</font>: A binary that runs alongside the application you want to monitor and exposes Prometheus metrics, often by converting metrics from a non-Prometheus format into one that Prometheus can understand.

- <font color="#ffff00">Pushgateway</font>: A component that persists the most recent push of metrics from batch jobs, allowing Prometheus to scrape their metrics after they have terminated.

- <font color="#ffff00">Client library</font>: A client library is a library in some language (e.g. Go, Java, Python, Ruby) that makes it easy to directly instrument your code, write custom collectors to pull metrics from other systems and expose the metrics to Prometheus.

## <font color="#95b3d7">Data Model</font>
<u>Prometheus fundamentally stores all data as time series</u>, Besides stored time series, Prometheus may generate temporary derived time series as the result of queries.

optional <u>key-value pairs called as labels</u> can also be stored along with metrics.

## <font color="#938953">METRIC TYPES</font>
- <font color="#ffff00">Counter</font>: A cumulative metric that <font color="#ffff00">represents a single monotonically increasing counter</font>, whose value can only increase or be reset to zero on restart. It's commonly used to represent the <font color="#ffff00">number of requests served</font>,<font color="#ffff00"> tasks completed</font>, or <font color="#ffff00">errors</font>.

- <font color="#fac08f">Gauge</font>: A metric that represents a<font color="#fac08f"> single numerical value that can arbitrarily go up and down.</font> Gauges are typically used for <font color="#fac08f">measured values like temperatures or current memory usage</font>, as well as "counts" that can go up and down, such as the number of concurrent requests.

- <font color="#31859b">Histogram</font>: A type of metric in Prometheus that <font color="#31859b">allows you to track the distribution of values in a given dataset</font>. It works by dividing the dataset into a set of "<font color="#31859b">buckets</font>" that <font color="#31859b">represent different ranges of values</font>. Each time a new value is recorded for the metric being tracked, it's added to the appropriate bucket based on its value. Histograms also <u>track other useful statistics, such as the total number of values recorded, the sum of all the values, and the count of values in each bucket</u>. These statistics can be used to compute things like the average value, the 90th percentile, and the standard deviation of the dataset.
	
	A <font color="#ffc000">histogram vector</font> is a type of metric in Prometheus that represents a<font color="#ffc000"> set of related histograms</font>. Each histogram in the vector tracks the <font color="#ffc000">distribution of values for a specific metric, with labels</font> used to differentiate between different instances of the metric.

- <font color="#95b3d7">Summaries</font>
	the Summary metric type is <font color="#95b3d7">similar to histograms</font> in that it provides a <font color="#95b3d7">way to measure the distribution of observed values</font>. However, unlike histograms, summaries calculate quantiles on the<font color="#95b3d7"> server side</font>, rather than relying on predefined buckets.<font color="#95b3d7">can be more efficient than histograms for high-cardinality data</font> (i.e., data with<font color="#95b3d7"> many distinct label values</font>) because they do not need to track each individual observation.
## <font color="#00b050">JOBS AND INSTANCES</font>
In Prometheus terms, <font color="#00b050">an endpoint you can scrape is called an instance</font>, usually corresponding to a single process. <u>A collection of instances with the same purpose</u>, a process replicated for scalability or reliability for example, <u>is called a job</u>.

**When Prometheus scrapes a target, it attaches some labels automatically to the scraped time series which serve to identify the scraped target**:

For collecting information from client or endpoint we can use client for that language like go client which we should implement the things we want expose manually or we can use exporters like postgres exporters or golang http middleware.