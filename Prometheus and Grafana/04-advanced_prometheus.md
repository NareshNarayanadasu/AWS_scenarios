Great! Let's move on to **Session 3: Advanced Prometheus Concepts and Alerting**.

---

### **Session 3: Advanced Prometheus Concepts**

---

#### **Step 1: Set Up Node Exporter for System Metrics**

1. **Install Node Exporter**:
   - Node Exporter is an agent that exposes hardware and OS-level metrics to Prometheus.
   - To install Node Exporter, use the following commands on the target machine:
     ```bash
     wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
     tar -xvzf node_exporter-1.5.0.linux-amd64.tar.gz
     cd node_exporter-1.5.0.linux-amd64
     ./node_exporter
     ```

2. **Verify Node Exporter Metrics**:
   - Node Exporter exposes metrics at `http://localhost:9100/metrics` by default.
   - You can verify it by visiting `http://localhost:9100/metrics` in your browser or by using `curl`:
     ```bash
     curl http://localhost:9100/metrics
     ```

3. **Add Node Exporter to Prometheus Configuration**:
   - Open your `prometheus.yml` file and add a new target under the `scrape_configs` section:
     ```yaml
     scrape_configs:
       - job_name: 'node-exporter'
         static_configs:
           - targets: ['localhost:9100']
     ```
   - Restart Prometheus to apply the new configuration:
     ```bash
     ./prometheus --config.file=prometheus.yml
     ```

---

#### **Step 2: Prometheus Query Language (PromQL)**

PromQL is the query language used by Prometheus to query and aggregate metrics. Let's go through some basic queries.

1. **Query CPU Usage**:
   - Query to get total CPU usage in seconds:
     ```promql
     rate(node_cpu_seconds_total{mode="user"}[5m])
     ```
   - This will show the rate of CPU time spent in "user" mode over the past 5 minutes.

2. **Query Memory Usage**:
   - Query for free memory in bytes:
     ```promql
     node_memory_MemFree_bytes
     ```

3. **Query Disk Space**:
   - Query for the total disk space:
     ```promql
     node_filesystem_size_bytes
     ```

You can use these queries in the **Prometheus** UI or in **Grafana** dashboards.

---

### **Step 3: Setting Up Alerts in Prometheus**

1. **Create Alerting Rules**:
   - Prometheus allows you to define alerting rules in the `prometheus.yml` configuration file. Add the following alerting rule:
     ```yaml
     alerting:
       alertmanagers:
         - static_configs:
           - targets: ['localhost:9093']

     rule_files:
       - "alert.rules"

     scrape_configs:
       - job_name: 'node-exporter'
         static_configs:
           - targets: ['localhost:9100']
     ```

2. **Write an Alert Rule for High CPU Usage**:
   - Create a file named `alert.rules` and add an alerting rule for high CPU usage:
     ```yaml
     groups:
       - name: example-alert
         rules:
           - alert: HighCPUUsage
             expr: rate(node_cpu_seconds_total{mode="user"}[5m]) > 0.8
             for: 5m
             labels:
               severity: critical
             annotations:
               summary: "CPU usage is over 80% for the past 5 minutes"
     ```
   - This rule triggers an alert if CPU usage exceeds 80% for 5 minutes.

3. **Restart Prometheus**:
   - Restart Prometheus to apply the new alerting rule:
     ```bash
     ./prometheus --config.file=prometheus.yml
     ```

4. **Verify Alerts**:
   - Visit the **Alerting** tab in the Prometheus UI (`http://localhost:9090/alerts`) to see if your alert appears.

---

#### **Step 4: Integrating Prometheus Alerts with Alertmanager**

Alertmanager handles alerts sent by Prometheus and can send notifications (email, Slack, etc.).

1. **Install Alertmanager**:
   - Download and extract Alertmanager:
     ```bash
     wget https://github.com/prometheus/alertmanager/releases/download/v0.24.0/alertmanager-0.24.0.linux-amd64.tar.gz
     tar -xvzf alertmanager-0.24.0.linux-amd64.tar.gz
     cd alertmanager-0.24.0.linux-amd64
     ```

2. **Configure Alertmanager**:
   - Create a `alertmanager.yml` configuration file with an example Slack integration:
     ```yaml
     global:
       resolve_timeout: 5m

     route:
       group_by: ['alertname']
       receiver: 'slack-notifications'

     receivers:
       - name: 'slack-notifications'
         slack_configs:
           - api_url: 'https://slack.com/api/your-webhook-url'
             channel: '#alerts'
     ```

3. **Start Alertmanager**:
   - Start Alertmanager:
     ```bash
     ./alertmanager --config.file=alertmanager.yml
     ```

4. **Link Prometheus to Alertmanager**:
   - In `prometheus.yml`, under the `alerting` section, make sure it points to Alertmanager:
     ```yaml
     alerting:
       alertmanagers:
         - static_configs:
           - targets: ['localhost:9093']
     ```

5. **Test the Alert**:
   - Trigger the alert by causing a high CPU usage condition and verify that the notification is sent to Slack.

---

