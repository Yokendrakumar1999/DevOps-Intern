Here's a step-by-step guide to create **Auto Scaling**, **Target Group**, and **Load Balancer** in AWS.

---

## **Step 1: Create a Target Group**
A **Target Group** is used to route traffic to one or more registered targets (like EC2 instances) behind a load balancer.

### **Step 1.1: Navigate to Target Groups**
1. **Log in to the AWS Management Console**.
2. **Go to the EC2 Dashboard**.
3. **Under "Load Balancing"** on the left sidebar, click **Target Groups**.

### **Step 1.2: Create a Target Group**
1. **Click "Create target group"**.
2. **Configure Target Group**:
   - **Target type**: Choose **Instance** if you’re targeting EC2 instances or **IP** if you're targeting IP addresses.
   - **Target group name**: Enter a meaningful name (e.g., `web-server-target-group`).
   - **Protocol**: Select the protocol for the traffic (e.g., **HTTP** or **HTTPS**).
   - **Port**: Enter the port number (e.g., `80` for HTTP).
   - **VPC**: Select the VPC in which your instances reside.
   
3. **Health Checks**:
   - Set the **Health Check protocol** (typically HTTP or HTTPS).
   - Configure other health check settings (default is often sufficient).

4. **Click "Create"**.

---

## **Step 2: Create a Load Balancer**
A **Load Balancer** distributes traffic across multiple targets, such as EC2 instances, in one or more Availability Zones.

### **Step 2.1: Navigate to Load Balancers**
1. **Go to the EC2 Dashboard**.
2. **Under "Load Balancing"** on the left sidebar, click **Load balancers**.

### **Step 2.2: Create a Load Balancer**
1. **Click "Create Load Balancer"**.
2. **Choose the Load Balancer type**:
   - **Application Load Balancer (ALB)**: Best for HTTP/HTTPS traffic.
   - **Network Load Balancer (NLB)**: Best for TCP/UDP traffic or extremely high-performance scenarios.
   - **Classic Load Balancer**: Older type, now less common.
   
3. **Configure Load Balancer**:
   - **Name**: Enter a name for your load balancer (e.g., `web-server-alb`).
   - **Scheme**: Choose whether the load balancer is **internet-facing** (public) or **internal** (private).
   - **Listeners**: By default, HTTP (port 80) will be created. You can add HTTPS (port 443) as well.
   - **VPC**: Select the VPC where your EC2 instances and target group are located.
   - **Availability Zones**: Select the Availability Zones where your instances are located.
   
4. **Security Groups**:
   - Attach an existing security group or create a new one to allow traffic (e.g., HTTP on port 80).

5. **Configure Routing**:
   - Under **Default action**, select the target group you created earlier.

6. **Click "Create"**.

---

## **Step 3: Create an Auto Scaling Group**
An **Auto Scaling Group** ensures your application scales by automatically adjusting the number of instances based on demand.

### **Step 3.1: Navigate to Auto Scaling Groups**
1. **Go to the EC2 Dashboard**.
2. **Under "Auto Scaling"** on the left sidebar, click **Auto Scaling Groups**.

### **Step 3.2: Create an Auto Scaling Group**
1. **Click "Create Auto Scaling group"**.
2. **Choose a launch template or configuration**:
   - **Launch template**: Use an existing launch template that specifies instance type, AMI, and other configurations.
   - **Launch configuration**: Alternatively, create a launch configuration (not recommended for new setups).
   
3. **Configure Auto Scaling Group**:
   - **Group Name**: Provide a name for your Auto Scaling group (e.g., `web-server-auto-scaling`).
   - **Launch template or configuration**: Choose the launch template you created earlier (if applicable).
   - **VPC**: Select the VPC and subnets.
   - **Load Balancer**: Select the **Application Load Balancer** you created in Step 2.
   - **Target Group**: Choose the target group you created earlier.

4. **Set Scaling Policies**:
   - **Desired Capacity**: Set the initial number of instances (e.g., 2).
   - **Minimum Size**: Set the minimum number of instances (e.g., 1).
   - **Maximum Size**: Set the maximum number of instances (e.g., 5).

5. **Configure Advanced Options**:
   - Set other options such as health checks, instance monitoring, and scaling policies.

6. **Create Auto Scaling Group**:
   - Review your settings and click **Create Auto Scaling group**.

---

## **Step 4: Verify and Test Setup**
1. **Check Auto Scaling**:
   - Go to **Auto Scaling Groups** and verify your group is created with the correct settings.
   
2. **Check Load Balancer**:
   - In the **Load Balancers** section, verify that your ALB is configured correctly and that it is routing traffic to the correct target group.
   
3. **Test Traffic Distribution**:
   - Once instances are running, you can test the load balancer by navigating to its DNS name (e.g., `web-server-alb-1234567890.us-west-1.elb.amazonaws.com`).

4. **Monitor Auto Scaling**:
   - The Auto Scaling Group will automatically adjust the number of instances based on demand, scaling up or down as needed.

---

## **Step 5: Set Up CloudWatch Alarms (Optional)**
For better control over the Auto Scaling behavior, you can set up CloudWatch alarms to trigger scaling actions based on metrics like CPU utilization.

1. **Navigate to CloudWatch**.
2. **Create Alarms**:
   - Set alarms for metrics like **CPU utilization**, **network traffic**, etc., and associate these alarms with Auto Scaling policies to scale in or out based on the conditions.

---

## **Conclusion**
You’ve now set up a scalable web application using AWS Auto Scaling, Target Groups, and Load Balancers! This setup will automatically manage traffic distribution and ensure high availability for your application.