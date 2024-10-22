It seems that the `aws-cli` is not installed on your system, and you're encountering issues while trying to authenticate Docker with AWS ECR.

Here are the steps to install the AWS CLI and fix the login issue:

### Step 1: Install AWS CLI
Since you're on a Debian/Ubuntu-based system (as suggested by the use of `apt`), you can install AWS CLI using the following command:

```bash
sudo apt update
sudo apt install awscli
```

If you prefer to install the latest version, follow these steps to install AWS CLI version 2:

1. **Download the AWS CLI package**:
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   ```
   
2. **Unzip the install file**:
   ```bash
   apt install unzip 
   ```
3. **Unzip the downloaded file**:
   ```bash
   unzip awscliv2.zip
   ```

4. **Install AWS CLI**:
   ```bash
   sudo ./aws/install
   ```

5. **Verify the installation**:
   ```bash
   aws --version
   ```

### Step 2: Authenticate Docker with AWS ECR
Once the AWS CLI is installed, you can authenticate Docker with AWS ECR. Run the following command:

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 784390659805.dkr.ecr.us-east-1.amazonaws.com
```

If you're getting the "Cannot perform an interactive login from a non TTY device" error, it could be related to the way you are trying to run the command. 


The error message indicates that the AWS CLI is installed, but you have not configured the AWS credentials on the server.

Here’s how you can resolve it:

### Step 1: Configure AWS CLI Credentials
You need to provide your AWS credentials to the AWS CLI by running the `aws configure` command. This will set up your access and secret keys for AWS services.

1. Run the following command:
   ```bash
   aws configure
   ```

2. You will be prompted to enter the following:
   - **AWS Access Key ID**: Enter your AWS access key ID.
   - **AWS Secret Access Key**: Enter your AWS secret access key.
   - **Default region name**: Set the default region (e.g., `us-east-1`).
   - **Default output format**: You can set this to `json` (or leave it blank).

   You can find your **AWS Access Key ID** and **Secret Access Key** in the AWS Management Console under **IAM > Users > Security credentials** (assuming you already have an IAM user with sufficient permissions).

### Step 2: Authenticate Docker to ECR
Once AWS credentials are configured, you can authenticate Docker with AWS ECR again:

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 784390659805.dkr.ecr.us-east-1.amazonaws.com
```

### Step 3: (If Still Seeing "Non TTY Device" Error)
If the `non TTY device` error persists, it may indicate you are running the command in a context that does not support interactive commands (e.g., inside a script or CI pipeline). To bypass this:

1. Make sure you're running the command directly from an interactive terminal.
2. Alternatively, you can store the password output manually and pass it to `docker login` like this:
   ```bash
   $(aws ecr get-login-password --region us-east-1) | docker login --username AWS --password-stdin 784390659805.dkr.ecr.us-east-1.amazonaws.com
   ```

After these steps, Docker should successfully authenticate to ECR. Let me know if you encounter any further issues!