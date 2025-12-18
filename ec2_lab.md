
# Creating and Working with an EC2 Instance — Hands-on Lab

**Provider:** A Cloud Guru / Linux Academy  
**Lab Type:** Instructional Hands-on Lab  
**Estimated Time:** ~22 minutes

---

## Introduction
In this hands-on lab, you will create and interact with an Amazon EC2 instance. The lab covers EC2 requirements, the choices available when creating EC2 instances, and the provisioning process.

---

## Solution Overview
1. Log in to the live AWS environment using the credentials provided. Ensure you are in the **N. Virginia (us-east-1)** region.
2. (Windows users) Review instructions for connecting to EC2 using **PuTTY**.

---

## Part 1 — Create Default VPC
- Navigate to **VPC → Your VPCs**.
- Click **Actions → Create Default VPC** (do **not** click *Create VPC*).
- Click **Create**, then **Close**.

---

## Part 2 — Launch EC2 Instance
- Navigate to **EC2 → Instances** and click **Launch Instance**.
- On the **AMI** page, select **Amazon Linux 2 AMI (HVM), SSD Volume Type**, ensuring **64-bit (x86)** is selected.
- Choose instance type: **t3.micro** (NOT `t3a.micro`).
- Click **Next: Configure Instance Details**.

### Configure Instance Details
- **Network**: Leave default.  
- **Subnet**: No preference.  
- **Auto-assign Public IP**: **Enable**.  
- **Tenancy**: Shared.  
- Expand **Advanced details**, and paste the following **User Data** script:

```bash
#!/bin/bash
yum update -y
yum install -y httpd
yum install -y wget
chkconfig httpd on
cd /var/www/html
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/index.html
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/catanimated.gif
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/rainbow.gif
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/penny.jpeg
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/roffle.jpeg
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/truffs.jpeg
wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/03_compute/creating_an_ec2_instance/winkie.jpeg
service httpd start
```

- Click **Next: Add Storage**; accept defaults (e.g., **8 GiB**, General Purpose SSD) or more.
- Click **Next: Add Tags**. Add the following tag:  
  - **Key**: `Name`  
  - **Value**: `Cat-Hall-o-Fame`

### Configure Security Group
- On the **Configure Security Group** page, choose **Create a new security group**:  
  - **Security group name**: `labSG`  
  - **Description**: `labSG`
- Click **Add Rule** and set:  
  - **Type**: `HTTP`  
  - **Source**: `My IP`

### Review and Launch
- Click **Review and Launch**, then **Launch**.
- In the key pair dialog:  
  - **Create a new key pair** named `ec2lab`.  
  - **Download Key Pair** (saves `ec2lab.pem`).  
  - Click **Launch Instances** → **View Instances**.

---

## Part 3 — Validate, Connect, and Manage

### Validate Web Server
- Wait until the instance is **running** with **2/2 status checks**.
- Select the instance **Cat-Hall-o-Fame**.
- In **Description**, copy the **Public DNS (IPv4)**.  
- Open it in a browser; you should see the website served by Apache.

### Connect via SSH
- Open a terminal and change to your downloads directory (e.g., `cd Downloads`).
- Adjust key permissions:  
  ```bash
  chmod 400 ec2lab.pem
  ```
- In the EC2 console, with the instance selected, click **Connect** and copy the SSH string under **Example**.  
- Paste the command into your terminal to connect (confirm `yes` at the prompt).  
- **Windows users**: use PuTTY as per linked instructions.

### Observe IP Behavior
- Note the **IPv4 Public IP** of the instance.
- **Reboot**: Right-click → **Instance State → Reboot** → confirm.  
  - Observe whether the IP changes (should **remain the same**).
- **Stop/Start**: Right-click → **Instance State → Stop** → confirm; then **Start**.  
  - Observe IP behavior (the IP may disappear while stopped and **reappear** after start; per instructions, it should be the **same IP**).

### Resize Instance
- Stop the instance: **Instance State → Stop** and wait until fully stopped.
- Right-click → **Instance Settings → Change Instance Type**.  
  - Change to **t3.small** and click **Apply**.
- Start the instance: **Instance State → Start**.

---

## Lab Checklist (Objectives)
1. **Create Default VPC** in us-east-1.  
2. **Launch Amazon Linux 2 EC2 (t3.micro)** with **User Data** to install Apache and content.  
3. **Tag** instance as `Cat-Hall-o-Fame`.  
4. **Security Group**: Allow **HTTP** from **My IP**.  
5. **Create Key Pair** `ec2lab` and connect via SSH (`chmod 400 ec2lab.pem`).  
6. Validate website via **Public DNS (IPv4)**.  
7. Manage lifecycle: **Reboot**, **Stop/Start**, observe IP consistency.  
8. **Change Instance Type** to **t3.small** and restart.

---

## Notes
- Open labs in an **incognito window** (recommended).  
- Ensure the **region** remains **N. Virginia (us-east-1)** throughout.

---

## Credits
This document captures the step-by-step instructions as presented in the lab page "Creating and Working with an EC2 Instance".
