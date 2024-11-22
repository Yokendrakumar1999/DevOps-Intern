To modify the file upload size for a **Spring Boot** application deployed on **Amazon Elastic Beanstalk** as a microservice, follow these step-by-step instructions:

---

### **Step 1: Update Spring Boot Application Configuration**

Spring Boot has a default file upload size limit of **1 MB** for multipart file uploads. You can increase this limit to **10 MB** by updating the `application.properties` or `application.yml` file.

1. **Modify `application.properties`**:

   Add the following properties:
   ```properties
   spring.servlet.multipart.max-file-size=10MB
   spring.servlet.multipart.max-request-size=10MB
   ```

2. **Alternatively, if using `application.yml`**:
   ```yaml
   spring:
     servlet:
       multipart:
         max-file-size: 10MB
         max-request-size: 10MB
   ```

   - `max-file-size`: Defines the maximum size of a single uploaded file.
   - `max-request-size`: Sets the maximum size of the entire request (useful for forms with multiple file uploads).

3. Commit and save these changes in your Spring Boot application.

---

### **Step 2: Update NGINX Configuration in Elastic Beanstalk**

Elastic Beanstalk uses **NGINX** as a reverse proxy by default. NGINX has a `client_max_body_size` directive that restricts the size of the HTTP request body, including file uploads. This limit is likely set to **1 MB** by default and must be increased to **10 MB**.

#### Steps to Update NGINX Configuration:

1. **Create an `.ebextensions` Directory**:
   If it doesn’t already exist, create a folder named `.ebextensions` in your project root.

2. **Create a Configuration File**:
   Inside the `.ebextensions` directory, create a file named `01_nginx.config` (the name can vary, but it must have the `.config` extension).

3. **Add Configuration to the File**:
   Add the following content to update the NGINX configuration:

   ```yaml
   files:
     "/etc/nginx/conf.d/proxy.conf":
       mode: "000644"
       owner: root
       group: root
       content: |
         client_max_body_size 10M;
   ```

   - This configuration tells Elastic Beanstalk to create or update the file `/etc/nginx/conf.d/proxy.conf` on the EC2 instances to set `client_max_body_size` to **10 MB**.

4. Save the file and commit the changes to your version control.

---

### **Step 3: Redeploy the Application**

1. **Package the Application**:
   Ensure your updated Spring Boot application and the `.ebextensions` directory are included in the deployment package (e.g., a `.jar` or `.zip` file).

2. **Deploy the Updated Application**:
   - Go to the AWS Elastic Beanstalk console.
   - Select your environment for the "Mindme" microservice.
   - Click **Upload and Deploy**, and upload the updated deployment package.

3. Elastic Beanstalk will apply the new NGINX configuration and deploy the updated Spring Boot application.

---

### **Step 4: Verify the Changes**

1. **Test File Upload**:
   Try uploading a file larger than **1 MB** but less than **10 MB** through your application's file upload functionality to confirm that the new limit is working.

2. **Check Logs** (if the upload fails):
   - Open the Elastic Beanstalk console.
   - Navigate to **Logs** and download the logs from the EC2 instance to troubleshoot issues.
   - Alternatively, SSH into the EC2 instance and inspect the NGINX logs or application logs.

---

### **Step 5: Optional Adjustments**

#### **Auto Scaling and Resource Configuration**:
If your application handles many large file uploads, monitor CPU and memory usage and adjust the EC2 instance type or configure **Auto Scaling** to ensure your environment can handle the load.

#### **Clean Up Temporary Files**:
Ensure your application cleans up temporary files after handling uploads to avoid unnecessary disk usage.

---

### **Summary**

1. **Update Spring Boot Configuration**:
   Modify `application.properties` or `application.yml` to set:
   ```properties
   spring.servlet.multipart.max-file-size=10MB
   spring.servlet.multipart.max-request-size=10MB
   ```

2. **Update NGINX Configuration**:
   Use `.ebextensions/01_nginx.config` to set:
   ```yaml
   client_max_body_size 10M;
   ```

3. **Deploy and Test**:
   Deploy the updated application to Elastic Beanstalk and verify that file uploads up to 10 MB work.

By following these steps, you'll successfully increase the file upload limit to **10 MB** for your Mindme Spring Boot microservice on Elastic Beanstalk. Let me know if you need further assistance!