
# Host a Static Website Using NGINX on an AWS EC2 Instance 

This guide provides step-by-step instructions for hosting a static website using the NGINX server on a Linux AWS EC2 instance.

---

## 1. Launch an EC2 Instance
1. **Log in to the AWS Management Console.**
2. **Navigate to EC2 Dashboard** and click **Launch Instance**.
3. **Configure the Instance:**
   - Choose an Amazon Machine Image (AMI): **Ubuntu** or **Amazon Linux**.
   - Select an instance type (e.g., `t2.micro` for free-tier eligibility).
   - Configure the security group:
     - Add rules to allow HTTP (port 80) and SSH (port 22) access.
4. Complete the launch process and connect to your instance via SSH.

---

## 2. Connect to the Instance
Use SSH to connect to the EC2 instance:
```bash
ssh -i your-key.pem ubuntu@<ec2-public-ip>
```

---

## 3. Update the System
Update the package manager to ensure the latest software is installed:
```bash
sudo apt update && sudo apt upgrade -y  # For Ubuntu
sudo yum update -y                     # For Amazon Linux
```

---

## 4. Install NGINX
Install the NGINX server:
```bash
sudo apt install nginx -y  # For Ubuntu
sudo yum install nginx -y  # For Amazon Linux
```

---

## 5. Start and Enable NGINX
Start the NGINX service and ensure it runs on boot:
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Check if NGINX is running:
```bash
sudo systemctl status nginx
```

---

## 6. Configure NGINX for the Static Website
1. **Create a directory** to host the static website files:
   ```bash
   sudo mkdir -p /var/www/static-site
   ```
   
2. **Set permissions**:
   ```bash
   sudo chown -R $USER:$USER /var/www/static-site
   ```

3. **Add your static website files**:
   Upload your `index.html` and other assets to `/var/www/static-site` using tools like SCP or manually create them:
   ```bash
   echo "<h1>Welcome to My Static Website</h1>" > /var/www/static-site/index.html
   ```

4. **Create an NGINX server block configuration file**:
   ```bash
   sudo nano /etc/nginx/sites-available/static-site
   ```
   Add the following configuration:
   ```nginx
   server {
       listen 80;
       server_name your-domain-or-ip;

       root /var/www/static-site;
       index index.html;

       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```

5. **Enable the configuration**:
   Create a symbolic link to enable the site:
   ```bash
   sudo ln -s /etc/nginx/sites-available/static-site /etc/nginx/sites-enabled/
   ```

6. **Disable the default configuration** (optional):
   ```bash
   sudo unlink /etc/nginx/sites-enabled/default
   ```

7. **Test the NGINX configuration**:
   ```bash
   sudo nginx -t
   ```

8. **Reload NGINX**:
   ```bash
   sudo systemctl reload nginx
   ```

---

## 7. Open the Website in a Browser
- Copy the **public IP address** of your EC2 instance.
- Open a browser and navigate to `http://<ec2-public-ip>`.

---

## 8. Optional: Use a Custom Domain
To use a custom domain:
1. Update the `server_name` in the NGINX configuration to your domain.
2. Configure your domain's DNS to point to your EC2 instance's public IP.
3. Reload NGINX:
   ```bash
   sudo systemctl reload nginx
   ```

---

## 9. (Optional) Secure with SSL
To add HTTPS:
1. Install Certbot:
   ```bash
   sudo apt install certbot python3-certbot-nginx -y  # Ubuntu
   sudo yum install certbot python3-certbot-nginx -y  # Amazon Linux
   ```
2. Obtain and configure an SSL certificate:
   ```bash
   sudo certbot --nginx -d your-domain
   ```
3. Test auto-renewal:
   ```bash
   sudo certbot renew --dry-run
   ```

---

You now have a fully functional static website hosted on NGINX on your AWS EC2 instance!
