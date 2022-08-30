1. Native Otel receiver for Apache takes out only 6 metrics - (https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/apachereceiver/documentation.md) These metrics are :
    a. Current_connection
    b. Requests
    c. Scoreboard
    d. Traffic
    e. Uptime
    f. Workers
Two of these metric (scoreboard and workers) have various states defined 
Scoreboard - open, waiting, starting, reading, sending, keepalive, dnslookup, closing, logging, finishing, idle_cleanup, unknown
Workers - busy, idle
These metrics with there state combination can counted as a separate metric (this is the way current app is using these metrics). Thus we can consider that the native receiver emits 17 metrics.

In Sumo App for Apache, we will additionally need the below metrics :

apache_BytesPerSec
apache_CPULoad
apache_Load5
apache_CPUChildrenSystem
apache_CPUSystem
apache_CPUChildrenUser
apache_Load15
apache_ReqPerSec
apache_DurationPerReq
apache_TotalDuration
apache_Load1
apache_CPUUser
apache_BytesPerReq

Since there are metrics missing in native otel receiver, we will need to create a new app based on only these metrics, or we will need to add the missing metrics in native receiver.

2. The metric names emitted by native receiver are not same to that of the once which are getting used in the app. Based on the decision for point 1, We will need to rename them either in the app or in Otel config. In addition to this the current metrics emitted by otel receiver has metric name with "state", but in the app this is considered as single metric. We will need to either make changes to App (to check for metric name and state seperately) or to change config for combining these metric name with state name to create a single metric. 

3. Currently the sumo app for apache expects server and port to be tagged in the metrics. If we use the current app, we will need to do otel config change for including these as part of metric send to sumo.

