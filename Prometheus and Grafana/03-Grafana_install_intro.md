Great! Let's move on to **Session 2: Building Dashboards with Grafana**.

---

### **Session 2: Building Dashboards with Grafana**  

---

#### **Step 1: Install Grafana**  
1. **Download Grafana**:
   - Visit the [Grafana download page](https://grafana.com/get) and download the appropriate version for your operating system.
   - Alternatively, if you're using a Linux system, you can install Grafana using the following commands:
     ```bash
     sudo apt-get install -y software-properties-common
     sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
     sudo apt-get update
     sudo apt-get install grafana
     ```

2. **Start Grafana**:
   - Start the Grafana service:
     ```bash
     sudo systemctl start grafana-server
     sudo systemctl enable grafana-server
     ```

3. **Access Grafana**:
   - Open Grafana in your browser at `http://localhost:3000`.
   - The default login credentials are:
     - **Username**: admin
     - **Password**: admin (you will be prompted to change the password on first login).

---

#### **Step 2: Connect Grafana to Prometheus**

1. **Add Prometheus as a Data Source**:
   - Once logged into Grafana, click the gear icon on the left to open the **Configuration** menu.
   - Select **Data Sources**, then click **Add data source**.
   - Choose **Prometheus** from the available options.
   - Set the **URL** field to `http://localhost:9090` (if Prometheus is running locally).
   - Click **Save & Test** to confirm the connection.

---

#### **Step 3: Create a Basic Dashboard**

1. **Create a New Dashboard**:
   - On the Grafana home page, click the "+" icon on the left and select **Dashboard**.
   - Click **Add new panel**.

2. **Add Metrics to the Panel**:
   - In the "Query" section, select **Prometheus** as the data source.
   - In the query box, type `up` to view the status of your Prometheus targets.
   - You should see a graph showing whether Prometheus is scraping metrics from the target (`up` metric).

3. **Customize the Panel**:
   - You can adjust the panel type (e.g., graph, table) depending on how you want to visualize the data.
   - Click **Apply** to add the panel to the dashboard.

4. **Add More Panels**:
   - You can add more panels for different metrics. For example, try the following queries to view CPU and memory usage:
     - `node_cpu_seconds_total` for CPU usage.
     - `node_memory_MemAvailable_bytes` for available memory.
   
5. **Save the Dashboard**:
   - Click the disk icon at the top to save your dashboard and give it a name.

---

#### **Step 4: Explore and Customize Your Dashboard**

- Play around with different metrics and visualizations (e.g., line graphs, bar charts).
- Explore additional Prometheus metrics like `node_disk_io_time_seconds_total` for disk IO stats or `node_network_receive_bytes_total` for network statistics.

---

### **Next Steps**
- Once you have a basic dashboard, you can start adding more advanced visualizations and explore **Alerting** in Grafana.
- In the next session, we will dive deeper into Prometheus and how to set up **Alerting** for specific conditions (e.g., CPU spikes, service downtime).

Let me know if you've completed the Grafana setup and dashboard creation, or if you encounter any issues!