### **Session 1: Introduction to Prometheus**  

---

#### **Step 1: What is Prometheus?**  
Prometheus is an open-source monitoring system that collects metrics from configured targets at specified intervals, stores them, and makes them available for querying.  

Key Concepts:  
1. **Metrics**: Data collected by Prometheus (e.g., CPU usage, memory consumption).  
2. **Exporters**: Components that expose metrics for Prometheus to scrape (e.g., Node Exporter for system metrics).  
3. **Targets**: Applications or services being monitored.  

---

#### **Step 2: Install Prometheus**  

You can install Prometheus on a local machine or a virtual machine. Let’s proceed with the local setup for simplicity.  

1. **Download Prometheus**:  
   - Visit the [Prometheus download page](https://prometheus.io/download/).  
   - Download the latest Prometheus binary suitable for your OS.  

2. **Extract Prometheus**:  
   - Extract the tarball:  
     ```bash
     tar -xvzf prometheus-*.tar.gz
     cd prometheus-*
     ```  

3. **Start Prometheus**:  
   - Run Prometheus:  
     ```bash
     ./prometheus --config.file=prometheus.yml
     ```  
   - Open Prometheus UI at `http://localhost:9090`.  

---

#### **Step 3: Configure Prometheus**  

The configuration file `prometheus.yml` tells Prometheus where to scrape metrics from.  

1. Open `prometheus.yml` in a text editor:  
   ```bash
   nano prometheus.yml
   ```  

2. Add a target for a sample application (e.g., Node Exporter):  
   ```yaml
   scrape_configs:
     - job_name: 'node-exporter'
       static_configs:
         - targets: ['localhost:9100']
   ```  

3. Save and restart Prometheus to apply the changes.  

---

#### **Step 4: Test Prometheus**  

1. Access Prometheus at `http://localhost:9090`.  
2. Go to the "Targets" page (`http://localhost:9090/targets`) to see if Prometheus is scraping metrics.  
3. Use the "Graph" tab to query metrics like:  
   ```promql
   up
   ```  

---

#### **Next Steps**  
- Set up **Node Exporter** in the next session to monitor your system's CPU, memory, and disk usage.  
