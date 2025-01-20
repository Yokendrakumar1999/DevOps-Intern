
---

1. **Create a Domain or Subdomain to be Used for Keycloak**  
   Create an Elastic IP in AWS for Keycloak. Use a subdomain for an existing domain by creating an “A” record.  

---

2. **Create the EC2 Server in AWS Console**  
   - Login to your AWS account and create an Amazon EC2 Server.  
   - Recommended hardware: 2 core CPU & 4 GB RAM.  
   - Allow inbound TCP ports **22, 80, 443, and 8081** in the security group.  
   - Map the Elastic IP address to the newly created Keycloak Server.  
   - Update packages:  
     ```bash
     sudo apt update && sudo apt upgrade
     ```

---

3. **Install and Configure NGINX**  
   - Install NGINX:  
     ```bash
     sudo apt install nginx
     ```  
   - Check NGINX version:  
     ```bash
     nginx -v
     ```  
   - Enable UFW for NGINX and SSH:  
     ```bash
     sudo ufw allow 'Nginx Full'
     sudo ufw allow ssh
     sudo ufw enable
     sudo ufw reload
     sudo ufw status
     ```
   - Install Certbot for SSL certificates:  
     ```bash
     sudo apt install certbot python3-certbot-nginx
     sudo certbot --nginx -d keycloak.example.com -d www.keycloak.example.com
     ```  
   - Test certificate renewal:  
     ```bash
     sudo certbot renew --dry-run
     ```

---

4. **NGINX Configuration**  
   - Edit NGINX configuration for Keycloak:  
     ```bash
     sudo unlink /etc/nginx/sites-enabled/default
     sudo nano /etc/nginx/sites-available/keycloak.example.conf
     ```  
     Add the following content:  
     ```nginx
     # Redirect HTTP to HTTPS
     server {
         listen 80;
         server_name keycloak.example.com www.keycloak.example.com;

         return 301 https://keycloak.example.com$request_uri;
     }

     # HTTPS Server Block
     server {
         listen 443 ssl;
         server_name keycloak.example.com www.keycloak.example.com;

         ssl_certificate /etc/letsencrypt/live/keycloak.example.com/fullchain.pem;
         ssl_certificate_key /etc/letsencrypt/live/keycloak.example.com/privkey.pem;

         ssl_session_cache shared:SSL:10m;
         ssl_session_timeout 1d;
         ssl_protocols TLSv1.2 TLSv1.3;
         ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256';
         ssl_prefer_server_ciphers on;

         location / {
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_set_header X-Forwarded-Proto $scheme;
             proxy_pass http://publicip:8081;
             proxy_http_version 1.1;
             proxy_set_header Connection "";
             proxy_buffering off;
             proxy_request_buffering off;
         }
     }
     ```
   - Enable configuration:  
     ```bash
     sudo ln -s /etc/nginx/sites-available/keycloak.example.conf /etc/nginx/sites-enabled/keycloak.example.conf
     sudo nginx -t
     sudo systemctl restart nginx
     ```

---

5. **Install Docker and Docker Compose**  
   - Install Docker:  
     ```bash
     sudo apt-get update
     sudo apt-get install ca-certificates curl
     sudo install -m 0755 -d /etc/apt/keyrings
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
     sudo apt install docker-compose
     sudo groupadd docker
     sudo usermod -aG docker $USER
     ```
   - Create a directory for Keycloak:  
     ```bash
     mkdir ~/keycloak
     cd ~/keycloak
     ```

---

6. **Create the Docker Compose File**  
   - Create `docker-compose.yml`:  
     ```bash
     nano docker-compose.yml
     ```  
     Add the following content:  
     ```yaml
     version: '3.8'
     services:
       keycloak:
         image: quay.io/keycloak/keycloak:latest
         environment:
           KC_DB: mysql
           KC_DB_SCHEMA: keycloak
           KC_DB_USERNAME: auth_admin
           KC_DB_PASSWORD: "Authdb_1234"
           KC_DB_URL_HOST: keycloak.cfw91zqgxoak.us-east-2.rds.amazonaws.com
           KC_DB_URL_PORT: 3306
           KEYCLOAK_ADMIN: mindme
           KEYCLOAK_ADMIN_PASSWORD: "X8$kJ2!qW#9pB@vL"
           KC_PROXY_HEADERS: xforwarded
           KC_HTTP_ENABLED: "true"
           KC_HOSTNAME_STRICT: "false"
           KC_HOSTNAME_URL: https://keycloak.example.com
         ports:
           - 8443:8443
           - 8081:8080
         container_name: keycloak
         restart: always
         networks:
           - keycloak-auth
         command: start
         healthcheck:
           test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
           interval: 30s
           timeout: 10s
           retries: 3
     networks:
       keycloak-auth:
         driver: bridge
     ```

---

Let me know if you need further clarification!