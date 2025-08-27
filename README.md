# Solution: Setting Up Custom Domains with AWS Route 53  
This repository demonstrates how to configure custom domains for both a GitHub Pages site and an AWS EC2 static web server using AWS Route 53 DNS management. Includes step-by-step instructions, DNS record setup, and server configuration examples.

# Project URL:
```
https://roadmap.sh/projects/basic-dns
```
*For GitHub Pages and AWS EC2 Static Site Server Projects*

This Solution will guide configuring a **custom domain** for:  
1. **GitHub/GitHub Action Pages Project**  
2. **AWS EC2 Static Site Server Project**

Need to **own a domain name** before proceeding.  
Since we are using **AWS Route 53**, you'll create your DNS records inside the AWS console.

---

## Prerequisites

- A registered domain name in AWS Route 53.  
- Access to the AWS Management Console.  
- Completed the **GitHub Pages project** (for Task #1).  
- Completed the **Static Site Server project using AWS EC2** (for Task #2).

---

## Task #1 – Custom Domain for GitHub Pages

### Step 1: Verify Your GitHub Pages Site
Make sure your GitHub Pages site is live at:  
https://sumit0920.github.io/gh-deployment-workflow/
<img width="1120" height="996" alt="Screenshot 2025-08-26 083939" src="https://github.com/user-attachments/assets/19db7861-d6f0-4797-8ad6-7ecaa03c989a" />


---

### Step 2: Add Custom Domain in GitHub
1. Go to your repository on GitHub.  
2. Navigate to **Settings → Pages**.  
3. Under **Custom domain**, enter your domain (e.g. `www.example.com`).  
4. Click **Save**.  
<img width="1091" height="170" alt="image" src="https://github.com/user-attachments/assets/488982e9-259d-4685-a587-339c8640a2be" />


---

### Step 3: Configure DNS in AWS Route 53
1. Open **Route 53 → Hosted Zones**.  
2. Click your domain name (e.g. `example.com`).  
3. Click **Create record**.  
4. Choose **Record type: CNAME**.  
5. Enter:  
   - **Record name:** `www` (or your chosen subdomain)  
   - **Value:** `sumit0920.github.io`  
   - **TTL:** leave default (e.g. 300 seconds)  
6. Save the record.  

Example Route 53 record:  
Type: CNAME
Name: www
Value: sumit0920.github.io
TTL: 300
<img width="1315" height="383" alt="image" src="https://github.com/user-attachments/assets/6e6868cd-0ca3-43a4-b97d-88392bed19a0" />


---

### Step 4: Test the Domain
Wait for DNS propagation (usually 5–30 minutes).  
Visit `https://www.example.com` — it should load your GitHub Pages site.  
<img width="1246" height="803" alt="image" src="https://github.com/user-attachments/assets/190ed141-264e-4f3a-b99c-9a12a69c0d29" />


---

## Task #2 – Custom Domain for AWS EC2 Static Site Server

### Step 1: Verify Your Static Site on EC2
Ensure your AWS EC2 instance serves your static site using its **public IP**:  
http://your-ec2-public-ip
<img width="1451" height="791" alt="Screenshot 2025-08-27 084327" src="https://github.com/user-attachments/assets/82921e2e-a62b-4858-9407-815f5f1b4d22" />


---

### Step 2: Get Your EC2 Public IP
Log in to the AWS Management Console → **EC2 → Instances → Select your instance** → copy the **Public IPv4 address**.  
<img width="1662" height="487" alt="Screenshot 2025-08-27 091358" src="https://github.com/user-attachments/assets/f266401d-3d13-4b53-9ec3-55291e3ff2d5" />


---

### Step 3: Configure DNS in AWS Route 53
1. Open **Route 53 → Hosted Zones**.  
2. Click your domain (e.g. `example.com`).  
3. Click **Create record**.  
4. Choose **Record type: A (IPv4 address)**.  
5. Enter:  
   - **Record name:** `@` (for root domain) or `www` (for subdomain)  
   - **Value:** `<Your EC2 Public IP>`  
   - **TTL:** leave default (e.g. 300 seconds)  
6. Save the record.  

Example Route 53 records:  
Type: A
Name: @
Value: 3.109.45.123
TTL: 300
Type: A
Name: www
Value: 3.109.45.123
TTL: 300
<img width="1364" height="336" alt="Screenshot 2025-08-27 232937" src="https://github.com/user-attachments/assets/f5180bfb-5169-4283-b822-e98eb30fec9b" />

---

### Step 4: Configure Web Server (Optional but Recommended)
If you are using **Nginx** or **Apache**, update your configuration to serve your domain:  

**Nginx Example:**
```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Then restart Nginx:
```
sudo systemctl restart nginx
```
________________________________________
Step 5: Test the Domain
Wait for DNS propagation.
Visit http://example.com — it should now display your static site from EC2.

 <img width="1875" height="975" alt="image" src="https://github.com/user-attachments/assets/df02665b-ce64-476b-93af-261bc6f44f2d" />

________________________________________
Troubleshooting
•	DNS not propagating? Use https://dnschecker.org/ to verify.
•	SSL required? Use Let’s Encrypt to enable HTTPS on your EC2 server.
•	Still not working? Double-check your Route 53 records and security group (allow inbound HTTP port 80).
________________________________________
Summary
•	For GitHub Pages, create a CNAME record in Route 53 pointing to username.github.io.
•	For AWS EC2 servers, create A records in Route 53 pointing to your EC2 Public IP.
•	Test both with your domain and configure HTTPS if needed.
Congratulations! 🎉 You now have custom domains configured for both GitHub Pages and AWS EC2 projects.

---


