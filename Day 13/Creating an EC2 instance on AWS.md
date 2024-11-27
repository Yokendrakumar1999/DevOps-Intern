Here's the corrected and streamlined version of your guide:

---

### **Step 1: Create a Key Pair**
1. **Navigate to EC2 Dashboard**:
   - Log in to the [AWS Management Console](https://aws.amazon.com/console/).
   - Search for **EC2** and open the EC2 service.

2. **Create a Key Pair**:
   - Click **Key Pairs** under **Network & Security** on the left sidebar.
   - Click **Create Key Pair** and provide:
     - **Name**: (e.g., `my-key-pair`).
     - **Key Pair Type**: Choose **RSA** (commonly used) or **ED25519**.
     - **Private Key File Format**:
       - **PEM**: Use for Linux/MacOS/OpenSSH.
       - **PPK**: Use for Windows (PuTTY users).
   - Click **Create Key Pair**.

3. **Download and Save Key File**:
   - A `.pem` or `.ppk` file will automatically download.
   - Save it securely (you cannot download it again).
   - For `.pem`, set permissions using:
     ```bash
     chmod 400 your-key.pem
     ```

---

### **Step 2: Create a Security Group**
1. **Navigate to Security Groups**:
   - On the EC2 dashboard, click **Security Groups** under **Network & Security**.

2. **Create a Security Group**:
   - Click **Create Security Group**.
   - Provide a **name** (e.g., `web-server-sg`) and a **description**.
   - Select your **VPC** (default VPC is fine if no custom VPC is created).

3. **Configure Inbound Rules**:
   - Add rules based on your requirements:
     - **SSH (Port 22)**:
       - Protocol: TCP
       - Port Range: 22
       - Source: Your IP (or `0.0.0.0/0` for global access; not recommended for security).
     - **HTTP (Port 80)**:
       - Protocol: TCP
       - Port Range: 80
       - Source: `0.0.0.0/0` (public access for web servers).
     - **HTTPS (Port 443)**:
       - Protocol: TCP
       - Port Range: 443
       - Source: `0.0.0.0/0` (for SSL/TLS traffic).
   - Add additional rules if needed.

4. **Configure Outbound Rules**:
   - Keep the default rule allowing all outbound traffic unless specific restrictions are required.

5. **Create Security Group**:
   - Click **Create Security Group**.

---

### **Step 3: Launch EC2 Instance**
1. **Navigate to EC2 Dashboard**:
   - Click **Launch Instance** under the "Instances" section.

2. **Configure the Instance**:
   - **Instance Name**: Enter a meaningful name (e.g., `web-server-instance`).
   - **AMI**: Choose an Amazon Machine Image (e.g., **Amazon Linux 2** for free tier).
   - **Instance Type**: Select (e.g., `t2.micro` for free tier or another instance type as needed).
   - **Key Pair**: Choose the previously created key pair.
   - **Network Settings**: Select the **Security Group** you just created.
   - **Storage**: Configure the root volume size (default is 8 GiB).

3. **Launch the Instance**:
   - Review the configurations and click **Launch Instance**.

---

### **Step 4: Connect to the EC2 Instance**
1. **Locate the Instance**:
   - In the **Instances** section, find your running instance.
   - Copy the **Public IP Address** or **Public DNS**.

2. **Connect via SSH**:
   - Use a terminal and run the command:
     ```bash
     ssh -i "your-key.pem" ec2-user@<public-ip>
     ```
   - Replace `<public-ip>` with your instance's IP address.

3. **Alternative - Use AWS Session Manager**:
   - If configured, click **Connect** from the AWS Management Console.

---

### **Step 5: Additional Configurations**
1. Install software or dependencies as needed:
   - For example, install a web server:
     ```bash
     sudo yum update -y
     sudo yum install httpd -y
     sudo systemctl start httpd
     sudo systemctl enable httpd
     ```

2. Deploy your application or set up services.

