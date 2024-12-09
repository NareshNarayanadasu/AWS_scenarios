Let’s continue from where we left off, diving deeper into **hands-on exercises** and **testing your Disaster Recovery (DR) strategy**.  

---

### **Step 5: Hands-On Exercises for DR**  

#### **Exercise 1: Setting Up a Simple Backup and Restore Process**  

1. **Database Backup with Automation**:  
   - **Scenario**: Your application uses a MySQL database, and you need daily backups.  
   - **Steps**:  
     a. Log in to your server with MySQL installed.  
     b. Write a backup script:  
        ```bash
        #!/bin/bash
        TIMESTAMP=$(date +%F)
        BACKUP_DIR="/backups"
        DB_NAME="my_database"
        USER="root"
        PASSWORD="your_password"

        mkdir -p $BACKUP_DIR
        mysqldump -u $USER -p$PASSWORD $DB_NAME > $BACKUP_DIR/$DB_NAME-$TIMESTAMP.sql
        ```  
     c. Make the script executable:  
        ```bash
        chmod +x backup_script.sh
        ```  
     d. Schedule it with a cron job:  
        ```bash
        crontab -e
        ```  
        Add the line:  
        ```bash
        0 3 * * * /path/to/backup_script.sh
        ```  

2. **Testing the Restore Process**:  
   - Simulate a failure by deleting your database.  
   - Restore it using:  
     ```bash
     mysql -u root -p my_database < /backups/my_database-<DATE>.sql
     ```  

---

Sure! Let’s continue with **Exercise 2: Multi-Region Deployment for a Web App**.

---

### **Exercise 2 (Continued): Multi-Region Deployment for a Web App**

#### Deploy EC2 Instances in Two Regions  
We’ll create EC2 instances in two AWS regions (`us-east-1` and `us-west-2`) with the same configuration using **Terraform**.  

1. **Terraform Script for Multi-Region EC2 Deployment**:  
   ```hcl
   provider "aws" {
     alias  = "us_east_1"
     region = "us-east-1"
   }

   provider "aws" {
     alias  = "us_west_2"
     region = "us-west-2"
   }

   resource "aws_instance" "nginx_us_east" {
     provider          = aws.us_east_1
     ami               = "ami-0c2b8ca1dad447f8a"
     instance_type     = "t2.micro"
     tags = {
       Name = "nginx-east"
     }
   }

   resource "aws_instance" "nginx_us_west" {
     provider          = aws.us_west_2
     ami               = "ami-0c2b8ca1dad447f8a"
     instance_type     = "t2.micro"
     tags = {
       Name = "nginx-west"
     }
   }
   ```

2. **Apply the Terraform Script**:  
   - Save the above script to a file, e.g., `multi-region.tf`.  
   - Run the commands:  
     ```bash
     terraform init
     terraform apply
     ```  

3. **Install and Configure Nginx**:  
   - After deployment, SSH into both instances:  
     ```bash
     ssh -i "your_key.pem" ec2-user@<instance_ip>
     ```  
   - Install Nginx:  
     ```bash
     sudo yum install nginx -y
     sudo systemctl start nginx
     sudo systemctl enable nginx
     ```  
   - Add a custom HTML page (optional):  
     ```bash
     echo "Welcome to Region $(curl -s http://169.254.169.254/latest/meta-data/placement/region)" | sudo tee /usr/share/nginx/html/index.html
     ```

4. **Test the Web Servers**:  
   - Access the web servers using their public IPs or DNS names.

#### Configure Route 53 for Failover  

1. **Create a Hosted Zone**:  
   - Go to the Route 53 console and create a hosted zone (e.g., `myapp.com`).  

2. **Add DNS Records**:  
   - Create two A records:  
     - One pointing to the IP of `nginx_us_east`.  
     - Another pointing to the IP of `nginx_us_west`.  

3. **Set Up Health Checks**:  
   - Create a health check for each instance (e.g., HTTP check on port 80).  

4. **Configure Failover Policy**:  
   - Set `nginx_us_east` as primary and `nginx_us_west` as secondary.  

5. **Test Failover**:  
   - Stop the `nginx_us_east` instance and check if traffic switches to `nginx_us_west`.

---

### **Exercise 3: Automating Disaster Recovery**

#### Automate Kubernetes Failover with Cross-Cluster DNS  

1. **Deploy Kubernetes Clusters in Two Regions**:  
   - Use managed services like **AWS EKS** or **GKE** in two regions.  

2. **Install KubeFed**:  
   - Federate the two clusters:  
     ```bash
     kubefedctl join cluster-east --host-cluster-context=host-cluster-context
     kubefedctl join cluster-west --host-cluster-context=host-cluster-context
     ```  

3. **Deploy an Application with Failover**:  
   - Create a DNSRecord object for the application:  
     ```yaml
     apiVersion: multiclusterdns.kubefed.io/v1alpha1
     kind: DNSRecord
     metadata:
       name: my-app
       namespace: default
     spec:
       records:
         - dnsName: my-app.example.com
           cluster: us-east
         - dnsName: my-app.example.com
           cluster: us-west
     ```

4. **Simulate Failover**:  
   - Delete pods or disrupt one cluster and observe traffic switching to the other.

---

### **Step 6: Advanced Alerting Setup**

After ensuring disaster recovery mechanisms are in place, set up **alerts** for potential issues.

#### Example Prometheus Alerts for Recovery Issues  
1. **Alert on Backup Failures**:  
   - Add the following rule in your Prometheus alert rules file:  
     ```yaml
     groups:
     - name: BackupAlerts
       rules:
       - alert: BackupFailed
         expr: time() - process_start_time_seconds{job="backup"} > 3600
         for: 5m
         labels:
           severity: critical
         annotations:
           summary: "Backup process failed to run"
           description: "Backup has not run for the past 1 hour. Investigate immediately."
     ```

2. **Alert for Cross-Region Failures**:  
   - Use blackbox_exporter to monitor the health of services in each region.  
   - Create alerts when one region becomes unreachable.

---

### Next Steps

- Complete **Exercise 2** and test the multi-region deployment.  
- Set up **alerts** in Prometheus or Grafana to monitor backup and failover processes.  

