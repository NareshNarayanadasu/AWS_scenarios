### **Session 4: Alerting in Grafana**

---

#### **Objective**  
In this session, you’ll learn how to configure **alerting** in Grafana using thresholds and notifications. By the end of this session, you will be able to:
- Set up alert rules in Grafana panels.
- Send notifications to email, Slack, or other tools via **Alert Channels**.

---

### **Step 1: Configure Notification Channels**

1. **Add a Notification Channel**:
   - In Grafana, go to **Alerting** > **Notification channels** from the left-hand menu.
   - Click **Add channel** and configure the following:
     - **Name**: e.g., "Slack Alerts" or "Email Alerts".
     - **Type**: Choose the desired method (e.g., Email, Slack, Microsoft Teams, PagerDuty).
     - **Configurations**:
       - For Slack: Provide the **Webhook URL** and the Slack channel.
       - For Email: Provide the SMTP server details and recipient email addresses.

2. **Test Notification**:
   - Click **Send Test** to confirm that the notification is successfully sent.

---

### **Step 2: Add Alerts to Your Panels**

1. **Choose a Panel for Alerting**:
   - Open the dashboard you created in Session 2.
   - Select a panel (e.g., CPU Usage) and click the **Edit** icon (pencil symbol).

2. **Set Up Alert Rules**:
   - Go to the **Alert** tab in the panel editor.
   - Click **Create Alert** and configure the following:
     - **Evaluate every**: Set how often the alert should be evaluated (e.g., 1 minute).
     - **Conditions**: Add conditions for triggering the alert:
       - Example: If the CPU usage metric exceeds `80%` for more than `5 minutes`.
       - Query: Use a PromQL query like:
         ```promql
         rate(node_cpu_seconds_total{mode="user"}[5m]) > 0.8
         ```
     - **Time range**: Select the time window Grafana should evaluate (e.g., "for 5 minutes").

3. **Add Notifications**:
   - Under the **Notifications** section, select the **Notification Channel** you configured earlier.
   - Optionally, add custom messages or annotations for the alert.

4. **Save the Alert**:
   - Save the panel and dashboard. The alert will now be active.

---

### **Step 3: View and Manage Alerts**

1. **Monitor Alerts**:
   - Go to the **Alerting** > **Alert rules** section in Grafana.
   - You’ll see a list of all active and inactive alerts.

2. **Test an Alert**:
   - Simulate a condition (e.g., high CPU usage) to trigger the alert.
   - Verify that you receive notifications in the configured channel.

---

### **Step 4: Advanced Alerting (Optional)**

1. **Group Alerts**:
   - You can group multiple alert rules for better organization.
   - Use the **Alert Group** feature in the **Alerting** tab.

2. **Templating Alerts**:
   - Use **variables** in Grafana to create dynamic alerts that apply to multiple systems or environments.

---

### **Step 5: Dashboard-Wide Alerts (Optional)**  
If you need alerts across multiple panels or metrics:
- Use **Synthetic Monitoring** or **Grafana Cloud Alerts** to centralize alert configurations.
- This is useful for monitoring trends across different dashboards.

---

### **Next Steps**  
1. Complete this setup by creating a few test alerts and ensuring they work.
2. In the next session, we’ll explore **Disaster Recovery Strategies**, covering:
   - Data backups and restores.
   - Multi-region failover setups.

