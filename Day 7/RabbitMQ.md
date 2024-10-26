Your guide to running RabbitMQ on an Ubuntu EC2 instance using Docker looks comprehensive and clear! Here's a slightly refined version, including specific steps for changing default credentials, for better clarity:

---

### Step-by-Step Guide to Running RabbitMQ on an Ubuntu EC2 Instance Using Docker

#### Step 1: Launch an Ubuntu EC2 Instance
1. **AWS Management Console**:
   - Open the **EC2 Dashboard**.
2. **Launch a New Instance**:
   - Select **Ubuntu 20.04 LTS** or **Ubuntu 22.04 LTS**.
   - Choose an instance type, such as **t2.micro** (free-tier eligible).
3. **Configure Security Group**:
   - Add **Inbound Rules** for:
     - **Port 15672** (RabbitMQ Management Console).
     - **Port 5672** (RabbitMQ messaging).
     - **Port 22** (SSH).
4. **Launch** the instance and **copy the Public IP**.

---

#### Step 2: Connect to Your EC2 Instance
1. **SSH Connection**:
   ```bash
   ssh -i "your-key.pem" ubuntu@your-ec2-public-ip
   ```

---

#### Step 3: Update System and Install Docker
1. **Update Packages**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. **Install Docker**:
   ```bash
   sudo apt install -y docker.io
   ```
3. **Start and Enable Docker**:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

---

#### Step 4: Pull and Run RabbitMQ Docker Image
1. **Pull RabbitMQ Image**:
   ```bash
   sudo docker pull rabbitmq:3-management
   ```
2. **Run RabbitMQ Container**:
   ```bash
   sudo docker run -d --name rabbitmq \
     -p 5672:5672 \
     -p 15672:15672 \
     rabbitmq:3-management
   ```

---

#### Step 5: Verify RabbitMQ is Running
1. **Check Running Containers**:
   ```bash
   sudo docker ps
   ```
   - Ensure the RabbitMQ container is running and ports are exposed.

---

#### Step 6: Adjust Security Group Rules (if needed)
1. **Edit Security Group** in EC2 Dashboard:
   - Confirm inbound rules for ports **15672** and **5672** are set correctly.

---

#### Step 7: Access the RabbitMQ Management Console
1. **Open a Browser**:
   - Go to `http://your-ec2-public-ip:15672`.
2. **Log In**:
   - **Username**: `guest`
   - **Password**: `guest`

---

#### Optional: Change Default Credentials
1. **Access the RabbitMQ Docker Container**:
   ```bash
   sudo docker exec -it rabbitmq bash
   ```
2. **Create a New User**:
   ```bash
   rabbitmqctl add_user <new-username> <new-password>
   ```
3. **Set Permissions for the New User**:
   ```bash
   rabbitmqctl set_permissions -p / <new-username> ".*" ".*" ".*"
   ```
4. **Set Administrator Tag for the New User**:
   ```bash
   rabbitmqctl set_user_tags <new-username> administrator
   ```
5. **Delete the Default Guest User** (Optional):
   ```bash
   rabbitmqctl delete_user guest
   ```
6. **Exit the Container**:
   ```bash
   exit
   ```

#### Step 8: Access RabbitMQ Management Console with New Credentials
1. **Open a Browser**:
   - Go to `http://your-ec2-public-ip:15672`.
2. **Log In**:
   - Use your new credentials:
   - **Username**: `<new-username>`
   - **Password**: `<new-password>`

---

With these steps, you should have a secure and operational RabbitMQ setup on your Ubuntu EC2 instance.